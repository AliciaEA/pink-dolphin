<script>
    let gameState = $state("start");
    let score = $state(0);
    let highScore = $state(0);
    let gameSpeed = $state(3);

    let canvas;
    let ctx;
    let animationFrameId;
    let playerX = 175; //Horiz Posit
    const playerY = 500; //Vert Posit
    let obstacles = [];
    let frameCount = 0;

    //inputs

    let leftPressed = false;
    let rightPressed = false;

    // Difficulty
    $effect(() => {
        let difficultyTimer;
        if (gameState === "playing") {
            difficultyTimer = setInterval(() => {
                gameSpeed += 0.5;
                console.log("Internal Stimuli: Speed increased to", gameSpeed);
            }, 5000);
        }
        return () => {
            if (difficultyTimer) clearInterval(difficultyTimer);
        };
    });

    // canvas and listeners

    $effect(() => {
        if (canvas) {
            ctx = canvas.getContext("2d");
        }

        window.addEventListener("keydown", handleKeyDown);
        window.addEventListener("keyup", handleKeyUp);

        return () => {
            window.removeEventListener("keydown", handleKeyDown);

            window.removeEventListener("keyup", handleKeyUp);
            cancelAnimationFrame(animationFrameId);
        };
    });
    function handleKeyDown(e) {
        if (e.key === "ArrowLeft" || e.key === "a") leftPressed = true;
        if (e.key === "ArrowRight" || e.key === "d") rightPressed = true;
        // Reiniciar con espacio
        if (e.code === "Space" && gameState !== "playing") startGame();
    }
    function handleKeyUp(e) {
        if (e.key === "ArrowLeft" || e.key === "a") leftPressed = false;
        if (e.key === "ArrowRight" || e.key === "d") rightPressed = false;
    }

    // external stimuli

    function touchLeftStart(e) {
        e.preventDefault();
        leftPressed = true;
    }

    function touchLeftEnd(e) {
        e.preventDefault();
        leftPressed = false;
    }
    function touchRightStart(e) {
        e.preventDefault();
        rightPressed = true;
    }
    function touchRightEnd(e) {
        e.preventDefault();
        rightPressed = false;
    }

    // Main Logic

    function startGame() {
        gameState = "playing";
        score = 0;
        gameSpeed = 3;
        obstacles = [];
        playerX = 175;
        frameCount = 0;
        loop();
    }

    function loop() {
        if (gameState !== "playing") return;

        frameCount++;
        score = Math.floor(frameCount / 10);

        if (leftPressed && playerX > 0) playerX -= 5;
        if (rightPressed && playerX < 350 - 40) playerX += 5;

        if (Math.random() < 0.02) {
            obstacles.push({
                x: Math.random() * (350 - 40),
                y: -50,
                width: 40,
                height: 40,
            });
        }

        // update obstacles positions
        obstacles.forEach((obs) => {
            obs.y += gameSpeed;
        });

        // collision detection
        obstacles.forEach((obs) => {
            if (
                playerX < obs.x + obs.width &&
                playerX + 40 > obs.x &&
                playerY < obs.y + obs.height &&
                playerY + 40 > obs.y
            ) {
                gameState = "gameover";
                if (score > highScore) highScore = score;
            }
        });

        // render
        if (ctx) {
            ctx.fillStyle = "#4fc3f7";
            ctx.fillRect(0, 0, 350, 600);

            ctx.fillStyle = "#ff6b81";
            ctx.fillRect(playerX, playerY, 40, 40);

            ctx.fillStyle = "white";
            ctx.fillRect(playerX + 5, playerY + 5, 10, 10);
            ctx.fillRect(playerX + 25, playerY + 5, 10, 10);

            ctx.fillStyle = "#5d4037";
            obstacles.forEach((obs) => {
                ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
            });
        }

        animationFrameId = requestAnimationFrame(loop);
    }
</script>

<div class="game-page">
    <div class="header">
        <h1>Pink dolphin Game🐬</h1>
        <a href="/" class="black-link">Return Home</a>
    </div>

    <div class="canvas-wrapper">
        <canvas bind:this={canvas} width="350" height="600"></canvas>

        {#if gameState === "start"}
            <div class="overlay">
                <h2>Amazon Run</h2>
                <p>Dodge!</p>
                <button onclick={startGame}>Tap To Start</button>
            </div>
        {/if}

        {#if gameState === "gameover"}
            <div class="overlay">
                <h2>Game Over</h2>
                <p>Score: {score}</p>
                <p>HighScore:{highScore}</p>
                <button onclick={startGame}>Try Again</button>
            </div>
        {/if}

        {#if gameState === "playing"}
            <div class="hud">
                <span>Score:{score}</span>
                <span class="speed-tag">⚡{gameSpeed.toFixed(1)}</span>
            </div>
        {/if}
    </div>
    <div class="mobile-controls">
        <button
            class="d-pad left"
            ontouchstart={touchLeftStart}
            ontouchend={touchLeftEnd}
            onmousedown={touchLeftStart}
            onmouseup={touchLeftEnd}
        >
            ⬅️
        </button>

        <button
            class="d-pad right"
            ontouchstart={touchRightStart}
            ontouchend={touchRightEnd}
            onmousedown={touchRightStart}
            onmouseup={touchRightEnd}
        >
            ➡️
        </button>
    </div>

    <p class="note">Controls: Keyboard Arrows or Touch Buttons</p>
</div>

<style>
    h1 {
        font-family: "Jim Nightshade";
        font-size: 1.8rem;
        margin: 0;
    }

    .game-page{
        display: flex;
        flex-direction: column;
        align-items: center;
        background: #cad9e3;
        min-height: 100vh;
        padding: 1rem;
        touch-action: none;
    }

    .header{
        text-align: center;
        margin-bottom: 0.5rem;
    }
    .black-link{
        font-size: 0.9rem;
        color: #0369a1;
        text-decoration: none;
    }

    .canvas-wrapper{
        position: relative;
		width: 350px;
		height: 600px;
		max-width: 100%;
		max-height: 65vh; 
		border: 4px solid #0284c7;
		border-radius: 12px;
		overflow: hidden;
		background: #4fc3f7;
		box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    }

    canvas{
        display: block;
        width: 100%;
        height: 100%;
    }

    /* ------ */

    .overlay{
        position: absolute;
		top: 0; left: 0;
		width: 100%; height: 100%;
		background: rgba(0, 20, 40, 0.7);
		color: white;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		z-index: 10;
    }

    h2{
        font-size: 2rem;
        margin-bottom: 0.5rem;
        color: #ff6b81;
    }

    button{
        margin-top: 1rem;
		padding: 12px 24px;
		font-size: 1.2rem;
		background: #ff6b81;
		border: none;
		color: white;
		border-radius: 50px;
		font-weight: bold;
		cursor: pointer;
		box-shadow: 0 4px 10px rgba(0,0,0,0.3);
		transition: transform 0.1s;
    }

    button:active{
        transform: scale(0.95);
    }

    .hud{
        position: absolute;
        top: 10px;
        width: 100%;
        display: flex;
        justify-content: space-between;
        padding: 0 15px;
        color: white;
        font-weight: bold;
        font-size: 1.1rem;
        text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
        pointer-events: none;
    }

    /* ----- */

    .mobile-controls{
        display: flex;
        gap: 15px;
        margin-top: 1rem;
        width: 100%;
        max-width: 350px;
    }

    .d-pad{
        flex: 1;
        padding: 20px 0;
        background: white;
        border: 2px solid #0284c7;
        color: #0284c7;
        border-radius: 12px;
        font-size: 1rem;
        text-transform: uppercase;
        user-select: none;
        touch-action: manipulation;
    }

    .d-pad:active{
        background: #0284c7;
        color: white;
    }

    .note{
        margin-top: 1rem;
        font-size: 0.8rem;
        color: #64748b;
        text-align: center;
    }

</style>
