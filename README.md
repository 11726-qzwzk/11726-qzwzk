## Hi there 👋

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
[Code_20260307(1).html](https://github.com/user-attachments/files/25800130/Code_20260307.1.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>斜面传送带物理模型</title>
    <style>
        *{box-sizing:border-box;font-family:system-ui}
        body{margin:0;padding:20px;background:#f7f8fb}
        .container{max-width:1000px;margin:0 auto}
        h2{text-align:center}
        .scene{width:100%;height:360px;background:white;border:2px solid #dbe2ef;border-radius:12px;position:relative;overflow:hidden}
        .belt{position:absolute;bottom:100px;left:50%;width:600px;height:10px;background:#333;transform:translateX(-50%) rotate(0deg);transform-origin:left center}
        .block{position:absolute;width:70px;height:70px;background:#ff7a7a;border-radius:6px}
        .control{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-top:10px}
        .item{background:white;padding:10px;border-radius:8px;border:1px solid #dbe2ef}
        label{display:inline-block;width:180px}
        input[type="range"]{width:200px}
        .value{display:inline-block;width:60px;text-align:right;font-weight:bold;color:#1d4ed8}
        .btn-box{text-align:center;margin-top:16px}
        button{padding:10px 20px;margin:0 6px;border:none;border-radius:8px;background:#3b82f6;color:white;cursor:pointer}
        .gray{background:#64748b}
        .info{background:#eef4ff;padding:12px;border-radius:8px;margin-top:10px;line-height:1.7}
    </style>
</head>
<body>
<div class="container">
    <h2>斜面传送带模型（可交互仿真）</h2>
    <div class="scene">
        <div class="belt" id="belt"></div>
        <div class="block" id="block"></div>
    </div>

    <div class="control">
        <div class="item">
            <label>斜面倾角 θ (度)</label>
            <input type="range" id="angle" min="0" max="40" value="20">
            <span id="angle_val" class="value">20</span>
        </div>
        <div class="item">
            <label>物块质量 m (kg)</label>
            <input type="range" id="m" min="0.2" max="5" value="1" step="0.1">
            <span id="m_val" class="value">1.0</span>
        </div>
        <div class="item">
            <label>动摩擦因数 μ</label>
            <input type="range" id="mu" min="0" max="1" value="0.2" step="0.01">
            <span id="mu_val" class="value">0.20</span>
        </div>
        <div class="item">
            <label>传送带速度 v₀ (m/s)</label>
            <input type="range" id="v0" min="0" max="8" value="2" step="0.1">
            <span id="v0_val" class="value">2.0</span>
        </div>
    </div>

    <div class="btn-box">
        <button id="start">开始</button>
        <button id="pause" class="gray">暂停</button>
        <button id="reset">重置</button>
    </div>

    <div class="info">
        加速度 a = <b id="a">0</b> m/s² ｜ 速度 v = <b id="v">0</b> m/s ｜ 位移 s = <b id="s">0</b> m<br>
        受力状态：<b id="state">静止</b>
    </div>
</div>

<script>
const angle=document.getElementById("angle"), m=document.getElementById("m"), mu=document.getElementById("mu"), v0=document.getElementById("v0");
const angle_val=document.getElementById("angle_val"), m_val=document.getElementById("m_val"), mu_val=document.getElementById("mu_val"), v0_val=document.getElementById("v0_val");
const belt=document.getElementById("belt"), block=document.getElementById("block");
let running=false, s=0, v=0, a=0, dt=0.016;

function show(){
    angle_val.textContent=angle.value;
    m_val.textContent=m.value;
    mu_val.textContent=mu.value;
    v0_val.textContent=v0.value;
    belt.style.transform=`translateX(-50%) rotate(${angle.value}deg)`;
}
angle.addEventListener("input",show); m.addEventListener("input",show); mu.addEventListener("input",show); v0.addEventListener("input",show);

function calc(){
    let θ=angle.value*Math.PI/180;
    let mm=parseFloat(m.value), μ=parseFloat(mu.value), v00=parseFloat(v0.value);
    let g=9.8;
    let sin=Math.sin(θ), cos=Math.cos(θ);
    a=g*(sin - μ*cos);
    if(a<0) a=0;
    v += a*dt;
    s += v*dt;
}

function render(){
    let θ=angle.value*Math.PI/180;
    let x=300 + s*100*Math.cos(θ);
    let y=200 - s*100*Math.sin(θ);
    block.style.left=x+"px";
    block.style.top=y+"px";
    document.getElementById("a").textContent=a.toFixed(2);
    document.getElementById("v").textContent=v.toFixed(2);
    document.getElementById("s").textContent=s.toFixed(2);
    document.getElementById("state").textContent=v>0.1?"加速下滑":"平衡静止";
}

function loop(){
    if(!running) return;
    calc(); render(); requestAnimationFrame(loop);
}
document.getElementById("start").onclick=()=>{running=true;loop()};
document.getElementById("pause").onclick=()=>{running=false};
document.getElementById("reset").onclick=()=>{running=false;s=0;v=0;a=0;render()};
show(); render();
</script>
</body>
</html>
