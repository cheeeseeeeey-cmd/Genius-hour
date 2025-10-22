<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Floating Logic Gates (Overlay)</title>
<style>
  html,body{height:100%;margin:0;padding:0;background:transparent;overflow:hidden}
  #float-root{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    pointer-events:none;
    z-index:0;
    overflow:hidden;
  }
  .gate {
    position:absolute;
    top:-80px; 
    will-change: transform, opacity;
    opacity:0.95;
    transform-origin:center;
    display:block;
  }
  .gate img{
    display:block;
    width:44px;
    height:auto;
    filter: drop-shadow(0 0 10px rgba(125,211,252,0.45));
  }
  @keyframes fall {
    0%   { transform: translateY(0) rotate(0deg) scale(1); opacity:1; }
    80%  { opacity:0.9; }
    100% { transform: translateY(120vh) rotate(540deg) scale(1.05); opacity:0.08; }
  }
</style>
</head>
<body>
  <div id="float-root" aria-hidden="true"></div>

<script>


(function(){
  const ROOT = document.getElementById('float-root');
  const ICONS = [
    "https://i.postimg.cc/QM7HzShk/logic-gate-and-svgrepo-com.png",
    "https://i.postimg.cc/DzG8N5nL/logic-gate-not-svgrepo-com.png",
    "https://i.postimg.cc/7LzfRXw1/logic-gate-or-svgrepo-com.png"
  ];


  const SPAWN_INTERVAL_MS = 700;   
  const MAX_ON_SCREEN = 18;   
  const MIN_DURATION = 8500;    
  const MAX_DURATION = 18000; 
  const MIN_SCALE = 0.75;
  const MAX_SCALE = 1.3;

  let active = 0;

  function rand(min, max){ return Math.random() * (max - min) + min; }
  function pick(arr){ return arr[Math.floor(Math.random() * arr.length)]; }

  function spawnGate(){
    if (active >= MAX_ON_SCREEN) return;
    const imgUrl = pick(ICONS);
    const el = document.createElement('div');
    el.className = 'gate';
    const leftPct = rand(-8, 102);
    el.style.left = leftPct + '%';

    const scale = rand(MIN_SCALE, MAX_SCALE);
    el.style.transform = `translateY(0) rotate(${rand(0,360)}deg) scale(${scale})`;

    const duration = Math.round(rand(MIN_DURATION, MAX_DURATION));
    el.style.animation = `fall ${duration}ms linear forwards`;

    el.style.rotate = `${rand(-45,45)}deg`;

    const img = document.createElement('img');
    img.src = imgUrl;
    img.alt = '';
    el.appendChild(img);
    ROOT.appendChild(el);
    active++;

    setTimeout(() => {
      if (el && el.parentNode) el.parentNode.removeChild(el);
      active = Math.max(0, active - 1);
    }, duration + 200); 
  }

  for (let i=0;i<6;i++){
    setTimeout(spawnGate, i * 200);
  }

  const intervalId = setInterval(spawnGate, SPAWN_INTERVAL_MS);

  window.__floatingGates = {
    stop: () => clearInterval(intervalId),
    spawnOnce: spawnGate
  };
})();
</script>
</body>
</html>
