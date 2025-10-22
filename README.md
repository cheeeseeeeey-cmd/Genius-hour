<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Falling Logic Gates Overlay</title>
<style>
  html, body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: transparent;
    height: 100vh;
    width: 100vw;
  }

  .gate {
    position: absolute;
    width: 40px;
    height: 40px;
    opacity: 0.85;
    animation: fall linear forwards, spin linear infinite;
  }

  @keyframes fall {
    0% {
      transform: translateY(-10vh) rotate(0deg);
      opacity: 1;
    }
    100% {
      transform: translateY(110vh) rotate(360deg);
      opacity: 0;
    }
  }

  @keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }
</style>
</head>
<body>
<script>
  const svgs = [
    'https://github.com/cheeeseeeeey-cmd/Genius-hour/tree/main/assets/logic-gate-and-svgrepo-com.svg',
    'https://github.com/cheeeseeeeey-cmd/Genius-hour/tree/main/assets/logic-gate-or-svgrepo-com.svg',
    'https://github.com/cheeeseeeeey-cmd/Genius-hour/tree/main/assets/logic-gate-not-svgrepo-com.svg'
  ];

  function createGate() {
    const gate = document.createElement("img");
    gate.src = svgs[Math.floor(Math.random() * svgs.length)];
    gate.classList.add("gate");
    gate.style.left = Math.random() * 100 + "vw";
    const duration = 6 + Math.random() * 6;
    gate.style.animationDuration = `${duration}s, ${duration * 0.5}s`;
    document.body.appendChild(gate);

    setTimeout(() => gate.remove(), duration * 1000);
  }

  // spawn gates at intervals
  setInterval(createGate, 500);
</script>
</body>
</html>
