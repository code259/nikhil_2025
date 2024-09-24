---
layout: page
title: Pong Game
permalink: /pong/
---

<canvas id="pong" width="800" height="600"></canvas>

<style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: black;
      margin: 0;
    }

    canvas {
      border: 2px solid white;
    }
</style>

<script>
    const canvas = document.getElementById('pong');
    const ctx = canvas.getContext('2d');

    // Create the paddles
    const paddleWidth = 10, paddleHeight = 100;
    const player = { x: 0, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight, color: "white", score: 0 };
    const computer = { x: canvas.width - paddleWidth, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight, color: "white", score: 0 };

    // Create the ball
    const ball = { x: canvas.width / 2, y: canvas.height / 2, radius: 10, speed: 5, velocityX: 5, velocityY: 5, color: "white" };

    // Draw rectangle
    function drawRect(x, y, w, h, color) {
      ctx.fillStyle = color;
      ctx.fillRect(x, y, w, h);
    }

    // Draw circle
    function drawCircle(x, y, r, color) {
      ctx.fillStyle = color;
      ctx.beginPath();
      ctx.arc(x, y, r, 0, Math.PI * 2, false);
      ctx.closePath();
      ctx.fill();
    }

    // Draw text
    function drawText(text, x, y, color) {
      ctx.fillStyle = color;
      ctx.font = "45px Arial";
      ctx.fillText(text, x, y);
    }

    // Move the paddle
    function movePaddle(event) {
      let rect = canvas.getBoundingClientRect();
      player.y = event.clientY - rect.top - player.height / 2;
    }

    canvas.addEventListener("mousemove", movePaddle);

    // Ball and paddle collision detection
    function collision(b, p) {
      p.top = p.y;
      p.bottom = p.y + p.height;
      p.left = p.x;
      p.right = p.x + p.width;

      b.top = b.y - b.radius;
      b.bottom = b.y + b.radius;
      b.left = b.x - b.radius;
      b.right = b.x + b.radius;

      return b.right > p.left && b.bottom > p.top && b.left < p.right && b.top < p.bottom;
    }

    // Reset the ball
    function resetBall() {
      ball.x = canvas.width / 2;
      ball.y = canvas.height / 2;
      ball.speed = 5;
      ball.velocityX = -ball.velocityX;
    }

    // Update game objects
    function update() {
      // Move the ball
      ball.x += ball.velocityX;
      ball.y += ball.velocityY;

      // Check if the ball hits the top or bottom wall
      if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
        ball.velocityY = -ball.velocityY;
      }

      // Move the computer paddle
      let computerLevel = 0.1;
      computer.y += (ball.y - (computer.y + computer.height / 2)) * computerLevel;

      // Check if the ball hits the player or computer paddle
      let playerPaddle = (ball.x < canvas.width / 2) ? player : computer;

      if (collision(ball, playerPaddle)) {
        let collidePoint = ball.y - (playerPaddle.y + playerPaddle.height / 2);
        collidePoint = collidePoint / (playerPaddle.height / 2);
        let angleRad = collidePoint * (Math.PI / 4);

        let direction = (ball.x < canvas.width / 2) ? 1 : -1;
        ball.velocityX = direction * ball.speed * Math.cos(angleRad);
        ball.velocityY = ball.speed * Math.sin(angleRad);
        ball.speed += 0.5;
      }

      // Check if the ball goes off screen
      if (ball.x - ball.radius < 0) {
        computer.score++;
        resetBall();
      } else if (ball.x + ball.radius > canvas.width) {
        player.score++;
        resetBall();
      }
    }

    // Render the game objects
    function render() {
      drawRect(0, 0, canvas.width, canvas.height, "black");
      drawText(player.score, canvas.width / 4, canvas.height / 5, "white");
      drawText(computer.score, 3 * canvas.width / 4, canvas.height / 5, "white");
      drawRect(player.x, player.y, player.width, player.height, player.color);
      drawRect(computer.x, computer.y, computer.width, computer.height, computer.color);
      drawCircle(ball.x, ball.y, ball.radius, ball.color);
    }

    // Game loop
    function game() {
      update();
      render();
    }

    let fps = 60;
    setInterval(game, 1000 / fps);
</script>