
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEON DASH 🚀</title>
<style>
*{box-sizing:border-box}
body{
 margin:0;background:#050514;color:white;
 font-family:Arial;text-align:center;
 overflow:hidden;
}
h1{color:#00eaff;text-shadow:0 0 15px #00eaff}
#game{
 display:block;margin:10px auto;
 width:min(95vw,420px);
 height:min(65vh,580px);
 background:linear-gradient(#10102e,#050510);
 border:2px solid #00eaff;
 border-radius:14px;
 box-shadow:0 0 20px #00eaff55;
}
.stats{display:flex;justify-content:center;gap:25px}
button{
 background:#00d9ff;color:#050514;
 border:0;padding:12px 22px;
 border-radius:10px;font-weight:bold;
 margin:5px;cursor:pointer;
}
#over{display:none;color:#ff4b85}
</style>
</head>
<body>

<h1>🚀 NEON DASH</h1>

<div class="stats">
 <span>⭐ Score: <b id="score">0</b></span>
 <span>❤️ Lives: <b id="lives">3</b></span>
 <span>🏆 Best: <b id="best">0</b></span>
</div>

<canvas id="game" width="420" height="580"></canvas>

<h2 id="over">GAME OVER!</h2>
<button onclick="startGame()">▶ START / RESTART</button>
<div>
 <button onclick="move(-1)">⬅ LEFT</button>
 <button onclick="move(1)">RIGHT ➡</button>
</div>

<script>
const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

let player,rocks,coins,score,lives,running;
let best=Number(localStorage.getItem("neonBest"))||0;
document.getElementById("best").textContent=best;

function startGame(){
 player={x:190,y:510,w:40,h:40};
 rocks=[];coins=[];score=0;lives=3;
 running=true;
 document.getElementById("over").style.display="none";
 updateStats();
 requestAnimationFrame(loop);
}

function move(dir){
 if(running) player.x=Math.max(5,Math.min(375,player.x+dir*35));
}

document.addEventListener("keydown",e=>{
 if(e.key==="ArrowLeft"||e.key.toLowerCase()==="a")move(-1);
 if(e.key==="ArrowRight"||e.key.toLowerCase()==="d")move(1);
});

function updateStats(){
 document.getElementById("score").textContent=score;
 document.getElementById("lives").textContent=lives;
}

function collision(a,b){
 return a.x<b.x+b.w&&a.x+a.w>b.x&&
 a.y<b.y+b.h&&a.y+a.h>b.y;
}

function loop(){
 if(!running)return;
 ctx.clearRect(0,0,420,580);

 // Background stars
 for(let i=0;i<35;i++){
  ctx.fillStyle="#ffffff55";
  ctx.fillRect((i*83)%420,(i*137+score*2)%580,2,2);
 }

 // Player spaceship
 ctx.font="38px Arial";
 ctx.fillText("🚀",player.x,player.y+35);

 // Generate obstacles
 if(Math.random()<0.035)
 rocks.push({x:Math.random()*380,y:-40,w:35,h:35,s:3+score/300});

 // Generate coins
 if(Math.random()<0.018)
 coins.push({x:Math.random()*380,y:-25,w:25,h:25});

 // Rocks
 rocks.forEach((r,i)=>{
  r.y+=r.s;
  ctx.font="30px Arial";
  ctx.fillText("☄️",r.x,r.y+30);

  if(collision(player,r)){
   rocks.splice(i,1);lives--;updateStats();
   if(lives<=0)endGame();
  }
 });

 // Coins
 coins.forEach((c,i)=>{
  c.y+=3;
  ctx.font="25px Arial";
  ctx.fillText("⚡",c.x,c.y+25);

  if(collision(player,c)){
   coins.splice(i,1);score+=10;updateStats();
  }
 });

 // Score
 score+=1;
 updateStats();

 rocks=rocks.filter(r=>r.y<620);
 coins=coins.filter(c=>c.y<620);

 requestAnimationFrame(loop);
}

function endGame(){
 running=false;
 document.getElementById("over").style.display="block";
 if(score>best){
  best=score;
  localStorage.setItem("neonBest",best);
  document.getElementById("best").textContent=best;
 }
}

startGame();
</script>
</body>
</html># CodeLab
