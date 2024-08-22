# Flappy Bird Game Project
## Overview

This project is a recreation of the popular Flappy Bird game, developed using JavaScript and HTML5 Canvas. The objective of the game is simple: control a bird that must fly between sets of pipes without hitting them. The game is both challenging and addictive, requiring precise timing and control to achieve high scores.

## Features

1. **Bird Control**: The player can control the bird by pressing the spacebar, up arrow, or 'X' key to make the bird jump.
2. **Dynamic Pipes**: Pipes are dynamically generated at random heights and move from right to left, providing a continuous challenge.
3. **Physics**: The game simulates gravity, causing the bird to fall if no input is provided, which adds to the difficulty.
4. **Collision Detection**: The game includes robust collision detection to determine if the bird has hit a pipe or the ground, resulting in a game over.
5. **Score Tracking**: Players score points by successfully navigating the bird through the gaps between pipes.
6. **Game Over State**: When a collision occurs, the game displays a "GAME OVER" message along with the final score.

## Code Structure

### Initial Setup

- **Variables**: The game initializes various variables to define dimensions for the game board, bird, and pipes. It also sets up game physics parameters such as gravity and jump strength, as well as the score and game over state.
- **Images**: The game loads images for the bird and pipes to render the graphics.

### Main Functions

1. **window.onload**: This function sets up the game board, loads images, and starts the main game loop and pipe placement interval.
2. **update**: The core game loop that updates the bird's position, moves pipes, checks for collisions, updates the score, and handles the game over state.
3. **placePipes**: Generates new pipes at regular intervals, ensuring they have random heights to increase the difficulty.
4. **moveBird**: Handles the bird's jump mechanics and resets the game on key press if the game is over.
5. **detectCollision**: Checks for collisions between the bird and the pipes or the ground.

### Files

1. **index.html**: The main HTML file containing the canvas element for rendering the game.
2. **script.js**: The JavaScript file containing the game logic and mechanics.
3. **flappybird.png**: Image file for the bird.
4. **toppipe.png**: Image file for the top section of the pipes.
5. **bottompipe.png**: Image file for the bottom section of the pipes.

### How Each Part of the Game Works

#### Initial Setup

1. **HTML**: Contains the basic structure of the document, including the canvas element where the game will be drawn.
   ```html
   <canvas id="gameCanvas" width="800" height="600"></canvas>
   <script src="script.js"></script>
   ```

2. **JavaScript (script.js)**: Where all the game logic is implemented.
   - **Global Variables**:
     ```javascript
     const canvas = document.getElementById('gameCanvas');
     const ctx = canvas.getContext('2d');
     const bird = { x: 50, y: 150, width: 20, height: 20, dy: 0 };
     const pipes = [];
     const gravity = 0.5;
     const jumpStrength = -10;
     let score = 0;
     let gameOver = false;
     ```

   - **Image Loading**:
     ```javascript
     const birdImg = new Image();
     const topPipeImg = new Image();
     const bottomPipeImg = new Image();
     birdImg.src = 'flappybird.png';
     topPipeImg.src = 'toppipe.png';
     bottomPipeImg.src = 'bottompipe.png';
     ```

#### window.onload Function

This function is executed when the page is loaded. It sets up the canvas and starts the game loop and pipe generation.

```javascript
window.onload = () => {
    setInterval(update, 20);
    setInterval(placePipes, 1500);
    document.addEventListener('keydown', moveBird);
};
```

#### update Function

This is the main game loop function. It is called repeatedly and updates the game state.

```javascript
function update() {
    if (gameOver) return;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    bird.dy += gravity;
    bird.y += bird.dy;
    ctx.drawImage(birdImg, bird.x, bird.y, bird.width, bird.height);

    pipes.forEach(pipe => {
        pipe.x -= 2;
        ctx.drawImage(topPipeImg, pipe.x, pipe.topPipeY, pipe.width, pipe.height);
        ctx.drawImage(bottomPipeImg, pipe.x, pipe.bottomPipeY, pipe.width, pipe.height);
        if (detectCollision(pipe)) {
            gameOver = true;
        }
        if (pipe.x + pipe.width < bird.x && !pipe.scored) {
            score++;
            pipe.scored = true;
        }
    });

    pipes = pipes.filter(pipe => pipe.x + pipe.width > 0);

    if (bird.y + bird.height > canvas.height || bird.y < 0) {
        gameOver = true;
    }

    ctx.fillText(`Score: ${score}`, 10, 20);

    if (gameOver) {
        ctx.fillText('GAME OVER', canvas.width / 2 - 50, canvas.height / 2);
    }
}
```

#### placePipes Function

This function generates new pipes at random heights and adds them to the pipes array.

```javascript
function placePipes() {
    if (gameOver) return;

    let pipeHeight = Math.floor(Math.random() * (canvas.height / 2)) + 20;
    let gap = 100;
    pipes.push({
        x: canvas.width,
        topPipeY: 0,
        bottomPipeY: pipeHeight + gap,
        width: 20,
        height: pipeHeight,
        scored: false
    });
}
```

#### moveBird Function

This function is called when a key is pressed, making the bird jump and restarting the game if it is in a game over state.

```javascript
function moveBird(e) {
    if (e.code === 'Space' || e.code === 'ArrowUp' || e.code === 'KeyX') {
        if (gameOver) {
            bird.y = 150;
            pipes.length = 0;
            score = 0;
            gameOver = false;
        } else {
            bird.dy = jumpStrength;
        }
    }
}
```

#### detectCollision Function

This function checks if the bird has collided with any pipe or the ground.

```javascript
function detectCollision(pipe) {
    if (bird.x < pipe.x + pipe.width &&
        bird.x + bird.width > pipe.x &&
        (bird.y < pipe.height || bird.y + bird.height > pipe.bottomPipeY)) {
        return true;
    }
    return false;
}
```

## Future Enhancements

1. **Sound Effects**: Add sound effects for jumping and collisions to enhance the gameplay experience.
2. **Start Screen and Restart Button**: Implement a start screen and a restart button to provide better control over game initiation and replay.
3. **Enhanced Graphics and Animations**: Improve the visual quality of the game with better graphics and smoother animations.
4. **Levels and Difficulty**: Introduce levels or increase the difficulty over time to keep the game challenging and engaging.

## Conclusion

This Flappy Bird game project is an excellent introduction to basic game development concepts using JavaScript and HTML5 Canvas. It covers essential elements such as dynamic object creation, collision detection, and user input handling. The project can be further enhanced with additional features to create a more polished and engaging game.
