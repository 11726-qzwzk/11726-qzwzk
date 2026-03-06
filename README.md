[Code_20260307(2).html](https://github.com/user-attachments/files/25800349/Code_20260307.2.html)## Hi there 👋

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

      
    
   [Uploading Code_20260307(2).ht<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>超重失重物理模型</title>
    <style>
        *{box-sizing:border-box;font-family:system-ui}
        body{padding:20px;background:#f7f8fb}
        .container{max-width:800px;margin:0 auto}
        .lift-box{height:400px;background:white;border:2px solid #dbe2ef;border-radius:12px;position:relative}
        .lift{position:absolute;left:50%;width:160px;height:220px;background:#94a3b8;transform:translateX(-50%);bottom:20px}
        .person{position:absolute;left:50%;transform:translateX(-50%);bottom:40px;width:50px;height:80px;background:#ff7a7a;border-radius:25px}
        .control{margin-top:10px}
        .item{background:white;padding:10px;border-radius:8px;margin-bottom:8px}
        label{display:inline-block;width:200px}
        input{width:220px}
        .value{display:inline-block;width:70px;text-align:right;font-weight:bold;color:#1d4ed8}
        .btn-box{text-align:center;margin-top:16px}
        button{padding:10px 20px;margin:0 6px;border:none;border-radius:8px;background:#3b82f6;color:white}
        .info{background:#eef4ff;padding:12px;border-radius:8px;margin-top:10px}
    </style>
</head>
<body>
<div class="container">
    <h2>超重与失重（电梯模型）</h2>
    <div class="lift-box">
        <div class="lift" id="lift"></div>
        <div class="person" id="p"></div>
    </div>
    <div class="control">
        <div class="item">
            <label>电梯加速度 a (m/s²)</label>
            <input type="range" min="-6" max="6" value="0" step="0.1" id="a">
            <span id="av" class="value">0.0</span>
        </div>
    </div>
    <div class="btn-box">
        <button id="start">运行</button>
        <button id="reset">重置</button>
    </div>
    <div class="info">
        支持力 N = <b id="N">98</b> N<br>
        状态：<b id="status">静止（正常）</b>
    </div>
</div>
<script>
const a=document.getElementById("a"), av=document.getElementById("av");
let lift=document.getElementById("lift"), p=document.getElementById("p");
let running=false, y=0, v=0;
a.oninput=()=>{av.textContent=a.value}
function calc(){
    let aa=parseFloat(a.value);
    let g=9.8, m=10;
    let N=m*(g+aa);
    document.getElementById("N").textContent=N.toFixed(1);
    let s="静止（正常）";
    if(aa>0.2) s="超重（加速上升/减速下降）";
    if(aa<-0.2) s="失重（减速上升/加速下降）";
    if(aa<-9.7) s="完全失重";
    document.getElementById("status").textContent=s;
    v += aa*0.016;
    y += v*0.016;
    lift.style.bottom=(20+y*30)+"px";
}
function loop(){
    if(!running) return;
    calc(); requestAnimationFrame(loop);
}
document.getElementById("start").onclick=()=>{running=true;loop()}
document.getElementById("reset").onclick=()=>{running=false;y=0;v=0;lift.style.bottom="20px"}
</script>
</body>
</html>ml…]()

