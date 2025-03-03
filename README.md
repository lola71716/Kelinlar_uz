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
            <a href="https://v0-fork-of-uzbek-ai-chat-phi.vercel.app" class="buy-button">
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
            "https://t.me/Guliruxso",
"https://t.me/mubicaaa",
"https://t.me/gipsofilyaa",
"https://t.me/ustozdst",
"https://t.me/Sabina9111",
"https://t.me/Farishtamcha",
"https://t.me/Lave0000",
"https://t.me/DILIIM10",
"https://t.me/amor_fati_1_7",
"https://t.me/mubicaaa",
"https://t.me/shaxrustamovnaoo1",
"https://t.me/SHAIK_244",
"https://t.me/Zeboijahon",
"https://t.me/Zilola_36_37",
"https://t.me/s135789",
"https://t.me/Nazli240",
"https://t.me/Qaysar7778",
"https://t.me/MNBasall",
"https://t.me/mlkm100",
"https://t.me/Lifelongdestination",
"https://t.me/Talabaaaaaaaaaaaaaaaa",
"https://t.me/tmmadin",
"https://t.me/Zilola96_96",
"https://t.me/aklonnn",
"https://t.me/BKAUZ",
"https://t.me/Qaysar7778",
"https://t.me/deo_0102",
"https://t.me/Dilbarim_28",
"https://t.me/allohim_ozinga_shukurrr",
"https://t.me/Dildoram_86",
"https://t.me/LYOMM444555666777888999",
"https://t.me/sevinchka96",
"https://t.me/Uzoqroo_Yur",
"https://t.me/Dilbarim_28",
"https://t.me/black_rose_fn",
"https://t.me/Qaysar7778",
"https://t.me/shaxrustamovnaoo1",
"https://t.me/HGM2804",
"https://t.me/Hasbuhu3",
"https://t.me/black_rose_fn",
"https://t.me/laylo013",
"https://t.me/Muchi_SamarkanskayaN1",
"https://t.me/Janatimsan2016",
"https://t.me/Vvvhffghkkl",
"https://t.me/laylo013",
"https://t.me/Gozal7899",
"https://t.me/nastarim77",
"https://t.me/Jisoo089",
"https://t.me/mubicaaa",
"https://t.me/deo_0102",
"https://t.me/Gozal7899",
"https://t.me/Onam_2020",
"https://t.me/Nargiza1122334455",
"https://t.me/xxxoo11122",
"https://t.me/huzunnnn",
"https://t.me/Zebo_12_28",
"https://t.me/Musulmonlar7778",
"https://t.me/az25901",
"https://t.me/rumayso_002",
"https://t.me/Allohimsangaishonaman",
"https://t.me/sinamataqdir",
"https://t.me/VATANIM81",
"https://t.me/hghg6yt56yt54r",
"https://t.me/isakova1",
"https://t.me/hggf6bvgh65t",
"https://t.me/Taqdirlar123",
"https://t.me/hghg6yter",
"https://t.me/Sjsjshzhsnsns",
"https://t.me/M_4_58",
"https://t.me/Asiraaman",
"https://t.me/xggc67ug5",
"https://t.me/Sheykxa_202",
"https://t.me/Hazon0101",
"https://t.me/anne_annecim",
"https://t.me/Armonim1725",
"https://t.me/+79040219058",
"https://t.me/Muallima786",
"https://t.me/Qs1914",
"https://t.me/Baxtizlab77",
"https://t.me/Bineslady",
"https://t.me/Qulmatovanigora",
"https://t.me/Habibin_11111",
"https://t.me/Bineslady",
"https://t.me/Xayot1311",
"https://t.me/beheppy_55",
"https://t.me/Mexrimsen",
"https://t.me/Ahmadjonn01",
"https://t.me/Maxmud1713",
"https://t.me/WWSH_999999",
"https://t.me/Maxmud1713",
"https://t.me/BDUTSh",
"https://t.me/L917M",
"https://t.me/Tarix0199",
"https://t.me/OORZULAR",
"https://t.me/Sazooooooooo",
"https://t.me/Sazooooooooo",
"https://t.me/dilozorbolmang",
"https://t.me/dunyooo571",
"https://t.me/Allhamdulloh888",
"https://t.me/parizoddddd",
"https://t.me/mmaddiinnamm",
"https://t.me/Xayot1311",
"https://t.me/Bineslady",
"https://t.me/Habibin_11111",
"https://t.me/Qulmatovanigora",
"https://t.me/Baxtizlab77",
"https://t.me/Qs1914",
"https://t.me/Soliq200118",
"https://t.me/Ellaayyyyyyyyyyyy",
"https://t.me/dili_9001",
"https://t.me/Z60040",
"https://t.me/Oxiraatim",
"https://t.me/fulugbekovnaaa98",
"https://t.me/Oylam30",
"https://t.me/tavakkulirabbi",
"https://t.me/Stamina_095",
"https://t.me/Muhammadsodiq05",
"https://t.me/LYOMM444555666777888999",
"https://t.me/Qaysar7778",
"https://t.me/Dillijon2992",
"https://t.me/Gul8889",
"https://t.me/black_rose_fn",
"https://t.me/mmaddiinnamm",
"https://t.me/parizoddddd",
"https://t.me/Allhamdulloh888",
"https://t.me/Osmonimyu",
"https://t.me/dunyooo571",
"https://t.me/dilozorbolmang",
"https://t.me/Sazooooooooo",
"https://t.me/ISBlli",
"https://t.me/loveZRYE",
"https://t.me/mubicaaa",
"https://t.me/Mehr011992",
"https://t.me/RRAAHHMMOONN",
"https://t.me/Maxmud1713",
"https://t.me/aaa87412",
"https://t.me/WWSH_999999",
"https://t.me/Maxmud1713",
"https://t.me/BDUTSh",
"https://t.me/L917M",
"https://t.me/Tarix0199",
"https://t.me/RRAAHHMMOONN",
"https://t.me/Onam_2020",
"https://t.me/az25901",
"https://t.me/rumayso_002",
"https://t.me/Allohimsangaishonaman",
"https://t.me/VATANIM81",
"https://t.me/hghg6yt56yt54r",
"https://t.me/isakova1",
"https://t.me/hggf6bvgh65t",
"https://t.me/Taqdirlar123",
"https://t.me/hghg6yter",
"https://t.me/Sjsjshzhsnsns",
"https://t.me/Dtfgujb",
"https://t.me/Xabibim0222",
"https://t.me/Llll88r",
"https://t.me/hgfftggvvv66",
"https://t.me/hjhgg67uyg",
"https://t.me/yagona123i",
"https://t.me/Maftu89",
"https://t.me/Bonucha93",
"https://t.me/Wow292929",
"https://t.me/yurak1984",
"https://t.me/Xadi0777",
"https://t.me/sinamataqdir",
"https://t.me/skinat_9999",
"https://t.me/Avacado28",
"https://t.me/Zilol_076",
"https://t.me/DILYA_56_56",
"https://t.me/Izora_2004",
"https://t.me/Lifeisstillahead1",
"https://t.me/Fatima_SSaida",
"https://t.me/Wifo_uz",
"https://t.me/Kabatullohn",
"https://t.me/Sevganim_Allohhh",
"https://t.me/La_ilaha_iloloh",
"https://t.me/shahzoda5191",
"https://t.me/ATT61S",
"https://t.me/Azizaqizz",
"https://t.me/ooo_drm",
"https://t.me/Kabatullohn",
"https://t.me/IN1816",
"https://t.me/Zxhio",
"https://t.me/mlkmshm",
"https://t.me/mubicaaa",
"https://t.me/Little_lady21",
"https://t.me/tttttfffffffshoira",
"https://t.me/Omad_84",
"https://t.me/Kabatullohn",
"https://t.me/FM0890",
"https://t.me/Zarina744",
"https://t.me/A_R5676",
"https://t.me/muniGu",
"https://t.me/Muslimova1234",
"https://t.me/shahzodaco",
"https://t.me/safiyya85",
"https://t.me/IN1816",
"https://t.me/SafiyaaaaAahshs",
"https://t.me/LeylaLayli",
"https://t.me/Tyue900",
"https://t.me/LightOffMoon",
"https://t.me/Qaysar_Qizaloq_111",
"https://t.me/sakinatunni",
"https://t.me/nanii88ii",
"https://t.me/DILYA_56_56",
"https://t.me/Hhayo1",
"https://t.me/Fkxnfdofke",
"https://t.me/Mehribon1234",
"https://t.me/aslanka7778",
"https://t.me/asal2904",
"https://t.me/raffae89",
"https://t.me/SHoiraopa7",
"https://t.me/Fza_05",
"https://t.me/yasminka_o5",
"https://t.me/barnow_1317",
"https://t.me/Nodiraaa1144",
"https://t.me/tasodf_560",
"https://t.me/Laylo_hon",
"https://t.me/Fazo111111",
"https://t.me/hctdf7",
"https://t.me/mmbbf_a",
"https://t.me/X_MOXIM",
"https://t.me/Nelll22",
"https://t.me/Yunusova1331",
"https://t.me/Fotima7133",
"https://t.me/Dil_28_28",
"https://t.me/Sumi6361",
"https://t.me/Shirin0110",
"https://t.me/Erkatoyka_1999",
"https://t.me/ddd6367",
"https://t.me/Baxtliman2025",
"https://t.me/Musfira2000",
"https://t.me/nazjed",
"https://t.me/BKAUZ",
"https://t.me/KMMMM022",
"https://t.me/Mimii0707",
"https://t.me/Loooooooooooooveeeeeeeeeeeeeeee",
"https://t.me/sakinatunni",
"https://t.me/Ir_4443",
"https://t.me/Mtttxh",
"https://t.me/AADJ00",
"https://t.me/Nur7668",
"https://t.me/sabina_a26",
"https://t.me/Qadarim_1111",
"https://t.me/MAUUAF",
"https://t.me/sunshinelab",
"https://t.me/jmkkahs",
"https://t.me/GMBJT",
"https://t.me/Lolo0987655",
"https://t.me/sakinatunni",
"https://t.me/Betakrorrrrr",
"https://t.me/Toliba_1111",
"https://t.me/Gulii_1988",
"https://t.me/millaa_milaa",
"https://t.me/Azizovna_234",
"https://t.me/culture773",
"https://t.me/megann6",
"https://t.me/Saida_Saiii",
"https://t.me/ZSFZAI",
"https://t.me/l01l29l",
"https://t.me/Gullililili",
"https://t.me/Stamina_095",
"https://t.me/olloh_sabr",
"https://t.me/FaFatimkaaAaaaaaaa",
"https://t.me/Shosha177M",
"https://t.me/Oishaprensess",
"https://t.me/Shaxzo_8181",
"https://t.me/Imtxonnn",
"https://t.me/Bobomurodova01",
"https://t.me/Dildora8877",
"https://t.me/Sabrligim070",
"https://t.me/Beautydollyy",
"https://t.me/Oyisham_002",
"https://t.me/sabr_11122",
"https://t.me/Maryam_7708",
"https://t.me/Feax_sam",
"https://t.me/Baxtkeldi",
"https://t.me/Fatima_SSaida",
"https://t.me/Wifo_uz",
"https://t.me/Afsona_1467",
"https://t.me/Alhamdulillah202323",
"https://t.me/baxt09bek",
"https://t.me/faya2305",
"https://t.me/Maxmud1713",
"https://t.me/Jannatga_yol_7790",
"https://t.me/Mina1892",
"https://t.me/lonely_0220",
"https://t.me/kuzmunchogim999",
"https://t.me/Sadoqatli_bul",
"https://t.me/nurmatov197628",
"https://t.me/Maftunaa2001y",
"https://t.me/Sa9fi"
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
