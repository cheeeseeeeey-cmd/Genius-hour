<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Straight Falling Logic Gates</title>
<style>
  html,body{
    height:100%;
    margin:0;
    background:transparent;
    overflow:hidden;
  }

  #float-root{
    position:fixed;
    inset:0;
    width:100%;
    height:100%;
    pointer-events:none;
    z-index:0;
    overflow:hidden;
  }

  .gate{
    position:absolute;
    top:-80px;
    will-change:transform,opacity;
    opacity:0.95;
    transform-origin:center;
  }

  .gate img{
    display:block;
    width:44px;
    height:auto;
    filter:drop-shadow(0 0 10px rgba(180,220,255,0.5));
  }

  @keyframes fall {
    0%   { transform: translateY(0) rotate(0deg) scale(1); opacity:1; }
    90%  { opacity:0.9; }
    100% { transform: translateY(120vh) rotate(720deg) scale(1); opacity:0.05; }
  }
</style>
</head>
<body>
  <div id="float-root" aria-hidden="true"></div>

<script>
(function(){
  const ROOT = document.getElementById('float-root');
  const ICONS = [
  `<svg viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg">
    <path fill="#00ffea" d="M105 105v302h151c148 0 148-302 0-302H105z"/>
  </svg>`,
  `<svg viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg">
    <path fill="#00ffea" d="M116.6 407c40-45.9 60.4-98.4 60.4-151H192c34.1 0 81.9 34 119.3 71.4z"/>
  </svg>`,
  `<svg viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg">
    <path fill="#00ffea" d="M105,111.3V400.7L365.5,256Z"/>
  </svg>`
];


  const MAX_ON_SCREEN = 18;
  const SPAWN_INTERVAL_MS = 800;
  const MIN_DURATION = 9000;
  const MAX_DURATION = 17000;
  const MIN_SCALE = 0.8;
  const MAX_SCALE = 1.3;
  let active = 0;

  function rand(min,max){return Math.random()*(max-min)+min;}
  function pick(a){return a[Math.floor(Math.random()*a.length)];}

  function spawnGate(){
    if(active >= MAX_ON_SCREEN) return;
    const gate = document.createElement('div');
    gate.className = 'gate';

    const img = document.createElement('img');
    img.src = pick(ICONS);
    img.alt = '';
    gate.appendChild(img);

    const left = rand(-5, 95);
    gate.style.left = left + '%';
    const scale = rand(MIN_SCALE, MAX_SCALE);
    const duration = rand(MIN_DURATION, MAX_DURATION);
    gate.style.transform = `translateY(0) rotate(${rand(0,360)}deg) scale(${scale})`;
    gate.style.animation = `fall ${duration}ms linear forwards`;

    ROOT.appendChild(gate);
    active++;

    setTimeout(()=>{
      gate.remove();
      active = Math.max(0, active-1);
    }, duration + 200);
  }

  // Initial burst
  for(let i=0;i<6;i++) setTimeout(spawnGate, i*250);
  // Repeat
  setInterval(spawnGate, SPAWN_INTERVAL_MS);
})();
</script>
</body>
</html>
