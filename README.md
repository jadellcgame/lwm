<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Healthy Steps Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Healthy Steps Game</h1>
        <div id="board"></div>
        <div class="controls">
            <button id="rollDice">Roll the Dice</button>
            <div id="message"></div>
        </div>
        <div id="playerPosition">You are on square: 1</div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.container {
    text-align: center;
}

#board {
    display: grid;
    grid-template-columns: repeat(10, 50px);
    grid-template-rows: repeat(10, 50px);
    gap: 2px;
    margin: 20px auto;
}

.square {
    width: 50px;
    height: 50px;
    background-color: #fff;
    border: 1px solid #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
}

.square.active {
    background-color: #cfe9fc;
}

.controls {
    margin-top: 20px;
}

button {
    padding: 10px 20px;
    font-size: 16px;
}
const board = document.getElementById('board');
const message = document.getElementById('message');
const playerPositionDiv = document.getElementById('playerPosition');

let playerPosition = 1;

// Generate the game board
function createBoard() {
    for (let i = 100; i >= 1; i--) {
        const square = document.createElement('div');
        square.classList.add('square');
        square.innerText = i;
        board.appendChild(square);

        // Assign ladders and snakes
        if (i === 3) square.dataset.move = 22; // Ladder
        if (i === 5) square.dataset.move = 8;  // Ladder
        if (i === 15) square.dataset.move = 26; // Ladder
        if (i === 20) square.dataset.move = 29; // Ladder
        if (i === 27) square.dataset.move = 1;  // Snake
        if (i === 21) square.dataset.move = 9;  // Snake
        if (i === 28) square.dataset.move = 6;  // Snake
        if (i === 35) square.dataset.move = 14; // Snake
    }
    updatePlayerPosition();
}

// Roll the dice
document.getElementById('rollDice').addEventListener('click', function() {
    const diceRoll = Math.floor(Math.random() * 6) + 1;
    message.innerText = `You rolled a ${diceRoll}!`;
    movePlayer(diceRoll);
});

// Move the player
function movePlayer(roll) {
    playerPosition += roll;
    if (playerPosition > 100) {
        playerPosition = 100; // Stop at square 100
    }
    const currentSquare = document.querySelector(`.square:nth-child(${101 - playerPosition})`);
    if (currentSquare.dataset.move) {
        playerPosition = currentSquare.dataset.move;
    }
    updatePlayerPosition();
}

// Update the player's position on the board
function updatePlayerPosition() {
    playerPositionDiv.innerText = `You are on square: ${playerPosition}`;
    const squares = document.querySelectorAll('.square');
    squares.forEach(square => square.classList.remove('active'));
    squares[100 - playerPosition].classList.add('active');
}

// Create the game board
createBoard();
