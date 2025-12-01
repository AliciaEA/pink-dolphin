<script>
    let gameState = $state("start");
    let score = $state(0);
    let highScore = $state(0);
    let gameSpeed = $state(3);
    // Orientation tracking
    let orientation = $state('portrait');
    let vw = $state(0);
    let vh = $state(0);
    // Breakpoint for mobile tweaks
    const SMALL_BREAKPOINT = 600; // px

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

        // Start the RAF loop once when mounted
        if (!animationFrameId) {
            animationFrameId = requestAnimationFrame(loop);
        }

        return () => {
            window.removeEventListener("keydown", handleKeyDown);

            window.removeEventListener("keyup", handleKeyUp);
            if (animationFrameId) {
                cancelAnimationFrame(animationFrameId);
                animationFrameId = undefined;
            }
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
        // No iniciar si aún no hay contexto del canvas
        if (!ctx) return;
        gameState = "playing";
        score = 0;
        gameSpeed = 3;
        obstacles = [];
        playerX = 175;
        frameCount = 0;
        // Loop is already running; no need to start again
    }

    function update() {
        if (gameState === "playing") {
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

            obstacles.forEach((obs) => {
                obs.y += gameSpeed;
            });

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
        }
    }

    function render() {
        if (!ctx) return;
        ctx.fillStyle = "#4fc3f7";
        ctx.fillRect(0, 0, 350, 600);

        // No translation needed; canvas is sized responsively in CSS
        ctx.save();

        // Player dolphin
        ctx.fillStyle = "#ff6b81";
        ctx.fillRect(playerX, playerY, 40, 40);
        ctx.fillStyle = "white";
        ctx.fillRect(playerX + 5, playerY + 5, 10, 10);
        ctx.fillRect(playerX + 25, playerY + 5, 10, 10);

        // Pupils (black) looking upward inside the eyes, slightly bigger
        ctx.fillStyle = "#000";
        ctx.fillRect(playerX + 7, playerY + 5, 6, 6);
        ctx.fillRect(playerX + 27, playerY + 5, 6, 6);

        // Nose (rectangular, pink) at the top of the head
        ctx.fillStyle = "#ff6b81";
        ctx.fillRect(playerX + 13, playerY - 4, 14, 4);

        // Tail fluke with two tips (inverted heart look)
        // Stem
        ctx.fillRect(playerX + 18, playerY + 40, 4, 4);
        // Left fluke
        ctx.fillRect(playerX - 2, playerY + 44, 16, 4);
        ctx.fillRect(playerX - 4, playerY + 48, 12, 4);
        // Right fluke
        ctx.fillRect(playerX + 26, playerY + 44, 16, 4);
        ctx.fillRect(playerX + 32, playerY + 48, 12, 4);

        // Obstacles
        ctx.fillStyle = "#5d4037";
        obstacles.forEach((obs) => {
            ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
        });

        ctx.restore();
    }

    function loop() {
        update();
        render();
        animationFrameId = requestAnimationFrame(loop);
    }

    // Pointer unified controls
    function pressLeft(e) {
        e.preventDefault();
        leftPressed = true;
    }
    function releaseLeft(e) {
        e.preventDefault();
        leftPressed = false;
    }
    function pressRight(e) {
        e.preventDefault();
        rightPressed = true;
    }
    function releaseRight(e) {
        e.preventDefault();
        rightPressed = false;
    }

    // Canvas-wide touch support: tap left/right half to move
    function handleCanvasPointerDown(e) {
        e.preventDefault();
        const rect = e.currentTarget.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const isLeft = x < rect.width / 2;
        leftPressed = isLeft;
        rightPressed = !isLeft;
    }
    function handleCanvasPointerUp(e) {
        e.preventDefault();
        leftPressed = false;
        rightPressed = false;
    }
    function handleCanvasTouchStart(e) {
        if (e.touches && e.touches.length) {
            const rect = e.currentTarget.getBoundingClientRect();
            const x = e.touches[0].clientX - rect.left;
            const isLeft = x < rect.width / 2;
            leftPressed = isLeft;
            rightPressed = !isLeft;
        }
        e.preventDefault();
    }
    function handleCanvasTouchEnd(e) {
        leftPressed = false;
        rightPressed = false;
        e.preventDefault();
    }
</script>

<!-- Orientation overlay appears only if landscape & small height -->


<div class="game-page" class:landscape={orientation === 'landscape'}>
    <div class="header">
        <h1>Pink dolphin Game🐬</h1>
        <a href="/" class="black-link">Return Home</a>
    </div>

    <div class="canvas-wrapper">
        <canvas
            bind:this={canvas}
            width="350"
            height="600"
            onpointerdown={handleCanvasPointerDown}
            onpointerup={handleCanvasPointerUp}
            onpointercancel={handleCanvasPointerUp}
            onpointerleave={handleCanvasPointerUp}
            ontouchstart={handleCanvasTouchStart}
            ontouchend={handleCanvasTouchEnd}
            ontouchcancel={handleCanvasTouchEnd}
        ></canvas>

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
            <!-- Invisible touch zones over the canvas as fallback -->
            <div class="touch-zone left" onpointerdown={pressLeft} onpointerup={releaseLeft} onpointerleave={releaseLeft} onpointercancel={releaseLeft}></div>
            <div class="touch-zone right" onpointerdown={pressRight} onpointerup={releaseRight} onpointerleave={releaseRight} onpointercancel={releaseRight}></div>
        {/if}
        {#if orientation === 'landscape' && vh < 420}
            <div class="overlay warn">
                <h2>Rotate Device</h2>
                <p>Portrait gives better play area</p>
            </div>
        {/if}
    </div>
    <p class="note">Controls: Tap left/right or keyboard arrows</p>
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
        
        --controls-space: 96px;
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

   

    .touch-zone{
        position: absolute;
        top: 0;
        bottom: 0;
        width: 50%;
        z-index: 15;
    }
    .touch-zone.left{ left: 0; }
    .touch-zone.right{ right: 0; }

    .note{
        margin-top: 1rem;
        font-size: 0.8rem;
        color: #64748b;
        text-align: center;
    }

    /* Landscape responsive adjustments */
    .game-page.landscape { flex-direction: row; align-items: flex-start; justify-content: center; gap: 1rem; }
    /* removed .mobile-controls in landscape */
    .canvas-wrapper { max-height: 80vh; }
    .overlay.warn { background: rgba(120,0,0,0.55); }

    /* Mobile: make canvas fill viewport height using flex */
    @media (max-width: 600px) {
        .game-page { height: 100dvh; min-height: 100dvh; padding: 0; overflow: hidden; }
        .header { padding: 8px 0; }
        .canvas-wrapper {
            width: 100%;
            max-width: 100%;
            max-height: none;
            height: auto;            /* override fixed height */
            flex: 1;                 /* take remaining height */
            min-height: 0;           /* allow flex item to shrink */
            display: block;
        }
        canvas { width: 100%; height: 100%; touch-action: none; }
        .note { padding-bottom: env(safe-area-inset-bottom); }
    }

</style>
