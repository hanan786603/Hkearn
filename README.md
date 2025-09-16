<!doctype html>
<html lang="ur">
<head>
  <meta charset="utf-8" />
  <title>Watch-to-Earn (PTC) Prototype</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    body { font-family: "Noto Nastaliq Urdu", sans-serif; direction: rtl; padding: 20px; max-width: 720px; margin:auto; }
    .card { border:1px solid #ddd; padding:16px; border-radius:8px; margin-bottom:12px; box-shadow: 0 2px 6px rgba(0,0,0,0.04); }
    button { padding:10px 14px; border-radius:6px; cursor:pointer; }
  </style>
</head>
<body>
  <h1>Watch-to-Earn (PTC) پروٹو ٹائپ</h1>

  <div class="card">
    <p><strong>آپ کے کریڈٹس:</strong> <span id="credits">0</span></p>
    <p id="status">حاضر</p>
  </div>

  <div class="card">
    <p>1) اشتہار دیکھ کر کریڈٹ حاصل کریں (یہاں سیمولیٹ کیا گیا ہے)</p>
    <button id="watchBtn">اشتہار دیکھیں (Simulate)</button>
  </div>

  <div class="card">
    <p>2) ریڈیم کریں</p>
    <button id="redeemBtn">10 کریڈٹس پر ایک ڈجیٹل پرک حاصل کریں</button>
    <p id="redeemMsg"></p>
  </div>

  <div class="card">
    <p><strong>تاریخچہ:</strong></p>
    <ul id="history"></ul>
  </div>

<script>
  // سادہ لوکل اسٹوریج بیسڈ والیٹ (صرف پروٹو ٹائپ کے لیے)
  const creditsEl = document.getElementById('credits');
  const statusEl = document.getElementById('status');
  const historyEl = document.getElementById('history');
  const watchBtn = document.getElementById('watchBtn');
  const redeemBtn = document.getElementById('redeemBtn');
  const redeemMsg = document.getElementById('redeemMsg');

  // init
  let data = JSON.parse(localStorage.getItem('ptc_demo') || '{"credits":0,"history":[]}');
  function save(){ localStorage.setItem('ptc_demo', JSON.stringify(data)); render(); }
  function render(){
    creditsEl.textContent = data.credits;
    historyEl.innerHTML = data.history.slice().reverse().map(h => `<li>${h}</li>`).join('') || '<li>کوئی ریکارڈ نہیں</li>';
  }

  // Simulate watching rewarded ad
  watchBtn.addEventListener('click', async () => {
    statusEl.textContent = 'اشتہار چل رہا ہے... (سیمولیٹ)';
    watchBtn.disabled = true;

    // یہاں آپ حقیقی rewarded ad SDK کال لگائیں گے؛ کامیابی پر reward دیں
    // سیمولیشن: 5 سیکنڈ انتظار پھر انعام
    await new Promise(r => setTimeout(r, 5000));

    const reward = 2; // مثال کے طور پر 2 کریڈٹس فی ویڈیو
    data.credits += reward;
    data.history.push(`+${reward} کریڈٹس (Watch) — ${new Date().toLocaleString()}`);
    save();

    statusEl.textContent = 'اشتہار مکمل۔ کریڈٹس مل گئے۔';
    watchBtn.disabled = false;
  });

  // Redeem button
  redeemBtn.addEventListener('click', () => {
    if (data.credits >= 10){
      data.credits -= 10;
      data.history.push(`-10 کریڈٹس ریڈیم کیے — ${new Date().toLocaleString()}`);
      save();
      redeemMsg.textContent = 'مبارک ہو! آپ نے ایک ڈجیٹل پرک ریڈیم کر لی۔';
    } else {
      redeemMsg.textContent = 'کافی کریڈٹس نہیں — مزید دیکھیں اور کمائیں۔';
    }
    setTimeout(()=> redeemMsg.textContent = '', 4000);
  });

  render();
</script>
</body>
</html>
