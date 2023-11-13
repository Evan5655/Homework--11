# Homework--11
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple Game</title>
  <style>
    canvas {
      border: 1px solid #000;
    }
  </style>
</head>
<body>

<script>
  const canvas = document.createElement("canvas");
  const ctx = canvas.getContext("2d");
  document.body.appendChild(canvas);

  canvas.width = 800;
  canvas.height = 400;

  // Player
  const player = {
    x: 50,
    y: canvas.height / 2,
    width: 20,
    height: 20,
    color: "blue",
    speed: 5,
  };

  // Obstacles
  const obstacles = [
    { x: 200, y: 100, width: 30, height: 30, color: "red", speed: 3 },
    { x: 500, y: 300, width: 40, height: 40, color: "green", speed: 2 },
  ];

  // Non-moving obstacle added by mouse click
  let nonMovingObstacle = null;

  // Visual exit
  const exit = {
    x: canvas.width - 30,
    y: canvas.height / 2 - 15,
    width: 30,
    height: 30,
    color: "yellow",
  };

  function drawPlayer() {
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x, player.y, player.width, player.height);
  }

  function drawObstacles() {
    obstacles.forEach((obstacle) => {
      ctx.fillStyle = obstacle.color;
      ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
    });

    if (nonMovingObstacle) {
      ctx.fillStyle = nonMovingObstacle.color;
      ctx.fillRect(
        nonMovingObstacle.x,
        nonMovingObstacle.y,
        nonMovingObstacle.width,
        nonMovingObstacle.height
      );
    }
  }

  function drawExit() {
    ctx.fillStyle = exit.color;
    ctx.fillRect(exit.x, exit.y, exit.width, exit.height);
  }

  function movePlayer() {
    document.addEventListener("keydown", (event) => {
      if (event.key === "ArrowUp" && player.y > 0) {
        player.y -= player.speed;
      } else if (event.key === "ArrowDown" && player.y < canvas.height - player.height) {
        player.y += player.speed;
      } else if (event.key === "ArrowLeft" && player.x > 0) {
        player.x -= player.speed;
      } else if (event.key === "ArrowRight" && player.x < canvas.width - player.width) {
        player.x += player.speed;
      }
    });
  }

  function moveObstacles() {
    obstacles.forEach((obstacle) => {
      obstacle.x += obstacle.speed;

      if (obstacle.x > canvas.width) {
        obstacle.x = 0 - obstacle.width;
      }
    });
  }

  function checkCollision() {
    if (
      player.x < exit.x + exit.width &&
      player.x + player.width > exit.x &&
      player.y < exit.y + exit.height &&
      player.y + player.height > exit.y
    ) {
      alert("Congratulations! You won!");
    }
  }

  canvas.addEventListener("click", (event) => {
    const mouseX = event.clientX - canvas.getBoundingClientRect().left;
    const mouseY = event.clientY - canvas.getBoundingClientRect().top;

    nonMovingObstacle = {
      x: mouseX,
      y: mouseY,
      width: 25,
      height: 25,
      color: "purple",
    };
  });

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    drawPlayer();
    drawObstacles();
    drawExit();

    moveObstacles();
    checkCollision();

    requestAnimationFrame(draw);
  }

  movePlayer();
  draw();
</script>

</body>
</html>
