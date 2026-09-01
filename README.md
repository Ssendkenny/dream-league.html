# dream-league.html
It's a simple game. l just created on phone
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>DLS 2026 V5 ULTRA REAL</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial}
body{background:#02060f;color:#fff;overflow:hidden}
#top{background:linear-gradient(90deg,#000,#002a5c);padding:8px;display:flex;justify-content:space-between;font-size:13px}
#game{position:relative;width:100%;height:78vh;background:#000}
canvas{width:100%;height:100%}
#joy{position:absolute;left:15px;bottom:22px;width:110px;height:110px;background:rgba(0,0,0,0.4);border-radius:50%;border:2px solid rgba(255,255,255,0.4)}
#knob{position:absolute;width:56px;height:56px;background:radial-gradient(#fff,#999);border-radius:50%;left:27px;top:27px;box-shadow:0 5px 12px #000}
#ctrl{position:absolute;right:10px;bottom:18px;display:grid;grid-template-columns:1fr 1fr;gap:8px}
.cbtn{width:84px;height:46px;border-radius:24px;border:none;font-weight:bold;box-shadow:0 4px 10px #000}
#hud{background:#000;display:flex;justify-content:space-between;padding:7px;font-size:12px}
</style>
</head>
<body>
<div id="top"><span>⚽ DLS 2026 V5 ULTRA REAL</span><span style="background:gold;color:#000;padding:3px 10px;border-radius:12px">💰2500 | 84 OVR</span><span>Kawanda FC</span></div>
<div id="game">
<canvas id="c" width="900" height="700"></canvas>
<div id="joy"><div id="knob"></div></div>
<div id="ctrl">
<button class="cbtn" style="background:#00c853" id="sprint">SPRINT</button>
<button class="cbtn" style="background:#ffab00;color:#000" id="skill">SKILL</button>
<button class="cbtn" style="background:#2962ff" id="pass">PASS</button>
<button class="cbtn" style="background:#00bcd4" id="thru">THRU</button>
<button class="cbtn" style="background:#d50000" id="shoot">SHOOT</button>
<button class="cbtn" style="background:#444" id="switch">SWITCH</button>
</div>
</div>
<div id="hud"><span id="score">KAWANDA FC 0-0 VIPERS • NAMBOOLE 42,000</span><span id="sta">STA 92% • 4-3-3 • YOU: #9 MUKWALA</span></div>

<script>
let canvas=document.getElementById('c'),ctx=canvas.getContext('2d');
let joy=document.getElementById('joy'),knob=document.getElementById('knob');
let vec={x:0,y:0},drag=false,anim=0,ctrl=8;
let ball={x:450,y:520,vx:0,vy:0};
let yellows=[],blues=[];

function init(){
 // 11 yellow - real formation
 let yPos=[[450,600],[200,520],[350,520],[550,520],[700,520],[300,400],[450,380],[600,400],[250,260],[450,220],[650,260]];
 let bPos=[[450,100],[200,180],[350,180],[550,180],[700,180],[300,300],[450,320],[600,300],[250,440],[450,480],[650,440]];
 for(let i=0;i<11;i++){
  yellows.push({x:yPos[i][0],y:yPos[i][1],n:[1,2,4,5,3,6,8,10,7,9,11][i],skin:i%2==0?'#8d5524':'#6b3a1f',h: 5.9 + Math.random()*0.5});
  blues.push({x:bPos[i][0],y:bPos[i][1],n:[1,2,4,5,3,6,8,10,7,9,11][i],skin:'#e8c39e',h:6.0});
 }
}
init();

function drawStadiumPro(){
 // night sky
 let g=ctx.createLinearGradient(0,0,0,130); g.addColorStop(0,'#020a1a'); g.addColorStop(1,'#0a2a5a'); ctx.fillStyle=g; ctx.fillRect(0,0,900,130);
 // stadium lights glare
 ctx.fillStyle='rgba(255,255,230,0.18)'; ctx.beginPath(); ctx.ellipse(200,80,120,40,0,0,Math.PI*2); ctx.fill(); ctx.beginPath(); ctx.ellipse(700,80,120,40,0,0,Math.PI*2); ctx.fill();
 // crowd upper - detailed
 for(let x=0;x<900;x+=18){ctx.fillStyle=x%36==0?'#ffcc00':'#222'; ctx.fillRect(x,15,14,10); ctx.fillStyle='#000'; ctx.fillRect(x+4,18,4,6);}
 // pitch with 4 shades + mowing
 let base='#2a9a2a'; ctx.fillStyle=base; ctx.fillRect(0,130,900,570);
 for(let y=130;y<700;y+=28){ctx.fillStyle=y%56==0?'#2fb12f':'#259025'; ctx.fillRect(0,y,900,14);}
 // lines crisp white
 ctx.strokeStyle='#fff'; ctx.lineWidth=4; ctx.strokeRect(30,150,840,520);
 ctx.beginPath(); ctx.moveTo(30,410); ctx.lineTo(870,410); ctx.stroke();
 ctx.beginPath(); ctx.arc(450,410,80,0,Math.PI*2); ctx.stroke();
 ctx.strokeRect(280,150,340,130); ctx.strokeRect(280,540,340,130);
 ctx.strokeRect(350,150,200,60); ctx.strokeRect(350,610,200,60);
 // goals with net
 ctx.fillStyle='#fff'; ctx.fillRect(380,150,140,8); ctx.fillRect(380,662,140,8);
 ctx.strokeStyle='rgba(255,255,255,0.4)'; for(let i=0;i<8;i++){ctx.beginPath(); ctx.moveTo(380+i*20,150); ctx.lineTo(380+i*20,158); ctx.stroke();}
 // ads boards - moving
 ctx.fillStyle='#ff0000'; ctx.fillRect(30,135,860,14); ctx.fillStyle='#fff'; ctx.font='bold 10px Arial'; ctx.fillText('AIRTEL 5G • MTN MOMO • SPORTPESA • FOR GOD AND MY COUNTRY • NAMBOOLE 42,180 • DLS 2026',40,144);
}

function drawUltraHuman(x,y,skin,color,num,isYou,phase){
 // shadow large
 ctx.fillStyle='rgba(0,0,0,0.4)'; ctx.beginPath(); ctx.ellipse(x,y+32,18,7,0,0,Math.PI*2); ctx.fill();
 let legSwing=Math.sin(phase)*14, armSwing=Math.sin(phase+1)*10;
 // legs with knees
 ctx.fillStyle=skin;
 // thigh
 ctx.fillRect(x-8+legSwing*0.2,y+10,8,18); ctx.fillRect(x+2-legSwing*0.2,y+10,8,18);
 // calf
 ctx.fillRect(x-7+legSwing*0.4,y+26,7,16); ctx.fillRect(x+3-legSwing*0.4,y+26,7,16);
 // boots with studs
 ctx.fillStyle=isYou?'#ffcc00':'#ffffff'; ctx.fillRect(x-9+legSwing*0.4,y+40,11,6); ctx.fillRect(x+2-legSwing*0.4,y+40,11,6);
 ctx.fillStyle='#000'; for(let i=0;i<3;i++){ctx.fillRect(x-7+legSwing*0.4+i*3,y+44,1,2);}
 // shorts
 ctx.fillStyle=color=='yellow'?'#000':'#001a66'; ctx.fillRect(x-12,y-2,24,15);
 // jersey body - bigger
 ctx.fillStyle=color=='yellow'?'#ffcc00':'#1646ff';
 ctx.fillRect(x-16,y-26,32,27);
 // pattern stripes for kawanda
 if(color=='yellow'){ctx.fillStyle='#000'; ctx.fillRect(x-7,y-26,6,27); ctx.fillRect(x+3,y-26,6,27);}
 // jersey sleeves
 ctx.fillRect(x-22,y-24,8,10); ctx.fillRect(x+14,y-24,8,10);
 // arms with elbows
 ctx.fillStyle=skin;
 ctx.fillRect(x-24+armSwing*0.2,y-20,7,16); ctx.fillRect(x+17-armSwing*0.2,y-20,7,16);
 // forearms
 ctx.fillRect(x-23+armSwing*0.3,y-6,6,14); ctx.fillRect(x+17-armSwing*0.3,y-6,6,14);
 // head big
 ctx.beginPath(); ctx.arc(x,y-36,13,0,Math.PI*2); ctx.fill();
 // face detail - eyes
 ctx.fillStyle='#000'; ctx.fillRect(x-5,y-38,2,2); ctx.fillRect(x+3,y-38,2,2);
 // hair - fade style
 ctx.fillStyle='#0a0a0a'; ctx.fillRect(x-13,y-50,26,10); ctx.fillRect(x-11,y-42,22,4);
 // number large
 ctx.fillStyle=color=='yellow'?'#000':'#fff'; ctx.font='bold 12px Arial'; ctx.fillText(num,x-5,y-8);
 // YOU ring
 if(isYou){
  ctx.strokeStyle='#00ff88'; ctx.lineWidth=4; ctx.beginPath(); ctx.arc(x,y+5,32,0,Math.PI*2); ctx.stroke();
  ctx.fillStyle='#00ff88'; ctx.font='bold 11px Arial'; ctx.fillText('YOU • #'+num,x-18,y-56);
 }
}

function drawBallPro(x,y){
 ctx.fillStyle='rgba(0,0,0,0.35)'; ctx.beginPath(); ctx.ellipse(x,y+12,10,5,0,0,Math.PI*2); ctx.fill();
 ctx.fillStyle='#fff'; ctx.beginPath(); ctx.arc(x,y,11,0,Math.PI*2); ctx.fill();
 ctx.fillStyle='#000'; ctx.beginPath(); ctx.arc(x+3,y-3,3.5,0,Math.PI*2); ctx.fill(); ctx.beginPath(); ctx.arc(x-3,y+3,2.5,0,Math.PI*2); ctx.fill(); ctx.beginPath(); ctx.arc(x+2,y+4,2,0,Math.PI*2); ctx.fill();
}

function loop(){
 anim+=0.18;
 drawStadiumPro();
 let you=yellows[ctrl]; if(you){you.x+=vec.x*7; you.y+=vec.y*7; you.x=Math.max(45,Math.min(855,you.x)); you.y=Math.max(165,Math.min(655,you.y));}
 // AI move
 blues.forEach(b=>{let dx=ball.x-b.x,dy=ball.y-b.y; if(Math.hypot(dx,dy)<350){b.x+=dx*0.013; b.y+=dy*0.013;}});
 yellows.forEach((p,i)=>{if(i!=ctrl){let dx=ball.x-p.x,dy=ball.y-p.y; if(Math.hypot(dx,dy)<280){p.x+=dx*0.008; p.y+=dy*0.008;}}});
 // ball
 let dx=you.x-ball.x,dy=you.y-ball.y; if(Math.hypot(dx,dy)<36){ball.vx=-dx*0.28; ball.vy=-dy*0.28;}
 ball.x+=ball.vx; ball.y+=ball.vy; ball.vx*=0.981; ball.vy*=0.981;
 if(ball.y<162 && ball.x>380 && ball.x<520){document.getElementById('score').innerText='GOOOAL! KAWANDA FC 1-0 • Mukwala 90+3 • NAMBOOLE ERUPTS!'; ball.x=450; ball.y=410;}
 blues.forEach(b=>drawUltraHuman(b.x,b.y,b.skin,'blue',b.n,false,anim+b.x*0.01));
 yellows.forEach((p,i)=>drawUltraHuman(p.x,p.y,p.skin,'yellow',p.n,i==ctrl,anim* (i==ctrl?1.5:0.7)+p.x*0.01));
 drawBallPro(ball.x,ball.y);
 requestAnimationFrame(loop);
}

// controls
joy.addEventListener('touchstart',()=>drag=true);
joy.addEventListener('touchend',()=>{drag=false; vec={x:0,y:0}; knob.style.left='27px'; knob.style.top='27px';});
joy.addEventListener('touchmove',e=>{
 if(!drag)return; let r=joy.getBoundingClientRect(),t=e.touches[0],x=t.clientX-r.left-55,y=t.clientY-r.top-55,d=Math.min(42,Math.hypot(x,y)),a=Math.atan2(y,x);
 knob.style.left=(27+Math.cos(a)*d)+'px'; knob.style.top=(27+Math.sin(a)*d)+'px'; vec={x:Math.cos(a)*d/42,y:Math.sin(a)*d/42};
});
document.getElementById('shoot').onclick=()=>{ball.vy=-18; ball.vx=(Math.random()-0.5)*10;};
document.getElementById('pass').onclick=()=>{let near=yellows.filter(p=>p!=yellows[ctrl]).sort((a,b)=>Math.hypot(a.x-ball.x,a.y-ball.y)-Math.hypot(b.x-ball.x,b.y-ball.y))[0]; if(near){ball.vx=(near.x-ball.x)*0.2; ball.vy=(near.y-ball.y)*0.2;}};
document.getElementById('thru').onclick=()=>{ball.vy=-26; ball.vx=(Math.random()-0.5)*6;};
document.getElementById('sprint').ontouchstart=()=>{vec.x*=2; vec.y*=2;};
document.getElementById('switch').onclick=()=>{ctrl=(ctrl+1)%11; document.getElementById('sta').innerText='YOU: #'+yellows[ctrl].n;};

loop();
</script>
</body>
</html>