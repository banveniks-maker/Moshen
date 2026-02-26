<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>Защита от мошенников — Интерактивный справочник</title>
    <meta property="og:title" content="🛡️ Проверь себя: не дай мошенникам шанс!">
    <meta property="og:description" content="Интерактивный гид по безопасности: чат-бот, разбор схем обмана и тест на бдительность.">
    <meta property="og:type" content="website">
    <meta property="og:image" content="https://via.placeholder.com/1200x630.png?text=Stop+Scam+Guide">

    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --dark-blue: #1a3a4a;
            --light-blue: #2c5aa0;
            --danger: #e74c3c;
            --warning: #f39c12;
            --success: #27ae60;
            --light-bg: #f8f9fa;
            --white: #ffffff;
            --gray: #7f8c8d;
            --text: #2c3e50;
        }

        body {
            /* Улучшенный шрифт для мобильных устройств */
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
            background-color: var(--light-bg);
            color: var(--text);
            line-height: 1.6;
        }

        /* Остальные стили без изменений */
        .header {
            background: linear-gradient(135deg, var(--dark-blue) 0%, var(--light-blue) 100%);
            color: var(--white);
            padding: 40px 20px;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
        }

        .header-title {
            font-size: 2.5em;
            font-weight: bold;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }

        .header-subtitle {
            font-size: 1.2em;
            opacity: 0.95;
        }

        .language-selector {
            position: absolute;
            top: 10px;
            right: 10px;
            display: flex;
            gap: 10px;
        }

        .lang-btn {
            padding: 8px 12px;
            border: 2px solid var(--white);
            background: rgba(255, 255, 255, 0.2);
            color: var(--white);
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }

        .lang-btn.active { background: var(--white); color: var(--light-blue); }

        .warning-banner {
            background: var(--danger);
            color: white;
            padding: 15px;
            text-align: center;
            font-size: 1.1em;
            font-weight: bold;
        }

        .nav-tabs {
            display: flex;
            overflow-x: auto;
            background: var(--white);
            border-bottom: 2px solid var(--light-blue);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .nav-tab {
            padding: 15px 25px;
            border: none;
            background: none;
            cursor: pointer;
            font-size: 1em;
            font-weight: 500;
            color: var(--gray);
            white-space: nowrap;
            border-bottom: 3px solid transparent;
        }

        .nav-tab.active { color: var(--light-blue); border-bottom-color: var(--light-blue); }

        .chatbot-icon {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: var(--light-blue);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            cursor: pointer;
            z-index: 999;
            animation: bounce 2s infinite;
        }

        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }

        .chatbot-window {
            position: fixed;
            bottom: 90px;
            right: 20px;
            width: 350px;
            height: 500px;
            background: var(--white);
            border-radius: 12px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
            display: none;
            flex-direction: column;
            z-index: 999;
        }

        .chatbot-window.open { display: flex; }

        .chatbot-header {
            background: var(--light-blue);
            color: white;
            padding: 15px;
            border-radius: 12px 12px 0 0;
            display: flex;
            justify-content: space-between;
        }

        .chatbot-messages { flex: 1; overflow-y: auto; padding: 15px; display: flex; flex-direction: column; gap: 10px; }

        .message { padding: 10px 15px; border-radius: 8px; max-width: 85%; font-size: 0.95em; }
        .message.user { background: var(--light-blue); color: white; align-self: flex-end; }
        .message.bot { background: var(--light-bg); border: 1px solid #ddd; align-self: flex-start; }

        .chatbot-input { display: flex; padding: 10px; gap: 10px; border-top: 1px solid #ddd; }
        .chatbot-input input { flex: 1; padding: 10px; border: 1px solid #ddd; border-radius: 6px; }

        .qr-modal {
            display: none;
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0, 0, 0, 0.6);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .qr-modal.open { display: flex; }
        .qr-content { background: var(--white); padding: 30px; border-radius: 12px; text-align: center; }

        .container { max-width: 1200px; margin: 0 auto; padding: 30px 20px; }
        .section { display: none; }
        .section.active { display: block; }

        .cards-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .card { background: var(--white); border-radius: 8px; padding: 25px; border-left: 5px solid var(--warning); cursor: pointer; }
        .card-content { display: none; margin-top: 15px; }
        .card-content.open { display: block; }

        .warning-block { background: #fff3cd; border-left: 5px solid var(--warning); padding: 20px; margin: 20px 0; }
        .success-block { background: #d4edda; border-left: 5px solid var(--success); padding: 20px; margin: 20px 0; }

        .hotline { background: linear-gradient(135deg, var(--danger) 0%, #c0392b 100%); color: var(--white); padding: 30px; border-radius: 8px; text-align: center; }
        .hotline-item { background: rgba(0, 0, 0, 0.1); padding: 15px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center; margin-top: 10px; }

        .option-btn { width: 100%; padding: 12px; border: 2px solid var(--light-blue); background: white; cursor: pointer; border-radius: 6px; margin-bottom: 5px; text-align: left; }
        
        .footer { background: var(--dark-blue); color: var(--white); text-align: center; padding: 30px; margin-top: 50px; }
        
        body.rtl { direction: rtl; text-align: right; }
        @media (max-width: 768px) { .chatbot-window { width: 90vw; } }
    </style>
</head>
<body>
    <div class="language-selector">
        <button class="lang-btn active" onclick="setLanguage('ru')">РУС</button>
        <button class="lang-btn" onclick="setLanguage('en')">ENG</button>
        <button class="lang-btn" onclick="setLanguage('he')">עברית</button>
    </div>

    <div class="header">
        <div class="header-title">🛡️ Защита от мошенников</div>
        <div class="header-subtitle">Справочник и рекомендации для всех возрастов</div>
    </div>

    <div class="warning-banner">
        ⚠️ ПОМНИТЕ: Мошенники становятся умнее. Будьте осторожны!
    </div>

    <div class="nav-tabs">
        <button class="nav-tab active" onclick="switchTab('types')">📱 Типы</button>
        <button class="nav-tab" onclick="switchTab('signs')">🔍 Признаки</button>
        <button class="nav-tab" onclick="switchTab('protect')">🔐 Защита</button>
        <button class="nav-tab" onclick="switchTab('help')">🆘 Помощь</button>
        <button class="nav-tab" onclick="switchTab('quiz')">❓ Тест</button>
    </div>

    <div class="container">
        <div id="types" class="section active">
            <h2>Основные типы мошенничества</h2>
            <div class="cards-grid">
                <div class="card" onclick="toggleCard(this)">
                    <div style="font-size: 2em;">📞</div>
                    <h3>Звонки</h3>
                    <p>Выдают себя за банк или полицию.</p>
                    <div class="card-content">
                        <div class="warning-block">Никогда не называйте коды из SMS!</div>
                    </div>
                </div>
                <div class="card" onclick="toggleCard(this)">
                    <div style="font-size: 2em;">📱</div>
                    <h3>SMS / WhatsApp</h3>
                    <p>Ссылки на поддельные сайты.</p>
                    <div class="card-content">
                        <div class="warning-block">Не переходите по ссылкам от незнакомцев!</div>
                    </div>
                </div>
            </div>
        </div>

        <div id="help" class="section">
            <h2>Что делать?</h2>
            <div class="hotline">
                <div class="hotline-item">
                    <span>🇷🇺 Россия: 8-800-700-40-30</span>
                    <button onclick="generateQR('tel:88007004030', 'МВД РФ')">QR</button>
                </div>
                <div class="hotline-item">
                    <span>🇮🇱 Израиль: 100</span>
                    <button onclick="generateQR('tel:100', 'Полиция Израиля')">QR</button>
                </div>
            </div>
        </div>
        
        </div>

    <div class="chatbot-icon" onclick="toggleChatbot()">💬</div>
    <div class="chatbot-window" id="chatbot">
        <div class="chatbot-header"><span>🤖 Помощник</span><button onclick="toggleChatbot()" style="background:none; border:none; color:white; cursor:pointer;">✕</button></div>
        <div class="chatbot-messages" id="messages">
            <div class="message bot">Привет! Опишите ситуацию, и я помогу понять, мошенники ли это.</div>
        </div>
        <div class="chatbot-input">
            <input type="text" id="userInput" placeholder="Напишите вопрос..." onkeypress="if(event.key=='Enter') sendMessage()">
            <button onclick="sendMessage()" style="background:var(--light-blue); color:white; border:none; padding:10px; border-radius:5px; cursor:pointer;">></button>
        </div>
    </div>

    <div class="qr-modal" id="qrModal">
        <div class="qr-content">
            <h3 id="qrTitle"></h3>
            <div id="qrcode" style="margin: 20px auto;"></div>
            <button onclick="closeQR()" style="padding:10px 20px; cursor:pointer;">Закрыть</button>
        </div>
    </div>

    <div class="footer">
        <p>🛡️ Создано для вашей безопасности</p>
    </div>

    <script>
        const chatbotResponses = {
            'банк': 'Банк никогда не просит CVC-код или пароль из SMS! Сбросьте вызов.',
            'пароль': 'Никому не говорите пароли! Даже сотрудникам банка.',
            'деньги': 'Если просят срочно перевести деньги — это мошенники.',
            'default': 'Повесьте трубку и перезвоните по официальному номеру вашего банка.'
        };

        function toggleChatbot() { document.getElementById('chatbot').classList.toggle('open'); }

        function sendMessage() {
            const input = document.getElementById('userInput');
            const msg = input.value.toLowerCase();
            if(!msg) return;

            const container = document.getElementById('messages');
            container.innerHTML += `<div class="message user">${input.value}</div>`;
            
            let reply = chatbotResponses['default'];
            for(let key in chatbotResponses) { if(msg.includes(key)) reply = chatbotResponses[key]; }
            
            setTimeout(() => {
                container.innerHTML += `<div class="message bot">${reply}</div>`;
                container.scrollTop = container.scrollHeight;
            }, 500);
            input.value = '';
        }

        function switchTab(id) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            event.target.classList.add('active');
        }

        function toggleCard(card) { card.querySelector('.card-content').classList.toggle('open'); }

        function generateQR(text, title) {
            document.getElementById('qrTitle').innerText = title;
            document.getElementById('qrcode').innerHTML = '';
            new QRCode(document.getElementById('qrcode'), { text: text, width: 200, height: 200 });
            document.getElementById('qrModal').classList.add('open');
        }

        function closeQR() { document.getElementById('qrModal').classList.remove('open'); }

        function setLanguage(lang) {
            document.body.classList.toggle('rtl', lang === 'he');
            document.querySelectorAll('.lang-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
        }
    </script>
</body>
</html>
