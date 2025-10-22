<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Falling Logic Gates Overlay</title>
<style>
  html,body{height:100%;margin:0;background:transparent;overflow:hidden}
  .gate{
    position:absolute;
    top:-80px;
    width:48px;
    height:auto;
    opacity:0.95;
    will-change: transform, opacity;
  }
  .gate img{
    display:block;
    width:100%;
    height:auto;
    filter: drop-shadow(0 0 10px rgba(125,211,252,0.45));
  }
  @keyframes fall {
    0%   { transform: translateY(0) rotate(0deg); opacity:1; }
    100% { transform: translateY(120vh) rotate(720deg); opacity:0.05; }
  }
</style>
</head>
<body>
<script>
(function(){
  // === RAW URLs from your repo (use raw.githubusercontent.com)
  const base = 'https://raw.githubusercontent.com/cheeeseeeeey-cmd/Genius-hour/main/assets/';
  const svgs = [
    base + 'logic-gate-and-svgrepo-com.svg',
    base + 'logic-gate-or-svgrepo-com.svg',
    base + 'logic-gate-not-svgrepo-com.svg'
  ];

  const SPAWN_INTERVAL = 600;      // ms between spawns
  const MAX_ON_SCREEN = 18;
  const MIN_DURATION = 9000;       // ms
  const MAX_DURATION = 17000;      // ms
  let active = 0;

  function rand(min,max){ return Math.random() * (max - min) + min; }
  function pick(a){ return a[Math.floor(Math.random()*a.length)]; }

  function spawn(){
    if(active >= MAX_ON_SCREEN) return;
    const url = pick(svgs);
    const el = document.createElement('div');
    el.className = 'gate';

    // create img element (raw SVG served properly)
    const img = document.createElement('img');
    img.src = url;
    img.alt = '';
    el.appendChild(img);

    // horizontal position: allow slight off-edge
    el.style.left = rand(-6, 96) + '%';

    // rotation start
    el.style.transform = 'rotate(' + rand(0,360).toFixed(1) + 'deg)';

    // duration & animation
    const duration = Math.round(rand(MIN_DURATION, MAX_DURATION));
    el.style.animation = 'fall ' + duration + 'ms linear forwards';

    // append and count
    document.body.appendChild(el);
    active++;

    // cleanup after done
    setTimeout(() => {
      if (el.parentNode) el.parentNode.removeChild(el);
      active = Math.max(0, active - 1);
    }, duration + 300);
  }

  // small initial burst
  for(let i=0;i<6;i++) setTimeout(spawn, i * 200);

  // ongoing spawns
  setInterval(spawn, SPAWN_INTERVAL);
})();
</script>
</body>
</html>
