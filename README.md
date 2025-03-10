<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PIN Code Entry</title>
    <style>
        /* Base styles */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, Helvetica, sans-serif;
        }

        body {
            background-color: #000;
            color: #fff;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            overflow-x: hidden;
        }

        /* Search animation styles */
        .search-animation {
            width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 0 1rem;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 100;
            background-color: #000;
        }

        .search-title {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 2.5rem;
            text-align: center;
        }

        @media (min-width: 768px) {
            .search-title {
                font-size: 1.75rem;
            }
        }

        .progress-bar {
            width: 100%;
            max-width: 28rem;
            position: relative;
        }

        .progress-bg {
            height: 0.5rem;
            background-color: #374151;
            border-radius: 9999px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(to right, #8b5cf6, #3b82f6);
            width: 0%;
            transition: width 0.1s ease;
        }

        .spinner-container {
            margin-top: 2rem;
            display: flex;
            justify-content: center;
        }

        .spinner {
            width: 4rem;
            height: 4rem;
            border: 0.25rem solid;
            border-color: #3b82f6 #8b5cf6 #ec4899 #6366f1;
            border-radius: 50%;
            animation: spin 2s linear infinite;
        }

        .dots-container {
            margin-top: 2rem;
            display: flex;
            justify-content: center;
            gap: 0.5rem;
            animation: fade 1.5s ease-in-out infinite;
        }

        .dot {
            width: 0.75rem;
            height: 0.75rem;
            background-color: #3b82f6;
            border-radius: 50%;
        }

        .dot:nth-child(1) {
            animation: bounce 1s ease-in-out infinite;
        }

        .dot:nth-child(2) {
            animation: bounce 1s ease-in-out 0.2s infinite;
        }

        .dot:nth-child(3) {
            animation: bounce 1s ease-in-out 0.4s infinite;
        }

        /* PIN input styles */
        .pin-container {
            width: 100%;
            max-width: 28rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 0 1rem;
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.5s ease, transform 0.5s ease;
        }

        .pin-container.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .pin-title {
            font-size: 1.75rem;
            font-weight: bold;
            margin-bottom: 2rem;
            text-align: center;
            background: linear-gradient(to right, #a78bfa, #ec4899);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        @media (min-width: 768px) {
            .pin-title {
                font-size: 2rem;
            }
        }

        .pin-input-group {
            display: flex;
            gap: 0.75rem;
            margin-bottom: 2rem;
        }

        @media (min-width: 768px) {
            .pin-input-group {
                gap: 1rem;
            }
        }

        .pin-input-wrapper {
            position: relative;
            transform: translateY(20px);
            opacity: 0;
            animation: slideUp 0.5s forwards;
        }

        .pin-input-wrapper:nth-child(1) {
            animation-delay: 0.5s;
        }

        .pin-input-wrapper:nth-child(2) {
            animation-delay: 0.6s;
        }

        .pin-input-wrapper:nth-child(3) {
            animation-delay: 0.7s;
        }

        .pin-input-wrapper:nth-child(4) {
            animation-delay: 0.8s;
        }

        .pin-input {
            width: 3.5rem;
            height: 4rem;
            font-size: 1.5rem;
            text-align: center;
            background-color: #111827;
            border: 2px solid #8b5cf6;
            border-radius: 0.5rem;
            color: white;
            outline: none;
        }

        .pin-input:focus {
            box-shadow: 0 0 0 2px rgba(139, 92, 246, 0.5);
        }

        @media (min-width: 768px) {
            .pin-input {
                width: 4rem;
                height: 5rem;
                font-size: 1.75rem;
            }
        }

        .pin-indicator {
            position: absolute;
            top: -0.25rem;
            right: -0.25rem;
            width: 1rem;
            height: 1rem;
            background-color: #10b981;
            border-radius: 50%;
            transform: scale(0);
            transition: transform 0.2s ease;
        }

        .pin-indicator.active {
            transform: scale(1);
        }

        .error-message {
            color: #ef4444;
            margin-bottom: 1rem;
            text-align: center;
            opacity: 0;
            transform: translateY(10px);
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .error-message.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .submit-button {
            padding: 0.75rem 2rem;
            background: linear-gradient(to right, #8b5cf6, #3b82f6);
            border: none;
            border-radius: 0.5rem;
            color: white;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            opacity: 0;
            transform: translateY(20px);
            animation: slideUp 0.5s forwards;
            animation-delay: 0.7s;
        }

        .submit-button:hover {
            transform: scale(1.05);
            box-shadow: 0 20px 25px -5px rgba(139, 92, 246, 0.1), 0 10px 10px -5px rgba(139, 92, 246, 0.04);
        }

        .submit-button:active {
            transform: scale(0.95);
        }

        /* Info section styles */
        .info-section {
            max-width: 28rem;
            margin: 2rem auto;
            text-align: center;
            padding: 0 1rem;
            opacity: 0;
            transform: translateY(20px);
            animation: slideUp 0.5s forwards;
            animation-delay: 0.9s;
        }

        .info-text {
            color: #94a3b8;
            font-size: 1rem;
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }

        .buy-button {
            display: inline-block;
            padding: 0.75rem 2rem;
            background: linear-gradient(to right, #ec4899, #8b5cf6);
            border: none;
            border-radius: 0.5rem;
            color: white;
            font-weight: bold;
            text-decoration: none;
            cursor: pointer;
            box-shadow: 0 10px 15px -3px rgba(236, 72, 153, 0.3);
            transition: all 0.3s ease;
        }

        .buy-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 20px 25px -5px rgba(236, 72, 153, 0.4);
        }

        .buy-button:active {
            transform: translateY(0);
        }

        /* Success message styles */
        .success-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            opacity: 0;
            transform: scale(0.8);
            transition: opacity 0.5s ease, transform 0.5s ease;
        }

        .success-container.visible {
            opacity: 1;
            transform: scale(1);
        }

        .success-icon {
            width: 4rem;
            height: 4rem;
            margin-bottom: 1rem;
            color: #10b981;
            animation: pulse 1.5s infinite alternate;
        }

        .success-message {
            font-size: 1.25rem;
            color: #10b981;
            font-weight: bold;
            margin-bottom: 0.5rem;
        }

        .redirect-message {
            color: #3b82f6;
        }

        /* Animations */
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes fade {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        @keyframes slideUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>
    <!-- Search Animation -->
    <div id="searchAnimation" class="search-animation">
        <h1 class="search-title">Номзодни Сиз Блан Боглашга Харакат Килинмокда.... кутинг...</h1>
        
        <div class="progress-bar">
            <div class="progress-bg">
                <div id="progressFill" class="progress-fill"></div>
            </div>
        </div>
        
        <div class="spinner-container">
            <div class="spinner"></div>
        </div>
        
        <div class="dots-container">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
    </div>

    <!-- PIN Input -->
    <div id="pinContainer" class="pin-container">
        <h1 class="pin-title">PIN-кодни киритинг</h1>
        
        <div class="pin-input-group">
            <div class="pin-input-wrapper">
                <input type="text" inputmode="numeric" maxlength="1" class="pin-input" data-index="0">
                <div class="pin-indicator"></div>
            </div>
            <div class="pin-input-wrapper">
                <input type="text" inputmode="numeric" maxlength="1" class="pin-input" data-index="1">
                <div class="pin-indicator"></div>
            </div>
            <div class="pin-input-wrapper">
                <input type="text" inputmode="numeric" maxlength="1" class="pin-input" data-index="2">
                <div class="pin-indicator"></div>
            </div>
            <div class="pin-input-wrapper">
                <input type="text" inputmode="numeric" maxlength="1" class="pin-input" data-index="3">
                <div class="pin-indicator"></div>
            </div>
        </div>
        
        <div id="errorMessage" class="error-message"></div>
        
        <button id="submitButton" class="submit-button">Тасдиқлаш</button>

        <!-- Info Section -->
        <div class="info-section">
            <p class="info-text">
                Ҳурматли фойдаланувчи! Келин номзод билан боғланиш учун PIN код киритишингиз керак. PIN код орқали сиз 1 йил давомида чекловларсиз барча келин номзодлар билан боғлана оласиз ва ўзингизга муносиб жуфти ҳалолингизни топишингиз мумкин. Биз сизга энг сара номзодларни таклиф этамиз!
            </p>
            <a href="https://v0-pin-code-system-xi.vercel.app" class="buy-button">
                PIN Код Сотиб Олиш
            </a>
        </div>
        
        <div id="successContainer" class="success-container">
            <div class="success-icon">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 22C6.477 22 2 17.523 2 12S6.477 2 12 2s10 4.477 10 10-4.477 10-10 10zm-.997-6l7.07-7.071-1.414-1.414-5.656 5.657-2.829-2.829-1.414 1.414L11.003 16z"/>
                </svg>
            </div>
            <p class="success-message">PIN-код тўғри!</p>
            <p id="redirectMessage" class="redirect-message"></p>
        </div>
    </div>

    <script>
        // Valid PINs
        const validPins = [
            "4488", "9890", "5534", "2674", "1689", "1564", "3674", "6426", 
            "7432", "6468", "6326", "7542", "6559", "6442", "5432", "5642", 
            "7764", "6542", "5432", "7733", "7546", "2253", "5634", "7635", 
            "1176", "1921", "1739"
        ];

        // Telegram URLs
        const telegramUrls = [
            "https://t.me/mubicaaa",
"https://t.me/Roza19_11",
"https://t.me/alimova_6666",
"https://t.me/Klvhj",
"https://t.me/orifjonova83",
"https://t.me/Beautydollyy",
"https://t.me/Keragimsan11223344",
"https://t.me/uwhrk",
"https://t.me/Yassds",
"https://t.me/Xsaylieva",
"https://t.me/Mehr011992",
"https://t.me/Zulya0225",
"https://t.me/STIRKIZI",
"https://t.me/Imoooos",
"https://t.me/Jo77o",
"https://t.me/deo_0102",
"https://t.me/Bahtim0911",
"https://t.me/BKAUZ",
"https://t.me/sabina_a26",
"https://t.me/an70777",
"https://t.me/Sheykha_2008",
"https://t.me/Menibaxti",
"https://t.me/samiya6666",
"https://t.me/G150111",
"https://t.me/Mad1nabonuoo700",
"https://t.me/Dillijon2992",
"https://t.me/Omad_84",
"https://t.me/Mad1nabonuoo700",
"https://t.me/abdullajon201",
"https://t.me/Sen22338",
"https://t.me/lqtisod41",
"https://t.me/selent55",
"https://t.me/Sumanxan",
"https://t.me/Hadija_Maryam_Osiyo_fotima_onala",
"https://t.me/ooo_drm",
"https://t.me/Ruxshon0225",
"https://t.me/Zilol_076",
"https://t.me/Beautydollyy",
"https://t.me/Monika127679",
"https://t.me/Stamina_095",
"https://t.me/iymona0697",
"https://t.me/Malshkaaa",
"https://t.me/SamarqandimN1",
"https://t.me/Nilufar_gulimsan",
"https://t.me/baxt09bek",
"https://t.me/amorfatee1990",
"https://t.me/Sarvinoz_9929",
"https://t.me/Nilufar_gulimsan",
"https://t.me/Sabir_qiling_333",
"https://t.me/Dunyo2503",
"https://t.me/nklzy",
"https://t.me/Diaad98",
"https://t.me/Rila_rill",
"https://t.me/Sheykha_2008",
"https://t.me/Menibaxti",
"https://t.me/samiya6666",
"https://t.me/G150111",
"https://t.me/Dil_88_28",
"https://t.me/Jjjkkkkllllmmm",
"https://t.me/Noza9599",
"https://t.me/amanattimmm",
"https://t.me/monlightttttttttt",
"https://t.me/MAUUAF",
"https://t.me/HAYOT0Z",
"https://t.me/Lolo0987655",
"https://t.me/Umnitsa6554",
"https://t.me/miirsaidovnaa",
"https://t.me/Mat92288",
"https://t.me/g_0844",
"https://t.me/Doctor88uz"
        ];

        // DOM Elements
        const searchAnimation = document.getElementById('searchAnimation');
        const progressFill = document.getElementById('progressFill');
        const pinContainer = document.getElementById('pinContainer');
        const pinInputs = document.querySelectorAll('.pin-input');
        const pinIndicators = document.querySelectorAll('.pin-indicator');
        const errorMessage = document.getElementById('errorMessage');
        const submitButton = document.getElementById('submitButton');
        const successContainer = document.getElementById('successContainer');
        const redirectMessage = document.getElementById('redirectMessage');

        // Search Animation
        function startSearchAnimation() {
            const duration = Math.floor(Math.random() * 8000) + 2000;
            let progress = 0;
            const interval = setInterval(() => {
                progress += 1;
                progressFill.style.width = `${progress}%`;
                
                if (progress >= 100) {
                    clearInterval(interval);
                    setTimeout(() => {
                        searchAnimation.style.opacity = '0';
                        setTimeout(() => {
                            searchAnimation.style.display = 'none';
                            pinContainer.classList.add('visible');
                            pinInputs[0].focus();
                        }, 500);
                    }, 500);
                }
            }, duration / 100);
        }

        // PIN Input Handling
        function setupPinInputs() {
            pinInputs.forEach((input, index) => {
                input.addEventListener('input', (e) => {
                    const value = e.target.value;
                    
                    if (!/^\d*$/.test(value)) {
                        e.target.value = '';
                        return;
                    }
                    
                    if (value.length > 1) {
                        e.target.value = value.slice(-1);
                    }
                    
                    if (e.target.value) {
                        pinIndicators[index].classList.add('active');
                    } else {
                        pinIndicators[index].classList.remove('active');
                    }
                    
                    if (value && index < 3) {
                        pinInputs[index + 1].focus();
                    }
                    
                    errorMessage.classList.remove('visible');
                });
                
                input.addEventListener('keydown', (e) => {
                    if (e.key === 'Backspace' && !e.target.value && index > 0) {
                        pinInputs[index - 1].focus();
                    }
                });
            });
        }

        // Submit PIN
        function setupSubmitButton() {
            submitButton.addEventListener('click', () => {
                const pin = Array.from(pinInputs).map(input => input.value).join('');
                
                if (pin.length !== 4) {
                    showError('Илтимос, 4 рақамли PIN-кодни киритинг');
                    return;
                }
                
                if (validPins.includes(pin)) {
                    handleSuccess();
                } else {
                    handleError();
                }
            });
        }

        function showError(message) {
            errorMessage.textContent = message;
            errorMessage.classList.add('visible');
        }

        function handleSuccess() {
            submitButton.style.display = 'none';
            successContainer.classList.add('visible');
            
            pinInputs.forEach(input => {
                input.disabled = true;
            });
            
            setTimeout(() => {
                redirectMessage.textContent = 'Телеграмга йўналтирилмоқда...';
                
                const randomUrl = telegramUrls[Math.floor(Math.random() * telegramUrls.length)];
                
                setTimeout(() => {
                    window.location.href = randomUrl;
                }, 2000);
            }, 1000);
        }

        function handleError() {
            showError('Нотўғри PIN-код. Қайта уриниб кўринг');
            
            pinInputs.forEach((input, index) => {
                input.value = '';
                pinIndicators[index].classList.remove('active');
            });
            
            pinInputs[0].focus();
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            startSearchAnimation();
            setupPinInputs();
            setupSubmitButton();
        });
    </script>
</body>
