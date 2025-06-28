<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>江江江</title>
    <link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">
    <style>
        /* 优化后的CSS */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(45deg, #ff6b6b, #ff8e8e);
            font-family: 'Pacifico', cursive;
            overflow-x: hidden;
        }

        .hero {
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .title {
            color: #fff;
            font-size: 4rem;
            text-shadow: 0 2px 4px rgba(0,0,0,0.2);
            animation: pulse 2s infinite;
        }

        .celebration {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 999;
        }

        .confetti {
            width: 10px;
            height: 10px;
            position: absolute;
            animation: confetti-fall 3s linear forwards;
            transform: translateZ(0);
            will-change: transform, opacity;
        }

        .firework {
            width: 50px;
            height: 50px;
            position: absolute;
            animation: explode 1s ease-out forwards;
            transform: translateZ(0);
            will-change: transform, opacity;
        }

        .countdown-end {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 3rem;
            color: #fff;
            text-shadow: 0 0 10px #ff0000;
            animation: glow 2s infinite;
        }

        .balloons {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .balloon {
            width: 50px;
            height: 60px;
            border-radius: 50%;
            position: absolute;
            bottom: -100px;
            animation: float 8s infinite;
            opacity: 0.8;
            background: linear-gradient(45deg, #ff3366, #ff8e8e);
            box-shadow: 2px 2px 6px rgba(0,0,0,0.2);
            transform: translateZ(0);
        }

        .gift-box {
            width: 200px;
            height: 200px;
            background: #ffd700;
            position: fixed;
            bottom: 20px;
            right: 20px;
            cursor: pointer;
            transition: transform 0.5s;
            border-radius: 10px;
            box-shadow: 4px 4px 10px rgba(0,0,0,0.2);
        }

        .ribbon {
            position: absolute;
            width: 100%;
            height: 20px;
            background: #ff3366;
            top: 50%;
            transform: translateY(-50%);
        }

        .countdown {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(255,255,255,0.9);
            padding: 15px;
            border-radius: 10px;
            box-shadow: 2px 2px 8px rgba(0,0,0,0.1);
        }

        .countdown span {
            font-size: 1.5rem;
            color: #ff6b6b;
            margin: 0 5px;
        }

        /* 响应式优化 */
        @media (max-width: 768px) {
            .title {
                font-size: 2.5rem;
                text-align: center;
            }
            .countdown {
                top: 10px;
                left: 10px;
                padding: 10px;
            }
            .gift-box {
                width: 120px;
                height: 120px;
            }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        @keyframes float {
            0% {
                transform: translateX(0);
                opacity: 0.8;
            }
            50% {
                transform: translateX(30px);
            }
            100% {
                bottom: 100vh;
                opacity: 0;
                transform: translateX(-30px);
            }
        }

        @keyframes confetti-fall {
            0% { transform: translateY(-100vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }

        @keyframes explode {
            0% { transform: scale(0); opacity: 1; }
            100% { transform: scale(5); opacity: 0; }
        }

        @keyframes glow {
            50% { text-shadow: 0 0 20px #ffd700; }
        }
    </style>
</head>
<body>
    <div class="hero">
        <h1 class="title">江大美女!</h1>
        <div class="balloons"></div>
    </div>

    <div class="gift-box" onclick="openGift()">
        <div class="ribbon"></div>
    </div>

    <div class="countdown">
        <span id="days">00</span>天
        <span id="hours">00</span>时
    </div>
    <div class="celebration"></div>

    <script>
        let hasTriggered = false;
        let countdownInterval;
        const audio = new Audio('https://actions.google.com/sounds/v1/celebrations/party_horn.ogg');
        audio.preload = 'auto';

        window.onload = function() {
            createBalloons();
            updateCountdown();
            countdownInterval = setInterval(updateCountdown, 1000);
            
            document.body.addEventListener('click', () => {
                document.hasUserInteracted = true;
            }, { once: true });
        };

        function createBalloons() {
            const container = document.querySelector('.balloons');
            const colors = ['#ff3366', '#4CAF50', '#2196F3', '#ffd700'];
            
            for(let i = 0; i < 30; i++) {
                const balloon = document.createElement('div');
                balloon.className = 'balloon';
                balloon.style.left = Math.random() * 100 + '%';
                balloon.style.animationDelay = Math.random() * 5 + 's';
                balloon.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                container.appendChild(balloon);
            }
        }

        function updateCountdown() {
            const targetDate = new Date('2025-05-28T00:00:00+08:00');
            const now = new Date();
            let diff = targetDate - now;

            if (diff < 0 && !hasTriggered) {
                hasTriggered = true;
                handleCountdownEnd();
                document.getElementById('days').textContent = "00";
                document.getElementById('hours').textContent = "00";
                return;
            }

            const days = Math.floor(diff / (1000*60*60*24));
            const hours = Math.floor((diff % (1000*60*60*24))/(1000*60*60));

            document.getElementById('days').textContent = days.toString().padStart(2,'0');
            document.getElementById('hours').textContent = hours.toString().padStart(2,'0');
        }

        function handleCountdownEnd() {
            clearInterval(countdownInterval);
            
            const endText = document.createElement('div');
            endText.className = 'countdown-end';
            endText.textContent = '生日快乐！🎉';
            document.body.appendChild(endText);

            createConfetti(50);
            createFireworks(5);
            document.body.style.background = 'linear-gradient(45deg, #ff0000, #ffd700)';
            playCelebrationMusic();
        }

        function createConfetti(amount) {
            const container = document.querySelector('.celebration');
            for (let i = 0; i < amount; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
                confetti.style.animation = `confetti-fall ${2 + Math.random() * 2}s linear forwards`;
                
                confetti.addEventListener('animationend', () => {
                    confetti.remove();
                });
                container.appendChild(confetti);
            }
        }

        function createFireworks(amount) {
            const container = document.querySelector('.celebration');
            for (let i = 0; i < amount; i++) {
                const firework = document.createElement('div');
                firework.className = 'firework';
                firework.style.left = Math.random() * 100 + '%';
                firework.style.top = Math.random() * 100 + '%';
                firework.style.background = `radial-gradient(circle, 
                    hsl(${Math.random() * 360}, 100%, 50%) 20%, 
                    rgba(255,255,255,0.5) 100%)`;
                firework.style.animation = `explode 1s ease-out forwards`;
                
                firework.addEventListener('animationend', () => {
                    firework.remove();
                });
                container.appendChild(firework);
            }
        }

        function playCelebrationMusic() {
            if (document.hasUserInteracted) {
                audio.play();
            } else {
                document.addEventListener('click', () => {
                    audio.play();
                    document.hasUserInteracted = true;
                }, { once: true });
            }
        }

        function openGift(){
            const gift = document.querySelector('.gift-box');
            gift.style.transform = 'rotate(360deg) scale(0)';
            
            const message = document.createElement('div');
            message.textContent = '江大美女,天天开心!';
            message.style.position = 'fixed';
            message.style.top = '50%';
            message.style.left = '50%';
            message.style.transform = 'translate(-50%, -50%)';
            message.style.color = '#fff';
            message.style.fontSize = '2rem';
            message.style.textShadow = '0 2px 4px rgba(0,0,0,0.5)';
            message.style.animation = 'pulse 1s 2';

            setTimeout(() => {
                document.body.appendChild(message);
                setTimeout(() => message.remove(), 2000);
                gift.style.transform = 'none';
            }, 500);
        }
    </script>
</body>
</html>
