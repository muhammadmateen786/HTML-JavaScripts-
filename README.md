# HTML-JavaScripts-
Game 
// Create the canvas element
const canvas = document.createElement('canvas');
const ctx = canvas.getContext('2d');
document.body.appendChild(canvas);

// Set the canvas size
const canvasWidth = 400;
const canvasHeight = 400;
canvas.width = canvasWidth;
canvas.height = canvasHeight;

// Define the size of each grid cell
const gridSize = 20;
const gridWidth = canvasWidth / gridSize;
const gridHeight = canvasHeight / gridSize;

// Initialize the snake's position and direction
let snake = [{ x: 10, y: 10 }];
let direction = 'right';

// Generate random food position
let food = generateFood();

// Main game loop
function gameLoop() {
  // Clear the canvas
  ctx.clearRect(0, 0, canvasWidth, canvasHeight);

  // Move the snake
  moveSnake();

  // Check for collisions
  if (checkCollision()) {
    gameOver();
    return;
  }

  // Check if the snake has eaten the food
  if (snake[0].x === food.x && snake[0].y === food.y) {
    // Increase the snake's length
    snake.push({});
    // Generate new food position
    food = generateFood();
  }

  // Draw the snake
  drawSnake();

  // Draw the food
  drawFood();

  // Call the game loop again
  requestAnimationFrame(gameLoop);
}

// Function to move the snake
function moveSnake() {
  // Create a new head for the snake
  const newHead = { x: snake[0].x, y: snake[0].y };

  // Update the new head's position based on the direction
  if (direction === 'up') newHead.y--;
  if (direction === 'down') newHead.y++;
  if (direction === 'left') newHead.x--;
  if (direction === 'right') newHead.x++;

  // Add the new head to the snake
  snake.unshift(newHead);

  // Remove the tail of the snake
  snake.pop();
}

// Function to check for collisions
function checkCollision() {
  // Check if the snake has collided with itself
  for (let i = 1; i < snake.length; i++) {
    if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) {
      return true;
    }
  }

  // Check if the snake has collided with the boundaries of the screen
  if (
    snake[0].x < 0 ||
    snake[0].x >= gridWidth ||
    snake[0].y < 0 ||
    snake[0].y >= gridHeight
  ) {
    return true;
  }

  return false;
}

// Function to generate random food position
function generateFood() {
  return {
    x: Math.floor(Math.random() * gridWidth),
    y: Math.floor(Math.random() * gridHeight),
  };
}

// Function to draw the snake
function drawSnake() {
  ctx.fillStyle = 'green';
  for (let i = 0; i < snake.length; i++) {
    ctx.fillRect(
      snake[i].x * gridSize,
      snake[i].y * gridSize,
      gridSize,
      gridSize
    );
  }
}

// Function to draw the food
function drawFood() {
  ctx.fillStyle = 'red';
  ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize, gridSize);
}

// Function to handle game over
function gameOver() {
  alert('Game Over');
  // Reset the snake's position and direction
  snake = [{ x: 10, y: 10 }];
  direction = 'right';
  // Generate new food position
  food = generateFood();
}

// Event listener for keyboard input
document.addEventListener('keydown', (event) => {
  if (event.key === 'ArrowUp' && direction !== 'down') direction = 'up';
  if (event.key === 'ArrowDown' && direction !== 'up') direction = 'down';
  if (event.key === 'ArrowLeft' && direction !== 'right') direction = 'left';
  if (event.key === 'ArrowRight' && direction !== 'left') direction = 'right';
});

// Start the game loop
gameLoop();
