<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Linkbot AI</title>
    <style>
        /* Reset and base styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #FF1B6B 0%, #45CAFF 100%);
            height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* Header styles */
        .header {
            background-color: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 1rem;
            display: flex;
            align-items: center;
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #fff;
            margin-right: 1rem;
        }

        .header-text h1 {
            color: #fff;
            font-size: 1.2rem;
        }

        .header-text p {
            color: rgba(255, 255, 255, 0.8);
            font-size: 0.8rem;
        }

        /* Chat container styles */
        .chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 1rem;
        }

        .message {
            max-width: 80%;
            margin-bottom: 1rem;
            padding: 0.5rem 1rem;
            border-radius: 1rem;
            color: #fff;
        }

        .user-message {
            background-color: #fff;
            color: #FF1B6B;
            align-self: flex-end;
            margin-left: auto;
        }

        .bot-message {
            background-color: #03030395;
        }

        /* Input area styles */
        .input-area {
            background-color: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 1rem;
            display: flex;
            align-items: center;
        }

        .input-area input {
            flex: 1;
            padding: 0.5rem;
            border: none;
            border-radius: 1rem;
            margin-right: 0.5rem;
            background-color: rgba(255, 255, 255, 0.2);
            color: #fff;
        }

        .input-area button {
            background-color: #fff;
            color: #FF1B6B;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
        }

        /* Modal styles */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background-color: #fff;
            padding: 2rem;
            border-radius: 1rem;
            text-align: center;
        }

        .modal-content button {
            margin-top: 1rem;
            padding: 0.5rem 1rem;
            background-color: #FF1B6B;
            color: #fff;
            border: none;
            border-radius: 0.5rem;
            cursor: pointer;
        }

        /* 3D Cube Animation */
        .cube-container {
            width: 100px;
            height: 100px;
            perspective: 1000px;
            margin: 0 auto 1rem;
        }

        .cube {
            width: 100%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            animation: rotate 5s infinite linear;
        }

        .cube div {
            position: absolute;
            width: 100px;
            height: 100px;
            background: rgba(255, 27, 107, 0.8);
            border: 2px solid #fff;
        }

        .front  { transform: rotateY(0deg) translateZ(50px); }
        .back   { transform: rotateY(180deg) translateZ(50px); }
        .right  { transform: rotateY(90deg) translateZ(50px); }
        .left   { transform: rotateY(-90deg) translateZ(50px); }
        .top    { transform: rotateX(90deg) translateZ(50px); }
        .bottom { transform: rotateX(-90deg) translateZ(50px); }

        @keyframes rotate {
            0% { transform: rotateX(0deg) rotateY(0deg); }
            100% { transform: rotateX(360deg) rotateY(360deg); }
        }

        /* Welcome Modal */
        .welcome-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.6);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .welcome-content {
            background-color: #fff;
            padding: 2rem;
            border-radius: 1rem;
            max-width: 90%;
            width: 400px;
        }

        .welcome-header {
            display: flex;
            align-items: center;
            margin-bottom: 1rem;
        }

        .welcome-avatar {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background-color: #FF1B6B;
            margin-right: 1rem;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #fff;
            font-size: 1.5rem;
        }

        .welcome-title {
            font-size: 1.5rem;
            background: linear-gradient(to right, #FF1B6B, #45CAFF);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .welcome-subtitle {
            color: #666;
            font-size: 0.9rem;
        }

        .welcome-instructions {
            margin-top: 1rem;
        }

        .instruction-item {
            display: flex;
            align-items: flex-start;
            margin-bottom: 1rem;
        }

        .instruction-icon {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background-color: rgba(255, 27, 107, 0.1);
            display: flex;
            justify-content: center;
            align-items: center;
            margin-right: 1rem;
            flex-shrink: 0;
        }

        .instruction-text {
            flex: 1;
        }

        .instruction-text strong {
            display: block;
            margin-bottom: 0.25rem;
        }

        .welcome-note {
            background-color: rgba(255, 27, 107, 0.1);
            padding: 1rem;
            border-radius: 0.5rem;
            margin-top: 1rem;
            font-size: 0.9rem;
        }

        .welcome-buttons {
            margin-top: 1rem;
        }

        .welcome-button {
            display: block;
            width: 100%;
            padding: 0.75rem;
            border: none;
            border-radius: 0.5rem;
            background: linear-gradient(135deg, #FF1B6B 0%, #45CAFF 100%);
            color: #fff;
            font-size: 1rem;
            cursor: pointer;
            margin-bottom: 0.5rem;
        }

        .telegram-button {
            background: #0088cc;
        }
    </style>
</head>
<body>
    <div class="header">
        <div class="avatar"></div>
        <div class="header-text">
            <h1>Linkbot AI</h1>
            <p>Online</p>
        </div>
    </div>

    <div class="chat-container" id="chatContainer"></div>

    <div class="input-area">
        <input type="file" id="fileInput" style="display: none;" accept="image/*">
        <button onclick="document.getElementById('fileInput').click()">📷</button>
        <input type="text" id="messageInput" placeholder="Xabaringizni yozing...">
        <button onclick="sendMessage()">📤</button>
    </div>

    <div id="modal" class="modal" style="display: none;">
        <div class="modal-content">
            <div class="cube-container">
                <div class="cube">
                    <div class="front"></div>
                    <div class="back"></div>
                    <div class="right"></div>
                    <div class="left"></div>
                    <div class="top"></div>
                    <div class="bottom"></div>
                </div>
            </div>
            <p>Qidirilmoqda...</p>
            <button id="messageButton" style="display: none;">Xabar Yozish</button>
        </div>
    </div>

    <div id="welcomeModal" class="welcome-modal">
        <div class="welcome-content">
            <div class="welcome-header">
                <div class="welcome-avatar">🤖</div>
                <div>
                    <h2 class="welcome-title">Linkbot AI Qo'llanmasi</h2>
                    <p class="welcome-subtitle">Botdan foydalanish yo'riqnomasi</p>
                </div>
            </div>
            <div class="welcome-instructions">
                <div class="instruction-item">
                    <div class="instruction-icon">📋</div>
                    <div class="instruction-text">
                        <strong>Qizning elonini nusxalang:</strong>
                        Telegram kanallaridan qiziqtirgan qizning elon havolasini nusxalab oling
                    </div>
                </div>
                <div class="instruction-item">
                    <div class="instruction-icon">💬</div>
                    <div class="instruction-text">
                        <strong>Elonni menga yuboring:</strong>
                        Nusxalangan havola yoki rasmni chatga tashlang
                    </div>
                </div>
                <div class="instruction-item">
                    <div class="instruction-icon">🖼️</div>
                    <div class="instruction-text">
                        <strong>Rasm ham yuborishingiz mumkin:</strong>
                        Qizning rasmini yuklash orqali ham izlash mumkin
                    </div>
                </div>
                <div class="instruction-item">
                    <div class="instruction-icon">➡️</div>
                    <div class="instruction-text">
                        <strong>Men sizga yordam beraman:</strong>
                        Qizning profilini topib, siz bilan bog'lanish uchun havola beraman
                    </div>
                </div>
            </div>
            <div class="welcome-note">
                <p><strong>Eslatma:</strong></p>
                <p>Men robotman, faqat elon va rasmlar bilan ishlay olaman. Ortiqcha savollarga javob bera olmayman. Iltimos, faqat elon havolasi yoki rasm yuboring.</p>
            </div>
            <div class="welcome-buttons">
                <button class="welcome-button" onclick="closeWelcomeModal()">Tushundim, boshlaylik!</button>
                <button class="welcome-button telegram-button" onclick="window.open('https://t.me/Munis_Admin4', '_blank')">TELEGRAM KANAL</button>
            </div>
        </div>
    </div>

    <script>
        const chatContainer = document.getElementById('chatContainer');
        const messageInput = document.getElementById('messageInput');
        const modal = document.getElementById('modal');
        const messageButton = document.getElementById('messageButton');
        const fileInput = document.getElementById('fileInput');
        const welcomeModal = document.getElementById('welcomeModal');

        const TELEGRAM_URLS = ["https://t.me/musli_ma02",
"https://t.me/Bonushka24",
"https://t.me/Lol4107",
"https://t.me/Sabbit_qolbi",
"https://t.me/GBU2025yil",
"https://t.me/Robbim1",
"https://t.me/zuza1011",
"https://t.me/Mundirji",
"https://t.me/laylo013",
"https://t.me/Z60040",
"https://t.me/Lave0000",
"https://t.me/mss_snk",
"https://t.me/eshonqulova_1995",
"https://t.me/Rb_n0707",
"https://t.me/rstmvna02",
"https://t.me/mubina0211",
"https://t.me/Hasbunalloh1907",
"https://t.me/Milllayaaa",
"https://t.me/Mira_Xanim",
"https://t.me/Irodakhoon"];
        const BUSY_MESSAGES = [
            "Кичрасиз бу киз хозирда банд экан узур 2 ёки 3 дакикадан сунг ёзмнг манга",
            "Кичрасиз топа олмадим",
            "Узур бу киз банд",
        ];
        const TEXT_RESPONSE_MESSAGES = [
            "🤖 Узур, мен сиз ёзган матнга тушунмадим. Менга матн ўқиш имкониятини беришмаган, шу учун ёзган матнингизни кўрмадим. 📝❌",
            "🙏 Кечирасиз, мен сиз ёзган матнни ўқишга ҳаракат қилдим, аммо тушунмадим. Менга фақат Келинликка номзоднинг расми ёки элондан нусхасини жўнатинг. 👰🏻‍♀️🔍",
            "🚫 Менга хабар ёзиш чекланган. Сизга ёрдам беришим учун расм ёки номзоднинг манзил��дан нусха олиб жўнатинг. 🖼️🔗",
        ];

        let lastVisitTime = localStorage.getItem('lastVisitTime') || 0;
        let currentUrl = '';

        function saveLastVisitTime() {
            localStorage.setItem('lastVisitTime', Date.now());
        }

        function checkReturnTime() {
            const currentTime = Date.now();
            const timeDiff = currentTime - lastVisitTime;
            return timeDiff >= 15000 && timeDiff <= 120000;
        }

        function showQuickReturnModal() {
            const quickReturnModal = document.createElement('div');
            quickReturnModal.className = 'modal';
            quickReturnModal.innerHTML = `
                <div class="modal-content">
                    <div class="cube-container">
                        <div class="cube">
                            <div class="front"></div>
                            <div class="back"></div>
                            <div class="right"></div>
                            <div class="left"></div>
                            <div class="top"></div>
                            <div class="bottom"></div>
                        </div>
                    </div>
                    <p>Qidirilmoqda...</p>
                    <button id="quickMessageButton" style="display: none;">Xabar yozish</button>
                </div>
            `;
            document.body.appendChild(quickReturnModal);

            setTimeout(() => {
                const quickMessageButton = document.getElementById('quickMessageButton');
                quickMessageButton.style.display = 'block';
                quickMessageButton.addEventListener('click', () => {
                    window.open("https://www3.affhone.fyi/click?pid=81683&offer_id=54", '_blank');
                    quickReturnModal.remove();
                });
            }, Math.floor(Math.random() * 5000) + 1000); // 1-6 soniya orasida
        }

        function handleQuickReturn(isImage = false) {
            if (checkReturnTime()) {
                showQuickReturnModal();
            } else {
                if (isImage) {
                    showModal();
                } else {
                    currentUrl = getRandomTelegramUrl();
                    showModal();
                }
            }
        }

        function addMessage(content, isUser = false, isImage = false) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('message');
            messageElement.classList.add(isUser ? 'user-message' : 'bot-message');

            if (isImage) {
                const img = document.createElement('img');
                img.src = content;
                img.style.maxWidth = '100%';
                img.style.borderRadius = '0.5rem';
                messageElement.appendChild(img);
            } else {
                messageElement.textContent = content;
            }

            chatContainer.appendChild(messageElement);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        function sendMessage() {
            const message = messageInput.value.trim();
            if (message) {
                addMessage(message, true);
                messageInput.value = '';
                
                if (message.startsWith('http://') || message.startsWith('https://')) {
                    handleQuickReturn();
                } else {
                    setTimeout(() => {
                        const randomIndex = Math.floor(Math.random() * TEXT_RESPONSE_MESSAGES.length);
                        addMessage(TEXT_RESPONSE_MESSAGES[randomIndex]);
                    }, 1000);
                }
            }
        }

        function handleImageUpload(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    addMessage(e.target.result, true, true);
                    handleQuickReturn(true);
                }
                reader.readAsDataURL(file);
            }
        }

        function showModal() {
            modal.style.display = 'flex';
            setTimeout(() => {
                messageButton.style.display = 'block';
            }, Math.floor(Math.random() * 7000) + 3000);
        }

        function closeModal() {
            modal.style.display = 'none';
            messageButton.style.display = 'none';
        }

        function getRandomTelegramUrl() {
            return TELEGRAM_URLS[Math.floor(Math.random() * TELEGRAM_URLS.length)];
        }

        messageButton.addEventListener('click', () => {
            window.open(getRandomTelegramUrl(), '_blank');
            closeModal();
        });

        fileInput.addEventListener('change', handleImageUpload);

        function closeWelcomeModal() {
            welcomeModal.style.display = 'none';
            addMessage("🎉 САЛОМ, ХУШ КЕЛИБСИЗ! 🎉\n\nСиз хозир менга:\n\n📸 РАСМ\n🔗 ЭЛОН МАНЗИЛИ\n\nжўнатинг. Мен сизга тақдим қиламан! 🕵️‍♂️\n\nМаълумотлар базасидан қидириб кўраман! 🚀\n\n💡 Эслатма: Фақат расм ёки ҳавола юборинг!");
        }

        // Always show the welcome modal on page load
        welcomeModal.style.display = 'flex';

        window.addEventListener('load', () => {
            saveLastVisitTime();
        });

        window.addEventListener('beforeunload', () => {
            saveLastVisitTime();
        });
    </script>
</body>

