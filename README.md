<!DOCTYPE html><html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ana Clara Milionária</title>
  <style>
    /* Estilos gerais */
    body {
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, sans-serif;
      background: linear-gradient(270deg, #ff9a9e, #fad0c4, #fad0c4, #ff9a9e);
      background-size: 800% 800%;
      animation: gradientBG 10s ease infinite;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      overflow: hidden;
      text-align: center;
    }@keyframes gradientBG {
  0% {background-position: 0% 50%;}
  50% {background-position: 100% 50%;}
  100% {background-position: 0% 50%;}
}

h1 {
  font-size: 3rem;
  color: #fff;
  text-shadow: 2px 2px 5px rgba(0,0,0,0.3);
  animation: fadeIn 2s ease-out;
}

@keyframes fadeIn {
  from {opacity: 0; transform: translateY(-20px);} 
  to {opacity: 1; transform: translateY(0);} 
}

.buttons {
  position: relative;
  margin-top: 30px;
}

button {
  padding: 15px 30px;
  font-size: 1.1rem;
  font-weight: bold;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  outline: none;
}

button:active {
  transform: scale(0.95);
}

#yesBtn {
  background-color: #28a745;
  color: #fff;
  box-shadow: 0 5px 15px rgba(40,167,69,0.4);
}

#noBtn {
  position: absolute;
  background-color: #dc3545;
  color: #fff;
  box-shadow: 0 5px 15px rgba(220,53,69,0.4);
}

/* Estilo para o canvas de confetti */
canvas {
  position: fixed;
  top: 0;
  left: 0;
  pointer-events: none;
  width: 100%;
  height: 100%;
  z-index: 999;
}

  </style>
</head>
<body>
  <h1>Ana Clara vai ser milionária! 🎉</h1>  <div class="buttons">
    <button id="yesBtn">Sim, eu acredito!</button>
    <button id="noBtn">Não</button>
  </div><canvas id="confetti"></canvas>

  <script>
    // Elementos
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const confettiCanvas = document.getElementById('confetti');
    const ctx = confettiCanvas.getContext('2d');
    let confettiPieces = [];

    // Ajusta canvas para toda tela
    function resizeCanvas() {
      confettiCanvas.width = window.innerWidth;
      confettiCanvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    // Cria partículas de confetti
    function createConfetti() {
      for (let i = 0; i < 150; i++) {
        confettiPieces.push({
          x: Math.random() * confettiCanvas.width,
          y: Math.random() * confettiCanvas.height - confettiCanvas.height,
          r: Math.random() * 6 + 4,
          d: Math.random() * 50 + 10,
          color: `hsl(${Math.random() * 360}, 100%, 50%)`,
          tilt: Math.floor(Math.random() * 10) - 10,
          tiltAngleIncremental: Math.random() * 0.07 + 0.05,
          tiltAngle: 0
        });
      }
    }

    // Anima confetti
    function drawConfetti() {
      ctx.clearRect(0, 0, confettiCanvas.width, confettiCanvas.height);
      confettiPieces.forEach((p, index) => {
        ctx.beginPath();
        ctx.lineWidth = p.r;
        ctx.strokeStyle = p.color;
        ctx.moveTo(p.x + p.tilt + p.r / 2, p.y);
        ctx.lineTo(p.x + p.tilt, p.y + p.tilt + p.r / 2);
        ctx.stroke();

        p.tiltAngle += p.tiltAngleIncremental;
        p.y += (Math.cos(p.d) + 3 + p.r / 2) / 2;
        p.x += Math.sin(p.d);
        p.tilt = Math.sin(p.tiltAngle) * 15;

        if (p.y > confettiCanvas.height) {
          confettiPieces[index] = {
            x: Math.random() * confettiCanvas.width,
            y: -10,
            r: p.r,
            d: p.d,
            color: p.color,
            tilt: p.tilt,
            tiltAngleIncremental: p.tiltAngleIncremental,
            tiltAngle: p.tiltAngle
          };
        }
      });
      requestAnimationFrame(drawConfetti);
    }

    // Evento ao clicar em 'Sim'
    yesBtn.addEventListener('click', () => {
      // Voz
      const msg = new SpeechSynthesisUtterance('Parabéns, Ana Clara! Você será milionária!');
      msg.lang = 'pt-BR';
      window.speechSynthesis.speak(msg);

      // Confetti
      createConfetti();
      drawConfetti();
    });

    // Botão 'Não' foge do cursor
    noBtn.addEventListener('mouseenter', () => {
      const shiftX = Math.random() * 200 + 100;
      const shiftY = Math.random() * 200 - 100;
      const newX = Math.min(Math.max(noBtn.offsetLeft + shiftX, 0), window.innerWidth - noBtn.offsetWidth);
      const newY = Math.min(Math.max(noBtn.offsetTop + shiftY, 0), window.innerHeight - noBtn.offsetHeight);
      noBtn.style.left = newX + 'px';
      noBtn.style.top = newY + 'px';
    });
  </script></body>
</html>
