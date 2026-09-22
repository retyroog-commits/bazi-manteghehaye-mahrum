bazi-manteghehaye-mahrum
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>click to 10000000000!</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Tahoma,Arial,sans-serif;background:linear-gradient(135deg,#17172b,#24244a);color:white;text-align:center;min-height:100vh;padding:18px}
.game{max-width:650px;margin:auto}
h1{margin:8px 0 18px;font-size:30px}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:15px}
.card,.shop{background:#ffffff12;border:1px solid #ffffff22;border-radius:18px;padding:14px;box-shadow:0 8px 25px #0004}
.big{font-size:25px;font-weight:bold}
#clickButton{width:90%;max-width:420px;padding:22px;border:0;border-radius:22px;background:#ffcc33;color:#171717;font-size:25px;font-weight:bold;cursor:pointer;box-shadow:0 7px 0 #b48b00;margin:10px 0 24px}
#clickButton:active{transform:translateY(6px);box-shadow:0 1px 0 #b48b00}
.shop h2{margin-top:4px}
.item{background:#0003;border-radius:15px;padding:12px;margin:10px 0}
.item button{width:100%;padding:11px;border:0;border-radius:12px;background:#5ee6a8;color:#10261c;font-weight:bold;font-size:16px;cursor:pointer}
.item button:disabled{opacity:.45;cursor:not-allowed}
.level{font-size:13px;color:#ddd;margin:5px}
#rainNotice{min-height:30px;color:#ffd95a;font-weight:bold}
#reset{margin-top:15px;padding:9px 18px;border:1px solid #ffffff33;background:#ffffff10;color:white;border-radius:10px}
</style>
</head>
<body>
<div class="game">
<h1>🎮 بازی مناطق محروم</h1>

<div class="stats">
  <div class="card">💰 پول<br><span id="money" class="big">0</span></div>
  <div class="card">💥 قدرت ضربه<br><span id="power" class="big">5</span></div>
</div>

<button id="clickButton">💥 بزن!</button>

<div id="rainNotice">🌧️ باران پول: هنوز فعال نشده</div>

<div class="shop">
<h2>🛒 فروشگاه</h2>

<div class="item">
  <b>💪 ارتقای قدرت ضربه</b>
  <div class="level">سطح: <span id="powerLevel">0</span> | قیمت: <span id="powerCost">25</span> 💰</div>
  <button id="powerBtn">خرید</button>
</div>

<div class="item">
  <b>🤖 اتوکلیکر</b>
  <div class="level">سطح: <span id="autoLevel">0</span> | سرعت: <span id="autoSpeed">خاموش</span></div>
  <div class="level">قیمت: <span id="autoCost">100</span> 💰</div>
  <button id="autoBtn">خرید</button>
</div>

<div class="item">
  <b>🌧️ باران پول</b>
  <div class="level">سطح: <span id="rainLevel">0</span> | فاصله: <span id="rainSpeed">خاموش</span></div>
  <div class="level">قیمت: <span id="rainCost">150</span> 💰</div>
  <button id="rainBtn">خرید</button>
</div>
</div>

<button id="reset">🔄 شروع دوباره</button>
</div>

<script>
let money = 0;
let power = 5;
let powerLevel = 0;

let autoLevel = 0;
let rainLevel = 0;

let autoTimer = null;
let rainTimer = null;

/* هزینه‌ها */
function powerCost(){ return 25 + powerLevel * 35; }
function autoCost(){ return 100 + autoLevel * 150; }
function rainCost(){ return 150 + rainLevel * 100; }

/* سرعت اتوکلیکر: 1s → 0.75s → 0.5s → 0.25s → 0.1s */
const autoIntervals = [1000,750,500,250,100];

/* سرعت باران: سطح 1 = 20 ثانیه، سطح 20 = 1 ثانیه */
