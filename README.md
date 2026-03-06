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

      
    
   [Uploading<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>水平传送带模型</title>
    <style>
        *{box-sizing:border-box;font-family:system-ui}
        body{padding:20px;background:#f7f8fb}
        .container{max-width:1000px;margin:0 auto}
        .scene{height:240px;background:white;border:2px solid #dbe2ef;border-radius:12px;position:relative}
        .belt{position:absolute;bottom:80px;left:100px;right:100px;height:10px;background:#444}
        .block{position:absolute;bottom:90px;left:120px;width:70px;height:70px;background:#ff7a7a;border-radius:6px}
        .control{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-top:10px}
        .item{background:white;padding:10px;border-radius:8px}
        label{display:inline-block;width:180px}
        input{width:200px}
        .value{display:inline-block;width:60px;text-align:right;font-weight:bold;color:#1d4ed8}
        .btn-box{text-align:center;margin-top:16px}
        button{padding:10px 20px;margin:0 6px;border:none;border-radius:8px;background:#3b82f6;color:white}
        .info{background:#eef4ff;padding:12px;border-radius:8px;margin-top:10px}
    </style>
</head>
<body>
<div class="container">
    <h2>水平传送带模型</h2>
    <div class="scene">
        <div class="belt"></div>
        <div class="block" id="b"></div>
    </div>
    <div class="control">
        <div class="item">
            <label>传送带速度 v (m/s)</label>
            <input type="range" min="1" max="8" value="3" id="v">
            <span id="vv" class="value">3</span>
        </div>
        <div class="item">
            <label>摩擦因数 μ</label>
            <input type="range" min="0" max="1" value="0.3" step="0.01" id="mu">
            <span id="muu" class="value">0.30</span>
        </div>
    </div>
    <div class="btn-box">
        <button id="go">释放物块</button>
        <button id="reset">重置</button>
    </div>
    <div class="info">
        加速度 a = <b id="a">0</b> m/s² ｜ 速度 = <b id="bv">0</b> m/s ｜ 状态：<b id="st">静止</b>
    </div>
</div>
<script>
let b=document.getElementById("b"), v=document.getElementById("v"), mu=document.getElementById("mu");
let x=120, vx=0, run=false;
function update(){document.getElementById("vv").textContent=v.value;document.getElementById("muu").textContent=mu.value}
v.oninput=update; mu.oninput=update;
function calc(){
    let μ=parseFloat(mu.value), V=parseFloat(v.value);
    let a=μ*9.8;
    if(vx<V){ vx += a*0.016; if(vx>V)vx=V; }
    x += vx*50;
    b.style.left=x+"px";
    document.getElementById("a").textContent=vx<V ? a.toFixed(2):0;
    document.getElementById("bv").textContent=vx.toFixed(2);
    document.getElementById("st").textContent=vx<V ? "加速":"共速匀速"
}
function loop(){if(!run)return;calc();requestAnimationFrame(loop)}
document.getElementById("go").onclick=()=>{run=true;loop()}
document.getElementById("reset").onclick=()=>{run=false;x=120;vx=0;b.style.left="120px"}
update();
</script>
</body>
</html> Code_20260307(3).html…]()
