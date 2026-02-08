<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valentine Interactive</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;}
body{
  height:100vh;
  display:flex;
  align-items:center;
  justify-content:center;
  background:linear-gradient(135deg,#ffd1dc,#ffe4e1,#e0c3fc);
  overflow:hidden;
  transition:background 0.6s ease;
}
.container{
  width:90%;
  max-width:520px;
  text-align:center;
  background:rgba(255,255,255,0.18);
  backdrop-filter:blur(18px);
  border-radius:28px;
  padding:45px 30px;
  box-shadow:0 12px 35px rgba(0,0,0,0.18);
  position:relative;
  animation:floatBox 4s ease-in-out infinite;
}
@keyframes floatBox{0%,100%{transform:translateY(0);}50%{transform:translateY(-8px);}}
h1{font-size:2rem;margin-bottom:15px;}
.message{margin-top:20px;font-size:0.95rem;line-height:1.7;opacity:0;transform:translateY(15px);transition:all 0.8s ease;}
.message.show{opacity:1;transform:translateY(0);}
.emoji{font-size:2.3rem;margin-bottom:10px;animation:heartbeat 1.2s infinite;display:block;}
@keyframes heartbeat{0%,100%{transform:scale(1);}50%{transform:scale(1.25);}}
button{padding:12px 30px;border:none;border-radius:30px;font-size:1rem;cursor:pointer;transition:all 0.3s ease;box-shadow:0 6px 18px rgba(0,0,0,0.2);margin:10px;}
button:hover{transform:translateY(-3px) scale(1.05);}
.yes-btn{background:#ff4d6d;color:white;position:relative;z-index:2;}
.no-btn{background:#6c757d;color:white;position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);z-index:1;}
.hidden{display:none;}
canvas{position:fixed;top:0;left:0;pointer-events:none;}
.heart{
  position:absolute;
  font-size:20px;
  animation:rise 3s linear forwards;
}
@keyframes rise{
  0%{opacity:1; transform:translateY(0) scale(1);}
  100%{opacity:0; transform:translateY(-150px) scale(1.5);}
}
</style>
</head>
<body>
<div class="container">
    <span id="heartEmoji" class="emoji">💖</span>
    <h1>Will you be my Valentine?</h1>

    <div id="buttons">
        <button class="yes-btn" id="yesBtn">YES</button>
        <button class="no-btn" id="noBtn">NO</button>
    </div>

    <div id="loveMessage" class="message"></div>
</div>

<canvas id="confetti"></canvas>

<script>
let noClicks = 0;
const yesBtn = document.getElementById('yesBtn');
const noBtn = document.getElementById('noBtn');
const buttonsDiv = document.getElementById('buttons');
const msg = document.getElementById('loveMessage');
const canvas = document.getElementById('confetti');
const ctx = canvas.getContext('2d');
let confettiPieces = [];
let confettiActive = false;

// Resize canvas
function resize(){canvas.width=window.innerWidth;canvas.height=window.innerHeight;}
resize();
window.addEventListener('resize', resize);

// Move NO button randomly
noBtn.addEventListener('mouseenter', ()=>{
  const x = Math.random()*(window.innerWidth - 100);
  const y = Math.random()*(window.innerHeight - 50);
  noBtn.style.left = x + 'px';
  noBtn.style.top = y + 'px';
});

// YES button grows when NO is hovered
noBtn.addEventListener('mouseenter', ()=>{
  noClicks++;
  let newSize = 1 + noClicks*0.3;
  yesBtn.style.transform = `scale(${newSize})`;
  yesBtn.style.transition='transform 0.3s ease';
});

// YES button click
yesBtn.addEventListener('click', ()=>{
  msg.innerHTML=`<b>By clicking YES you agree to:</b><br><br>
  • Forever being my favorite person ❤️<br>
  • Unlimited hugs 🤗<br>
  • Night Walk karenge sath me<br>
  • Jhyada kalesh mat karna<br>
  • holding hands ✋❤️<br>
  • I LOVE YOU MY LOVE<br>
  <br>
  <b>Valid for lifetime. No cancellation. 💌</b>`;
  msg.classList.add('show');
  buttonsDiv.classList.add('hidden');
  startConfetti();
  startHeartParticles();
});

// Confetti animation
function startConfetti(){
  confettiActive = true;
  confettiPieces = Array.from({length:150},()=>({x:Math.random()*canvas.width,y:Math.random()*canvas.height-canvas.height,r:Math.random()*6+4}));
  animateConfetti();
  setTimeout(()=>{confettiActive=false;ctx.clearRect(0,0,canvas.width,canvas.height);},5000);
}

function animateConfetti(){
  if(!confettiActive) return;
  ctx.clearRect(0,0,canvas.width,canvas.height);
  ctx.fillStyle='rgba(255,105,180,0.9)';
  confettiPieces.forEach(p=>{
    ctx.beginPath();
    ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fill();
    p.y+=2.5;
    if(p.y>canvas.height)p.y=-10;
  });
  requestAnimationFrame(animateConfetti);
}

// Heart particles
function startHeartParticles(){
  const container = document.querySelector('.container');
  let count = 0;
  const interval = setInterval(()=>{
    if(count>20){clearInterval(interval); return;}
    const heart = document.createElement('span');
    heart.className='heart';
    heart.style.left=Math.random()*80 + '%';
    heart.style.fontSize=(10+Math.random()*20)+'px';
    heart.textContent='❤️';
    container.appendChild(heart);
    setTimeout(()=>heart.remove(),3000);
    count++;
  },150);
}
</script>
</body>
</html>
