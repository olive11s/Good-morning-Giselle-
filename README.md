# Good-morning-Giselle-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Good Morning Giselle ❤️</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; touch-action: manipulation; }
        html, body { width: 100%; height: 100%; overflow: hidden; background: #87CEEB; position: fixed; font-family: -apple-system, system-ui, sans-serif; }

        /* POV Balloon Effect */
        #sky {
            position: fixed; inset: 0;
            background: linear-gradient(to bottom, #2c3e50 0%, #000000 100%);
            transition: background 3s ease; z-index: -2;
        }
        
        /* Cloud Layers */
        .cloud-bg {
            position: absolute; bottom: 0; width: 200%; height: 30%;
            background: rgba(255, 255, 255, 0.8); border-radius: 50% 50% 0 0;
            filter: blur(20px); z-index: -1; animation: drift 20s infinite linear;
        }
        @keyframes drift { from { transform: translateX(0); } to { transform: translateX(-50%); } }

        /* Balloon Basket POV */
        #basket {
            position: absolute; bottom: -10px; width: 110%; left: -5%; height: 25%;
            background: #5d4037; border-top: 15px solid #3e2723;
            border-radius: 40% 40% 0 0; z-index: 10;
            box-shadow: inset 0 10px 30px rgba(0,0,0,0.5);
        }

        /* HUD */
        #hud {
            position: absolute; top: 60px; width: 100%; display: flex;
            justify-content: space-around; z-index: 100;
        }
        .stat-box {
            background: rgba(255,255,255,0.2); padding: 10px 20px;
            border-radius: 20px; backdrop-filter: blur(10px);
            color: white; font-weight: bold; border: 1px solid rgba(255,255,255,0.3);
        }

        /* Game Items */
        .item {
            position: absolute; font-size: 50px; z-index: 5;
            user-select: none; cursor: pointer;
            animation: float 3s linear forwards;
        }
        @keyframes float {
            0% { transform: scale(0.5) translateY(0); opacity: 0; }
            20% { opacity: 1; transform: scale(1.2) translateY(-20px); }
            100% { opacity: 0; transform: scale(1) translateY(-300px); }
        }

        /* Sun */
        #sun {
            position: absolute; bottom: -100px; left: 50%; transform: translateX(-50%);
            width: 120px; height: 120px; background: radial-gradient(circle, #fff700, #ff8c00);
            border-radius: 50%; box-shadow: 0 0 60px #ff8c00; z-index: -1;
            transition: bottom 2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        /* Overlays */
        #start-screen, #win-screen {
            position: fixed; inset: 0; z-index: 200;
            display: flex; flex-direction: column; justify-content: center;
            align-items: center; text-align: center; padding: 20px;
        }
        #start-screen { background: rgba(0,0,0,0.85); color: white; }
        #win-screen { display: none; background: rgba(255, 255, 255, 0.95); color: #333; }

        .btn {
            padding: 18px 40px; font-size: 1.2rem; border-radius: 50px;
            border: none; background: #ff7043; color: white;
            font-weight: bold; margin-top: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }
    </style>
</head>
<body>

<div id="sky"></div>
<div id="sun"></div>
<div class="cloud-bg"></div>
<div id="basket"></div>

<div id="start-screen">
    <h1 style="color: #ffcc80; font-size: 2rem;">Good Morning, Giselle! ❤️</h1>
    <p style="margin: 15px 0;">Catch 10 Sunbeams to start the day!</p>
    <p style="font-size: 14px; color: #ffab91;">(Avoid the clouds and rain drops!)</p>
    <button class="btn" onclick="startGame()">Fly Balloon 🎈</button>
</div>

<div id="hud">
    <div class="stat-box">Sunbeams: <span id="score">0</span>/10</div>
</div>

<div id="win-screen">
    <h1 style="color: #ff7043; margin-bottom: 15px;">You're my Sunshine! ☀️</h1>
    <p style="line-height: 1.6; font-size: 1.1rem;">
        Good morning, my love! I hope you have a beautiful day.
        <br><br>
        You are <b>perfect</b> and I'm so lucky to have you. I love you!
    </p>
    <div style="font-size: 3rem; margin-top: 20px;">❤️ 💋 ✨</div>
</div>

<audio id="music" loop><source src="https://www.bensound.com/bensound-music/bensound-tenderness.mp3" type="audio/mpeg"></audio>

<script>
    let score = 0;
    let gameActive = false;
    const sky = document.getElementById('sky');
    const sun = document.getElementById('sun');
    const scoreEl = document.getElementById('score');

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        gameActive = true;
        document.getElementById('music').play();
        spawnLoop();
    }

    function spawnLoop() {
        if (!gameActive) return;
        spawnItem();
        let delay = Math.max(500, 1500 - (score * 100));
        setTimeout(spawnLoop, delay);
    }

    function spawnItem() {
        const item = document.createElement('div');
        item.className = 'item';
        
        const rand = Math.random();
        let type = 'sun';
        if (rand > 0.7) type = 'cloud';
        else if (rand > 0.5) type = 'rain';

        item.innerHTML = (type === 'sun') ? '☀️' : (type === 'cloud' ? '☁️' : '💧');
        
        item.style.left = (Math.random() * 70 + 15) + '%';
        item.style.top = '70%';

        const handleTap = (e) => {
            if (!gameActive) return;
            e.preventDefault();
            if (type === 'sun') {
                score++;
                updateSky();
            } else {
                score = Math.max(0, score - 1);
                updateSky();
            }
            scoreEl.innerText = score;
            item.remove();
            if (score >= 10) win();
        };

        item.addEventListener('touchstart', handleTap, { passive: false });
        item.addEventListener('mousedown', handleTap);

        document.body.appendChild(item);
        setTimeout(() => { if(item.parentNode) item.remove(); }, 3000);
    }

    function updateSky() {
        const colors = [
            '#020111', '#191621', '#2c3e50', '#4a4e69', '#9a8c98', 
            '#ff9a9e', '#fecfef', '#ffecd2', '#87CEEB', '#E0F7FA'
        ];
        sky.style.background = colors[score] || '#E0F7FA';
        sun.style.bottom = (-100 + (score * 40)) + 'px';
    }

    function win() {
        gameActive = false;
        document.getElementById('hud').style.display = 'none';
        document.getElementById('win-screen').style.display = 'flex';
        document.querySelectorAll('.item').forEach(i => i.remove());
    }
</script>
</body>
</html>
