```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="google-adsense-account" content="ca-pub-8959158038889729">
    <meta name="google-adsense-account" content="ca-pub-7586778388234174">
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Sort Puzzle - Color Match</title>
    
    <!-- === START.IO SDK CONFIGURATION === -->
    <script>
        // Initialize Start.io Configuration
        window.startapp = window.startapp || [];
        window.startapp.push({
            'appId': '206105941' // Your Start.io App ID
        });
    </script>
    <!-- Start.io Web SDK Script -->
    <script async src="https://web.startappnetwork.com/sdk/startapp.js"></script>
    <!-- ================================== -->

    <style>
        /* === RESET & VARIABLES === */
        :root {
            --bg-color: #030408; 
            --text-main: #e0e0e0;
            --text-glow: #ffffff;
            --accent: #4a90e2;
            --accent-hover: #357abd;
            
            --tube-width: 76px;
            --tube-height: 290px;
            --ball-size: 62px;
            --tube-padding: 7px;
        }

        * {
            box-sizing: border-box;
            margin: 0; padding: 0;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        body, html {
            width: 100%; height: 100%;
            background-color: var(--bg-color);
            color: var(--text-main);
            overflow: hidden;
            touch-action: none;
            overscroll-behavior: none;
        }

        /* === PREMIUM ANIMATED MESH BACKGROUND === */
        .bg-animation {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: #030408;
            z-index: -3; overflow: hidden;
        }

        .bg-glass {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            backdrop-filter: blur(80px);
            -webkit-backdrop-filter: blur(80px);
            z-index: -2;
            pointer-events: none;
        }

        .bg-orb {
            position: absolute; border-radius: 50%; filter: blur(50px); opacity: 0.6;
            animation: floatOrb 15s infinite alternate cubic-bezier(0.4, 0, 0.2, 1);
            will-change: transform; /* GPU Optimization */
        }
        
        .orb-1 { width: 400px; height: 400px; background: #7b61ff; top: -10%; left: -10%; }
        .orb-2 { width: 500px; height: 500px; background: #4a90e2; bottom: -20%; right: -10%; animation-delay: -5s; animation-duration: 18s; }
        .orb-3 { width: 350px; height: 350px; background: #ff3b30; top: 30%; left: 40%; animation-delay: -8s; opacity: 0.3; animation-duration: 20s; }
        .orb-4 { width: 300px; height: 300px; background: #00e5ff; bottom: 20%; left: -5%; animation-delay: -12s; opacity: 0.4; animation-duration: 16s; }
        .orb-5 { width: 400px; height: 400px; background: #f53db3; top: -5%; right: 10%; animation-delay: -2s; opacity: 0.3; animation-duration: 22s; }

        @keyframes floatOrb {
            0% { transform: translate(0, 0) scale(1) rotate(0deg); }
            33% { transform: translate(60px, 80px) scale(1.1) rotate(45deg); }
            66% { transform: translate(-40px, 40px) scale(0.95) rotate(-15deg); }
            100% { transform: translate(20px, -50px) scale(1.05) rotate(20deg); }
        }

        /* === UI ELEMENTS === */
        .flex-center { display: flex; justify-content: center; align-items: center; }
        .flex-col { display: flex; flex-direction: column; }

        .screen {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            display: flex; flex-direction: column;
            transition: opacity 0.4s ease, transform 0.4s ease;
            opacity: 0; pointer-events: none; transform: scale(0.95);
            will-change: opacity, transform;
        }

        .screen.active { opacity: 1; pointer-events: all; transform: scale(1); }

        h1, h2, h3 {
            color: var(--text-glow); text-shadow: 0 4px 10px rgba(0,0,0,0.5);
            text-align: center; font-weight: 900; letter-spacing: 2px;
        }

        .btn {
            background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 255, 255, 0.2);
            color: var(--text-glow); padding: 15px 30px; border-radius: 30px;
            font-size: 1.2rem; font-weight: bold; cursor: pointer;
            text-transform: uppercase; letter-spacing: 1px;
            transition: all 0.2s; box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            margin: 10px 0; display: flex; justify-content: center; align-items: center; gap: 10px;
            will-change: transform;
        }

        .btn:active { transform: scale(0.95) translateY(2px); box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3); }
        .btn-icon { width: 50px; height: 50px; border-radius: 50%; padding: 0; }
        .btn-primary { background: linear-gradient(135deg, var(--accent), #7b61ff); color: #fff; border: none; }

        .top-bar { width: 100%; padding: 20px; display: flex; justify-content: space-between; align-items: center; z-index: 10; }

        .stat-badge {
            display: flex; align-items: center; gap: 6px; padding: 8px 12px; border-radius: 20px; 
            font-weight: bold; font-size: 1.1rem; background: rgba(0, 0, 0, 0.4); border: 1px solid rgba(255,255,255,0.1);
        }

        svg.icon { width: 22px; height: 22px; fill: none; stroke: currentColor; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }
        svg.icon-fill { fill: currentColor; stroke: none; }

        /* === PREMIUM LOADING SCREEN === */
        .splash-logo {
            font-size: 4.5rem; line-height: 1.1; 
            background: linear-gradient(135deg, #ffffff, #4a90e2, #7b61ff);
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
            filter: drop-shadow(0px 8px 15px rgba(0,0,0,0.8));
            animation: pulseLogo 2s infinite ease-in-out;
            will-change: transform;
        }
        @keyframes pulseLogo {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.03); }
        }

        .progress-container { 
            width: 80%; max-width: 320px; height: 16px; 
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); 
            border-radius: 15px; margin-top: 40px; overflow: hidden; position: relative; 
            box-shadow: inset 0 2px 10px rgba(0,0,0,0.5);
        }
        
        .progress-bar { 
            height: 100%; width: 0%; 
            background: linear-gradient(90deg, #4a90e2, #7b61ff, #00e5ff); 
            background-size: 200% 100%;
            animation: shimmerProgress 2s linear infinite;
            border-radius: 15px; 
            box-shadow: 0 0 15px rgba(74, 144, 226, 0.6);
            will-change: width;
        }
        @keyframes shimmerProgress {
            0% { background-position: 100% 0; }
            100% { background-position: -100% 0; }
        }

        /* === MAIN MENU === */
        .menu-title { font-size: 4.5rem; line-height: 1.1; margin-bottom: 40px; background: -webkit-linear-gradient(#fff, #4a90e2); -webkit-background-clip: text; -webkit-text-fill-color: transparent; filter: drop-shadow(0px 8px 10px rgba(0,0,0,0.8)); }
        .menu-buttons { width: 100%; max-width: 320px; display: flex; flex-direction: column; gap: 15px; }
        .menu-buttons .btn { width: 100%; padding: 20px; font-size: 1.3rem; border-radius: 15px; }

        /* === PAGINATED LEVEL CHART === */
        .level-pagination { display: flex; align-items: center; justify-content: center; gap: 15px; margin-bottom: 15px; }
        .page-btn { padding: 8px 16px; border-radius: 10px; font-weight: bold; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2); color: #fff; cursor: pointer; }
        
        .level-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 12px; width: 90%; max-width: 450px; max-height: 55vh; overflow-y: auto; padding: 10px; }
        .level-grid::-webkit-scrollbar { width: 5px; }
        .level-grid::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.2); border-radius: 5px; }
        .level-btn { height: 60px; border-radius: 12px; background: rgba(255,255,255,0.05); border: 2px solid rgba(255,255,255,0.1); display: flex; justify-content: center; align-items: center; font-size: 1.2rem; font-weight: bold; color: #fff; cursor: pointer; transition: 0.2s; }
        .level-btn.unlocked { background: rgba(74, 144, 226, 0.2); border-color: var(--accent); }
        .level-btn.unlocked:active { transform: scale(0.9); }
        .level-btn.locked { opacity: 0.3; pointer-events: none; }
        .level-btn.current { background: linear-gradient(135deg, var(--accent), #7b61ff); box-shadow: 0 0 15px rgba(74, 144, 226, 0.6); border: none; }

        /* === GAME BOARD === */
        #game-area { flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; position: relative; padding: 10px; }
        .board { display: flex; flex-wrap: wrap; justify-content: center; align-content: center; gap: 18px; width: 100%; max-width: 800px; transition: transform 0.3s ease; transform-origin: center center; }

        .tube-container { position: relative; width: var(--tube-width); height: var(--tube-height); cursor: pointer; border-radius: 0 0 38px 38px; background: linear-gradient(to right, rgba(20, 25, 40, 0.6), rgba(10, 15, 25, 0.8)); box-shadow: inset 0 0 15px rgba(0,0,0,0.8); }
        .balls-wrapper { position: absolute; bottom: var(--tube-padding); left: var(--tube-padding); width: calc(100% - (var(--tube-padding) * 2)); display: flex; flex-direction: column-reverse; align-items: center; z-index: 2; }
        
        .tube-front { position: absolute; top: 0; left: 0; width: 100%; height: 100%; border-radius: 0 0 38px 38px; border: 2px solid rgba(190, 225, 255, 0.3); border-top: none; background: linear-gradient(90deg, rgba(255,255,255,0.0) 0%, rgba(255,255,255,0.5) 8%, rgba(255,255,255,0.0) 15%, rgba(255,255,255,0.2) 95%, rgba(255,255,255,0.0) 100%); box-shadow: inset 0 -15px 25px rgba(0,0,0,0.8); pointer-events: none; z-index: 5; }
        .tube-rim { position: absolute; top: -10px; left: -5px; width: calc(100% + 10px); height: 22px; border-radius: 50%; border: 3px solid rgba(190, 225, 255, 0.6); background: rgba(10, 15, 25, 0.9); box-shadow: 0 6px 10px rgba(0,0,0,0.6); z-index: 6; pointer-events: none; }

        /* === TEXTURED SKINS SYSTEM === */
        .ball {
            width: var(--ball-size); height: var(--ball-size); margin-bottom: 0; position: relative; z-index: 3;
            transition: transform 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            will-change: transform;
        }

        /* Skins */
        body.skin-0 .ball { border-radius: 50%; box-shadow: inset -12px -15px 20px rgba(0,0,0,0.65), inset 2px 2px 5px rgba(255,255,255,0.3), 0 4px 6px rgba(0,0,0,0.4); }
        body.skin-0 .ball::before { content: ''; position: absolute; top: 15%; left: 20%; width: 28%; height: 18%; background: rgba(255, 255, 255, 0.95); border-radius: 50%; transform: rotate(-35deg); filter: blur(0.5px); z-index: 4; }
        body.skin-1 .ball { border-radius: 12px; box-shadow: 0 0 12px currentColor, inset 0 0 15px rgba(255,255,255,0.8), inset -5px -5px 10px rgba(0,0,0,0.5); border: 2px solid #fff; }
        body.skin-2 .ball { border-radius: 4px; clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%); background-image: repeating-linear-gradient(45deg, rgba(255,255,255,0.2) 0px, rgba(255,255,255,0.2) 4px, transparent 4px, transparent 8px); box-shadow: inset -8px -8px 15px rgba(0,0,0,0.7), inset 5px 5px 10px rgba(255,255,255,0.5); }
        body.skin-3 .ball { border-radius: 50%; background-image: radial-gradient(circle at 30% 30%, rgba(255, 223, 0, 0.8), rgba(0, 0, 0, 0.8)); border: 2px solid #ffe600; box-shadow: inset -10px -10px 15px rgba(0,0,0,0.9), inset 3px 3px 8px rgba(255,255,255,0.9); }
        body.skin-4 .ball { border-radius: 50%; background-image: radial-gradient(circle at 20% 20%, rgba(255,255,255,0.8), transparent), repeating-radial-gradient(circle at 50% 50%, rgba(255,255,255,0.15) 0, rgba(255,255,255,0.15) 3px, transparent 3px, transparent 6px); box-shadow: inset -12px -12px 20px rgba(0,0,0,0.8), 0 0 10px rgba(255,255,255,0.3); }

        /* EXTENDED PALETTE */
        .color-0 { background-color: #e81e1e; color: #e81e1e; }
        .color-1 { background-color: #9b22e0; color: #9b22e0; }
        .color-2 { background-color: #52e01a; color: #52e01a; }
        .color-3 { background-color: #f53db3; color: #f53db3; }
        .color-4 { background-color: #ff8c00; color: #ff8c00; }
        .color-5 { background-color: #0073ff; color: #0073ff; }
        .color-6 { background-color: #ffd500; color: #ffd500; }
        .color-7 { background-color: #00e5ff; color: #00e5ff; }
        .color-8 { background-color: #8d6e63; color: #8d6e63; }
        .color-9 { background-color: #607d8b; color: #607d8b; }
        .color-10 { background-color: #e91e63; color: #e91e63; }
        .color-11 { background-color: #009688; color: #009688; }

        .tube-container.selected .balls-wrapper .ball:last-child { transform: translateY(-55px); z-index: 10; }

        @keyframes shake { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-6px); } 50% { transform: translateX(6px); } 75% { transform: translateX(-6px); } }
        .shake-anim { animation: shake 0.3s ease-in-out; }

        /* Highly Optimized 90FPS Animator */
        #animator { position: absolute; z-index: 100; pointer-events: none; opacity: 0; will-change: transform, opacity; }

        .controls { padding: 20px; display: flex; justify-content: center; gap: 20px; width: 100%; z-index: 10; }

        /* MODALS */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); display: flex; justify-content: center; align-items: center; z-index: 1000; opacity: 0; pointer-events: none; transition: opacity 0.3s; will-change: opacity; }
        .modal-overlay.active { opacity: 1; pointer-events: all; }
        .modal-content { width: 90%; max-width: 400px; padding: 30px; border-radius: 25px; text-align: center; transform: translateY(50px); transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); background: linear-gradient(135deg, rgba(30, 35, 55, 0.95), rgba(15, 18, 25, 0.95)); border: 1px solid rgba(255, 255, 255, 0.1); box-shadow: 0 20px 50px rgba(0,0,0,0.8); will-change: transform; }
        .modal-overlay.active .modal-content { transform: translateY(0); }
        .modal-title { font-size: 2.2rem; margin-bottom: 10px; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
        .reward-box { font-size: 2.2rem; color: #ffd700; text-shadow: 0 2px 10px rgba(255, 215, 0, 0.5); margin: 20px 0; display: flex; justify-content: center; align-items: center; gap: 10px; font-weight: 900; }

        .hint-text { position: absolute; top: 15%; left: 50%; transform: translateX(-50%); width: max-content; text-align: center; font-size: 1.2rem; font-weight: bold; color: #fff; background: rgba(255, 50, 50, 0.95); padding: 10px 25px; border-radius: 30px; box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5); opacity: 0; pointer-events: none; transition: opacity 0.3s; z-index: 100; will-change: opacity; }

        /* SKINS SELECTION */
        .skin-options { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0; }
        .skin-card { display: flex; flex-direction: column; align-items: center; gap: 8px; cursor: pointer; }
        .skin-preview { width: 55px; height: 55px; background-color: #0073ff; color: #0073ff; border: 3px solid transparent; transition: 0.2s; position: relative;}
        .skin-card.active .skin-preview { border-color: #ffd700; transform: scale(1.1); box-shadow: 0 0 15px #ffd700; }
        
        .prev-sphere { border-radius: 50%; box-shadow: inset -10px -10px 15px rgba(0,0,0,0.6), inset 2px 2px 5px rgba(255,255,255,0.3); }
        .prev-neon { border-radius: 12px; box-shadow: 0 0 10px #0073ff, inset 0 0 10px #fff; border: 2px solid #fff !important; }
        .prev-diamond { border-radius: 4px; clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%); background-image: repeating-linear-gradient(45deg, rgba(255,255,255,0.3) 0px, rgba(255,255,255,0.3) 4px, transparent 4px, transparent 8px); }
        .prev-gold { border-radius: 50%; background-image: radial-gradient(circle at 30% 30%, #ffe600, #000); border: 2px solid #ffe600 !important; }
        .prev-galaxy { border-radius: 50%; background-image: radial-gradient(circle at 20% 20%, rgba(255,255,255,0.8), transparent), repeating-radial-gradient(circle at 50% 50%, rgba(255,255,255,0.2) 0, rgba(255,255,255,0.2) 3px, transparent 3px, transparent 6px); }

        /* AD MODAL STYLES */
        .ad-box { width: 100%; height: 160px; background: rgba(0,0,0,0.5); border: 2px dashed rgba(255,255,255,0.3); border-radius: 15px; display: flex; flex-direction: column; justify-content: center; align-items: center; margin: 15px 0; }
    </style>
</head>
<body class="skin-0">
    <!-- Premium Animated Background -->
    <div class="bg-animation">
        <div class="bg-orb orb-1"></div>
        <div class="bg-orb orb-2"></div>
        <div class="bg-orb orb-3"></div>
        <div class="bg-orb orb-4"></div>
        <div class="bg-orb orb-5"></div>
    </div>
    <div class="bg-glass"></div>

    <div id="animator"></div>

    <!-- Splash / Loading Screen -->
    <div id="screen-splash" class="screen flex-center flex-col active">
        <h1 class="splash-logo">SORT<br>PUZZLE</h1>
        <div class="progress-container">
            <div class="progress-bar" id="loading-bar"></div>
        </div>
        <p id="loading-text" style="margin-top: 15px; letter-spacing: 3px; font-weight: bold; color: #4a90e2;">INITIALIZING...</p>
    </div>

    <!-- Main Menu Screen -->
    <div id="screen-menu" class="screen flex-center flex-col">
        <div class="top-bar">
            <div class="stat-badge"><span style="color:#ffd700;">★</span> <span id="menu-coins">0</span></div>
        </div>
        <div style="flex:1; display:flex; flex-direction:column; justify-content:center; align-items:center; width: 100%;">
            <h1 class="menu-title">SORT<br>PUZZLE</h1>
            
            <div class="menu-buttons">
                <button class="btn btn-primary" onclick="Game.startNewLevel()">
                    <svg class="icon icon-fill" viewBox="0 0 24 24"><polygon points="5 3 19 12 5 21 5 3"/></svg> PLAY
                </button>
                <button class="btn" onclick="UI.showLevels()">
                    <svg class="icon" viewBox="0 0 24 24"><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg> LEVELS
                </button>
                <button class="btn" onclick="UI.showModal('modal-skins')">
                    <svg class="icon" viewBox="0 0 24 24"><path d="M12 2.69l5.66 4.2c.2.15.34.36.34.6v8.02c0 .24-.14.45-.34.6L12 20.31l-5.66-4.2a1 1 0 0 1-.34-.6V7.49c0-.24.14-.45.34-.6L12 2.69z"></path></svg> SKINS
                </button>
            </div>
        </div>
    </div>

    <!-- Levels Chart Screen -->
    <div id="screen-levels" class="screen flex-col">
        <div class="top-bar">
            <button class="btn btn-icon" style="background: rgba(0,0,0,0.4);" onclick="UI.showScreen('screen-menu')">
                <svg class="icon" viewBox="0 0 24 24"><line x1="19" y1="12" x2="5" y2="12"></line><polyline points="12 19 5 12 12 5"></polyline></svg>
            </button>
            <h3>SELECT LEVEL</h3>
            <div style="width: 50px;"></div>
        </div>
        <div class="level-pagination">
            <button class="page-btn" onclick="UI.changeLevelPage(-1)">◄ PREV</button>
            <span id="page-indicator" style="font-weight: bold; font-size: 1.1rem;">Page 1</span>
            <button class="page-btn" onclick="UI.changeLevelPage(1)">NEXT ►</button>
        </div>
        <div style="flex:1; display:flex; justify-content:center; align-items:flex-start;">
            <div class="level-grid" id="level-grid-container">
                <!-- Populated dynamically -->
            </div>
        </div>
    </div>

    <!-- Game Screen -->
    <div id="screen-game" class="screen">
        <div class="top-bar">
            <button class="btn btn-icon" style="background: rgba(0,0,0,0.4);" onclick="UI.showScreen('screen-menu')">
                <svg class="icon" viewBox="0 0 24 24"><line x1="19" y1="12" x2="5" y2="12"></line><polyline points="12 19 5 12 12 5"></polyline></svg>
            </button>
            <div style="display:flex; gap:10px;">
                <div class="stat-badge"><span style="color: #4a90e2;">LVL</span> <span id="game-level">1</span></div>
                <div class="stat-badge" style="color: #ff8c00;">
                    <svg class="icon" viewBox="0 0 24 24" style="margin-right: 5px;"><polyline points="13 17 18 12 13 7"></polyline><line x1="6" y1="12" x2="18" y2="12"></line></svg>
                    <span id="game-moves">0</span>
                </div>
            </div>
            <div class="stat-badge"><span style="color:#ffd700;">★</span> <span id="game-coins">0</span></div>
        </div>

        <div id="hint-msg" class="hint-text">Tube is full!</div>

        <div id="game-area">
            <div class="board" id="board">
                <!-- Tubes generated via JS -->
            </div>
        </div>

        <div class="controls">
            <button class="btn btn-icon" style="background: rgba(0,0,0,0.4);" onclick="Game.undo()" id="btn-undo">
                <svg class="icon" viewBox="0 0 24 24"><path d="M3 7v6h6"></path><path d="M21 17a9 9 0 00-9-9 9 9 0 00-6 2.3L3 13"></path></svg>
            </button>
            <button class="btn btn-icon" style="background: rgba(0,0,0,0.4);" onclick="Game.restart()">
                <svg class="icon" viewBox="0 0 24 24"><polyline points="23 4 23 10 17 10"></polyline><polyline points="1 20 1 14 7 14"></polyline><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"></path></svg>
            </button>
        </div>
    </div>

    <!-- Textured Skins Modal -->
    <div id="modal-skins" class="modal-overlay">
        <div class="modal-content">
            <h2 class="modal-title">CHOOSE TEXTURE</h2>
            <div class="skin-options">
                <div class="skin-card active" onclick="Game.setSkin(0)" id="skin-card-0">
                    <div class="skin-preview prev-sphere"></div>
                    <span style="font-size: 0.8rem; font-weight: bold;">Classic</span>
                </div>
                <div class="skin-card" onclick="Game.setSkin(1)" id="skin-card-1">
                    <div class="skin-preview prev-neon"></div>
                    <span style="font-size: 0.8rem; font-weight: bold;">Neon</span>
                </div>
                <div class="skin-card" onclick="Game.setSkin(2)" id="skin-card-2">
                    <div class="skin-preview prev-diamond"></div>
                    <span style="font-size: 0.8rem; font-weight: bold;">Diamond</span>
                </div>
                <div class="skin-card" onclick="Game.setSkin(3)" id="skin-card-3">
                    <div class="skin-preview prev-gold"></div>
                    <span style="font-size: 0.8rem; font-weight: bold;">Gold</span>
                </div>
                <div class="skin-card" onclick="Game.setSkin(4)" id="skin-card-4">
                    <div class="skin-preview prev-galaxy"></div>
                    <span style="font-size: 0.8rem; font-weight: bold;">Nebula</span>
                </div>
            </div>
            <button class="btn btn-primary" style="width: 100%; margin-top:10px;" onclick="UI.hideModal('modal-skins')">APPLY</button>
        </div>
    </div>

    <!-- Win Modal -->
    <div id="modal-win" class="modal-overlay">
        <div class="modal-content">
            <h2 class="modal-title" style="color: #4cd964;">LEVEL CLEARED!</h2>
            <div class="reward-box"><span>+50</span> <span style="color:#ffd700;">★</span></div>
            <button class="btn btn-primary" style="width: 100%;" onclick="Game.nextLevel()">NEXT LEVEL</button>
        </div>
    </div>

    <!-- Fallback Simulated Rewarded Ad Modal -->
    <div id="modal-ad" class="modal-overlay">
        <div class="modal-content">
            <h2 class="modal-title" style="color: #ffd700;">REWARDED AD</h2>
            <p style="font-size: 0.9rem; opacity: 0.8;">Watch full ad to claim +100 bonus coins!</p>
            <div class="ad-box">
                <svg class="icon" viewBox="0 0 24 24" style="width: 48px; height: 48px; stroke: #ffd700;"><polygon points="5 3 19 12 5 21 5 3"/></svg>
                <p id="ad-timer" style="margin-top: 10px; font-weight: bold;">Ad Playing: 3s</p>
            </div>
            <button id="btn-claim-ad" class="btn btn-primary" style="width: 100%; opacity: 0.5; pointer-events: none;" onclick="AdSystem.claimReward()">WAIT...</button>
        </div>
    </div>

    <!-- Game Over Modal -->
    <div id="modal-gameover" class="modal-overlay">
        <div class="modal-content" style="border-color: rgba(255,50,50,0.4);">
            <h2 class="modal-title" style="color: #ff3b30;">OUT OF MOVES!</h2>
            <p style="margin-bottom: 25px; font-size: 1.2rem; color: rgba(255,255,255,0.8);">You've used all your moves.</p>
            <button class="btn" style="background: #ff3b30; color:#fff; border:none; width: 100%;" onclick="Game.restart(); UI.hideModal('modal-gameover');">TRY AGAIN</button>
            <button class="btn" style="width: 100%;" onclick="UI.hideModal('modal-gameover'); UI.showScreen('screen-menu');">MAIN MENU</button>
        </div>
    </div>

    <script>
        const AudioSys = {
            ctx: null,
            init() {
                if (!this.ctx) {
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    if(AudioContext) this.ctx = new AudioContext();
                }
            },
            playTone(freq, type, duration, vol=0.1) {
                if (!this.ctx) return;
                if (this.ctx.state === 'suspended') this.ctx.resume();
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = type;
                osc.frequency.setValueAtTime(freq, this.ctx.currentTime);
                gain.gain.setValueAtTime(vol, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + duration);
                osc.connect(gain); gain.connect(this.ctx.destination);
                osc.start(); osc.stop(this.ctx.currentTime + duration);
            },
            pop() { this.playTone(600, 'sine', 0.1, 0.2); },
            drop() { this.playTone(300, 'triangle', 0.15, 0.3); },
            error() { this.playTone(150, 'sawtooth', 0.2, 0.15); },
            win() { 
                if(!this.ctx) return;
                [400, 500, 600, 800].forEach((f, i) => { setTimeout(() => this.playTone(f, 'sine', 0.2, 0.1), i * 100); });
            }
        };

        const UI = {
            screens: ['screen-splash', 'screen-menu', 'screen-game', 'screen-levels'],
            currentLevelPage: 1,
            levelsPerPage: 25,

            showScreen(id) {
                this.screens.forEach(s => document.getElementById(s).classList.remove('active'));
                document.getElementById(id).classList.add('active');
            },
            showModal(id) { document.getElementById(id).classList.add('active'); },
            hideModal(id) { document.getElementById(id).classList.remove('active'); },
            updateStats() {
                document.getElementById('menu-coins').innerText = Game.stats.coins;
                document.getElementById('game-coins').innerText = Game.stats.coins;
                document.getElementById('game-level').innerText = Game.stats.level;
                
                const movesEl = document.getElementById('game-moves');
                movesEl.innerText = Game.state.movesLeft;
                movesEl.style.color = Game.state.movesLeft <= 3 ? '#ff3b30' : 'inherit';
            },
            showMessage(text) {
                const msg = document.getElementById('hint-msg');
                msg.innerText = text; msg.style.opacity = 1;
                setTimeout(() => msg.style.opacity = 0, 1500);
            },
            showLevels() {
                this.currentLevelPage = Math.ceil(Game.stats.maxLevel / this.levelsPerPage);
                this.renderLevelPage();
                this.showScreen('screen-levels');
            },
            changeLevelPage(dir) {
                if (this.currentLevelPage + dir >= 1) {
                    this.currentLevelPage += dir;
                    this.renderLevelPage();
                }
            },
            renderLevelPage() {
                const container = document.getElementById('level-grid-container');
                container.innerHTML = '';
                
                const startLvl = (this.currentLevelPage - 1) * this.levelsPerPage + 1;
                const endLvl = startLvl + this.levelsPerPage - 1;
                
                document.getElementById('page-indicator').innerText = `Lvl ${startLvl} - ${endLvl}`;

                for (let i = startLvl; i <= endLvl; i++) {
                    const btn = document.createElement('div');
                    btn.className = `level-btn ${i < Game.stats.maxLevel ? 'unlocked' : (i === Game.stats.maxLevel ? 'current' : 'locked')}`;
                    btn.innerText = i;
                    if (i <= Game.stats.maxLevel) {
                        btn.onclick = () => { Game.loadLevel(i); this.showScreen('screen-game'); };
                    }
                    container.appendChild(btn);
                }
            }
        };

        // START.IO AD SYSTEM INTEGRATION
        const AdSystem = {
            onComplete: null,
            
            triggerAd(callback) {
                this.onComplete = callback;
                
                // Attempt to call Start.io Native Web SDK if it's successfully loaded
                if (typeof startapp !== 'undefined' && typeof startapp.showRewardedVideo === 'function') {
                    console.log("Start.io SDK found. Requesting Rewarded Video...");
                    
                    try {
                        startapp.showRewardedVideo({
                            rewardCompleted: () => {
                                console.log("Start.io Ad: Reward Completed!");
                                this.claimReward();
                            },
                            adClosed: () => {
                                console.log("Start.io Ad: Closed.");
                                // Proceed to next screen even if closed early to prevent getting stuck
                                if (this.onComplete) {
                                    this.onComplete();
                                    this.onComplete = null;
                                }
                            },
                            adError: (error) => {
                                console.error("Start.io Ad Error: ", error);
                                this.showFallbackAd(); // If ad fails to load, show our internal fake ad
                            }
                        });
                    } catch (e) {
                        console.error("Start.io integration error: ", e);
                        this.showFallbackAd();
                    }
                } else {
                    // Fallback to simulated ad if Start.io SDK is blocked by Adblock or failed to load
                    console.warn("Start.io SDK not detected. Falling back to simulated ad to ensure game flow.");
                    this.showFallbackAd();
                }
            },
            
            showFallbackAd() {
                UI.showModal('modal-ad');
                let timer = 3;
                const timerEl = document.getElementById('ad-timer');
                const btnEl = document.getElementById('btn-claim-ad');
                
                timerEl.innerText = `Ad Playing: ${timer}s`;
                btnEl.innerText = "WAIT...";
                btnEl.style.opacity = "0.5";
                btnEl.style.pointerEvents = "none";

                const interval = setInterval(() => {
                    timer--;
                    if (timer > 0) {
                        timerEl.innerText = `Ad Playing: ${timer}s`;
                    } else {
                        clearInterval(interval);
                        timerEl.innerText = "Ad Completed!";
                        btnEl.innerText = "CLAIM +100 ★ & CONTINUE";
                        btnEl.style.opacity = "1";
                        btnEl.style.pointerEvents = "auto";
                    }
                }, 1000);
            },
            
            claimReward() {
                Game.stats.coins += 100; // Reward for watching the ad
                Game.saveStats();
                UI.hideModal('modal-ad');
                if (this.onComplete) {
                    this.onComplete();
                    this.onComplete = null; // Reset callback to prevent double firing
                }
            }
        };

        const Game = {
            stats: { level: 1, maxLevel: 1, coins: 0, skin: 0 },
            state: { tubes: [], history: [], selected: -1, animating: false, movesLeft: 0, maxMoves: 0 },
            config: { maxBalls: 4, emptyTubes: 2 },
            boardEl: document.getElementById('board'),
            animatorEl: document.getElementById('animator'),
            currentScale: 1,

            init() {
                const saved = localStorage.getItem('sortpuzzle3d_stats_v6'); 
                if(saved) this.stats = JSON.parse(saved);
                
                this.setSkin(this.stats.skin || 0);
                UI.updateStats();
                this.simulateLoading();
            },

            simulateLoading() {
                let progress = 0;
                const bar = document.getElementById('loading-bar');
                const text = document.getElementById('loading-text');
                let lastTime = performance.now();

                const update = (time) => {
                    let dt = time - lastTime;
                    progress += (dt / 25); 
                    
                    if (progress >= 100) progress = 100;
                    bar.style.width = `${progress}%`;
                    text.innerText = `LOADING ${Math.floor(progress)}%`;

                    if (progress < 100) {
                        lastTime = time;
                        requestAnimationFrame(update);
                    } else {
                        setTimeout(() => UI.showScreen('screen-menu'), 400);
                    }
                };
                requestAnimationFrame(update);
            },

            saveStats() {
                localStorage.setItem('sortpuzzle3d_stats_v6', JSON.stringify(this.stats));
                UI.updateStats();
            },

            setSkin(skinIdx) {
                this.stats.skin = skinIdx;
                document.body.className = `skin-${skinIdx}`;
                [0,1,2,3,4].forEach(i => {
                    const card = document.getElementById(`skin-card-${i}`);
                    if(card) card.classList.remove('active');
                });
                const activeCard = document.getElementById(`skin-card-${skinIdx}`);
                if(activeCard) activeCard.classList.add('active');
                this.saveStats();
            },

            generateLevel(level) {
                let numColors = Math.min(12, 3 + Math.floor((level % 50) / 4));
                let totalTubes = numColors + this.config.emptyTubes;
                
                let tubes = [];
                for (let i = 0; i < numColors; i++) tubes.push([i, i, i, i]);
                for (let i = 0; i < this.config.emptyTubes; i++) tubes.push([]);

                let shuffles = numColors * 25 + (level % 10);
                let moves = 0;
                while(moves < shuffles) {
                    let from = Math.floor(Math.random() * totalTubes);
                    let to = Math.floor(Math.random() * totalTubes);
                    if (from !== to && tubes[from].length > 0 && tubes[to].length < this.config.maxBalls) {
                        tubes[to].push(tubes[from].pop());
                        moves++;
                    }
                }
                for (let tube of tubes) {
                    if (tube.length === this.config.maxBalls && tube.every(c => c === tube[0])) return this.generateLevel(level);
                }
                
                let baseMoves = Math.max(12, numColors * 3 + Math.floor((level % 20) / 2));
                this.state.maxMoves = Math.floor(baseMoves * 1.32); // 20% Extra Moves
                
                return tubes;
            },

            startNewLevel() {
                this.stats.level = this.stats.maxLevel;
                this.loadLevel(this.stats.level);
                UI.showScreen('screen-game');
            },

            loadLevel(level) {
                AudioSys.init();
                this.stats.level = level;
                
                this.state.tubes = this.generateLevel(level);
                this.state.movesLeft = this.state.maxMoves;

                this.state.history = [];
                this.state.selected = -1;
                this.state.animating = false;
                
                UI.updateStats();
                this.renderBoard();
                this.updateButtons();
            },

            restart() {
                if(this.state.history.length === 0 || this.state.animating) return;
                let initialState = JSON.parse(this.state.history[0]);
                this.state.tubes = initialState.tubes;
                this.state.movesLeft = this.state.maxMoves;
                this.state.history = []; this.state.selected = -1;
                this.renderBoard(); this.updateButtons();
                UI.updateStats();
            },

            undo() {
                if(this.state.history.length === 0 || this.state.animating) return;
                let prevState = JSON.parse(this.state.history.pop());
                this.state.tubes = prevState.tubes;
                this.state.movesLeft = prevState.moves;
                this.state.selected = -1;
                this.renderBoard(); this.updateButtons();
                UI.updateStats();
            },

            renderBoard() {
                this.boardEl.innerHTML = '';
                const tubesCount = this.state.tubes.length;
                
                let scale = 1;
                if(window.innerWidth < 600) {
                    if (tubesCount > 10) scale = 0.5;
                    else if (tubesCount > 6) scale = 0.65;
                    else scale = 0.8;
                } else {
                    if (tubesCount > 10) scale = 0.7;
                }
                this.currentScale = scale;
                this.boardEl.style.transform = `scale(${scale})`;

                this.state.tubes.forEach((tube, i) => {
                    const tubeContainer = document.createElement('div');
                    tubeContainer.className = 'tube-container';
                    tubeContainer.id = `tube-${i}`;
                    tubeContainer.onclick = () => this.handleTubeClick(i);

                    const rim = document.createElement('div'); rim.className = 'tube-rim';
                    const front = document.createElement('div'); front.className = 'tube-front';
                    
                    const ballsWrapper = document.createElement('div');
                    ballsWrapper.className = 'balls-wrapper';

                    tube.forEach((colorIdx, ballIdx) => {
                        const ball = document.createElement('div');
                        ball.className = `ball color-${colorIdx}`;
                        ball.id = `ball-${i}-${ballIdx}`;
                        ballsWrapper.appendChild(ball);
                    });

                    tubeContainer.appendChild(ballsWrapper);
                    tubeContainer.appendChild(front);
                    tubeContainer.appendChild(rim);

                    if (this.state.selected === i) tubeContainer.classList.add('selected');

                    this.boardEl.appendChild(tubeContainer);
                });
            },

            updateButtons() {
                document.getElementById('btn-undo').style.opacity = this.state.history.length > 0 ? '1' : '0.5';
                document.getElementById('btn-undo').style.pointerEvents = this.state.history.length > 0 ? 'auto' : 'none';
            },

            handleTubeClick(idx) {
                if(this.state.animating || this.state.movesLeft <= 0) return;
                const tube = this.state.tubes[idx];

                if (this.state.selected === idx) {
                    this.state.selected = -1; AudioSys.drop();
                    this.renderBoard(); return;
                }

                if (this.state.selected === -1) {
                    if (tube.length > 0) { 
                        this.state.selected = idx; AudioSys.pop();
                        this.renderBoard(); 
                    }
                    return;
                }

                const fromIdx = this.state.selected;
                const toTube = this.state.tubes[idx];
                const fromTube = this.state.tubes[fromIdx];
                
                const movingColor = fromTube[fromTube.length - 1];
                const targetTopColor = toTube.length > 0 ? toTube[toTube.length - 1] : null;

                if (toTube.length < this.config.maxBalls && (toTube.length === 0 || movingColor === targetTopColor)) {
                    this.state.animating = true;
                    
                    if(this.state.history.length === 0) {
                        this.state.history.push(JSON.stringify({tubes: this.state.tubes, moves: this.state.movesLeft}));
                    }
                    this.state.history.push(JSON.stringify({tubes: this.state.tubes, moves: this.state.movesLeft}));
                    
                    this.state.movesLeft--;
                    UI.updateStats(); this.updateButtons();

                    this.animateMove(fromIdx, idx, () => {
                        this.state.selected = -1; this.state.animating = false;
                        this.renderBoard(); this.checkWinOrLoss();
                    });
                } else {
                    AudioSys.error(); this.state.selected = -1;
                    this.renderBoard();
                    const toContainer = document.getElementById(`tube-${idx}`);
                    toContainer.classList.add('shake-anim');
                    setTimeout(() => toContainer.classList.remove('shake-anim'), 300);
                    
                    UI.showMessage(toTube.length >= this.config.maxBalls ? "Tube is full!" : "Colors must match!");
                }
            },

            // Advanced 90FPS/120FPS GPU Accelerated Animation Loop
            animateMove(fromIdx, toIdx, callback) {
                let color = this.state.tubes[fromIdx].pop();
                const fromContainer = document.getElementById(`tube-${fromIdx}`);
                const toContainer = document.getElementById(`tube-${toIdx}`);
                const fromWrapper = fromContainer.querySelector('.balls-wrapper');
                
                const ballEl = fromWrapper.lastElementChild;
                const rectFrom = ballEl.getBoundingClientRect();
                const rectToContainer = toContainer.getBoundingClientRect();
                
                const ballsInTarget = this.state.tubes[toIdx].length; 
                this.animatorEl.className = ballEl.className;
                
                const targetX = rectToContainer.left + (rectToContainer.width / 2) - (rectFrom.width / 2);
                const scaledBallSize = 62 * this.currentScale; 
                const scaledPaddingBottom = 7 * this.currentScale; 
                const targetY = rectToContainer.bottom - scaledPaddingBottom - (ballsInTarget * scaledBallSize) - rectFrom.height;

                this.animatorEl.style.width = rectFrom.width + 'px';
                this.animatorEl.style.height = rectFrom.height + 'px';
                this.animatorEl.style.left = '0px';
                this.animatorEl.style.top = '0px';
                this.animatorEl.style.opacity = 1;
                this.animatorEl.style.transition = 'none';
                this.animatorEl.style.transform = `translate(${rectFrom.left}px, ${rectFrom.top}px)`;

                ballEl.style.opacity = 0;
                void this.animatorEl.offsetWidth; 
                
                const upOffset = 90 * this.currentScale;

                this.animatorEl.style.transition = 'transform 0.12s cubic-bezier(0.2, 0, 0.8, 0.2)';
                this.animatorEl.style.transform = `translate(${rectFrom.left}px, ${rectFrom.top - upOffset}px)`;
                
                setTimeout(() => {
                    this.animatorEl.style.transition = 'transform 0.15s linear';
                    this.animatorEl.style.transform = `translate(${targetX}px, ${rectFrom.top - upOffset}px)`;
                    
                    setTimeout(() => {
                        this.animatorEl.style.transition = 'transform 0.15s cubic-bezier(0.2, 0.8, 0.5, 1)';
                        this.animatorEl.style.transform = `translate(${targetX}px, ${targetY}px)`;
                        
                        setTimeout(() => {
                            AudioSys.drop();
                            this.animatorEl.style.opacity = 0; 
                            ballEl.remove();
                            this.state.tubes[toIdx].push(color); 
                            callback();
                        }, 160);
                    }, 150);
                }, 120);
            },

            checkWinOrLoss() {
                let won = true;
                for (let tube of this.state.tubes) {
                    if (tube.length > 0) {
                        if (tube.length !== this.config.maxBalls) won = false;
                        let first = tube[0];
                        for (let color of tube) if (color !== first) won = false;
                    }
                }

                if (won) {
                    setTimeout(() => {
                        AudioSys.win();
                        this.stats.coins += 50; // Standard level complete coins
                        if (this.stats.level === this.stats.maxLevel) this.stats.maxLevel++;
                        this.saveStats();
                        
                        // TRIGGERS AD EVERY 5 LEVELS
                        if (this.stats.level % 5 === 0) {
                            AdSystem.triggerAd(() => UI.showModal('modal-win'));
                        } else {
                            UI.showModal('modal-win');
                        }
                    }, 400);
                } else if (this.state.movesLeft <= 0) {
                    setTimeout(() => { AudioSys.error(); UI.showModal('modal-gameover'); }, 400);
                }
            },

            nextLevel() {
                UI.hideModal('modal-win');
                this.loadLevel(this.stats.level + 1);
            }
        };

        window.onload = () => Game.init();
    </script>
</body>
</html>


```
