<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>Vòng quay may mắn</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background: #f5f6fa;
    }
    canvas {
      margin-top: 20px;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h2>🎡 Vòng quay may mắn</h2>
  <canvas id="wheel" width="400" height="400"></canvas>
  <button onclick="spin()">Quay</button>

  <script>
    const canvas = document.getElementById('wheel');
    const ctx = canvas.getContext('2d');

    // Danh sách phần thưởng (có thể chỉnh theo nội dung Canva)
    const items = ['Phần 1', 'Phần 2', 'Phần 3', 'Phần 4', 'Phần 5', 'Phần 6'];
    const colors = ['#ff7675', '#74b9ff', '#55efc4', '#ffeaa7', '#a29bfe', '#fab1a0'];

    let angle = 0;

    function drawWheel() {
      const radius = canvas.width / 2;
      const slice = (2 * Math.PI) / items.length;

      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.translate(radius, radius);
      ctx.rotate(angle);

      for (let i = 0; i < items.length; i++) {
        ctx.beginPath();
        ctx.fillStyle = colors[i % colors.length];
        ctx.moveTo(0, 0);
        ctx.arc(0, 0, radius, i * slice, (i + 1) * slice);
        ctx.fill();

        ctx.save();
        ctx.rotate(i * slice + slice / 2);
        ctx.textAlign = 'right';
        ctx.fillStyle = '#000';
        ctx.font = '16px Arial';
        ctx.fillText(items[i], radius - 10, 5);
        ctx.restore();
      }

      ctx.rotate(-angle);
      ctx.translate(-radius, -radius);

      // Kim chỉ
      ctx.fillStyle = '#000';
      ctx.beginPath();
      ctx.moveTo(radius - 10, 0);
      ctx.lineTo(radius + 10, 0);
      ctx.lineTo(radius, 20);
      ctx.fill();
    }

    drawWheel();

    function spin() {
      const spinAngle = Math.random() * 360 + 720; // quay ít nhất 2 vòng
      const spinTime = 2000;
      const start = performance.now();

      function animate(time) {
        const progress = Math.min((time - start) / spinTime, 1);
        angle = (spinAngle * (1 - Math.pow(1 - progress, 3))) * Math.PI / 180;
        drawWheel();
        if (progress < 1) requestAnimationFrame(animate);
      }
      requestAnimationFrame(animate);
    }
  </script>
</body>
</html>
