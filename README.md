<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ping Pong Game</title>
<style>
    body {
        margin: 0;
        padding: 0;
        font-family: 'Roboto', sans-serif;
        background-color: #f9f9f9;
    }

    #header {
        height: 13%;
        text-align: center;
        background-color: #333;
        color: #fff;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #game-container {
        height: 70%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #game {
        width: 70%;
        border: 1px solid #000;
        position: relative;
    }

    #player-paddle, #computer-paddle {
        position: absolute;
        width: 5px;
        height: 80px;
        background-color: #000;
    }

    #player-paddle {
        left: 10px;
    }

    #computer-paddle {
        right: 10px;
    }

    #ball {
        position: absolute;
        width: 10px;
        height: 10px;
        background-color: red;
        border-radius: 50%;
    }

    #score-bar {
        height: 2%;
        background-color: #333;
        color: #fff;
        text-align: center;
        line-height: 2;
        position: fixed;
        bottom: 0;
        width: 100%;
    }

    #button-container {
        height: 13%;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .button {
        padding: 10px 20px;
        font-size: 1rem;
        margin: 0 10px;
    }
</style>
</head>
<body>

<div id="header">
    <h1>Ping Pong Game</h1>
</div>

<div id="game-container">
    <div id="game">
        <div id="player-paddle"></div>
        <div id="computer-paddle"></div>
        <div id="ball"></div>
    </div>
</div>

<div id="score-bar">Puntos: <span id="score">0</span></div>

<div id="button-container">
    <button class="button" id="startButton" onclick="startGame()">Iniciar</button>
    <button class="button" id="stopButton" onclick="stopGame()" disabled>Parar</button>
    <button class="button" id="restartButton" onclick="restartGame()">Reiniciar</button>
</div>

<script>
    const gameContainer = document.getElementById("game");
    const playerPaddle = document.getElementById("player-paddle");
    const computerPaddle = document.getElementById("computer-paddle");
    const ball = document.getElementById("ball");
    const scoreDisplay = document.getElementById("score");
    const startButton = document.getElementById("startButton");
    const stopButton = document.getElementById("stopButton");
    const restartButton = document.getElementById("restartButton");

    let gameInterval;
    let ballX, ballY, ballSpeedX, ballSpeedY;
    let playerY, computerY;
    let score = 0;

    function startGame() {
        startButton.disabled = true;
        stopButton.disabled = false;
        restartButton.disabled = true;

        initializeGame();
        gameInterval = setInterval(updateGame, 20);
    }

    function stopGame() {
        startButton.disabled = false;
        stopButton.disabled = true;
        restartButton.disabled = false;

        clearInterval(gameInterval);
    }

    function restartGame() {
        stopGame();
        initializeGame();
    }

    function initializeGame() {
        score = 0;
        scoreDisplay.textContent = score;

        playerY = gameContainer.clientHeight / 2 - playerPaddle.clientHeight / 2;
        computerY = gameContainer.clientHeight / 2 - computerPaddle.clientHeight / 2;

        ballX = gameContainer.clientWidth / 2 - ball.clientWidth / 2;
        ballY = gameContainer.clientHeight / 2 - ball.clientHeight / 2;

        ballSpeedX = 3;
        ballSpeedY = 3;
    }

    function updateGame() {
        movePlayerPaddle();
        moveComputerPaddle();
        moveBall();
        checkCollision();
    }

    function movePlayerPaddle() {
        playerPaddle.style.top = playerY + "px";
    }

    function moveComputerPaddle() {
        let computerSpeed = 2;
        if (ballY > computerY + computerPaddle.clientHeight / 2) {
            computerY += computerSpeed;
        } else if (ballY < computerY + computerPaddle.clientHeight / 2) {
            computerY -= computerSpeed;
        }
        computerPaddle.style.top = computerY + "px";
    }

    function moveBall() {
        ballX += ballSpeedX;
        ballY += ballSpeedY;
        ball.style.left = ballX + "px";
        ball.style.top = ballY + "px";

        if (ballY <= 0 || ballY >= gameContainer.clientHeight - ball.clientHeight) {
            ballSpeedY = -ballSpeedY;
        }

        if (ballX <= 0) {
            score++;
            scoreDisplay.textContent = score;
            ballSpeedX = -ballSpeedX;
        }

        if (ballX >= gameContainer.clientWidth - ball.clientWidth) {
            stopGame();
            alert("¡Perdiste!");
        }
    }

    function checkCollision() {
        if (ballX < playerPaddle.clientWidth && ballY > playerY && ballY < playerY + playerPaddle.clientHeight) {
            ballSpeedX = -ballSpeedX;
        }

        if (
            ballX > gameContainer.clientWidth - (playerPaddle.clientWidth + ball.clientWidth) &&
            ballY > computerY &&
            ballY < computerY + computerPaddle.clientHeight
        ) {
            ballSpeedX = -ballSpeedX;
        }
    }
</script>

</body>
</html>
