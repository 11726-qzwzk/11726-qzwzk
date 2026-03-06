[Code_20260307(3).html](https://github.com/user-attachments/files/25800424/Code_20260307.3.html)[Code_20260307(2).html](https://github.com/user-attachments/files/25800349/Code_20260307.2.html)## Hi there 👋

<!--
**11726-qzwzk/11726-qzwzk** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

      
    
[Code_20260307(4).html](https://github.com/user-attachments/files/25800465/Code_20260307.4.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>连接体物理模型</title>
    <style>
        *{box-sizing:border-box;font-family:system-ui}
        body{margin:0;padding:20px;background:#f7f8fb}
        .container{max-width:1000px;margin:0 auto}
        h2{text-align:center}
        .scene{width:100%;height:320px;background:#fff;border:2px solid #dbe2ef;border-radius:12px;position:relative;overflow:hidden}
        .ground{position:absolute;bottom:60px;left:0;right:0;height:4px;background:#555}
        .table{position:absolute;bottom:64px;left:150px;width:500px;height:120px;background:#94a3b8;border-radius:6px}
        .rope{position:absolute;top:110px;left:250px;width:200px;height:2px;background:#333}
        .m1{position:absolute;bottom:124px;left:200px;width:70px;height:70px;background:#ff7a7a;border-radius:6px}
        .m2{position:absolute;top:100px;left:600px;width:70px;height:70px;background:#7eaaff;border-radius:6px}
        .control{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-top:10px}
        .item{background:#fff;padding:10px;border-radius:8px;border:1px solid #dbe2ef}
        label{display:inline-block;width:180px}
        input[type="range"]{width:200px}
        .value{display:inline-block;width:60px;text-align:right;font-weight:bold;color:#1d4ed8}
        .btn-box{text-align:center;margin-top:16px}
        button{padding:10px 20px;margin:0 6px;border:none;border-radius:8px;background:#3b82f6;color:#fff;cursor:pointer}
        .gray{background:#64748b}
        .info{background:#eef4ff;padding:12px;border-radius:8px;margin-top:10px;line-height:1.7}
    </style>
</head>
<body>
<div class="container">
    <h2>连接体模型（两物体+轻绳）物理仿真</h2>
    <div class="scene">
        <div class="ground"></div>
        <div class="table"></div>
        <div class="rope" id="rope"></div>
        <div class="m1" id="m1"></div>
        <div class="m2" id="m2"></div>
    </div>

    <div class="control">
        <div class="item">
            <label>物块1质量 m₁ (kg)</label>
            <input type="range" id="ma" min="0.2" max="5" value="1" step="0.1">
            <span id="ma_val" class="value">1.0</span>
        </div>
        <div class="item">
            <label>悬挂物块 m₂ (kg)</label>
            <input type="range" id="mb" min="0.1" max="3" value="0.5" step="0.1">
            <span id="mb_val" class="value">0.5</span>
        </div>
        <div class="item">
            <label>桌面摩擦因数 μ</label>
            <input type="range" id="mu" min="0" max="0.6" value="0.1" step="0.01">
            <span id="mu_val" class="value">0.10</span>
        </div>
    </div>

    <div class="btn-box">
        <button id="start">开始</button>
        <button id="pause" class="gray">暂停</button>
        <button id="reset">重置</button>
    </div>

    <div class="info">
        整体加速度 a = <b id="a">0</b> m/s² ｜ 
        绳子拉力 T = <b id="T">0</b> N ｜<br>
        速度 v = <b id="v">0</b> m/s ｜ 
        位移 x = <b id="x">0</b> m
    </div>
</div>

<script>
const ma=document.getElementById("ma");
const mb=document.getElementById("mb");
const mu=document.getElementById("mu");
const ma_val=document.getElementById("ma_val");
const mb_val=document.getElementById("mb_val");
const mu_val=document.getElementById("mu_val");
const rope=document.getElementById("rope");
const m1=document.getElementById("m1");
const m2=document.getElementById("m2");

let running=false;
let x=0, v=0, a=0, T=0;
const dt=0.016;
const g=9.8;

function showVals(){
    ma_val.textContent=parseFloat(ma.value).toFixed(1);
    mb_val.textContent=parseFloat(mb.value).toFixed(1);
    mu_val.textContent=parseFloat(mu.value).toFixed(2);
}
ma.addEventListener("input",showVals);
mb.addEventListener("input",showVals);
mu.addEventListener("input",showVals);

function calc(){
    let m1m=parseFloat(ma.value);
    let m2m=parseFloat(mb.value);
    let muu=parseFloat(mu.value);
    
    a = (m2m*g - muu*m1m*g) / (m1m + m2m);
    T = m1m*(a + muu*g);
    
    v += a*dt;
    x += v*dt;
}

function render(){
    const scale=100;
    const base1=200;
    const base2=100;
    
    m1.style.left = (base1 + x*scale) + "px";
    m2.style.top  = (base2 + x*scale) + "px";
    rope.style.width = (400 + x*scale) + "px";

    document.getElementById("a").textContent=a.toFixed(2);
    document.getElementById("T").textContent=T.toFixed(2);
    document.getElementById("v").textContent=v.toFixed(2);
    document.getElementById("x").textContent=x.toFixed(3);
}

function loop(){
    if(!running) return;
    calc();
    render();
    requestAnimationFrame(loop);
}

document.getElementById("start").onclick=()=>{running=true;loop()};
document.getElementById("pause").onclick=()=>{running=false};
document.getElementById("reset").onclick=()=>{
    running=false; x=0; v=0; a=0; T=0; render();
};

showVals();
render();
</script>
</body>
</html>
