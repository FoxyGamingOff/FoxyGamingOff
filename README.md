const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

// Set canvas size
canvas.width = 800;
canvas.height = 600;

// Game variables
const blockSize = 40;
const player = { x: 5, y: 5, width: 40, height: 40, color: 'blue' };
let blocks = [];

// Generate initial terrain
for (let i = 0; i < 20; i++) {
    blocks.push({ x: i * blockSize, y: canvas.height - blockSize, color: 'green' });
}

// Draw function
function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draw player
    ctx.fillStyle = player.color;
    ctx.fillRect(player.x, player.y, player.width, player.height);

    // Draw blocks
    blocks.forEach(block => {
        ctx.fillStyle = block.color;
        ctx.fillRect(block.x, block.y, blockSize, blockSize);
    });
}

// Player movement
document.addEventListener('keydown', (e) => {
    const speed = 5;
    if (e.key === 'ArrowRight') player.x += speed;
    if (e.key === 'ArrowLeft') player.x -= speed;
    if (e.key === 'ArrowUp' && player.y === canvas.height - player.height - blockSize) player.y -= 80;
});

// Gravity
function applyGravity() {
    if (player.y < canvas.height - player.height - blockSize) {
        player.y += 5; // Falling speed
    } else {
        player.y = canvas.height - player.height - blockSize; // Ground level
    }
}

// Main game loop
function gameLoop() {
    applyGravity();
    draw();
    requestAnimationFrame(gameLoop);
}

// Start the game loop
gameLoop();
