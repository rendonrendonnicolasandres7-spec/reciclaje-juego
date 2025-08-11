<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Reciclaje Rápido</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #e0f7fa;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    h1 {
      margin-top: 20px;
    }
    #game {
      margin: 20px auto;
      width: 300px;
      height: 300px;
      border: 2px solid #00796b;
      background: #ffffff;
      position: relative;
    }
    .item {
      width: 50px;
      height: 50px;
      position: absolute;
      cursor: pointer;
    }
    #score, #timer {
      font-size: 20px;
      margin: 10px;
    }
    #startBtn {
      padding: 10px 20px;
      font-size: 16px;
      background: #00796b;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>♻️ Reciclaje Rápido</h1>
  <div id="score">Puntos: 0</div>
  <div id="timer">Tiempo: 30</div>
  <button id="startBtn">Iniciar Juego</button>
  <div id="game"></div>

  <audio id="bgMusic" loop>
    <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
  </audio>

  <script>
    const game = document.getElementById("game");
    const scoreDisplay = document.getElementById("score");
    const timerDisplay = document.getElementById("timer");
    const startBtn = document.getElementById("startBtn");
    const bgMusic = document.getElementById("bgMusic");

    let score = 0;
    let timeLeft = 30;
    let gameInterval;
    let timerInterval;

    const items = [
      "🗑️", "♻️", "🍌", "📦", "🧃", "📰", "🥤", "🍕", "🧴", "🍎"
    ];

    function randomPosition() {
      const x = Math.floor(Math.random() * 250);
      const y = Math.floor(Math.random() * 250);
      return { x, y };
    }

    function spawnItem() {
      const item = document.createElement("div");
      item.classList.add("item");
      const emoji = items[Math.floor(Math.random() * items.length)];
      item.textContent = emoji;
      const pos = randomPosition();
      item.style.left = pos.x + "px";
      item.style.top = pos.y + "px";

      item.onclick = () => {
        score += 1;
        scoreDisplay.textContent = "Puntos: " + score;
        item.remove();
      };

      game.appendChild(item);

      setTimeout(() => {
        if (game.contains(item)) item.remove();
      }, 1500);
    }

    function startGame() {
      score = 0;
      timeLeft = 30;
      scoreDisplay.textContent = "Puntos: 0";
      timerDisplay.textContent = "Tiempo: 30";
      bgMusic.play();

      gameInterval = setInterval(spawnItem, 800);
      timerInterval = setInterval(() => {
        timeLeft--;
        timerDisplay.textContent = "Tiempo: " + timeLeft;
        if (timeLeft <= 0) {
          clearInterval(gameInterval);
          clearInterval(timerInterval);
          bgMusic.pause();
          alert("¡Tiempo terminado! Puntaje final: " + score);
        }
      }, 1000);
    }

    startBtn.onclick = startGame;
  </script>
</body>
</html>
