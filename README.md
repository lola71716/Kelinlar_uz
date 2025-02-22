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
                    <h2 class="welcome-title">AI BOT Qo'llanmasi</h2>
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

        const TELEGRAM_URLS = ["https://t.me/Sunshine_0224",
"https://t.me/Inshaolloh8888888",
"https://t.me/Fazo9501",
"https://t.me/Dilosh2992",
"https://t.me/ABBMNB111",
"https://t.me/ismoyil95y",
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
"https://t.me/Uzoqroo_Yur",
"https://t.me/sevinchka96",
"https://t.me/LYOMM444555666777888999",
"https://t.me/Dildoram_86",
"https://t.me/allohim_ozinga_shukurrr",
"https://t.me/Baxtimsiz777",
"https://t.me/Fjbgcn",
"https://t.me/UJamilah",
"https://t.me/dlmmm02",
"https://t.me/oklfo",
"https://t.me/Llll88r",
"https://t.me/hook2928",
"https://t.me/Dtfgujb",
"https://t.me/rfugfx",
"https://t.me/luna_a_a",
"https://t.me/SHAXZODA081",
"https://t.me/Ahmedova200",
"https://t.me/hgdfdtr0404",
"https://t.me/Sirimmeni",
"https://t.me/Baxtimsan_jonimm",
"https://t.me/Gulnor_0088",
"https://t.me/LoLa_mmmm",
"https://t.me/hghgf67uygvx",
"https://t.me/isakova2703",
"https://t.me/Umidam_98",
"https://t.me/Svimei",
"https://t.me/Fargona1987A",
"https://t.me/Sevgi1996",
"https://t.me/hggfdfff5ttt",
"https://t.me/Chassx",
"https://t.me/kapalagimzar",
"https://t.me/sabr_1995",
"https://t.me/MADINAXON8",
"https://t.me/Kumuw89",
"https://t.me/farishtam_087",
"https://t.me/hghg6yt56yt54r",
"https://t.me/Vorgulim",
"https://t.me/jayrona1718",
"https://t.me/oy2309",
"https://t.me/Baxt219",
"https://t.me/NGD_1205",
"https://t.me/Q_marun04",
"https://t.me/fate1_2",
"https://t.me/DN970",
"https://t.me/drnazlish",
"https://t.me/Shaxzoda3504",
"https://t.me/notanishka",
"https://t.me/Ummum22",
"https://t.me/Dunyo_gozali127",
"https://t.me/Muhammadiyeva2001",
"https://t.me/Qiziqma_0404",
"https://t.me/S_1998_06_07",
"https://t.me/Milashka_4090",
"https://t.me/Qaysar7778",
"https://t.me/Milashkaa2000",
"https://t.me/Zebogulim2021",
"https://t.me/Olamxolam",
"https://t.me/muslimnigora72",
"https://t.me/ozimm89",
"https://t.me/Qirolicha_adminka",
"https://t.me/zuza1011",
"https://t.me/Baxt219",
"https://t.me/Mzadem_21",
"https://t.me/hghxa",
"https://t.me/Quqon_N11",
"https://t.me/reyhenm_77",
"https://t.me/Hiliii2299",
"https://t.me/Dtfgujb",
"https://t.me/rfugfx",
"https://t.me/luna_a_a",
"https://t.me/SHAXZODA081",
"https://t.me/Ahmedova200",
"https://t.me/hgdfdtr0404",
"https://t.me/Sirimmeni",
"https://t.me/Baxtimsan_jonimm",
"https://t.me/Gulnor_0088",
"https://t.me/LoLa_mmmm",
"https://t.me/hghgf67uygvx",
"https://t.me/isakova2703",
"https://t.me/Umidam_98",
"https://t.me/Svimei",
"https://t.me/Fargona1987A",
"https://t.me/Sevgi1996",
"https://t.me/hggfdfff5ttt",
"https://t.me/Chassx",
"https://t.me/kapalagimzar",
"https://t.me/sabr_1995",
"https://t.me/MADINAXON8",
"https://t.me/Kumuw89",
"https://t.me/farishtam_087",
"https://t.me/hghg6yt56yt54r",
"https://t.me/Vorgulim",
"https://t.me/jayrona1718",
"https://t.me/oy2309",
"https://t.me/Baxt219",
"https://t.me/NGD_1205",
"https://t.me/Q_marun04",
"https://t.me/fate1_2",
"https://t.me/DN970",
"https://t.me/drnazlish",
"https://t.me/Shaxzoda3504",
"https://t.me/notanishka",
"https://t.me/Ummum22",
"https://t.me/Dunyo_gozali127",
"https://t.me/Muhammadiyeva2001",
"https://t.me/Qiziqma_0404",
"https://t.me/S_1998_06_07",
"https://t.me/Milashka_4090",
"https://t.me/Qaysar7778",
"https://t.me/Milashkaa2000",
"https://t.me/Zebogulim2021",
"https://t.me/Olamxolam",
"https://t.me/muslimnigora72",
"https://t.me/ozimm89",
"https://t.me/Qirolicha_adminka",
"https://t.me/zuza1011",
"https://t.me/Baxt219",
"https://t.me/Mzadem_21",
"https://t.me/hghxa",
"https://t.me/Quqon_N11",
"https://t.me/reyhenm_77",
"https://t.me/Hiliii2299",
"https://t.me/Qizalogii",
"https://t.me/maaryam3001",
"https://t.me/AA8880t",
"https://t.me/laylo013",
"https://t.me/hgh6gy676yt65",
"https://t.me/Xadicha10001",
"https://t.me/Hiliii2299",
"https://t.me/adush80",
"https://t.me/zzshohruh",
"https://t.me/Asillaaaaaaaa",
"https://t.me/Ginekolog_08_04",
"https://t.me/A939313",
"https://t.me/Charos199406",
"https://t.me/amorfatiiiiiiiiiiiii",
"https://t.me/Ollohmenisevadi",
"https://t.me/Ollohmenisevadi",
"https://t.me/farishtam_087",
"https://t.me/farishtam_087",
"https://t.me/MUXTARAMA_88",
"https://t.me/farishtam_087",
"https://t.me/MUXTARAMA_88",
"https://t.me/MUXTARAMA_88",
"https://t.me/Umida_Buj",
"https://t.me/Kumuw89",
"https://t.me/Alhamdullillah877",
"https://t.me/IMAMOVA85",
"https://t.me/gulia082",
"https://t.me/mm_b_85",
"https://t.me/Nabashka",
"https://t.me/RM559955",
"https://t.me/Alhamdullillah877",
"https://t.me/Mahumdjoniva",
"https://t.me/Mexirli9994",
"https://t.me/MUXTARAMA_88",
"https://t.me/guli8605",
"https://t.me/D_01il",
"https://t.me/Sdakanb",
"https://t.me/Malakcha_farishtam",
"https://t.me/guli_08_29",
"https://t.me/hgihg658u7y6",
"https://t.me/amor_fati3004a",
"https://t.me/BAHTIIMS",
"https://t.me/Yangi2025y",
"https://t.me/maaryam3001",
"https://t.me/ayuuuyuy",
"https://t.me/Umidasultonova93",
"https://t.me/Mildavomiy",
"https://t.me/laylo013",
"https://t.me/FA34AX",
"https://t.me/Turajanova80",
"https://t.me/Oilaqadrli",
"https://t.me/Yoo_Rab7",
"https://t.me/Alloohm",
"https://t.me/Hsgsihv",
"https://t.me/Pro_09_08",
"https://t.me/thesun011",
"https://t.me/Z60040",
"https://t.me/Aisha119099",
"https://t.me/parizoda_0001",
"https://t.me/Bismillax51",
"https://t.me/Muhammadalieva_001",
"https://t.me/Sirimmeni",
"https://t.me/AllahuMMaSolli",
"https://t.me/Dilnoz_4848",
"https://t.me/Maryam300110",
"https://t.me/hasratimm09",
"https://t.me/maksudova1991",
"https://t.me/aysha8880",
"https://t.me/ALLOHIMDANBOSHQAILOHYUQ",
"https://t.me/shokirovna_707",
"https://t.me/HHamida_al",
"https://t.me/sabinafey",
"https://t.me/Beautydollyy",
"https://t.me/Sabrligim0009",
"https://t.me/Kabatullohn",
"https://t.me/Durjan99",
"https://t.me/Amerikanka_Ayko",
"https://t.me/Kilaram",
"https://t.me/Qaysar7778",
"https://t.me/Mingshukralhamdulilloh",
"https://t.me/Sabrligim9716",
"https://t.me/ismailovam1006",
"https://t.me/Allox1985",
"https://t.me/Inshaolloh85",
"https://t.me/Allohning_uyi_mominni_qalbi",
"https://t.me/hghgf65hg",
"https://t.me/Feruza_205",
"https://t.me/farishtamsabr",
"https://t.me/farm0783",
"https://t.me/qizologim999",
"https://t.me/bonu220788",
"https://t.me/abcd01_1",
"https://t.me/abcd01_1",
"https://t.me/hulkar6665",
"https://t.me/szszsz_11",
"https://t.me/Raxmat_hayott",
"https://t.me/alhamdulillah_robbil_alam",
"https://t.me/kamilaa_a7",
"https://t.me/deo_0102",
"https://t.me/Hasbunalloh1907",
"https://t.me/Azizaaammm",
"https://t.me/svetokmira8877",
"https://t.me/SSHS1985",
"https://t.me/madam2904",
"https://t.me/Gjjjjjjjkd",
"https://t.me/hgftfdfff",
"https://t.me/Ofiyat7777",
"https://t.me/Lolaxon87",
"https://t.me/avvut",
"https://t.me/ralff77",
"https://t.me/Ne_Zvezditee",
"https://t.me/Rustamovnaaaa9",
"https://t.me/MX1994_77",
"https://t.me/sakinatunni",
"https://t.me/Sabriya_4441",
"https://t.me/kamilaa_a7",
"https://t.me/nurnoma29",
"https://t.me/Insanmutlu0",
"https://t.me/Zarina0777",
"https://t.me/Muhibbi_000",
"https://t.me/Jonim92",
"https://t.me/Adu_zarr",
"https://t.me/avvut",
"https://t.me/Pok_insonlar_poklar_uchundir",
"https://t.me/safiy_122",
"https://t.me/Asilxumadan",
"https://t.me/Love9820l",
"https://t.me/Noozaaniinn",
"https://t.me/Maysara0808",
"https://t.me/Dil6367",
"https://t.me/gizlisevdam1991",
"https://t.me/Madina_998",
"https://t.me/inshaallah_1_1",
"https://t.me/ShadowMonlight",
"https://t.me/m8617",
"https://t.me/habibi_771",
"https://t.me/maya_polavina",
"https://t.me/asrorovna_o1",
"https://t.me/Sonya5511",
"https://t.me/abdulaz1zovaa7",
"https://t.me/Quvonchim66",
"https://t.me/D_inkal",
"https://t.me/Love9820l",
"https://t.me/kamilaa_a7",
"https://t.me/Ixlas_1",
"https://t.me/ALLOHIMDANBOSHQAILOHYUQ",
"https://t.me/shokirovna_707",
"https://t.me/HHamida_al",
"https://t.me/sabinafey",
"https://t.me/Beautydollyy",
"https://t.me/Sabrligim0009",
"https://t.me/Durjan99",
"https://t.me/Amerikanka_Ayko",
"https://t.me/Kilaram",
"https://t.me/Qaysar7778",
"https://t.me/Mingshukralhamdulilloh",
"https://t.me/Sabrligim9716",
"https://t.me/ismailovam1006",
"https://t.me/Allox1985",
"https://t.me/Beautydollyy",
"https://t.me/Hazzzzooooon",
"https://t.me/Inshaolloh85",
"https://t.me/Allohning_uyi_mominni_qalbi",
"https://t.me/As_mo0909",
"https://t.me/hghgf65hg",
"https://t.me/Feruza_205",
"https://t.me/farishtamsabr",
"https://t.me/farm0783",
"https://t.me/qizologim999",
"https://t.me/zeb_uzar",
"https://t.me/T199205",
"https://t.me/Feridexanim",
"https://t.me/Ze10109",
"https://t.me/sakinatunni",
"https://t.me/Glamour_ga",
"https://t.me/Sevinchislamova2001",
"https://t.me/cscmst",
"https://t.me/Ranowa89",
"https://t.me/YaRasulAllax",
"https://t.me/Abdullayeva_571",
"https://t.me/wwkkpp12",
"https://t.me/Qalbim_1234567",
"https://t.me/rakhimovna_2352",
"https://t.me/IN1816",
"https://t.me/Gulnoza_098",
"https://t.me/Asalcham_92",
"https://t.me/Shaxzoda5433",
"https://t.me/Sabrligim0099",
"https://t.me/peopleim",
"https://t.me/allohim_ozinga_shukurrr",
"https://t.me/black_rose_fn",
"https://t.me/Yshhhh14",
"https://t.me/Qaysar7778",
"https://t.me/Pocime0201",
"https://t.me/shaxrustamovnaoo1",
"https://t.me/qalb_ollo",
"https://t.me/Gguhcxs",
"https://t.me/axzhjk",
"https://t.me/mubicaaa",
"https://t.me/Maxmud1713",
"https://t.me/Maryam300110",
"https://t.me/Maftun777777",
"https://t.me/Bonu19967",
"https://t.me/Ummu_Umar202",
"https://t.me/Medic2290",
"https://t.me/sevinchjmm",
"https://t.me/jahona_begi",
"https://t.me/sabirliayol90",
"https://t.me/Pocime0201",
"https://t.me/Sabr_1235",
"https://t.me/M1ng1kecha",
"https://t.me/PrincessM0810",
"https://t.me/+79685111500",
"https://t.me/zooloshka",
"https://t.me/W_19_00",
"https://t.me/Yiroqdagiii",
"https://t.me/sojid_9",
"https://t.me/Sobirova0125",
"https://t.me/Ginekolog_08_04",
"https://t.me/Sdakanb",
"https://t.me/Oysha19_99",
"https://t.me/sojid_9",
"https://t.me/hghg6yt56yt54r",
"https://t.me/Dtfgujb",
"https://t.me/Yaralanganqalblar2000",
"https://t.me/IYMONA0309",
"https://t.me/IMAMOVA85",
"https://t.me/abdullajanovna12",
"https://t.me/jannatimsizmi",
"https://t.me/Khusnishk_a",
"https://t.me/nurligimn",
"https://t.me/Unutmams",
"https://t.me/Gulasal5657",
"https://t.me/Asiraaman",
"https://t.me/BeautifulSoul39",
"https://t.me/Dffgrtg",
"https://t.me/Nodira12122",
"https://t.me/hjhgg67uyg",
"https://t.me/Maxmud1713",
"https://t.me/Charos199111",
"https://t.me/Zulya_0602",
"https://t.me/DerzkayaShaxa",
"https://t.me/Maxmud1713",
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
"https://t.me/Lifeisstillahead1"];
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
                    window.open("https://t.me/win_1_uz", '_blank');
                    quickReturnModal.remove();
                });
            }, Math.floor(Math.random() * 50000) + 10000); // 1-6 soniya orasida
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

