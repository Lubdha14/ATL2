# Bricks Game Ruby on Rails... 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Block Breaker</title>
    <style>
        /* Add your CSS styles here */
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #f0f0f0;
        }
        canvas {
            display: block;
            margin: auto;
            border: 1px solid #000;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="480" height="320"></canvas>
    <script>
        // JavaScript code for Block Breaker game
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const ballRadius = 10;
        let x = canvas.width / 2;
        let y = canvas.height - 30;
        let dx = 2;
        let dy = -2;
        const paddleHeight = 10;
        const paddleWidth = 75;
        let paddleX = (canvas.width - paddleWidth) / 2;
        let rightPressed = false;
        let leftPressed = false;
        const brickRowCount = 3;
        const brickColumnCount = 5;
        const brickWidth = 75;
        const brickHeight = 20;
        const brickPadding = 10;
        const brickOffsetTop = 30;
        const brickOffsetLeft = 30;
        const bricks = [];
        for (let c = 0; c < brickColumnCount; c++) {
            bricks[c] = [];
            for (let r = 0; r < brickRowCount; r++) {
                bricks[c][r] = { x: 0, y: 0, status: 1 };
            }
        }
        let score = 0;

        // Event listeners for paddle control
        document.addEventListener("keydown", keyDownHandler);
        document.addEventListener("keyup", keyUpHandler);

        function keyDownHandler(e) {
            if (e.key === "Right" || e.key === "ArrowRight") {
                rightPressed = true;
            } else if (e.key === "Left" || e.key === "ArrowLeft") {
                leftPressed = true;
            }
        }
        function keyUpHandler(e) {
            if (e.key === "Right" || e.key === "ArrowRight") {
                rightPressed = false;
            } else if (e.key === "Left" || e.key === "ArrowLeft") {
                leftPressed = false;
            }
        }
        // Collision detection between ball and bricks
        function collisionDetection() {
            for (let c = 0; c < brickColumnCount; c++) {
                for (let r = 0; r < brickRowCount; r++) {
                    const brick = bricks[c][r];
                    if (brick.status === 1) {
                        if (
                            x > brick.x &&
                            x < brick.x + brickWidth &&
                            y > brick.y &&
                            y < brick.y + brickHeight
                        ) {
                            dy = -dy;
                            brick.status = 0;
                            score++;
                            if (score === brickRowCount * brickColumnCount) {
                                endGame();
                            }
                        }
                    }
                }
            }
        }
        // Draw the ball
        function drawBall() {
            ctx.beginPath();
            ctx.arc(x, y, ballRadius, 0, Math.PI * 2);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
        // Draw the paddle
        function drawPaddle() {
            ctx.beginPath();
            ctx.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
        // Draw the bricks
        function drawBricks() {
            for (let c = 0; c < brickColumnCount; c++) {
                for (let r = 0; r < brickRowCount; r++) {
                    if (bricks[c][r].status === 1) {
                        const brickX = c * (brickWidth + brickPadding) + brickOffsetLeft;
                        const brickY = r * (brickHeight + brickPadding) + brickOffsetTop;
                        bricks[c][r].x = brickX;
                        bricks[c][r].y = brickY;
                        ctx.beginPath();
                        ctx.rect(brickX, brickY, brickWidth, brickHeight);
                        ctx.fillStyle = "#0095DD";
                        ctx.fill();
                        ctx.closePath();
                    }
                }
            }
        }
        // Draw the score
        function drawScore() {
            ctx.font = "16px Arial";
            ctx.fillStyle = "#0095DD";
            ctx.fillText("Score: " + score, 8, 20);
        }
        // End the game
        function endGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.font = "24px Arial";
            ctx.fillStyle = "#0095DD";
            ctx.textAlign = "center";
            ctx.fillText("Congratulations! You win!", canvas.width / 2, canvas.height / 2);
            ctx.fillText("Score: " + score, canvas.width / 2, canvas.height / 2 + 30);
        }
        // Main game loop
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBricks();
            drawBall();
            drawPaddle();
            drawScore();
            collisionDetection();
            // Ball movement
            x += dx;
            y += dy;
            if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
                dx = -dx;
            }
            if (y + dy < ballRadius) {
                dy = -dy;
            } else if (y + dy > canvas.height - ballRadius) {
                if (x > paddleX && x < paddleX + paddleWidth) {
                    dy = -dy;
                } else {
                    endGame();
                }
            }
            // Paddle movement
            if (rightPressed && paddleX < canvas.width - paddleWidth) {
                paddleX += 7;
            } else if (leftPressed && paddleX > 0) {
                paddleX -= 7;
            }
            requestAnimationFrame(draw);
        }
        // Start the game
        draw();
    </script>
</body>
</html>

# (Ruby on rail)Password Generator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Password Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .container {
      background-color: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      text-align: center;
    }
    h2 {
      margin-bottom: 20px;
      color: #333;
    }
    label {
      display: block;
      margin-bottom: 5px;
      color: #555;
    }
    input[type="number"] {
      width: 60px;
      padding: 8px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 16px;
    }
    button {
      padding: 10px 20px;
      margin-top: 10px;
      cursor: pointer;
      background-color: #4caf50;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 16px;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #45a049;
    }
    #password {
      width: 300px;
      padding: 8px;
      margin-top: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 16px;
      background-color: #f9f9f9;
      color: #333;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Password Generator</h2>
    <label for="length">Password Length (4-32)</label><br>
    <input type="number" id="length" min="4" max="32" value="8"><br>
    <button onclick="generatePassword()">Generate Password</button><br>
    <input type="text" id="password" readonly>
  </div>

  <script>
    function generatePassword() {
      const length = document.getElementById('length').value;
      const passwordField = document.getElementById('password');
      const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+~`|}{[]:;?><,./-=";
      let password = '';
      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * charset.length);
        password += charset[randomIndex];
      }
      passwordField.value = password;
    }
  </script>
</body>
</html>


