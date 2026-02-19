<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Discord · Сервер</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #000000;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            position: relative;
            overflow-x: hidden;
            padding: 1rem;
        }

        /* КРАСИВЫЙ ЭФФЕКТ СВЕЧЕНИЯ */
        body::before {
            content: "";
            position: absolute;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 50%, rgba(88, 101, 242, 0.2) 0%, transparent 50%),
                        radial-gradient(circle at 70% 30%, rgba(114, 137, 218, 0.15) 0%, transparent 50%),
                        radial-gradient(circle at 10% 80%, rgba(255, 255, 255, 0.05) 0%, transparent 40%);
            pointer-events: none;
            animation: glowPulse 8s ease-in-out infinite;
        }

        @keyframes glowPulse {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        /* ПАРТИКЛЫ (анимированные частицы) */
        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: rgba(88, 101, 242, 0.4);
            border-radius: 50%;
            box-shadow: 0 0 15px #5865F2, 0 0 30px #5865F2;
            pointer-events: none;
            z-index: 1;
        }

        @keyframes floatParticle {
            0% {
                transform: translateY(0) translateX(0) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 0.8;
            }
            90% {
                opacity: 0.8;
            }
            100% {
                transform: translateY(-100vh) translateX(20px) rotate(360deg);
                opacity: 0;
            }
        }

        /* КОНТЕЙНЕР ДЛЯ ПАРТИКЛОВ */
        .particles-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            z-index: 1;
        }

        /* ОСНОВНОЙ КОНТЕЙНЕР */
        .widget-container {
            position: relative;
            z-index: 10;
            width: 100%;
            max-width: 420px;
            animation: fadeIn 1s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* КАРТОЧКА С АНИМАЦИЕЙ */
        .discord-card {
            width: 100%;
            border-radius: 32px;
            overflow: hidden;
            box-shadow: 0 0 0 1px rgba(88, 101, 242, 0.3),
                        0 20px 40px rgba(0, 0, 0, 0.8),
                        0 0 80px rgba(88, 101, 242, 0.4);
            background: rgba(10, 10, 15, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            animation: cardGlow 3s ease-in-out infinite;
        }

        @keyframes cardGlow {
            0%, 100% { box-shadow: 0 0 0 1px rgba(88, 101, 242, 0.3), 0 20px 40px rgba(0, 0, 0, 0.8), 0 0 80px rgba(88, 101, 242, 0.4); }
            50% { box-shadow: 0 0 0 2px rgba(88, 101, 242, 0.6), 0 25px 50px rgba(0, 0, 0, 0.9), 0 0 120px rgba(88, 101, 242, 0.7); }
        }

        .discord-card:hover {
            transform: scale(1.02) translateY(-5px);
        }

        /* ХЕДЕР С АНИМИРОВАННОЙ ПОЛОСКОЙ */
        .custom-header {
            background: linear-gradient(145deg, rgba(13, 15, 26, 0.9), rgba(8, 10, 18, 0.95));
            padding: 1.5rem;
            border-bottom: 1px solid rgba(88, 101, 242, 0.3);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(5px);
        }

        .custom-header::after {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, #5865F2, #9b87f5, #5865F2, transparent);
            animation: shimmer 3s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* ПРОФИЛЬ СЕРВЕРА */
        .server-profile {
            display: flex;
            align-items: center;
            gap: 1.2rem;
            position: relative;
        }

        /* АВАТАРКА С ПУЛЬСАЦИЕЙ */
        .avatar {
            width: 70px;
            height: 70px;
            border-radius: 20px;
            background: linear-gradient(135deg, #2b2f4c, #1a1d33);
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.6);
            border: 2px solid rgba(255, 255, 255, 0.1);
            overflow: hidden;
            animation: avatarPulse 3s ease-in-out infinite;
            position: relative;
        }

        @keyframes avatarPulse {
            0%, 100% { box-shadow: 0 8px 20px rgba(0, 0, 0, 0.6), 0 0 0 0 rgba(88, 101, 242, 0); }
            50% { box-shadow: 0 8px 25px rgba(0, 0, 0, 0.8), 0 0 20px 5px rgba(88, 101, 242, 0.5); }
        }

        .avatar::before {
            content: "";
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            border-radius: 22px;
            background: linear-gradient(135deg, #5865F2, #9b87f5);
            opacity: 0;
            transition: opacity 0.3s;
            z-index: -1;
        }

        .avatar:hover::before {
            opacity: 0.5;
        }

        .avatar img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* ИНФОРМАЦИЯ О СЕРВЕРЕ */
        .server-info {
            flex: 1;
        }

        .server-name {
            font-size: 1.8rem;
            font-weight: 800;
            background: linear-gradient(135deg, #ffffff, #c5cbff);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 0.3rem;
            animation: textGlow 3s ease-in-out infinite;
            text-shadow: 0 0 10px rgba(88, 101, 242, 0.3);
        }

        @keyframes textGlow {
            0%, 100% { text-shadow: 0 0 10px rgba(88, 101, 242, 0.3); }
            50% { text-shadow: 0 0 20px rgba(88, 101, 242, 0.8); }
        }

        /* ИНДИКАТОР ОНЛАЙН (МАЛЕНЬКИЙ) */
        .online-indicator {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.85rem;
            color: #b5bac9;
        }

        .online-dot {
            width: 8px;
            height: 8px;
            background: #57f287;
            border-radius: 50%;
            box-shadow: 0 0 10px #57f287;
            animation: dotPulse 2s ease-in-out infinite;
        }

        @keyframes dotPulse {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.2); opacity: 0.8; }
        }

        /* ОБЕРТКА ДЛЯ КОНТЕНТА */
        .content-wrapper {
            padding: 1.5rem;
            background: rgba(10, 10, 15, 0.8);
            backdrop-filter: blur(5px);
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        /* БОЛЬШАЯ КРАСИВАЯ КНОПКА */
        .join-button {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            width: 100%;
            padding: 1.2rem;
            background: linear-gradient(135deg, #5865F2, #4752c4);
            border: none;
            border-radius: 20px;
            color: white;
            font-size: 1.4rem;
            font-weight: 700;
            text-decoration: none;
            cursor: pointer;
            box-shadow: 0 8px 0 #2d37a0, 0 10px 20px rgba(0, 0, 0, 0.4);
            transition: all 0.2s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
            animation: buttonGlow 3s ease-in-out infinite;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .join-button::before {
            content: "";
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s;
        }

        .join-button:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 0 #2d37a0, 0 15px 25px rgba(0, 0, 0, 0.5);
            background: linear-gradient(135deg, #6775f0, #5865F2);
        }

        .join-button:hover::before {
            left: 100%;
        }

        .join-button:active {
            transform: translateY(4px);
            box-shadow: 0 4px 0 #2d37a0, 0 8px 15px rgba(0, 0, 0, 0.4);
        }

        @keyframes buttonGlow {
            0%, 100% { filter: brightness(1); }
            50% { filter: brightness(1.2); }
        }

        .discord-icon {
            font-size: 2rem;
            filter: drop-shadow(0 2px 5px rgba(0, 0, 0, 0.3));
            animation: iconWiggle 3s ease-in-out infinite;
        }

        @keyframes iconWiggle {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(5deg); }
            75% { transform: rotate(-5deg); }
        }

        .button-text {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            line-height: 1.2;
        }

        .button-text-small {
            font-size: 0.8rem;
            opacity: 0.8;
            font-weight: 400;
        }

        /* ВИДЖЕТ (теперь ниже кнопки) */
        .discord-widget {
            width: 100%;
            height: 350px;
            border-radius: 20px;
            overflow: hidden;
            border: 1px solid rgba(88, 101, 242, 0.2);
            background: #1e1f29;
            transition: all 0.3s ease;
            animation: widgetGlow 4s ease-in-out infinite;
        }

        @keyframes widgetGlow {
            0%, 100% { border-color: rgba(88, 101, 242, 0.2); box-shadow: 0 0 20px rgba(88, 101, 242, 0.1); }
            50% { border-color: rgba(88, 101, 242, 0.6); box-shadow: 0 0 30px rgba(88, 101, 242, 0.3); }
        }

        .discord-widget:hover {
            border-color: #5865F2;
            transform: scale(1.01);
        }

        .discord-widget iframe {
            width: 100%;
            height: 100%;
            border: none;
        }

        /* НИЖНЯЯ ПАНЕЛЬ */
        .footer-bar {
            margin-top: 1.2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0.8rem 1rem;
            background: rgba(10, 10, 15, 0.7);
            backdrop-filter: blur(10px);
            border-radius: 40px;
            border: 1px solid rgba(88, 101, 242, 0.2);
            color: #7f85b5;
            width: 100%;
            transition: all 0.3s;
        }

        .footer-bar:hover {
            border-color: rgba(88, 101, 242, 0.5);
            background: rgba(20, 20, 30, 0.8);
        }

        .server-id {
            font-family: monospace;
            background: rgba(0, 0, 0, 0.4);
            padding: 0.3rem 0.8rem;
            border-radius: 40px;
            border: 1px solid #2a2e4a;
            color: #b5bbff;
            font-size: 0.8rem;
        }

        .json-link {
            color: #9ca3ff;
            text-decoration: none;
            display: flex;
            align-items: center;
            gap: 5px;
            padding: 0.3rem 0.8rem;
            border-radius: 40px;
            background: rgba(88, 101, 242, 0.1);
            transition: all 0.2s;
            font-size: 0.8rem;
        }

        .json-link:hover {
            background: rgba(88, 101, 242, 0.25);
            color: white;
            transform: scale(1.05);
        }

        /* НЕОНОВАЯ ПОДПИСЬ */
        .neon-caption {
            text-align: center;
            margin-top: 1.5rem;
            color: rgba(255, 255, 255, 0.3);
            font-size: 0.8rem;
            letter-spacing: 3px;
            text-transform: uppercase;
            animation: captionGlow 3s ease-in-out infinite;
        }

        @keyframes captionGlow {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        .neon-caption span {
            color: #5865F2;
            animation: spanGlow 2s ease-in-out infinite;
        }

        @keyframes spanGlow {
            0%, 100% { text-shadow: 0 0 5px #5865F2; }
            50% { text-shadow: 0 0 15px #5865F2; }
        }
    </style>
</head>
<body>
    <!-- КОНТЕЙНЕР ДЛЯ ПАРТИКЛОВ -->
    <div class="particles-container" id="particles-container"></div>

    <div class="widget-container">
        <div class="discord-card">
            <div class="custom-header">
                <div class="server-profile">
                    <!-- ТВОЯ АВАТАРКА -->
                    <div class="avatar">
                        <img src="https://i.ibb.co/RT9vMCGQ/photo-2025-04-03-18-19-13.jpg" alt="Server Avatar">
                    </div>
                    <div class="server-info">
                        <div class="server-name" id="server-name">Загрузка...</div>
                        <div class="online-indicator">
                            <span class="online-dot"></span>
                            <span id="online-count">⋯</span> участников онлайн
                        </div>
                    </div>
                </div>
            </div>

            <div class="content-wrapper">
                <!-- БОЛЬШАЯ КРАСИВАЯ КНОПКА ВОЙТИ -->
                <a href="https://discord.gg/your-invite-link" target="_blank" class="join-button" id="join-button">
                    <span class="discord-icon">🎮</span>
                    <span class="button-text">
                        <span>Войти в сервер</span>
                        <span class="button-text-small">присоединяйся к нам</span>
                    </span>
                    <span class="discord-icon">⚡</span>
                </a>

                <!-- ВИДЖЕТ DISCORD -->
                <div class="discord-widget">
                    <iframe src="https://discord.com/widget?id=1474003052884132033&theme=dark" 
                            allowtransparency="true" 
                            frameborder="0" 
                            sandbox="allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts">
                    </iframe>
                </div>
            </div>
        </div>

        <div class="footer-bar">
            <span class="server-id">1474003052884132033</span>
            <a href="https://discord.com/api/guilds/1474003052884132033/widget.json" target="_blank" class="json-link">
                <span>⚡</span> widget.json
            </a>
        </div>

        <div class="neon-caption">
            <span>✦</span> DISCORD COMMUNITY <span>✦</span>
        </div>
    </div>

    <script>
        const SERVER_ID = '1474003052884132033';
        // ЗАМЕНИ ЭТУ ССЫЛКУ НА ТВОЕ ПРИГЛАШЕНИЕ В DISCORD
        const INVITE_LINK = 'https://discord.gg/your-invite-link'; // Сюда вставь свою ссылку-приглашение
        
        async function loadServerInfo() {
            try {
                const response = await fetch(`https://discord.com/api/guilds/${SERVER_ID}/widget.json`);
                const data = await response.json();
                
                if (data.name) {
                    document.getElementById('server-name').textContent = data.name;
                }
                
                if (data.presence_count !== undefined) {
                    document.getElementById('online-count').textContent = data.presence_count;
                }
                
                // Если в API есть instant_invite, используем его
                if (data.instant_invite) {
                    document.getElementById('join-button').href = data.instant_invite;
                } else {
                    // Иначе используем нашу ссылку
                    document.getElementById('join-button').href = INVITE_LINK;
                }
                
            } catch (error) {
                console.log('Не удалось загрузить данные');
                document.getElementById('server-name').textContent = 'Discord Сервер';
                document.getElementById('online-count').textContent = '--';
                document.getElementById('join-button').href = INVITE_LINK;
            }
        }

        loadServerInfo();
        setInterval(loadServerInfo, 60000);

        // ФУНКЦИЯ ДЛЯ СОЗДАНИЯ ПАРТИКЛОВ
        function createParticles() {
            const container = document.getElementById('particles-container');
            const particleCount = 25;
            
            for (let i = 0; i < particleCount; i++) {
                setTimeout(() => {
                    const particle = document.createElement('div');
                    particle.className = 'particle';
                    
                    const startX = Math.random() * 100;
                    const startY = 100 + Math.random() * 20;
                    const size = 2 + Math.random() * 5;
                    const delay = Math.random() * 10;
                    const duration = 5 + Math.random() * 8;
                    const colors = ['88, 101, 242', '114, 137, 218', '255, 255, 255'];
                    const color = colors[Math.floor(Math.random() * colors.length)];
                    
                    particle.style.cssText = `
                        left: ${startX}%;
                        top: ${startY}%;
                        width: ${size}px;
                        height: ${size}px;
                        background: rgba(${color}, 0.6);
                        box-shadow: 0 0 ${size * 5}px rgba(${color}, 0.8);
                        animation: floatParticle ${duration}s linear ${delay}s infinite;
                    `;
                    
                    container.appendChild(particle);
                    
                    setTimeout(() => {
                        particle.remove();
                        createSingleParticle();
                    }, duration * 1000);
                }, i * 200);
            }
        }

        function createSingleParticle() {
            const container = document.getElementById('particles-container');
            const particle = document.createElement('div');
            particle.className = 'particle';
            
            const startX = Math.random() * 100;
            const startY = 100 + Math.random() * 20;
            const size = 2 + Math.random() * 5;
            const duration = 5 + Math.random() * 8;
            const colors = ['88, 101, 242', '114, 137, 218', '255, 255, 255'];
            const color = colors[Math.floor(Math.random() * colors.length)];
            
            particle.style.cssText = `
                left: ${startX}%;
                top: ${startY}%;
                width: ${size}px;
                height: ${size}px;
                background: rgba(${color}, 0.6);
                box-shadow: 0 0 ${size * 5}px rgba(${color}, 0.8);
                animation: floatParticle ${duration}s linear infinite;
            `;
            
            container.appendChild(particle);
            
            setTimeout(() => {
                particle.remove();
                createSingleParticle();
            }, duration * 1000);
        }

        // Запускаем создание частиц
        createParticles();
    </script>
</body>
</html>
