
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Verse — Royal Arena</title>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
    <style>
        :root {
            --bg: #0a0515;
            --bg-card: rgba(120,80,200,0.05);
            --fg: #F5F0FF;
            --muted: #8B7DA8;
            --accent: #FFD700;
            --accent2: #C41E3A;
            --accent3: #D4A843;
            --accent4: #7C3AED;
            --royal-purple: #4C1D95;
            --royal-deep: #1E0A3C;
            --glow-gold: 0 0 30px rgba(255,215,0,0.25);
            --glow-crimson: 0 0 30px rgba(196,30,58,0.25);
            --glow-amber: 0 0 30px rgba(212,168,67,0.25);
            --radius: 16px;
            --font-display: 'Cinzel', serif;
            --font-body: 'Poppins', sans-serif;
        }
        *,*::before,*::after{margin:0;padding:0;box-sizing:border-box;}
        body{font-family:var(--font-body);background:var(--bg);color:var(--fg);min-height:100vh;overflow-x:hidden;position:relative;}

        #bg-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;pointer-events:none;}

        .bg-blob{position:fixed;border-radius:50%;filter:blur(130px);pointer-events:none;z-index:0;animation:blobFloat 14s ease-in-out infinite alternate;}
        .bg-blob.b1{width:550px;height:550px;background:var(--royal-purple);top:-12%;left:-8%;opacity:0.2;}
        .bg-blob.b2{width:420px;height:420px;background:var(--accent2);bottom:-12%;right:-8%;opacity:0.1;animation-delay:-5s;}
        .bg-blob.b3{width:360px;height:360px;background:var(--accent3);top:35%;left:55%;opacity:0.08;animation-delay:-9s;}
        .bg-blob.b4{width:280px;height:280px;background:var(--accent4);bottom:15%;left:15%;opacity:0.1;animation-delay:-3s;}
        @keyframes blobFloat{0%{transform:translate(0,0) scale(1);}50%{transform:translate(50px,-35px) scale(1.15);}100%{transform:translate(-25px,25px) scale(0.92);}}

        .screen{position:fixed;inset:0;z-index:10;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;opacity:0;pointer-events:none;transform:scale(0.92) translateY(20px);transition:opacity .5s cubic-bezier(.4,0,.2,1),transform .5s cubic-bezier(.4,0,.2,1);}
        .screen.active{opacity:1;pointer-events:all;transform:scale(1) translateY(0);}

        .crown-icon{font-size:2.5rem;color:var(--accent);margin-bottom:10px;filter:drop-shadow(0 0 14px rgba(255,215,0,0.45));animation:crownFloat 3s ease-in-out infinite;}
        @keyframes crownFloat{0%,100%{transform:translateY(0);}50%{transform:translateY(-8px);}}

        .royal-divider{width:200px;height:2px;background:linear-gradient(90deg,transparent,var(--accent),transparent);margin:14px auto;position:relative;}
        .royal-divider::before{content:'\f521';font-family:'Font Awesome 6 Free';font-weight:900;position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);color:var(--accent);font-size:0.8rem;background:var(--bg);padding:0 14px;}

        .welcome-logo{font-family:var(--font-display);font-size:clamp(2.2rem,6vw,4.2rem);font-weight:900;letter-spacing:6px;background:linear-gradient(135deg,var(--accent),#FFF1AA,var(--accent3),var(--accent));background-size:300% 300%;-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;animation:goldShimmer 5s ease-in-out infinite;margin-bottom:4px;text-align:center;}
        @keyframes goldShimmer{0%,100%{background-position:0% 50%;}50%{background-position:100% 50%;}}

        .welcome-sub{font-family:var(--font-display);font-size:0.82rem;color:var(--accent3);margin-bottom:6px;letter-spacing:8px;text-transform:uppercase;}
        .welcome-tagline{color:var(--muted);font-size:0.88rem;margin-bottom:36px;font-style:italic;}

        /* ===== Credits — Welcome Screen ===== */
        .credit-welcome{
            position:absolute;
            bottom:28px;
            left:50%;
            transform:translateX(-50%);
            display:flex;
            align-items:center;
            gap:8px;
            font-size:0.78rem;
            color:var(--muted);
            letter-spacing:1px;
            opacity:0.7;
        }
        .credit-welcome i{color:var(--accent3);font-size:0.7rem;}
        .credit-welcome .credit-name{color:var(--accent);font-family:var(--font-display);font-weight:700;font-size:0.82rem;letter-spacing:2px;}

        .name-input-wrap{position:relative;width:min(420px,90vw);margin-bottom:28px;}
        .name-input-wrap i{position:absolute;left:18px;top:50%;transform:translateY(-50%);color:var(--accent3);font-size:1.1rem;}
        .name-input{width:100%;padding:16px 20px 16px 48px;font-family:var(--font-body);font-size:1.05rem;background:linear-gradient(135deg,rgba(30,10,60,0.8),rgba(76,29,149,0.12));border:2px solid rgba(255,215,0,0.15);border-radius:12px;color:var(--fg);outline:none;transition:border-color .3s,box-shadow .3s;}
        .name-input::placeholder{color:var(--muted);font-style:italic;}
        .name-input:focus{border-color:var(--accent);box-shadow:var(--glow-gold);}
        .name-error{color:var(--accent2);font-size:0.85rem;margin-top:8px;min-height:20px;}

        .btn{display:inline-flex;align-items:center;gap:10px;padding:14px 36px;font-family:var(--font-display);font-size:0.82rem;font-weight:700;letter-spacing:3px;text-transform:uppercase;border:none;border-radius:12px;cursor:pointer;position:relative;overflow:hidden;transition:transform .2s,box-shadow .3s;}
        .btn:hover{transform:translateY(-2px);}
        .btn:active{transform:translateY(0) scale(0.97);}
        .btn::after{content:'';position:absolute;inset:0;background:radial-gradient(circle at var(--x,50%) var(--y,50%),rgba(255,255,255,0.2) 0%,transparent 60%);opacity:0;transition:opacity .4s;}
        .btn:active::after{opacity:1;}

        .btn-gold{background:linear-gradient(135deg,var(--accent),#CCA800);color:#1a0a2e;box-shadow:var(--glow-gold);}
        .btn-gold:hover{box-shadow:0 0 50px rgba(255,215,0,0.45);}
        .btn-crimson{background:linear-gradient(135deg,var(--accent2),#9B1830);color:#fff;box-shadow:var(--glow-crimson);}
        .btn-crimson:hover{box-shadow:0 0 50px rgba(196,30,58,0.45);}
        .btn-ghost{background:rgba(255,215,0,0.06);color:var(--accent3);border:1px solid rgba(255,215,0,0.15);}
        .btn-ghost:hover{background:rgba(255,215,0,0.12);}

        .select-header{text-align:center;margin-bottom:36px;}
        .select-header h1{font-family:var(--font-display);font-size:clamp(1.5rem,4vw,2.5rem);font-weight:900;margin-bottom:6px;}
        .select-header h1 span{color:var(--accent);}
        .select-header p{color:var(--muted);font-size:0.95rem;}

        .player-badge{display:inline-flex;align-items:center;gap:8px;background:linear-gradient(135deg,rgba(76,29,149,0.2),rgba(255,215,0,0.06));border:1px solid rgba(255,215,0,0.15);padding:7px 16px;border-radius:24px;font-size:0.82rem;}
        .player-badge .avatar{width:24px;height:24px;border-radius:50%;background:linear-gradient(135deg,var(--accent),var(--accent2));display:flex;align-items:center;justify-content:center;font-size:0.6rem;font-weight:700;color:#1a0a2e;}

        .game-cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:28px;width:min(750px,95vw);margin-bottom:28px;}
        .game-card{background:linear-gradient(160deg,rgba(30,10,60,0.6),rgba(76,29,149,0.1));border:1px solid rgba(255,215,0,0.08);border-radius:var(--radius);padding:36px 28px;text-align:center;cursor:pointer;position:relative;overflow:hidden;transition:transform .3s,border-color .3s,box-shadow .3s;}
        .game-card::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at 50% 0%,var(--card-glow,rgba(255,215,0,0.06)) 0%,transparent 70%);opacity:0;transition:opacity .4s;}
        .game-card:hover::before{opacity:1;}
        .game-card:hover{transform:translateY(-6px);border-color:var(--card-border,var(--accent));box-shadow:var(--card-shadow,var(--glow-gold));}
        .game-card .card-icon{width:80px;height:80px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:2rem;margin:0 auto 20px;position:relative;z-index:1;}
        .game-card .card-icon.gold{background:linear-gradient(135deg,rgba(255,215,0,0.15),rgba(255,215,0,0.04));color:var(--accent);border:1px solid rgba(255,215,0,0.2);}
        .game-card .card-icon.crimson{background:linear-gradient(135deg,rgba(196,30,58,0.15),rgba(196,30,58,0.04));color:var(--accent2);border:1px solid rgba(196,30,58,0.2);}
        .game-card h3{font-family:var(--font-display);font-size:1.2rem;font-weight:700;margin-bottom:10px;position:relative;z-index:1;}
        .game-card p{color:var(--muted);font-size:0.88rem;line-height:1.6;position:relative;z-index:1;}
        .game-card .card-tag{display:inline-block;margin-top:16px;padding:4px 14px;border-radius:20px;font-size:0.7rem;font-weight:600;letter-spacing:1px;text-transform:uppercase;position:relative;z-index:1;}
        .card-tag.gold{background:rgba(255,215,0,0.1);color:var(--accent);}
        .card-tag.crimson{background:rgba(196,30,58,0.1);color:var(--accent2);}
        .card-gold{--card-glow:rgba(255,215,0,0.08);--card-border:var(--accent);--card-shadow:var(--glow-gold);}
        .card-crimson{--card-glow:rgba(196,30,58,0.08);--card-border:var(--accent2);--card-shadow:var(--glow-crimson);}

        /* ===== Credits — Select Screen ===== */
        .credit-section{
            margin-top:8px;
            display:flex;
            flex-direction:column;
            align-items:center;
            gap:6px;
        }
        .credit-divider{
            width:120px;height:1px;
            background:linear-gradient(90deg,transparent,rgba(255,215,0,0.25),transparent);
        }
        .credit-main{
            display:flex;
            align-items:center;
            gap:10px;
            font-size:0.8rem;
            color:var(--muted);
        }
        .credit-main i{color:var(--accent3);font-size:0.75rem;}
        .credit-main .credit-creator{
            color:var(--accent);
            font-family:var(--font-display);
            font-weight:700;
            font-size:0.88rem;
            letter-spacing:2px;
            text-shadow:0 0 12px rgba(255,215,0,0.3);
        }
        .credit-sub{
            font-size:0.68rem;
            color:rgba(139,125,168,0.6);
            letter-spacing:1px;
            text-transform:uppercase;
        }

        .game-topbar{display:flex;align-items:center;justify-content:space-between;width:min(700px,95vw);margin-bottom:20px;flex-wrap:wrap;gap:12px;}
        .back-btn{background:rgba(255,215,0,0.06);border:1px solid rgba(255,215,0,0.12);color:var(--accent3);padding:8px 16px;border-radius:8px;cursor:pointer;font-family:var(--font-body);font-size:0.85rem;display:flex;align-items:center;gap:6px;transition:background .2s;}
        .back-btn:hover{background:rgba(255,215,0,0.12);}
        .stat-group{display:flex;gap:12px;}
        .stat-box{background:linear-gradient(160deg,rgba(30,10,60,0.5),rgba(76,29,149,0.08));border:1px solid rgba(255,215,0,0.1);border-radius:10px;padding:8px 16px;text-align:center;min-width:76px;}
        .stat-box .stat-label{font-size:0.62rem;color:var(--muted);text-transform:uppercase;letter-spacing:1px;font-weight:600;}
        .stat-box .stat-value{font-family:var(--font-display);font-size:1.25rem;font-weight:700;}
        .stat-box .stat-value.score-val{color:var(--accent);}
        .stat-box .stat-value.timer-val{color:var(--accent3);}
        .stat-box .stat-value.timer-val.danger{color:var(--accent2);animation:pulse .5s ease-in-out infinite;}
        .stat-box .stat-value.best-val{color:var(--accent4);}
        @keyframes pulse{0%,100%{opacity:1;}50%{opacity:0.4;}}

        .game-area{width:min(700px,95vw);height:min(450px,60vh);background:linear-gradient(180deg,rgba(15,5,35,0.9),rgba(30,10,60,0.6));border:1px solid rgba(255,215,0,0.08);border-radius:var(--radius);position:relative;overflow:hidden;}

        /* ===== Credits — Game Screens (small) ===== */
        .credit-game{
            position:absolute;
            bottom:10px;
            right:14px;
            font-size:0.62rem;
            color:rgba(139,125,168,0.35);
            letter-spacing:1px;
            z-index:1;
            font-family:var(--font-display);
            pointer-events:none;
        }
        .credit-game span{color:rgba(255,215,0,0.35);}

        .game-instructions{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:5;background:rgba(10,5,21,0.94);backdrop-filter:blur(10px);transition:opacity .4s;}
        .game-instructions.hidden{opacity:0;pointer-events:none;}
        .game-instructions h2{font-family:var(--font-display);font-size:1.6rem;margin-bottom:12px;}
        .game-instructions p{color:var(--muted);margin-bottom:28px;text-align:center;max-width:380px;line-height:1.7;padding:0 16px;}

        .game-over-overlay{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:10;background:rgba(10,5,21,0.96);backdrop-filter:blur(14px);opacity:0;pointer-events:none;transition:opacity .5s;}
        .game-over-overlay.active{opacity:1;pointer-events:all;}
        .game-over-overlay h2{font-family:var(--font-display);font-size:1.8rem;margin-bottom:8px;color:var(--accent);}
        .game-over-overlay .final-score{font-family:var(--font-display);font-size:3.5rem;font-weight:900;margin-bottom:4px;}
        .game-over-overlay .final-label{color:var(--muted);font-size:0.82rem;margin-bottom:8px;text-transform:uppercase;letter-spacing:2px;}
        .game-over-overlay .best-score{color:var(--accent3);font-size:0.88rem;margin-bottom:32px;}
        .game-over-overlay .over-btns{display:flex;gap:12px;flex-wrap:wrap;justify-content:center;}

        .color-target-display{position:absolute;top:16px;left:50%;transform:translateX(-50%);font-family:var(--font-display);font-size:0.9rem;padding:8px 22px;background:rgba(10,5,21,0.8);border-radius:20px;border:1px solid rgba(255,215,0,0.15);z-index:2;display:flex;align-items:center;gap:10px;}
        .color-target-swatch{width:22px;height:22px;border-radius:6px;border:2px solid rgba(255,215,0,0.3);}
        .color-target-name{font-weight:700;color:var(--accent);letter-spacing:2px;}
        .color-option{position:absolute;width:90px;height:90px;border-radius:50%;cursor:pointer;border:3px solid rgba(255,215,0,0.15);transition:transform .15s,box-shadow .15s;animation:popIn .3s cubic-bezier(.34,1.56,.64,1);}
        .color-option:hover{transform:scale(1.12);box-shadow:0 0 20px rgba(255,215,0,0.15);}
        .color-option.correct-flash{animation:correctFlash .4s ease;}
        .color-option.wrong-flash{animation:wrongFlash .4s ease;}
        @keyframes popIn{0%{transform:scale(0);opacity:0;}100%{transform:scale(1);opacity:1;}}
        @keyframes correctFlash{0%{box-shadow:0 0 0 0 rgba(255,215,0,0.8);}100%{box-shadow:0 0 0 35px rgba(255,215,0,0);}}
        @keyframes wrongFlash{0%,50%{transform:translateX(-8px);}25%,75%{transform:translateX(8px);}}

        .score-popup{position:absolute;font-family:var(--font-display);font-weight:700;font-size:1.15rem;pointer-events:none;z-index:8;animation:floatUp .8s ease forwards;}
        .score-popup.positive{color:var(--accent);}
        .score-popup.negative{color:var(--accent2);}
        @keyframes floatUp{0%{opacity:1;transform:translateY(0) scale(1);}100%{opacity:0;transform:translateY(-60px) scale(0.7);}}

        .tap-target{position:absolute;border-radius:50%;cursor:pointer;animation:tapAppear .25s cubic-bezier(.34,1.56,.64,1),tapShrink var(--shrink-time,2s) linear forwards;display:flex;align-items:center;justify-content:center;border:2px solid rgba(255,215,0,0.25);z-index:3;box-shadow:0 0 15px rgba(255,215,0,0.1);}
        .tap-target::after{content:'';position:absolute;inset:-4px;border-radius:50%;border:2px solid rgba(255,215,0,0.08);animation:tapRing var(--shrink-time,2s) linear forwards;}
        @keyframes tapAppear{0%{transform:scale(0);}100%{transform:scale(1);}}
        @keyframes tapShrink{0%{opacity:1;}80%{opacity:1;}100%{opacity:0;transform:scale(0.3);}}
        @keyframes tapRing{0%{transform:scale(1);opacity:0.5;}100%{transform:scale(2.2);opacity:0;}}
        .tap-target.hit{animation:tapHit .3s ease forwards !important;}
        @keyframes tapHit{0%{transform:scale(1);opacity:1;}50%{transform:scale(1.3);opacity:0.8;}100%{transform:scale(0);opacity:0;}}
        .combo-display{position:absolute;bottom:16px;left:50%;transform:translateX(-50%);font-family:var(--font-display);font-size:0.85rem;color:var(--accent);z-index:4;opacity:0;transition:opacity .3s;text-shadow:0 0 10px rgba(255,215,0,0.4);}
        .combo-display.active{opacity:1;}

        .toast-container{position:fixed;top:24px;right:24px;z-index:1000;display:flex;flex-direction:column;gap:10px;}
        .toast{background:linear-gradient(135deg,rgba(15,5,35,0.95),rgba(30,10,60,0.95));backdrop-filter:blur(12px);border:1px solid rgba(255,215,0,0.12);border-radius:10px;padding:12px 20px;font-size:0.88rem;color:var(--fg);display:flex;align-items:center;gap:10px;animation:slideIn .3s ease,slideOut .3s ease 2.5s forwards;min-width:240px;}
        .toast i{font-size:1.1rem;}
        .toast.success i{color:var(--accent);}
        .toast.error i{color:var(--accent2);}
        .toast.info i{color:var(--accent4);}
        @keyframes slideIn{from{transform:translateX(100%);opacity:0;}to{transform:translateX(0);opacity:1;}}
        @keyframes slideOut{from{opacity:1;}to{opacity:0;transform:translateX(40px);}}

        #confetti-canvas{position:fixed;inset:0;z-index:100;pointer-events:none;}

        @media(max-width:600px){
            .game-area{height:min(380px,55vh);}
            .color-option{width:68px;height:68px;}
            .stat-box{padding:6px 10px;min-width:60px;}
            .stat-box .stat-value{font-size:1.05rem;}
            .game-over-overlay .final-score{font-size:2.5rem;}
            .game-cards{grid-template-columns:1fr;}
            .credit-welcome{bottom:16px;font-size:0.7rem;}
        }
        @media(prefers-reduced-motion:reduce){*,*::before,*::after{animation-duration:0.01ms !important;transition-duration:0.01ms !important;}}
    </style>
</head>
<body>

    <div class="bg-blob b1"></div>
    <div class="bg-blob b2"></div>
    <div class="bg-blob b3"></div>
    <div class="bg-blob b4"></div>
    <canvas id="bg-canvas"></canvas>
    <canvas id="confetti-canvas"></canvas>
    <div class="toast-container" id="toastContainer"></div>

    <!-- WELCOME SCREEN -->
    <section class="screen active" id="welcomeScreen" aria-label="Welcome">
        <div class="crown-icon"><i class="fa-solid fa-crown"></i></div>
        <div class="welcome-logo">GAME VERSE</div>
        <div class="royal-divider"></div>
        <div class="welcome-sub">Royal Arena</div>
        <div class="welcome-tagline">"Fortune favours the bold, player."</div>
        <div class="name-input-wrap">
            <i class="fa-solid fa-chess-king"></i>
            <input type="text" class="name-input" id="nameInput" placeholder="Declare your name, noble one..." maxlength="16" autocomplete="off" aria-label="Player name">
            <div class="name-error" id="nameError"></div>
        </div>
        <button class="btn btn-gold" id="enterBtn" aria-label="Enter the arena">
            <i class="fa-solid fa-door-open"></i> Enter Arena
        </button>
        <!-- Credit on welcome -->
        <div class="credit-welcome">
            <i class="fa-solid fa-diamond"></i>
            Crafted by <span class="credit-name">MANEESH</span>
            <i class="fa-solid fa-diamond"></i>
        </div>
    </section>

    <!-- GAME SELECT SCREEN -->
    <section class="screen" id="selectScreen" aria-label="Game selection">
        <div class="select-header">
            <div class="player-badge" id="playerBadge">
                <div class="avatar" id="avatarLetter">G</div>
                <span id="displayName">Player</span>
            </div>
            <h1 style="margin-top:20px">Choose Your <span>Battle</span></h1>
            <p>Prove your worth in the royal arena</p>
        </div>
        <div class="game-cards">
            <div class="game-card card-gold" id="cardColorRush" tabindex="0" role="button" aria-label="Play Color Rush">
                <div class="card-icon gold"><i class="fa-solid fa-palette"></i></div>
                <h3>Color Rush</h3>
                <p>Match the target colour as fast as you can. Beware of tricky look-alikes!</p>
                <span class="card-tag gold">Reaction</span>
            </div>
            <div class="game-card card-crimson" id="cardTapFrenzy" tabindex="0" role="button" aria-label="Play Tap Frenzy">
                <div class="card-icon crimson"><i class="fa-solid fa-crosshairs"></i></div>
                <h3>Tap Frenzy</h3>
                <p>Smash appearing targets before they vanish. Build combos for bonus points!</p>
                <span class="card-tag crimson">Speed</span>
            </div>
        </div>
        <!-- Credits on select screen -->
        <div class="credit-section">
            <div class="credit-divider"></div>
            <div class="credit-main">
                <i class="fa-solid fa-wand-magic-sparkles"></i>
                Created by <span class="credit-creator">MANEESH</span>
                <i class="fa-solid fa-wand-magic-sparkles"></i>
            </div>
            <div class="credit-sub">Designed & Developed with passion</div>
            <button class="btn btn-ghost" id="logoutBtn" style="margin-top:12px" aria-label="Change name">
                <i class="fa-solid fa-arrow-right-from-bracket"></i> Change Name
            </button>
        </div>
    </section>

    <!-- COLOR RUSH SCREEN -->
    <section class="screen" id="colorRushScreen" aria-label="Color Rush game">
        <div class="game-topbar">
            <button class="back-btn" id="crBackBtn"><i class="fa-solid fa-chevron-left"></i> Back</button>
            <div class="stat-group">
                <div class="stat-box"><div class="stat-label">Score</div><div class="stat-value score-val" id="crScore">0</div></div>
                <div class="stat-box"><div class="stat-label">Time</div><div class="stat-value timer-val" id="crTimer">30</div></div>
                <div class="stat-box"><div class="stat-label">Best</div><div class="stat-value best-val" id="crBest">0</div></div>
            </div>
        </div>
        <div class="game-area" id="crGameArea">
            <div class="credit-game">by <span>Maneesh</span></div>
            <div class="color-target-display" id="crTargetDisplay" style="display:none">
                <div class="color-target-swatch" id="crTargetSwatch"></div>
                <span class="color-target-name" id="crTargetName">RED</span>
            </div>
            <div class="game-instructions" id="crInstructions">
                <h2 style="color:var(--accent)">Color Rush</h2>
                <div class="royal-divider" style="margin:10px auto 16px"></div>
                <p>A colour name appears at the top. Tap the circle that matches that colour. You have 30 seconds. Wrong taps cost you points!</p>
                <button class="btn btn-gold" id="crStartBtn"><i class="fa-solid fa-play"></i> Begin</button>
            </div>
            <div class="game-over-overlay" id="crGameOver">
                <h2>Time's Up!</h2>
                <div class="final-label">Final Score</div>
                <div class="final-score" id="crFinalScore" style="color:var(--accent)">0</div>
                <div class="best-score" id="crBestMsg"></div>
                <div class="over-btns">
                    <button class="btn btn-gold" id="crRetryBtn"><i class="fa-solid fa-rotate-right"></i> Retry</button>
                    <button class="btn btn-ghost" id="crMenuBtn"><i class="fa-solid fa-chess-rook"></i> Menu</button>
                </div>
            </div>
        </div>
    </section>

    <!-- TAP FRENZY SCREEN -->
    <section class="screen" id="tapFrenzyScreen" aria-label="Tap Frenzy game">
        <div class="game-topbar">
            <button class="back-btn" id="tfBackBtn"><i class="fa-solid fa-chevron-left"></i> Back</button>
            <div class="stat-group">
                <div class="stat-box"><div class="stat-label">Score</div><div class="stat-value score-val" id="tfScore">0</div></div>
                <div class="stat-box"><div class="stat-label">Time</div><div class="stat-value timer-val" id="tfTimer">30</div></div>
                <div class="stat-box"><div class="stat-label">Best</div><div class="stat-value best-val" id="tfBest">0</div></div>
            </div>
        </div>
        <div class="game-area" id="tfGameArea">
            <div class="credit-game">by <span>Maneesh</span></div>
            <div class="combo-display" id="tfCombo">COMBO x2</div>
            <div class="game-instructions" id="tfInstructions">
                <h2 style="color:var(--accent2)">Tap Frenzy</h2>
                <div class="royal-divider" style="margin:10px auto 16px"></div>
                <p>Targets pop up randomly — tap them before they shrink away! Consecutive hits build combos for bonus points. Miss one and the combo resets.</p>
                <button class="btn btn-crimson" id="tfStartBtn"><i class="fa-solid fa-play"></i> Begin</button>
            </div>
            <div class="game-over-overlay" id="tfGameOver">
                <h2>Time's Up!</h2>
                <div class="final-label">Final Score</div>
                <div class="final-score" id="tfFinalScore" style="color:var(--accent2)">0</div>
                <div class="best-score" id="tfBestMsg"></div>
                <div class="over-btns">
                    <button class="btn btn-crimson" id="tfRetryBtn"><i class="fa-solid fa-rotate-right"></i> Retry</button>
                    <button class="btn btn-ghost" id="tfMenuBtn"><i class="fa-solid fa-chess-rook"></i> Menu</button>
                </div>
            </div>
        </div>
    </section>

<script>
/* =============================================================
   GAME VERSE — Royal Arena — Created by Maneesh
   ============================================================= */

let playerName = '';
let bestScores = { colorRush: 0, tapFrenzy: 0 };

try {
    const saved = JSON.parse(localStorage.getItem('gameverse_scores'));
    if (saved) bestScores = { ...bestScores, ...saved };
} catch(e) {}

function saveScores() {
    try { localStorage.setItem('gameverse_scores', JSON.stringify(bestScores)); } catch(e) {}
}

const screens = {
    welcome: document.getElementById('welcomeScreen'),
    select: document.getElementById('selectScreen'),
    colorRush: document.getElementById('colorRushScreen'),
    tapFrenzy: document.getElementById('tapFrenzyScreen'),
};
function showScreen(name) {
    Object.values(screens).forEach(s => s.classList.remove('active'));
    screens[name].classList.add('active');
}

function showToast(message, type = 'info') {
    const c = document.getElementById('toastContainer');
    const t = document.createElement('div');
    t.className = 'toast ' + type;
    const icons = { success: 'fa-circle-check', error: 'fa-circle-xmark', info: 'fa-circle-info' };
    t.innerHTML = '<i class="fa-solid ' + (icons[type]||icons.info) + '"></i><span>' + message + '</span>';
    c.appendChild(t);
    setTimeout(() => t.remove(), 3200);
}

document.querySelectorAll('.btn').forEach(btn => {
    btn.addEventListener('pointerdown', e => {
        const r = btn.getBoundingClientRect();
        btn.style.setProperty('--x', ((e.clientX-r.left)/r.width*100)+'%');
        btn.style.setProperty('--y', ((e.clientY-r.top)/r.height*100)+'%');
    });
});

/* --- Welcome --- */
const nameInput = document.getElementById('nameInput');
const nameError = document.getElementById('nameError');

function validateAndEnter() {
    const name = nameInput.value.trim();
    if (!name) { nameError.textContent = 'Please declare your name to enter'; nameInput.style.borderColor='var(--accent2)'; nameInput.focus(); return; }
    if (name.length < 2) { nameError.textContent = 'Name must be at least 2 characters'; nameInput.style.borderColor='var(--accent2)'; return; }
    nameError.textContent = '';
    nameInput.style.borderColor = '';
    playerName = name;
    document.getElementById('displayName').textContent = name;
    document.getElementById('avatarLetter').textContent = name.charAt(0).toUpperCase();
    document.getElementById('crBest').textContent = bestScores.colorRush;
    document.getElementById('tfBest').textContent = bestScores.tapFrenzy;
    showScreen('select');
    showToast('Welcome, ' + name + '! Choose your battle.', 'success');
}

document.getElementById('enterBtn').addEventListener('click', validateAndEnter);
nameInput.addEventListener('keydown', e => { if (e.key === 'Enter') validateAndEnter(); });
nameInput.addEventListener('input', () => { nameError.textContent = ''; nameInput.style.borderColor = ''; });
setTimeout(() => nameInput.focus(), 600);

/* --- Selection --- */
document.getElementById('cardColorRush').addEventListener('click', () => showScreen('colorRush'));
document.getElementById('cardColorRush').addEventListener('keydown', e => { if (e.key==='Enter') showScreen('colorRush'); });
document.getElementById('cardTapFrenzy').addEventListener('click', () => showScreen('tapFrenzy'));
document.getElementById('cardTapFrenzy').addEventListener('keydown', e => { if (e.key==='Enter') showScreen('tapFrenzy'); });
document.getElementById('logoutBtn').addEventListener('click', () => { showScreen('welcome'); nameInput.value=''; nameInput.focus(); });
document.getElementById('crBackBtn').addEventListener('click', () => { stopColorRush(); showScreen('select'); });
document.getElementById('tfBackBtn').addEventListener('click', () => { stopTapFrenzy(); showScreen('select'); });
document.getElementById('crMenuBtn').addEventListener('click', () => showScreen('select'));
document.getElementById('tfMenuBtn').addEventListener('click', () => showScreen('select'));

function showScorePopup(parent, x, y, text, positive) {
    const el = document.createElement('div');
    el.className = 'score-popup ' + (positive ? 'positive' : 'negative');
    el.textContent = text;
    el.style.left = x + 'px';
    el.style.top = y + 'px';
    parent.appendChild(el);
    setTimeout(() => el.remove(), 800);
}

/* =============================================================
   GAME 1: COLOR RUSH
   ============================================================= */
const COLORS = [
    { name:'RED',hex:'#e63946' },{ name:'GREEN',hex:'#2d936c' },{ name:'BLUE',hex:'#457b9d' },
    { name:'YELLOW',hex:'#f4d35e' },{ name:'ORANGE',hex:'#f77f00' },{ name:'PINK',hex:'#e07aab' },
    { name:'PURPLE',hex:'#7b2d8e' },{ name:'CYAN',hex:'#00b4d8' },{ name:'LIME',hex:'#80b918' },{ name:'CORAL',hex:'#ff6b6b' },
];
const TRICKY = [
    { name:'CORAL',hex:'#ff6b6b' },{ name:'RED',hex:'#e63946' },{ name:'ORANGE',hex:'#f77f00' },
    { name:'PINK',hex:'#e07aab' },{ name:'PURPLE',hex:'#7b2d8e' },{ name:'BLUE',hex:'#457b9d' },
    { name:'CYAN',hex:'#00b4d8' },{ name:'LIME',hex:'#80b918' },{ name:'GREEN',hex:'#2d936c' },{ name:'YELLOW',hex:'#f4d35e' },
];

let crState = { score:0, timer:30, interval:null, active:false, round:0 };

function resetColorRush() {
    crState = { score:0, timer:30, interval:null, active:false, round:0 };
    document.getElementById('crScore').textContent = '0';
    document.getElementById('crTimer').textContent = '30';
    document.getElementById('crTimer').classList.remove('danger');
    document.getElementById('crTargetDisplay').style.display = 'none';
    document.querySelectorAll('#crGameArea .color-option').forEach(el => el.remove());
}

function stopColorRush() {
    clearInterval(crState.interval);
    crState.active = false;
    resetColorRush();
    document.getElementById('crInstructions').classList.remove('hidden');
    document.getElementById('crGameOver').classList.remove('active');
}

function spawnColorRound() {
    if (!crState.active) return;
    const area = document.getElementById('crGameArea');
    const areaRect = area.getBoundingClientRect();
    document.querySelectorAll('#crGameArea .color-option').forEach(el => el.remove());
    crState.round++;

    const numOptions = Math.min(4 + Math.floor(crState.round / 4), 8);
    const useTricky = crState.round > 6;
    const pool = useTricky ? TRICKY : COLORS;
    const targetIdx = Math.floor(Math.random() * pool.length);
    const target = pool[targetIdx];

    document.getElementById('crTargetDisplay').style.display = 'flex';
    document.getElementById('crTargetSwatch').style.background = target.hex;
    document.getElementById('crTargetName').textContent = target.name;

    let options = [target];
    const available = pool.filter((_,i) => i !== targetIdx);
    while (options.length < numOptions && available.length > 0) {
        const idx = Math.floor(Math.random() * available.length);
        options.push(available.splice(idx, 1)[0]);
    }
    for (let i = options.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [options[i], options[j]] = [options[j], options[i]];
    }

    const topOffset = 70;
    const usableW = areaRect.width - 110;
    const usableH = areaRect.height - topOffset - 110;
    const cols = Math.min(numOptions, 4);
    const rows = Math.ceil(numOptions / cols);
    const cellW = usableW / cols;
    const cellH = usableH / rows;

    options.forEach((color, i) => {
        const col = i % cols;
        const row = Math.floor(i / cols);
        const el = document.createElement('div');
        el.className = 'color-option';
        el.style.background = color.hex;
        el.style.left = (55 + col*cellW + cellW/2 - 45 + (Math.random()-0.5)*20) + 'px';
        el.style.top = (topOffset + 10 + row*cellH + cellH/2 - 45 + (Math.random()-0.5)*20) + 'px';
        el.dataset.isTarget = color.name === target.name ? '1' : '0';

        el.addEventListener('click', () => {
            if (!crState.active) return;
            const rect = el.getBoundingClientRect();
            const areaR = area.getBoundingClientRect();
            const px = rect.left - areaR.left + rect.width/2;
            const py = rect.top - areaR.top;

            if (el.dataset.isTarget === '1') {
                crState.score += 10;
                showScorePopup(area, px - 20, py - 10, '+10', true);
                el.classList.add('correct-flash');
                setTimeout(spawnColorRound, 200);
            } else {
                crState.score = Math.max(0, crState.score - 5);
                showScorePopup(area, px - 15, py - 10, '-5', false);
                el.classList.add('wrong-flash');
                setTimeout(() => el.classList.remove('wrong-flash'), 400);
            }
            document.getElementById('crScore').textContent = crState.score;
        });
        area.appendChild(el);
    });
}

function startColorRush() {
    resetColorRush();
    document.getElementById('crInstructions').classList.add('hidden');
    document.getElementById('crGameOver').classList.remove('active');
    crState.active = true;
    spawnColorRound();
    crState.interval = setInterval(() => {
        crState.timer--;
        const timerEl = document.getElementById('crTimer');
        timerEl.textContent = crState.timer;
        if (crState.timer <= 5) timerEl.classList.add('danger');
        if (crState.timer <= 0) endColorRush();
    }, 1000);
}

function endColorRush() {
    crState.active = false;
    clearInterval(crState.interval);
    document.getElementById('crTargetDisplay').style.display = 'none';
    document.querySelectorAll('#crGameArea .color-option').forEach(el => el.remove());
    const isNew = crState.score > bestScores.colorRush;
    if (isNew) { bestScores.colorRush = crState.score; saveScores(); document.getElementById('crBest').textContent = bestScores.colorRush; launchConfetti(); }
    document.getElementById('crFinalScore').textContent = crState.score;
    document.getElementById('crBestMsg').textContent = isNew ? 'New High Score!' : 'Best: ' + bestScores.colorRush;
    document.getElementById('crGameOver').classList.add('active');
}

document.getElementById('crStartBtn').addEventListener('click', startColorRush);
document.getElementById('crRetryBtn').addEventListener('click', startColorRush);

/* =============================================================
   GAME 2: TAP FRENZY
   ============================================================= */
const TF_COLORS = ['#FFD700','#C41E3A','#D4A843','#7C3AED','#ff6b6b','#e07aab','#80b918','#00b4d8'];

let tfState = { score:0, timer:30, interval:null, spawnTimeout:null, active:false, combo:0 };

function resetTapFrenzy() {
    tfState = { score:0, timer:30, interval:null, spawnTimeout:null, active:false, combo:0 };
    document.getElementById('tfScore').textContent = '0';
    document.getElementById('tfTimer').textContent = '30';
    document.getElementById('tfTimer').classList.remove('danger');
    document.getElementById('tfCombo').classList.remove('active');
    document.querySelectorAll('#tfGameArea .tap-target').forEach(el => el.remove());
}

function stopTapFrenzy() {
    clearInterval(tfState.interval);
    clearTimeout(tfState.spawnTimeout);
    tfState.active = false;
    resetTapFrenzy();
    document.getElementById('tfInstructions').classList.remove('hidden');
    document.getElementById('tfGameOver').classList.remove('active');
}

function spawnTapTarget() {
    if (!tfState.active) return;
    const area = document.getElementById('tfGameArea');
    const areaRect = area.getBoundingClientRect();
    const elapsed = 30 - tfState.timer;
    const size = Math.max(30, 65 - elapsed * 1.2);
    const shrinkTime = Math.max(0.8, 2.2 - elapsed * 0.05);
    const maxX = areaRect.width - size - 20;
    const maxY = areaRect.height - size - 50;
    const x = 10 + Math.random() * Math.max(1, maxX);
    const y = 10 + Math.random() * Math.max(1, maxY);
    const color = TF_COLORS[Math.floor(Math.random() * TF_COLORS.length)];

    const el = document.createElement('div');
    el.className = 'tap-target';
    el.style.width = size + 'px';
    el.style.height = size + 'px';
    el.style.left = x + 'px';
    el.style.top = y + 'px';
    el.style.background = color;
    el.style.setProperty('--shrink-time', shrinkTime + 's');

    const dot = document.createElement('div');
    dot.style.cssText = 'width:' + (size*0.3) + 'px;height:' + (size*0.3) + 'px;border-radius:50%;background:rgba(255,255,255,0.35);';
    el.appendChild(dot);

    let hit = false;
    el.addEventListener('pointerdown', e => {
        e.preventDefault();
        if (hit || !tfState.active) return;
        hit = true;
        el.classList.add('hit');
        tfState.combo++;
        let points = 10;
        let comboText = '';
        if (tfState.combo >= 3) {
            const mult = Math.min(tfState.combo - 2, 5);
            points += mult * 5;
            comboText = ' x' + (mult + 1);
        }
        tfState.score += points;
        document.getElementById('tfScore').textContent = tfState.score;
        if (tfState.combo >= 3) {
            const comboEl = document.getElementById('tfCombo');
            comboEl.textContent = 'COMBO x' + Math.min(tfState.combo - 1, 6);
            comboEl.classList.add('active');
        }
        showScorePopup(area, x + size/2 - 15, y - 5, '+' + points + comboText, true);
        setTimeout(() => el.remove(), 300);
    });

    el.addEventListener('animationend', e => {
        if (e.animationName === 'tapShrink' && !hit) {
            tfState.combo = 0;
            document.getElementById('tfCombo').classList.remove('active');
            el.remove();
        }
    });
    area.appendChild(el);
    const nextDelay = Math.max(300, 900 - elapsed * 20);
    tfState.spawnTimeout = setTimeout(spawnTapTarget, nextDelay);
}

function startTapFrenzy() {
    resetTapFrenzy();
    document.getElementById('tfInstructions').classList.add('hidden');
    document.getElementById('tfGameOver').classList.remove('active');
    tfState.active = true;
    tfState.spawnTimeout = setTimeout(spawnTapTarget, 500);
    tfState.interval = setInterval(() => {
        tfState.timer--;
        const timerEl = document.getElementById('tfTimer');
        timerEl.textContent = tfState.timer;
        if (tfState.timer <= 5) timerEl.classList.add('danger');
        if (tfState.timer <= 0) endTapFrenzy();
    }, 1000);
}

function endTapFrenzy() {
    tfState.active = false;
    clearInterval(tfState.interval);
    clearTimeout(tfState.spawnTimeout);
    document.querySelectorAll('#tfGameArea .tap-target').forEach(el => el.remove());
    document.getElementById('tfCombo').classList.remove('active');
    const isNew = tfState.score > bestScores.tapFrenzy;
    if (isNew) { bestScores.tapFrenzy = tfState.score; saveScores(); document.getElementById('tfBest').textContent = bestScores.tapFrenzy; launchConfetti(); }
    document.getElementById('tfFinalScore').textContent = tfState.score;
    document.getElementById('tfBestMsg').textContent = isNew ? 'New High Score!' : 'Best: ' + bestScores.tapFrenzy;
    document.getElementById('tfGameOver').classList.add('active');
}

document.getElementById('tfStartBtn').addEventListener('click', startTapFrenzy);
document.getElementById('tfRetryBtn').addEventListener('click', startTapFrenzy);

/* =============================================================
   CONFETTI
   ============================================================= */
const confCanvas = document.getElementById('confetti-canvas');
const confCtx = confCanvas.getContext('2d');
let confPieces = [];
let confRunning = false;

function resizeConf() { confCanvas.width = window.innerWidth; confCanvas.height = window.innerHeight; }
resizeConf();
window.addEventListener('resize', resizeConf);

function launchConfetti() {
    confPieces = [];
    const colors = ['#FFD700','#C41E3A','#D4A843','#7C3AED','#fff','#ff6b6b','#e07aab','#80b918'];
    for (let i = 0; i < 160; i++) {
        confPieces.push({
            x: Math.random() * confCanvas.width,
            y: -20 - Math.random() * 300,
            w: 4 + Math.random() * 6,
            h: 8 + Math.random() * 10,
            color: colors[Math.floor(Math.random() * colors.length)],
            vy: 2 + Math.random() * 4,
            vx: (Math.random() - 0.5) * 5,
            rot: Math.random() * 360,
            rotS: (Math.random() - 0.5) * 14,
            opacity: 1,
        });
    }
    if (!confRunning) { confRunning = true; animConf(); }
}

function animConf() {
    confCtx.clearRect(0, 0, confCanvas.width, confCanvas.height);
    let alive = false;
    confPieces.forEach(p => {
        p.x += p.vx; p.y += p.vy; p.rot += p.rotS; p.vy += 0.06;
        if (p.y > confCanvas.height + 20) p.opacity -= 0.03;
        if (p.opacity <= 0) return;
        alive = true;
        confCtx.save();
        confCtx.translate(p.x, p.y);
        confCtx.rotate(p.rot * Math.PI / 180);
        confCtx.globalAlpha = Math.max(0, p.opacity);
        confCtx.fillStyle = p.color;
        confCtx.fillRect(-p.w/2, -p.h/2, p.w, p.h);
        confCtx.restore();
    });
    if (alive) requestAnimationFrame(animConf);
    else { confRunning = false; confCtx.clearRect(0, 0, confCanvas.width, confCanvas.height); }
}

/* =============================================================
   PARTICLE BACKGROUND — Royal Edition
   ============================================================= */
const bgCanvas = document.getElementById('bg-canvas');
const bgCtx = bgCanvas.getContext('2d');
let particles = [];
let mouseX = 0, mouseY = 0;

function resizeBg() { bgCanvas.width = window.innerWidth; bgCanvas.height = window.innerHeight; }
resizeBg();
window.addEventListener('resize', resizeBg);
document.addEventListener('mousemove', e => { mouseX = e.clientX; mouseY = e.clientY; });

function initParticles() {
    particles = [];
    const count = Math.min(90, Math.floor(window.innerWidth * window.innerHeight / 14000));
    for (let i = 0; i < count; i++) {
        particles.push({
            x: Math.random() * bgCanvas.width,
            y: Math.random() * bgCanvas.height,
            size: 0.8 + Math.random() * 2,
            vx: (Math.random() - 0.5) * 0.35,
            vy: (Math.random() - 0.5) * 0.35,
            alpha: 0.15 + Math.random() * 0.35,
            color: ['#FFD700','#C41E3A','#D4A843','#7C3AED','#e07aab'][Math.floor(Math.random()*5)],
        });
    }
}
initParticles();

function animParticles() {
    bgCtx.clearRect(0, 0, bgCanvas.width, bgCanvas.height);
    particles.forEach(p => {
        const dx = mouseX - p.x;
        const dy = mouseY - p.y;
        const dist = Math.sqrt(dx*dx + dy*dy);
        if (dist < 200 && dist > 1) {
            p.vx += dx / dist * 0.012;
            p.vy += dy / dist * 0.012;
        }
        p.vx *= 0.99; p.vy *= 0.99;
        p.x += p.vx; p.y += p.vy;
        if (p.x < 0) p.x = bgCanvas.width;
        if (p.x > bgCanvas.width) p.x = 0;
        if (p.y < 0) p.y = bgCanvas.height;
        if (p.y > bgCanvas.height) p.y = 0;

        bgCtx.beginPath();
        bgCtx.arc(p.x, p.y, Math.max(0.5, p.size), 0, Math.PI * 2);
        bgCtx.fillStyle = p.color;
        bgCtx.globalAlpha = p.alpha;
        bgCtx.fill();
    });

    bgCtx.globalAlpha = 1;
    for (let i = 0; i < particles.length; i++) {
        for (let j = i + 1; j < particles.length; j++) {
            const dx = particles[i].x - particles[j].x;
            const dy = particles[i].y - particles[j].y;
            const dist = Math.sqrt(dx*dx + dy*dy);
            if (dist < 110) {
                bgCtx.beginPath();
                bgCtx.moveTo(particles[i].x, particles[i].y);
                bgCtx.lineTo(particles[j].x, particles[j].y);
                bgCtx.strokeStyle = 'rgba(124,58,237,' + (0.07 * (1 - dist/110)).toFixed(3) + ')';
                bgCtx.lineWidth = 0.5;
                bgCtx.stroke();
            }
        }
    }
    requestAnimationFrame(animParticles);
}
animParticles();
</script>
</body>
</html>
