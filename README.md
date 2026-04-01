<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Ver de terre Snake 🪱</title>
<style>
    body {
        text-align: center;
        background: #222;
        color: white;
        font-family: Arial;
    }

    canvas {
        background: #333;
        margin-top: 20px;
        border: 2px solid white;
    }

    #joystick {
        margin-top: 20px;
    }

    .row {
        display: flex;
        justify-content: center;
    }

    button {
        width: 60px;
        height: 60px;
        margin: 5px;
        font-size: 24px;
        border-radius: 10px;
        border: none;
    }

    #resetBtn {
        width: 150px;
        font-size: 18px;
        margin-top: 15px;
        background: #ff4444;
        color: white;
    }
</style>
</head>
<body>

<h1>Ver de terre 🪱 mange du caca 💩</h1>
<p>Clavier OU joystick 👇</p>

<canvas id="game" width="400" height="400"></canvas>

<div id="joystick">
    <div class="row">
        <button onclick="setDir('UP')">⬆️</button>
    </div>
    <div class="row">
        <button onclick="setDir('LEFT')">⬅️</button>
        <button onclick="setDir('DOWN')">⬇️</button>
        <button onclick="setDir('RIGHT')">➡️</button>
    </div>
</div>

<button id="resetBtn" onclick="resetGame()">Recommencer 🔄</button>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let box = 20;
let worm;
let direction;
let food;
let game;

// initialisation
function initGame() {
    worm = [{x: 9 * box, y: 10 * box}];
    direction = "";
    food = {
        x: Math.floor(Math.random()*20)*box,
        y: Math.floor(Math.random()*20)*box
    };
}

// clavier
document.addEventListener("keydown", e => {
    if (e.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
    else if (e.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
    else if (e.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
    else if (e.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
});

// joystick
function setDir(dir) {
    if (dir === "LEFT" && direction !== "RIGHT") direction = "LEFT";
    if (dir === "UP" && direction !== "DOWN") direction = "UP";
    if (dir === "RIGHT" && direction !== "LEFT") direction = "RIGHT";
    if (dir === "DOWN" && direction !== "UP") direction = "DOWN";
}

function draw() {
    ctx.fillStyle = "#333";
    ctx.fillRect(0, 0, 400, 400);

    // ver
    for (let i = 0; i < worm.length; i++) {
        ctx.fillStyle = i === 0 ? "#7CFC00" : "#32CD32";
        ctx.fillRect(worm[i].x, worm[i].y, box, box);
    }

    // caca
    ctx.font = "20px Arial";
    ctx.fillText("💩", food.x, food.y + 18);

    let wormX = worm[0].x;
    let wormY = worm[0].y;

    if (direction === "LEFT") wormX -= box;
    if (direction === "UP") wormY -= box;
    if (direction === "RIGHT") wormX += box;
    if (direction === "DOWN") wormY += box;

    if (wormX === food.x && wormY === food.y) {
        food = {
            x: Math.floor(Math.random()*20)*box,
            y: Math.floor(Math.random()*20)*box
        };
    } else {
        worm.pop();
    }

    let newHead = {x: wormX, y: wormY};

    if (
        wormX < 0 || wormY < 0 ||
        wormX >= 400 || wormY >= 400 ||
        collision(newHead, worm)
    ) {
        clearInterval(game);
        alert("Game Over 😢");
    }

    worm.unshift(newHead);
}

function collision(head, array) {
    for (let i = 0; i < array.length; i++) {
        if (head.x === array[i].x && head.y === array[i].y) {
            return true;
        }
    }
    return false;
}

// bouton reset
function resetGame() {
    clearInterval(game);
    initGame();
    game = setInterval(draw, 100);
}

// démarrage
initGame();
game = setInterval(draw, 100);
</script>

</body>
</html>
