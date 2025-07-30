<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aqua-Guard: Ocean Cleanup</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: 'Comic Sans MS', cursive, sans-serif;
            background-color: #1a6bb0;
        }
        
        #landing-page, #game-container, #rules-screen {
            position: relative;
            width: 100vw;
            height: 100vh;
            background-image: url('https://images.unsplash.com/photo-1505118380757-91f5f5632de0?ixlib=rb-1.2.1&auto=format&fit=crop&w=1920&q=80');
            background-size: cover;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
        }
        
        #game-container, #rules-screen {
            display: none;
        }
        
        #landing-content, #rules-content {
            background-color: rgba(0, 0, 0, 0.7);
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
        }
        
        h1 {
            font-size: 48px;
            margin-bottom: 30px;
            color: #4CAF50;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        #name-input {
            padding: 15px;
            font-size: 20px;
            width: 300px;
            border-radius: 10px;
            border: none;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .game-button {
            padding: 15px 30px;
            font-size: 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 20px;
            transition: transform 0.2s;
        }
        
        .game-button:hover {
            transform: scale(1.1);
            background-color: #45a049;
        }
        
        .item-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
            margin: 30px 0;
        }
        
        .item-card {
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 10px;
            padding: 15px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .item-image {
            width: 80px;
            height: 80px;
            margin-bottom: 10px;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }
        
        .item-label {
            font-size: 18px;
            color: #333;
            font-weight: bold;
        }
        
        .trash-label {
            color: #e53935;
        }
        
        .distractor-label {
            color: #2196F3;
        }
        
        #ui {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 10px;
            border-radius: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            z-index: 100;
        }
        
        #timer {
            font-size: 24px;
            font-weight: bold;
            color: #d32f2f;
            margin-bottom: 10px;
        }
        
        #lives {
            display: flex;
            gap: 5px;
        }
        
        .life {
            width: 30px;
            height: 30px;
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23d32f2f"><path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/></svg>');
            background-size: contain;
        }
        
        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 24px;
            font-weight: bold;
            color: white;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 10px;
            border-radius: 10px;
            z-index: 100;
        }
        
        #start-screen, #end-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 200;
            color: white;
        }
        
        #end-screen {
            display: none;
        }
        
        .trash {
            position: absolute;
            width: 70px;
            height: 70px;
            cursor: pointer;
            transition: transform 0.2s;
            z-index: 10;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }
        
        .trash:hover {
            transform: scale(1.1);
        }
        
        .fish {
            position: absolute;
            width: 80px;
            height: 50px;
            z-index: 5;
            transition: transform 0.5s;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }
        
        .distractor {
            position: absolute;
            width: 70px;
            height: 70px;
            z-index: 5;
            cursor: pointer;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
        }
        
        #message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(255, 255, 255, 0.9);
            padding: 20px;
            border-radius: 10px;
            font-size: 24px;
            color: #333;
            display: none;
            z-index: 150;
            text-align: center;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
        }
        
        #sound-toggle {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background-color: rgba(255, 255, 255, 0.7);
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            font-size: 24px;
            cursor: pointer;
            z-index: 100;
        }
        
        p {
            font-size: 24px;
            max-width: 600px;
            text-align: center;
            margin-bottom: 20px;
        }
        
        .rules-text {
            font-size: 20px;
            text-align: left;
            margin: 20px 0;
            line-height: 1.5;
        }
        
        .section-title {
            font-size: 28px;
            color: #4CAF50;
            margin: 30px 0 15px 0;
            text-align: center;
        }
    </style>
</head>
<body>
    <!-- Landing Page -->
    <div id="landing-page">
        <div id="landing-content">
            <h1>WELCOME TO AQUA-GUARD!</h1>
            <p>Help clean the ocean by removing trash to protect the fish!</p>
            <input type="text" id="name-input" placeholder="Enter your name" maxlength="15">
            <button class="game-button" id="start-game-button">Start Game</button>
        </div>
    </div>

    <!-- Rules Screen -->
    <div id="rules-screen">
        <div id="rules-content">
            <h1>GAME RULES</h1>
            
            <div class="rules-text">
                <p>Welcome, <span id="player-name-display"></span>!</p>
                <p>Your mission is to clean the ocean by removing harmful trash while protecting sea creatures.</p>
            </div>
            
            <div class="section-title">HOW TO PLAY</div>
            <div class="rules-text">
                <p>1. You have <strong>60 seconds</strong> to collect <strong>20 pieces of trash</strong></p>
                <p>2. Click on trash items to remove them from the ocean</p>
                <p>3. Avoid clicking on sea creatures - you'll lose a life!</p>
                <p>4. You start with <strong>3 lives</strong></p>
            </div>
            
            <div class="section-title">TRASH TO COLLECT</div>
            <div class="item-grid">
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><rect x=\'30\' y=\'20\' width=\'40\' height=\'60\' fill=\'%23f5e942\'/><rect x=\'35\' y=\'25\' width=\'30\' height=\'50\' fill=\'%23fff\'/><rect x=\'40\' y=\'70\' width=\'20\' height=\'10\' fill=\'%23f5e942\'/><circle cx=\'50\' cy=\'30\' r=\'5\' fill=\'%23f5e942\'/></svg>')"></div>
                    <div class="item-label trash-label">Plastic Bottle</div>
                </div>
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><rect x=\'20\' y=\'30\' width=\'60\' height=\'40\' rx=\'5\' ry=\'5\' fill=\'%23e53935\'/><rect x=\'25\' y=\'35\' width=\'50\' height=\'30\' rx=\'3\' ry=\'3\' fill=\'%23fff\'/><text x=\'50\' y=\'55\' font-family=\'Arial\' font-size=\'20\' text-anchor=\'middle\' fill=\'%23e53935\'>Soda</text></svg>')"></div>
                    <div class="item-label trash-label">Soda Can</div>
                </div>
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><rect x=\'20\' y=\'40\' width=\'60\' height=\'20\' fill=\'%23ffffff\'/><rect x=\'25\' y=\'45\' width=\'50\' height=\'10\' fill=\'%2333b5e5\'/><path d=\'M20,40 L50,20 L80,40 Z\' fill=\'%23ffffff\'/><path d=\'M25,45 L50,25 L75,45 Z\' fill=\'%2333b5e5\'/></svg>')"></div>
                    <div class="item-label trash-label">Plastic Bag</div>
                </div>
            </div>
            
            <div class="section-title">SEA CREATURES TO PROTECT</div>
            <div class="item-grid">
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><circle cx=\'50\' cy=\'50\' r=\'40\' fill=\'%23FF9800\'/><circle cx=\'35\' cy=\'40\' r=\'5\' fill=\'%23fff\'/><circle cx=\'35\' cy=\'40\' r=\'2\' fill=\'%23000\'/><path d=\'M30,70 Q50,90 70,70\' stroke=\'%23000\' stroke-width=\'2\' fill=\'none\'/></svg>')"></div>
                    <div class="item-label distractor-label">Happy Crab</div>
                </div>
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><path d=\'M20,50 Q50,20 80,50 Q50,80 20,50\' fill=\'%23E91E63\'/><circle cx=\'65\' cy=\'45\' r=\'5\' fill=\'%23fff\'/><circle cx=\'65\' cy=\'45\' r=\'2\' fill=\'%23000\'/><path d=\'M40,60 Q50,70 60,60\' stroke=\'%23000\' stroke-width=\'2\' fill=\'none\'/></svg>')"></div>
                    <div class="item-label distractor-label">Jellyfish</div>
                </div>
                <div class="item-card">
                    <div class="item-image" style="background-image: url('data:image/svg+xml;utf8,<svg xmlns=\'http://www.w3.org/2000/svg\' viewBox=\'0 0 100 100\'><path d=\'M50,10 L90,50 L50,90 L10,50 Z\' fill=\'%2300BCD4\' opacity=\'0.8\'/><circle cx=\'50\' cy=\'50\' r=\'20\' fill=\'%23fff\'/><circle cx=\'50\' cy=\'50\' r=\'15\' fill=\'%2300BCD4\'/></svg>')"></div>
                    <div class="item-label distractor-label">Starfish</div>
                </div>
            </div>
            
            <button class="game-button" id="continue-button">Continue to Game</button>
        </div>
    </div>

    <!-- Game Container -->
    <div id="game-container">
        <div id="ui">
            <div id="timer">60</div>
            <div id="lives">
                <div class="life"></div>
                <div class="life"></div>
                <div class="life"></div>
            </div>
        </div>
        <div id="score">Trash Collected: 0/20</div>
        <button id="sound-toggle">🔊</button>
        
        <div id="start-screen">
            <h1>Aqua-Guard</h1>
            <p>Ready to clean the ocean, <span id="game-start-name"></span>?</p>
            <button class="game-button" id="start-button">Start Game</button>
        </div>
        
        <div id="end-screen">
            <h1 id="end-title">Game Over</h1>
            <p id="end-message">You collected 0 pieces of trash!</p>
            <button class="game-button" id="restart-button">Play Again</button>
        </div>
        
        <div id="message"></div>
    </div>

    <!-- Sound Effects -->
    <audio id="landing-sound" loop preload="auto" src="https://assets.mixkit.co/music/preview/mixkit-happy-children-958.mp3"></audio>
    <audio id="gameplay-sound" loop preload="auto" src="https://assets.mixkit.co/music/preview/mixkit-game-show-suspense-waiting-668.mp3"></audio>
    <audio id="endgame-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3"></audio>
    <audio id="click-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-arcade-game-jump-coin-216.mp3"></audio>
    <audio id="success-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-achievement-bell-600.mp3"></audio>
    <audio id="error-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-retro-arcade-lose-2027.mp3"></audio>
    <audio id="bubble-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-bubbles-in-water-1173.mp3"></audio>
    <audio id="splash-sound" preload="auto" src="https://assets.mixkit.co/sfx/preview/mixkit-water-splash-1183.mp3"></audio>

    <script>
        // Game variables
        let score = 0;
        let timeLeft = 60;
        let lives = 3;
        let gameInterval;
        let timerInterval;
        let trashItems = [];
        let fishItems = [];
        let distractorItems = [];
        let gameActive = false;
        let soundOn = true;
        let playerName = "";
        
        // DOM elements
        const landingPage = document.getElementById('landing-page');
        const gameContainer = document.getElementById('game-container');
        const rulesScreen = document.getElementById('rules-screen');
        const nameInput = document.getElementById('name-input');
        const playerNameDisplay = document.getElementById('player-name-display');
        const gameStartName = document.getElementById('game-start-name');
        const startGameButton = document.getElementById('start-game-button');
        const continueButton = document.getElementById('continue-button');
        const timerElement = document.getElementById('timer');
        const livesElement = document.getElementById('lives');
        const scoreElement = document.getElementById('score');
        const startScreen = document.getElementById('start-screen');
        const endScreen = document.getElementById('end-screen');
        const startButton = document.getElementById('start-button');
        const restartButton = document.getElementById('restart-button');
        const endTitle = document.getElementById('end-title');
        const endMessage = document.getElementById('end-message');
        const messageElement = document.getElementById('message');
        const soundToggle = document.getElementById('sound-toggle');
        
        // Sound elements
        const landingSound = document.getElementById('landing-sound');
        const gameplaySound = document.getElementById('gameplay-sound');
        const endgameSound = document.getElementById('endgame-sound');
        const clickSound = document.getElementById('click-sound');
        const successSound = document.getElementById('success-sound');
        const errorSound = document.getElementById('error-sound');
        const bubbleSound = document.getElementById('bubble-sound');
        const splashSound = document.getElementById('splash-sound');
        
        // Image data
        const trashImages = [
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect x="30" y="20" width="40" height="60" fill="%23f5e942"/><rect x="35" y="25" width="30" height="50" fill="%23fff"/><rect x="40" y="70" width="20" height="10" fill="%23f5e942"/><circle cx="50" cy="30" r="5" fill="%23f5e942"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect x="20" y="30" width="60" height="40" rx="5" ry="5" fill="%23e53935"/><rect x="25" y="35" width="50" height="30" rx="3" ry="3" fill="%23fff"/><text x="50" y="55" font-family="Arial" font-size="20" text-anchor="middle" fill="%23e53935">Soda</text></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect x="20" y="40" width="60" height="20" fill="%23ffffff"/><rect x="25" y="45" width="50" height="10" fill="%2333b5e5"/><path d="M20,40 L50,20 L80,40 Z" fill="%23ffffff"/><path d="M25,45 L50,25 L75,45 Z" fill="%2333b5e5"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="30" fill="%23ff9800"/><circle cx="50" cy="50" r="25" fill="%23fff"/><path d="M50,25 L55,40 L70,40 L57,50 L62,65 L50,55 L38,65 L43,50 L30,40 L45,40 Z" fill="%23ff9800"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><rect x="30" y="20" width="40" height="60" fill="%23b0bec5"/><rect x="35" y="25" width="30" height="50" fill="%23eceff1"/><rect x="40" y="30" width="20" height="40" fill="%23b0bec5" opacity="0.5"/></svg>'
        ];
        
        const fishImages = [
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 50"><path d="M10,25 Q30,5 50,25 Q70,45 90,25 Q70,5 50,25 Q30,45 10,25" fill="%23FF5722"/><circle cx="75" cy="20" r="3" fill="%23000"/><path d="M5,15 L15,25 L5,35 Z" fill="%23FF9800"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 50"><path d="M10,25 Q30,0 50,25 Q70,50 90,25 Q70,0 50,25 Q30,50 10,25" fill="%23009688"/><circle cx="75" cy="20" r="3" fill="%23000"/><path d="M5,15 L15,25 L5,35 Z" fill="%2300BCD4"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 50"><path d="M10,25 Q30,10 50,25 Q70,40 90,25 Q70,10 50,25 Q30,40 10,25" fill="%23E91E63"/><circle cx="75" cy="20" r="3" fill="%23000"/><path d="M5,15 L15,25 L5,35 Z" fill="%23FF4081"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 50"><path d="M10,25 Q30,15 50,25 Q70,35 90,25 Q70,15 50,25 Q30,35 10,25" fill="%23FFC107"/><circle cx="75" cy="20" r="3" fill="%23000"/><path d="M5,15 L15,25 L5,35 Z" fill="%23FFEB3B"/></svg>'
        ];
        
        const distractorImages = [
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="%23FF9800"/><circle cx="35" cy="40" r="5" fill="%23fff"/><circle cx="35" cy="40" r="2" fill="%23000"/><path d="M30,70 Q50,90 70,70" stroke="%23000" stroke-width="2" fill="none"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M20,50 Q50,20 80,50 Q50,80 20,50" fill="%23E91E63"/><circle cx="65" cy="45" r="5" fill="%23fff"/><circle cx="65" cy="45" r="2" fill="%23000"/><path d="M40,60 Q50,70 60,60" stroke="%23000" stroke-width="2" fill="none"/></svg>',
            'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M50,10 L90,50 L50,90 L10,50 Z" fill="%2300BCD4" opacity="0.8"/><circle cx="50" cy="50" r="20" fill="%23fff"/><circle cx="50" cy="50" r="15" fill="%2300BCD4"/></svg>'
        ];

        // Initialize game
        function init() {
            // Initialize all audio elements
            initAudio();
            
            // Set up event listeners
            startGameButton.addEventListener('click', startFromLanding);
            continueButton.addEventListener('click', continueToGame);
            soundToggle.addEventListener('click', toggleSound);
            startButton.addEventListener('click', startGame);
            restartButton.addEventListener('click', restartGame);
            
            // Focus on name input
            nameInput.focus();
        }

        // Initialize audio elements
        function initAudio() {
            const audioElements = [
                landingSound, gameplaySound, endgameSound, 
                clickSound, successSound, errorSound,
                bubbleSound, splashSound
            ];
            
            audioElements.forEach(audio => {
                audio.volume = 0.3; // Set default volume
                audio.load(); // Preload audio
            });
            
            // Try to play landing sound (may fail due to autoplay policies)
            if (soundOn) {
                landingSound.play().catch(e => {
                    console.log("Autoplay prevented. User interaction required to play sounds.");
                });
            }
        }
        
        function startFromLanding() {
            playerName = nameInput.value.trim() || "Player";
            playerNameDisplay.textContent = playerName;
            gameStartName.textContent = playerName;
            
            // Transition to rules screen
            landingPage.style.display = 'none';
            rulesScreen.style.display = 'flex';
            
            // Stop landing sound
            landingSound.pause();
        }
        
        function continueToGame() {
            // Transition to game container
            rulesScreen.style.display = 'none';
            gameContainer.style.display = 'block';
            
            // Show start screen
            startScreen.style.display = 'flex';
        }
        
        function toggleSound() {
            soundOn = !soundOn;
            soundToggle.textContent = soundOn ? "🔊" : "🔇";
            
            const audioElements = [
                landingSound, gameplaySound, endgameSound, 
                clickSound, successSound, errorSound,
                bubbleSound, splashSound
            ];
            
            if (soundOn) {
                // Resume appropriate sound based on game state
                if (landingPage.style.display !== 'none') {
                    landingSound.play().catch(e => console.log("Audio play failed:", e));
                } else if (gameActive) {
                    gameplaySound.play().catch(e => console.log("Audio play failed:", e));
                }
            } else {
                // Pause all sounds
                audioElements.forEach(audio => audio.pause());
            }
        }
        
        function startGame() {
            // Reset game state
            score = 0;
            timeLeft = 60;
            lives = 3;
            gameActive = true;
            
            // Clear any existing elements
            trashItems.forEach(item => item.element.remove());
            fishItems.forEach(item => item.element.remove());
            distractorItems.forEach(item => item.element.remove());
            trashItems = [];
            fishItems = [];
            distractorItems = [];
            
            // Update UI
            timerElement.textContent = timeLeft;
            scoreElement.textContent = `Trash Collected: ${score}/20`;
            updateLives();
            
            // Hide screens
            startScreen.style.display = 'none';
            endScreen.style.display = 'none';
            
            // Play gameplay sound
            if (soundOn) {
                gameplaySound.currentTime = 0;
                gameplaySound.play().catch(e => console.log("Audio play failed:", e));
                splashSound.currentTime = 0;
                splashSound.play().catch(e => console.log("Audio play failed:", e));
            }
            
            // Create game elements
            createTrash();
            createFish();
            createDistractors();
            
            // Start game loop
            gameInterval = setInterval(moveGameElements, 100);
            
            // Start timer
            timerInterval = setInterval(() => {
                timeLeft--;
                timerElement.textContent = timeLeft;
                
                if (timeLeft <= 0) {
                    endGame(false);
                }
            }, 1000);
        }
        
        function restartGame() {
            // Clear any existing game elements
            trashItems.forEach(item => item.element.remove());
            fishItems.forEach(item => item.element.remove());
            distractorItems.forEach(item => item.element.remove());
            trashItems = [];
            fishItems = [];
            distractorItems = [];
            
            // Clear intervals
            clearInterval(gameInterval);
            clearInterval(timerInterval);
            
            // Stop all sounds
            gameplaySound.pause();
            endgameSound.pause();
            
            // Reset game state
            score = 0;
            timeLeft = 60;
            lives = 3;
            gameActive = false;
            
            // Update UI
            timerElement.textContent = timeLeft;
            scoreElement.textContent = `Trash Collected: ${score}/20`;
            updateLives();
            
            // Hide end screen and show start screen
            endScreen.style.display = 'none';
            startScreen.style.display = 'flex';
            
            // Reset the sound toggle if needed
            if (!soundOn) {
                soundToggle.textContent = "🔇";
            }
        }
        
        function createTrash() {
            for (let i = 0; i < 20; i++) {
                const trash = document.createElement('div');
                trash.className = 'trash';
                
                // Random position
                const x = Math.random() * (gameContainer.offsetWidth - 70);
                const y = Math.random() * (gameContainer.offsetHeight - 70);
                
                // Random trash image
                const imgIndex = Math.floor(Math.random() * trashImages.length);
                trash.style.backgroundImage = `url('${trashImages[imgIndex]}')`;
                
                trash.style.left = `${x}px`;
                trash.style.top = `${y}px`;
                
                // Random movement
                const speed = 0.5 + Math.random() * 1.5;
                const angle = Math.random() * Math.PI * 2;
                const dx = Math.cos(angle) * speed;
                const dy = Math.sin(angle) * speed;
                
                // Add click event
                trash.addEventListener('click', () => {
                    if (!gameActive) return;
                    
                    if (soundOn) {
                        clickSound.currentTime = 0;
                        clickSound.play().catch(e => console.log("Audio play failed:", e));
                        bubbleSound.currentTime = 0;
                        bubbleSound.play().catch(e => console.log("Audio play failed:", e));
                    }
                    
                    // Remove trash
                    trash.remove();
                    
                    // Update score
                    score++;
                    scoreElement.textContent = `Trash Collected: ${score}/20`;
                    
                    // Show success effect
                    showMessage('Good job!', '#4CAF50');
                    if (soundOn) {
                        successSound.currentTime = 0;
                        successSound.play().catch(e => console.log("Audio play failed:", e));
                    }
                    
                    // Remove from array
                    trashItems = trashItems.filter(item => item.element !== trash);
                    
                    // Check win condition
                    if (score >= 20) {
                        endGame(true);
                    }
                });
                
                gameContainer.appendChild(trash);
                trashItems.push({ element: trash, x, y, dx, dy });
            }
        }
        
        function createFish() {
            for (let i = 0; i < 5; i++) {
                const fish = document.createElement('div');
                fish.className = 'fish';
                
                // Random position
                const x = Math.random() * (gameContainer.offsetWidth - 80);
                const y = Math.random() * (gameContainer.offsetHeight - 50);
                
                // Random fish image
                const imgIndex = Math.floor(Math.random() * fishImages.length);
                fish.style.backgroundImage = `url('${fishImages[imgIndex]}')`;
                
                fish.style.left = `${x}px`;
                fish.style.top = `${y}px`;
                
                // Random direction and speed
                const speed = 1 + Math.random() * 2;
                const angle = Math.random() * Math.PI * 2;
                const dx = Math.cos(angle) * speed;
                const dy = Math.sin(angle) * speed;
                
                gameContainer.appendChild(fish);
                fishItems.push({ element: fish, x, y, dx, dy });
            }
        }
        
        function createDistractors() {
            for (let i = 0; i < 3; i++) {
                const distractor = document.createElement('div');
                distractor.className = 'distractor';
                
                // Random position
                const x = Math.random() * (gameContainer.offsetWidth - 70);
                const y = Math.random() * (gameContainer.offsetHeight - 70);
                
                // Random distractor image
                const imgIndex = Math.floor(Math.random() * distractorImages.length);
                distractor.style.backgroundImage = `url('${distractorImages[imgIndex]}')`;
                
                distractor.style.left = `${x}px`;
                distractor.style.top = `${y}px`;
                
                // Random movement
                const speed = 0.3 + Math.random() * 1;
                const angle = Math.random() * Math.PI * 2;
                const dx = Math.cos(angle) * speed;
                const dy = Math.sin(angle) * speed;
                
                // Add click event (this is bad - lose a life)
                distractor.addEventListener('click', () => {
                    if (!gameActive) return;
                    
                    if (soundOn) {
                        clickSound.currentTime = 0;
                        clickSound.play().catch(e => console.log("Audio play failed:", e));
                    }
                    
                    // Show error effect
                    showMessage('Oops! That\'s a sea creature!', '#f44336');
                    if (soundOn) {
                        errorSound.currentTime = 0;
                        errorSound.play().catch(e => console.log("Audio play failed:", e));
                    }
                    
                    // Lose a life
                    lives--;
                    updateLives();
                    
                    if (lives <= 0) {
                        endGame(false);
                    }
                });
                
                gameContainer.appendChild(distractor);
                distractorItems.push({ element: distractor, x, y, dx, dy });
            }
        }
        
        function moveGameElements() {
            // Move fish
            fishItems.forEach(fish => {
                moveElement(fish);
                
                // Flip fish based on direction
                if (fish.dx > 0) {
                    fish.element.style.transform = 'scaleX(1)';
                } else {
                    fish.element.style.transform = 'scaleX(-1)';
                }
            });
            
            // Move trash
            trashItems.forEach(trash => {
                moveElement(trash);
            });
            
            // Move distractors
            distractorItems.forEach(distractor => {
                moveElement(distractor);
            });
        }
        
        function moveElement(obj) {
            // Update position
            obj.x += obj.dx;
            obj.y += obj.dy;
            
            // Bounce off walls
            const width = obj.element.offsetWidth;
            const height = obj.element.offsetHeight;
            const containerWidth = gameContainer.offsetWidth;
            const containerHeight = gameContainer.offsetHeight;
            
            if (obj.x <= 0 || obj.x >= containerWidth - width) {
                obj.dx *= -1;
            }
            if (obj.y <= 0 || obj.y >= containerHeight - height) {
                obj.dy *= -1;
            }
            
            // Apply new position
            obj.element.style.left = `${obj.x}px`;
            obj.element.style.top = `${obj.y}px`;
        }
        
        function updateLives() {
            // Update lives display
            const lifeElements = livesElement.querySelectorAll('.life');
            lifeElements.forEach((life, index) => {
                if (index < lives) {
                    life.style.display = 'block';
                } else {
                    life.style.display = 'none';
                }
            });
        }
        
        function showMessage(text, color) {
            messageElement.textContent = text;
            messageElement.style.color = color;
            messageElement.style.display = 'block';
            
            setTimeout(() => {
                messageElement.style.display = 'none';
            }, 1000);
        }
        
        function endGame(won) {
            gameActive = false;
            clearInterval(gameInterval);
            clearInterval(timerInterval);
            
            // Stop gameplay sound
            gameplaySound.pause();
            
            if (soundOn) {
                endgameSound.currentTime = 0;
                endgameSound.play().catch(e => console.log("Audio play failed:", e));
            }
            
            // Show end screen
            endScreen.style.display = 'flex';
            
            if (won) {
                endTitle.textContent = `Congratulations, ${playerName}!`;
                endMessage.textContent = `You cleaned the ocean in ${60 - timeLeft} seconds!`;
            } else {
                endTitle.textContent = `Game Over, ${playerName}`;
                endMessage.textContent = `You collected ${score} pieces of trash. Try again!`;
            }
        }

        // Start the game
        init();
    </script>
</body>
</html>
