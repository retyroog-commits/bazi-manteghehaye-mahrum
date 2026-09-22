# bazi-manteghehaye-mahrum
from pathlib import Path
import zipfile

site = Path("/mnt/data/bazi_manteghehaye_mahrum_site")
site.mkdir(exist_ok=True)

html = r'''<!doctype html>
<html lang="fa" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#17172b">
<title>بازی مناطق محروم</title>
<meta name="description" content="بازی کلیکی و دیوانه‌وار بازی مناطق محروم! پول جمع کن و ارتقا بخر.">
<style>
*{box-sizing:border-box}body{margin:0;min-height:100vh;font-family:Tahoma,Arial,sans-serif;background:radial-gradient(circle at top,#30305d,#11111f 65%);color:#fff;padding:18px}
main{max-width:680px;margin:auto}.hero{text-align:center}.hero h1{font-size:clamp(28px,7vw,44px);margin:8px 0}.hero p{opacity:.8;margin:0 0 18px}
.stats{display:grid;grid-template-columns:1fr 1fr;gap:12px}.card,.shop{background:#ffffff10;border:1px solid #ffffff1f;border-radius:20px;padding:16px;box-shadow:0 10px 30px #0004;backdrop-filter:blur(8px)}
.label{opacity:.8}.big{display:block;font-size:clamp(25px,7vw,38px);font-weight:800;margin-top:5px;overflow-wrap:anywhere}
#clickButton{display:block;width:min(94%,480px);margin:22px auto;padding:24px;border:0;border-radius:24px;background:#ffd43b;color:#171717;font-size:clamp(22px,6vw,30px);font-weight:900;box-shadow:0 8px 0 #b68f00;cursor:pointer;touch-action:manipulation}
#clickButton:active{transform:translateY(7px);box-shadow:0 1px 0 #b68f00}
.notice{text-align:center;min-height:30px;color:#ffd95a;font-weight:bold;margin-bottom:12px}
.shop h2{text-align:center;margin:4px 0 15px}.item{background:#0003;border-radius:16px;padding:14px;margin:11px 0}.item b{font-size:18px}.info{font-size:13px;color:#ddd;margin:7px 0 10px}
button.buy{width:100%;padding:12px;border:0;border-radius:12px;background:#62e6aa;color:#10261c;font-size:16px;font-weight:800;cursor:pointer}
button.buy:disabled{opacity:.45;cursor:not-allowed}
#reset{display:block;margin:15px auto 5px;padding:9px 17px;border-radius:10px;border:1px solid #ffffff33;background:#ffffff0d;color:#fff;cursor:pointer}
footer{text-align:center;opacity:.55;font-size:12px;margin-top:18px}
</style>
</head>
<body>
<main>
<section class="hero">
<h1>🎮 بازی مناطق محروم</h1>
<p>پول جمع کن، ارتقا بخر، اقتصاد بازی رو منفجر کن! 💰</p>
</section>

<section class="stats">
<div class="card"><span class="label">💰 پول</span><span id="money" class="big">0</span></div>
<div class="card"><span class="label">💥 قدرت ضربه</span><span id="power" class="big">5</span></div>
</section>

<button id="clickButton">💥 بزن!</button>
<div id="rainNotice" class="notice">🌧️ باران پول: هنوز فعال نشده</div>

<section class="shop">
<h2>🛒 فروشگاه</h2>

<div class="item">
<b>💪 ارتقای قدرت ضربه</b>
<div class="info">سطح: <span id="powerLevel">0</span> | قیمت: <span id="powerCost">25</span> 💰</div>
<button class="buy" id="powerBtn">خرید</button>
</div>

<div class="item">
<b>🤖 اتوکلیکر</b>
<div class="info">سطح: <span id="autoLevel">0</span> | سرعت: <span id="autoSpeed">خاموش</span> | قیمت: <span id="autoCost">100</span> 💰</div>
<button class="buy" id="autoBtn">خرید</button>
</div>

<div class="item">
<b>🌧️ باران پول</b>
<div class="info">سطح: <span id="rainLevel">0</span> | فاصله: <span id="rainSpeed">خاموش</span> | قیمت: <span id="rainCost">150</span> 💰</div>
<button class="buy" id="rainBtn">خرید</button>
</div>
</section>

<button id="reset">🔄 شروع دوباره</button>
<footer>نسخه وب — بازی مناطق محروم</footer>
</main>

<script>
let money=0,power=5,powerLevel=0,autoLevel=0,rainLevel=0;
let autoTimer=null,rainTimer=null;

const autoIntervals=[1000,750,500,250,100];

const $=id=>document.getElementById(id);
const powerCost=()=>25+powerLevel*35;
const autoCost=()=>100+autoLevel*150;
const rainCost=()=>150+rainLevel*100;
const addMoney=n=>{money+=n;update();};

$("clickButton").onclick=()=>addMoney(power);

$("powerBtn").onclick=()=>{
 const c=powerCost();
 if(money>=c){money-=c;powerLevel++;power+=5;update();}
};

$("autoBtn").onclick=()=>{
 if(autoLevel>=5)return;
 const c=autoCost();
 if(money>=c){money-=c;autoLevel++;startAuto();update();}
};

function startAuto(){
 if(autoTimer)clearInterval(autoTimer);
 if(autoLevel>0)autoTimer=setInterval(()=>addMoney(power),autoIntervals[autoLevel-1]);
}

$("rainBtn").onclick=()=>{
 if(rainLevel>=19)return;
 const c=rainCost();
 if(money>=c){money-=c;rainLevel++;startRain();update();}
};

function startRain(){
 if(rainTimer)clearInterval(rainTimer);
 if(rainLevel>0){
  const ms=(20-rainLevel)*1000;
  rainTimer=setInterval(()=>{
   const bonus=power*5;
   addMoney(bonus);
   $("rainNotice").textContent="🌧️ باران پول! +"+bonus+" 💰";
   setTimeout(update,700);
  },ms);
 }
}

function update(){
 $("money").textContent=Math.floor(money).toLocaleString("en-US");
 $("power").textContent=Math.floor(power).toLocaleString("en-US");
 $("powerLevel").textContent=powerLevel;
 $("powerCost").textContent=powerCost().toLocaleString("en-US");

 $("autoLevel").textContent=autoLevel;
 $("autoCost").textContent=autoLevel>=5?"MAX":autoCost().toLocaleString("en-US");
 $("autoSpeed").textContent=autoLevel?autoIntervals[autoLevel-1]/1000+" ثانیه":"خاموش";
 $("autoBtn").disabled=autoLevel>=5;

 $("rainLevel").textContent=rainLevel;
 $("rainCost").textContent=rainLevel>=19?"MAX":rainCost().toLocaleString("en-US");
 $("rainSpeed").textContent=rainLevel?(20-rainLevel)+" ثانیه":"خاموش";
 $("rainBtn").disabled=rainLevel>=19;

 if(rainLevel===0)$("rainNotice").textContent="🌧️ باران پول: هنوز فعال نشده";
 else if(!$("rainNotice").textContent.includes("!"))$("rainNotice").textContent="🌧️ باران پول هر "+(20-rainLevel)+" ثانیه";
}

$("reset").onclick=()=>{
 money=0;power=5;powerLevel=0;autoLevel=0;rainLevel=0;
 if(autoTimer)clearInterval(autoTimer);if(rainTimer)clearInterval(rainTimer);
 autoTimer=null;rainTimer=null;update();
};
update();
</script>
</body>
</html>'''

(site/"index.html").write_text(html, encoding="utf-8")
readme = """# بازی مناطق محروم — نسخه وب

فایل اصلی سایت: index.html

برای انتشار، کل این پوشه را روی یک سرویس میزبانی سایت استاتیک آپلود کنید.
"""
(site/"README.txt").write_text(readme, encoding="utf-8")

zip_path=Path("/mnt/data/bazi_manteghehaye_mahrum_web.zip")
with zipfile.ZipFile(zip_path,"w",zipfile.ZIP_DEFLATED) as z:
    for f in site.iterdir():
        z.write(f, f.name)

print(f"پکیج آماده شد: {zip_path}")
