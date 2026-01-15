<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <title>App Futebol 2D</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #0b3d2e;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .top-bar {
      display: flex;
      gap: 20px;
      margin: 10px;
    }

    .team-name {
      background: #fff;
      padding: 8px 12px;
      border-radius: 6px;
      border: 2px solid #333;
      min-width: 140px;
      text-align: center;
      font-weight: bold;
    }

    .controls {
      margin: 10px;
      display: flex;
      gap: 10px;
    }

    button {
      padding: 8px 14px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
    }

    #blueBtn { background: #1e90ff; color: white; }
    #redBtn { background: #e63946; color: white; }
    #saveBtn { background: #222; color: white; }

    canvas {
      background: #1f7a3f;
      border: 4px solid white;
      border-radius: 10px;
    }
  </style>
</head>
<body>

  <div class="top-bar">
    <div contenteditable="true" class="team-name" id="teamBlue">TIME AZUL</div>
    <div contenteditable="true" class="team-name" id="teamRed">TIME VERMELHO</div>
  </div>

  <div class="controls">
    <button id="blueBtn">Jogador Azul</button>
    <button id="redBtn">Jogador Vermelho</button>
    <button id="saveBtn">Salvar</button>
  </div>

  <canvas id="field" width="800" height="500"></canvas>

  <script>
    const canvas = document.getElementById('field');
    const ctx = canvas.getContext('2d');

    let currentColor = 'blue';
    let players = [];

    function drawField() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      ctx.strokeStyle = 'white';
      ctx.lineWidth = 2;

      // Contorno
      ctx.strokeRect(20, 20, 760, 460);

      // Meio campo
      ctx.beginPath();
      ctx.moveTo(400, 20);
      ctx.lineTo(400, 480);
      ctx.stroke();

      // Círculo central
      ctx.beginPath();
      ctx.arc(400, 250, 60, 0, Math.PI * 2);
      ctx.stroke();

      // Marca central
      ctx.beginPath();
      ctx.arc(400, 250, 3, 0, Math.PI * 2);
      ctx.fillStyle = 'white';
      ctx.fill();

      // Área esquerda
      ctx.strokeRect(20, 150, 120, 200);
      ctx.strokeRect(20, 200, 60, 100);

      // Área direita
      ctx.strokeRect(660, 150, 120, 200);
      ctx.strokeRect(720, 200, 60, 100);

      // Pênaltis
      ctx.beginPath();
      ctx.arc(110, 250, 3, 0, Math.PI * 2);
      ctx.arc(690, 250, 3, 0, Math.PI * 2);
      ctx.fill();

      drawPlayers();
    }

    function drawPlayers() {
      players.forEach(p => {
        ctx.beginPath();
        ctx.arc(p.x, p.y, 12, 0, Math.PI * 2);
        ctx.fillStyle = p.color;
        ctx.fill();
      });
    }

    canvas.addEventListener('click', e => {
      const rect = canvas.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;

      players.push({ x, y, color: currentColor });
      drawField();
    });

    document.getElementById('blueBtn').onclick = () => currentColor = 'blue';
    document.getElementById('redBtn').onclick = () => currentColor = 'red';

    document.getElementById('saveBtn').onclick = () => {
      const data = {
        teamBlue: document.getElementById('teamBlue').innerText,
        teamRed: document.getElementById('teamRed').innerText,
        players
      };
      localStorage.setItem('futebol2D', JSON.stringify(data));
      alert('Formação salva com sucesso!');
    };

    drawField();
  </script>

</body>
</html>