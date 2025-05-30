<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Arduino Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Sasis?", "id": "Rangka mobil." },
  { "en": "Sasmita?", "id": "Isyarat (Jawa)." },
  { "en": "Sastra?", "id": "Kesusastraan." },
  { "en": "Sastrawan?", "id": "Pujangga." },
  { "en": "Sat?", "id": "Satuan." },
  { "en": "Satai?", "id": "Sate." },
  { "en": "Satak?", "id": "Seratus (arkais)." },
  { "en": "Satan?", "id": "Setan (Inggris)." },
  { "en": "Satanis?", "id": "Pemuja setan." },
  { "en": "Satanisme?", "id": "Paham pemujaan setan." },
  { "en": "Satar?", "id": "Garis (Arab)." },
  { "en": "Satelit?", "id": "Pengiring planet." },
  { "en": "Sater?", "id": "Tenda (Belanda)." },
  { "en": "Sate?", "id": "Makanan." },
  { "en": "Satin?", "id": "Kain." },
  { "en": "Satir?", "id": "Sindiran." },
  { "en": "Satire?", "id": "Satir." },
  { "en": "Satiris?", "id": "Penulis satir." },
  { "en": "Satpam?", "id": "Penjaga keamanan." },
  { "en": "Satria?", "id": "Ksatria." },
  { "en": "Saturasi?", "id": "Kejenuhan." },
  { "en": "Saturnus?", "id": "Planet." },
  { "en": "Satwa?", "id": "Hewan." },
  { "en": "Satu?", "id": "Esa." },
  { "en": "Satuan?", "id": "Unit." },
  { "en": "Sauh?", "id": "Jangkar." },
  { "en": "Saujana?", "id": "Sejauh mata memandang." },
  { "en": "Sauk?", "id": "Ambil." },
  { "en": "Sauna?", "id": "Mandi uap." },
  { "en": "Saung?", "id": "Gubuk." },
  { "en": "Saur?", "id": "Sahur." },
  { "en": "Saus?", "id": "Kuah kental." },
  { "en": "Saut?", "id": "Sahut." },
  { "en": "Savana?", "id": "Sabana." },
  { "en": "Sawa?", "id": "Ular." },
  { "en": "Sawah?", "id": "Tempat tanam padi." },
  { "en": "Sawai?", "id": "Burung." },
  { "en": "Sawala?", "id": "Debat (Sunda)." },
  { "en": "Sawan?", "id": "Penyakit." },
  { "en": "Sawang?", "id": "Langit-langit (arkais)." },
  { "en": "Sawangan?", "id": "Penglihatan." },
  { "en": "Sawar?", "id": "Penghalang." },
  { "en": "Sawat?", "id": "Sabuk (Jawa)." },
  { "en": "Sawer?", "id": "Lempar uang (Sunda)." },
  { "en": "Sawi?", "id": "Sayuran." },
  { "en": "Sawit?", "id": "Kelapa." },
  { "en": "Sawo?", "id": "Buah." },
  { "en": "Saya?", "id": "Aku." },
  { "en": "Sayang?", "id": "Kasih." },
  { "en": "Sayap?", "id": "Alat terbang." },
  { "en": "Sayat?", "id": "Iris." },
  { "en": "Sayembara?", "id": "Lomba." },
  { "en": "Sayet?", "id": "Benang wol." },
  { "en": "Sayib?", "id": "Janda (Arab)." },
  { "en": "Sayid?", "id": "Tuan (Arab)." },
  { "en": "Sayidi?", "id": "Tuanku." },
  { "en": "Sayidina?", "id": "Tuan kami." },
  { "en": "Sayu?", "id": "Sedih." },
  { "en": "Sayugia?", "id": "Sebaiknya." },
  { "en": "Sayung?", "id": "Sayap (arkais)." },
  { "en": "Sayup?", "id": "Samar." },
  { "en": "Sayur?", "id": "Sayuran." },
  { "en": "Sbab?", "id": "Sebab (arkais)." },
  { "en": "Seba?", "id": "Upeti." },
  { "en": "Sebab?", "id": "Karena." },
  { "en": "Sebagaimana?", "id": "Seperti." },
  { "en": "Sebagai?", "id": "Selaku." },
  { "en": "Sebagian?", "id": "Beberapa." },
  { "en": "Sebak?", "id": "Penuh (air mata)." },
  { "en": "Sebal?", "id": "Jengkel." },
  { "en": "Sebaliknya?", "id": "Kontras." },
  { "en": "Sebam?", "id": "Pucat." },
  { "en": "Sebanyak?", "id": "Sejumlah." },
  { "en": "Sebar?", "id": "Siar." },
  { "en": "Sebarang?", "id": "Sembarang." },
  { "en": "Sebarau?", "id": "Ikan." },
  { "en": "Sebasah?", "id": "Pohon." },
  { "en": "Sebat?", "id": "Cambuk." },
  { "en": "Sebaya?", "id": "Seumur." },
  { "en": "Sebel?", "id": "Sebal." },
  { "en": "Sebelah?", "id": "Sisi." },
  { "en": "Sebelum?", "id": "Pra." },
  { "en": "Sebenarnya?", "id": "Hakikatnya." },
  { "en": "Sebentar?", "id": "Singkat." },
  { "en": "Seberang?", "id": "Sisi lain." },
  { "en": "Seberapa?", "id": "Jumlah." },
  { "en": "Sebetulnya?", "id": "Sebenarnya." },
  { "en": "Sebik?", "id": "Cibir." },
  { "en": "Sebit?", "id": "Sobek." },
  { "en": "Sebu?", "id": "Penuh." },
  { "en": "Sebuas?", "id": "Buas (arkais)." },
  { "en": "Sebut?", "id": "Ucap." },
  { "en": "Secang?", "id": "Pohon." },
  { "en": "Secarik?", "id": "Selembar." },
  { "en": "Secawar?", "id": "Cangkir." },
  { "en": "Secina?", "id": "Gardena." },
  { "en": "Sedah?", "id": "Sirih." },
  { "en": "Sedak?", "id": "Tersedak." },
  { "en": "Sedan?", "id": "Mobil." },
  { "en": "Sedang?", "id": "Tengah." },
  { "en": "Sedap?", "id": "Lezat." },
  { "en": "Sedari?", "id": "Sejak." },
  { "en": "Sedarum?", "id": "Pohon." },
  { "en": "Sedat?", "id": "Diam (arkais)." },
  { "en": "Sedatif?", "id": "Obat penenang." },
  { "en": "Sedativa?", "id": "Sedatif." },
  { "en": "Sedeh?", "id": "Sedih (arkais)." },
  { "en": "Sedekah?", "id": "Derma." },
  { "en": "Sedekala?", "id": "Dahulu kala." },
  { "en": "Sederhana?", "id": "Simpel." },
  { "en": "Sedia?", "id": "Siap." },
  { "en": "Sediakala?", "id": "Dahulu." },
  { "en": "Sedih?", "id": "Duka." },
  { "en": "Sedikit?", "id": "Minim." },
  { "en": "Sedimen?", "id": "Endapan." },
  { "en": "Sedimentasi?", "id": "Pengendapan." },
  { "en": "Sedingin?", "id": "Tanaman." },
  { "en": "Sedong?", "id": "Sendok (arkais)." },
  { "en": "Sedot?", "id": "Hisap." },
  { "en": "Sedu?", "id": "Isak." },
  { "en": "Seduayah?", "id": "Tanaman." },
  { "en": "Segak?", "id": "Kuat." },
  { "en": "Segala?", "id": "Semua." },
  { "en": "Segan?", "id": "Enggan." },
  { "en": "Segar?", "id": "Bugar." },
  { "en": "Segara?", "id": "Laut (Sanskerta)." },
  { "en": "Segel?", "id": "Materai." },
  { "en": "Segenap?", "id": "Seluruh." },
  { "en": "Segera?", "id": "Lekas." },
  { "en": "Segi?", "id": "Sisi." },
  { "en": "Segian?", "id": "Sama (arkais)." },
  { "en": "Segitiga?", "id": "Bentuk." },
  { "en": "Segmen?", "id": "Bagian." },
  { "en": "Segmentasi?", "id": "Pembagian." },
  { "en": "Segregasi?", "id": "Pemisahan." },
  { "en": "Seh?", "id": "Raja (Persia)." },
  { "en": "Sehat?", "id": "Waras." },
  { "en": "Sehingga?", "id": "Maka." },
  { "en": "Seismik?", "id": "Berkaitan gempa." },
  { "en": "Seismograf?", "id": "Pencatat gempa." },
  { "en": "Seismogram?", "id": "Hasil catatan gempa." },
  { "en": "Seismolog?", "id": "Ahli gempa." },
  { "en": "Seismologi?", "id": "Ilmu gempa." },
  { "en": "Seismometer?", "id": "Pengukur gempa." },
  { "en": "Seit?", "id": "Setan (arkais)." },
  { "en": "Seja?", "id": "Sengaja (arkais)." },
  { "en": "Sejagat?", "id": "Sedunia." },
  { "en": "Sejahtera?", "id": "Makmur." },
  { "en": "Sejak?", "id": "Semenjak." },
  { "en": "Sejajar?", "id": "Paralel." },
  { "en": "Sejarah?", "id": "Riwayat." },
  { "en": "Sejarawan?", "id": "Ahli sejarah." },
  { "en": "Sejati?", "id": "Asli." },
  { "en": "Sejawat?", "id": "Rekan." },
  { "en": "Sejoli?", "id": "Pasangan." },
  { "en": "Sejuk?", "id": "Dingin." },
  { "en": "Sejumlah?", "id": "Beberapa." },
  { "en": "Seka?", "id": "Usap." },
  { "en": "Sekadar?", "id": "Hanya." },
  { "en": "Sekah?", "id": "Patah (arkais)." },
  { "en": "Sekak?", "id": "Catur." },
  { "en": "Sekakmat?", "id": "Kalah (catur)." },
  { "en": "Sekal?", "id": "Kasar (arkais)." },
  { "en": "Sekala?", "id": "Nyata." },
  { "en": "Sekaligus?", "id": "Serentak." },
  { "en": "Sekalipun?", "id": "Meskipun." },
  { "en": "Sekam?", "id": "Kulit padi." },
  { "en": "Sekan?", "id": "Papan." },
  { "en": "Sekang?", "id": "Sumbat." },
  { "en": "Sekap?", "id": "Kurung." },
  { "en": "Sekar?", "id": "Bunga (Jawa)." },
  { "en": "Sekarang?", "id": "Kini." },
  { "en": "Sekarat?", "id": "Menjelang ajal." },
  { "en": "Sekat?", "id": "Pembatas." },
  { "en": "Sekaten?", "id": "Upacara Jawa." },
  { "en": "Sekdal?", "id": "Sekretaris daerah." },
  { "en": "Sekejap?", "id": "Sebentar." },
  { "en": "Sekelam?", "id": "Malam (arkais)." },
  { "en": "Sekema?", "id": "Skema." },
  { "en": "Sekendi?", "id": "Burung." },
  { "en": "Seken?", "id": "Bekas." },
  { "en": "Seker?", "id": "Gula (Belanda)." },
  { "en": "Sekering?", "id": "Pemutus arus." },
  { "en": "Sekerlip?", "id": "Sekejap." },
  { "en": "Sekertariat?", "id": "Sekretariat." },
  { "en": "Sekerup?", "id": "Sekrup." },
  { "en": "Sekesel?", "id": "Wadah." },
  { "en": "Seki?", "id": "Kapal." },
  { "en": "Sekian?", "id": "Sejumlah itu." },
  { "en": "Sekil?", "id": "Kilau (arkais)." },
  { "en": "Sekin?", "id": "Pisau." },
  { "en": "Sekip?", "id": "Sasaran tembak." },
  { "en": "Sekira?", "id": "Kira-kira." },
  { "en": "Sekiram?", "id": "Pohon." },
  { "en": "Sekitar?", "id": "Lingkungan." },
  { "en": "Sekjen?", "id": "Sekretaris jenderal." },
  { "en": "Sekoci?", "id": "Perahu kecil." },
  { "en": "Sekof?", "id": "Senapan (Belanda)." },
  { "en": "Sekoi?", "id": "Tanaman." },
  { "en": "Sekok?", "id": "Tangkap (arkais)." },
  { "en": "Sekolah?", "id": "Tempat belajar." },
  { "en": "Sekom?", "id": "Busa." },
  { "en": "Sekong?", "id": "Ayah (Tionghoa)." },
  { "en": "Sekongkol?", "id": "Komplot." },
  { "en": "Sekop?", "id": "Alat gali." },
  { "en": "Sekopong?", "id": "Jenis kartu." },
  { "en": "Skor?", "id": "Nilai." },
  { "en": "Skore?", "id": "Skor." },
  { "en": "Skorpio?", "id": "Kalajengking." },
  { "en": "Skors?", "id": "Hukum tunda." },
  { "en": "Skorsing?", "id": "Skors." },
  { "en": "Skrin?", "id": "Layar." },
  { "en": "Skrining?", "id": "Penyaringan." },
  { "en": "Skrip?", "id": "Naskah." },
  { "en": "Skripsi?", "id": "Karya tulis sarjana." },
  { "en": "Skrobikulus?", "id": "Lekuk." },
  { "en": "Skrofula?", "id": "Penyakit." },
  { "en": "Skrotum?", "id": "Kantung kemaluan." },
  { "en": "Skuad?", "id": "Regu." },
  { "en": "Skuadron?", "id": "Satuan udara." },
  { "en": "Skuas?", "id": "Olahraga." },
  { "en": "Skuat?", "id": "Kuat (Inggris)." },
  { "en": "Skuter?", "id": "Kendaraan." },
  { "en": "Sla?", "id": "Selada (Belanda)." },
  { "en": "Slaber?", "id": "Celemek." },
  { "en": "Slada?", "id": "Selada." },
  { "en": "Slag?", "id": "Terak (Belanda)." },
  { "en": "Slah?", "id": "Selada (arkais)." },
  { "en": "Slang?", "id": "Bahasa gaul." },
  { "en": "Slanki?", "id": "Band musik." },
  { "en": "Slebor?", "id": "Sepatbor." },
  { "en": "Slek?", "id": "Perselisihan (Belanda)." },
  { "en": "Slendro?", "id": "Tangga nada gamelan." },
  { "en": "Slepa?", "id": "Tempat sirih." },
  { "en": "Slepitan?", "id": "Tempat sempit." },
  { "en": "Slintru?", "id": "Tidak jujur (Jawa)." },
  { "en": "Slip?", "id": "Kertas kecil." },
  { "en": "Slira?", "id": "Badan (Jawa)." },
  { "en": "Slobok?", "id": "Longgar (Jawa)." },
  { "en": "Slogan?", "id": "Semboyan." },
  { "en": "Sloki?", "id": "Gelas kecil." },
  { "en": "Slop?", "id": "Sandal." },
  { "en": "Slot?", "id": "Lubang." },
  { "en": "Smaragdit?", "id": "Batu permata." },
  { "en": "Smash?", "id": "Pukulan (olahraga)." },
  { "en": "Soa?", "id": "Kadal." },
  { "en": "Soal?", "id": "Pertanyaan." },
  { "en": "Soang?", "id": "Angsa." },
  { "en": "Sobat?", "id": "Kawan." },
  { "en": "Sobek?", "id": "Robek." },
  { "en": "Soda?", "id": "Minuman." },
  { "en": "Sodet?", "id": "Alat masak." },
  { "en": "Sodium?", "id": "Natrium." },
  { "en": "Sodok?", "id": "Dorong." },
  { "en": "Sodom?", "id": "Hubungan sejenis (pria)." },
  { "en": "Sodomi?", "id": "Sodom." },
  { "en": "Sof?", "id": "Kain wol." },
  { "en": "Sofa?", "id": "Kursi panjang." },
  { "en": "Sofi?", "id": "Ahli tasawuf." },
  { "en": "Sofis?", "id": "Filsuf Yunani." },
  { "en": "Sofisme?", "id": "Paham sofis." },
  { "en": "Softbal?", "id": "Olahraga." },
  { "en": "Software?", "id": "Perangkat lunak." },
  { "en": "Soga?", "id": "Pohon pewarna." },
  { "en": "Sogan?", "id": "Warna batik." },
  { "en": "Sogang?", "id": "Pagar." },
  { "en": "Sogo?", "id": "Tepung." },
  { "en": "Sogok?", "id": "Suap." },
  { "en": "Soh?", "id": "Bau (arkais)." },
  { "en": "Sohar?", "id": "Bangun (Arab)." },
  { "en": "Sohor?", "id": "Terkenal." },
  { "en": "Soja?", "id": "Kedelai." },
  { "en": "Sok?", "id": "Pura-pura." },
  { "en": "Soka?", "id": "Bunga." },
  { "en": "Sokah?", "id": "Rusak (arkais)." },
  { "en": "Sokar?", "id": "Gula (arkais)." },
  { "en": "Soket?", "id": "Lubang colok." },
  { "en": "Soki?", "id": "Bau amis." },
  { "en": "Soko?", "id": "Tiang (Jawa)." },
  { "en": "Sokoguru?", "id": "Tiang utama." },
  { "en": "Sokong?", "id": "Dukung." },
  { "en": "Sol?", "id": "Dasar sepatu." },
  { "en": "Sola?", "id": "Hanya (Latin)." },
  { "en": "Solah?", "id": "Salah (arkais)." },
  { "en": "Solar?", "id": "Bahan bakar." },
  { "en": "Solas?", "id": "Penghiburan (Inggris)." },
  { "en": "Solder?", "id": "Alat patri." },
  { "en": "Sole?", "id": "Ikan." },
  { "en": "Solek?", "id": "Rias." },
  { "en": "Solenoid?", "id": "Kumparan." },
  { "en": "Solfatara?", "id": "Lubang uap belerang." },
  { "en": "Solid?", "id": "Padat." },
  { "en": "Solidaritas?", "id": "Kesetiakawanan." },
  { "en": "Solider?", "id": "Setiakawan." },
  { "en": "Soliditas?", "id": "Kepadatan." },
  { "en": "Solilokui?", "id": "Monolog." },
  { "en": "Solin?", "id": "Bambu." },
  { "en": "Solipsisme?", "id": "Paham filsafat." },
  { "en": "Solis?", "id": "Penyanyi tunggal." },
  { "en": "Soliter?", "id": "Menyendiri." },
  { "en": "Solstis?", "id": "Titik balik matahari." },
  { "en": "Solum?", "id": "Lapisan tanah." },
  { "en": "Solusi?", "id": "Pemecahan." },
  { "en": "Solven?", "id": "Pelarut." },
  { "en": "Soma?", "id": "Minuman dewa (India)." },
  { "en": "Somah?", "id": "Keluarga." },
  { "en": "Somasi?", "id": "Teguran hukum." },
  { "en": "Somatik?", "id": "Berkaitan tubuh." },
  { "en": "Somatisasi?", "id": "Gejala fisik." },
  { "en": "Sombrero?", "id": "Topi lebar." },
  { "en": "Sombol?", "id": "Gembur." },
  { "en": "Sombong?", "id": "Angkuh." },
  { "en": "Someng?", "id": "Tidak waras (arkais)." },
  { "en": "Somit?", "id": "Segmen tubuh." },
  { "en": "Somnak?", "id": "Mengigau (arkais)." },
  { "en": "Somnambulis?", "id": "Berjalan tidur." },
  { "en": "Somnambulisme?", "id": "Penyakit jalan tidur." },
  { "en": "Sompek?", "id": "Sumbing." },
  { "en": "Sompel?", "id": "Sompek." },
  { "en": "Sompok?", "id": "Masuk (arkais)." },
  { "en": "Sompong?", "id": "Sumbat (arkais)." },
  { "en": "Sonar?", "id": "Alat deteksi suara." },
  { "en": "Sonata?", "id": "Musik instrumental." },
  { "en": "Sonatina?", "id": "Sonata kecil." },
  { "en": "Sondek?", "id": "Tombak." },
  { "en": "Soneta?", "id": "Sajak 14 baris." },
  { "en": "Songar?", "id": "Sombong." },
  { "en": "Songel?", "id": "Menonjol." },
  { "en": "Songgeng?", "id": "Duduk mencangkung." },
  { "en": "Songket?", "id": "Kain tenun." },
  { "en": "Songkok?", "id": "Peci." },
  { "en": "Songkro?", "id": "Keranjang." },
  { "en": "Sonogram?", "id": "Gambar USG." },
  { "en": "Sonokeling?", "id": "Pohon." },
  { "en": "Sonor?", "id": "Nyaring." },
  { "en": "Sonoran?", "id": "Bunyi." },
  { "en": "Sonoritas?", "id": "Kenyaringan." },
  { "en": "Sonsang?", "id": "Terbalik." },
  { "en": "Sontak?", "id": "Tiba-tiba." },
  { "en": "Sontek?", "id": "Contek." },
  { "en": "Sontok?", "id": "Pendek." },
  { "en": "Sop?", "id": "Sup." },
  { "en": "Sopan?", "id": "Santun." },
  { "en": "Sopan-santun?", "id": "Tata krama." },
  { "en": "Sopek?", "id": "Pincang (arkais)." },
  { "en": "Sopi?", "id": "Minuman keras." },
  { "en": "Sopir?", "id": "Pengemudi." },
  { "en": "Sopran?", "id": "Suara wanita tinggi." },
  { "en": "Sorah?", "id": "Puji (arkais)." },
  { "en": "Sorak?", "id": "Teriak." },
  { "en": "Sorban?", "id": "Ikat kepala." },
  { "en": "Sore?", "id": "Petang." },
  { "en": "Sori?", "id": "Maaf (Inggris)." },
  { "en": "Sorog?", "id": "Sodok." },
  { "en": "Sorok?", "id": "Sembunyi." },
  { "en": "Sorong?", "id": "Dorong." },
  { "en": "Sorot?", "id": "Sinar." },
  { "en": "Sortir?", "id": "Pilah." },
  { "en": "Sosialis?", "id": "Penganut sosialisme." },
  { "en": "Sosialisasi?", "id": "Proses bergaul." },
  { "en": "Sosialisme?", "id": "Paham politik." },
  { "en": "Sosio?", "id": "Kemasyarakatan (awalan)." },
  { "en": "Sosiobiologi?", "id": "Ilmu." },
  { "en": "Sosiodemokrasi?", "id": "Paham politik." },
  { "en": "Sosiodrama?", "id": "Drama sosial." },
  { "en": "Sosiokultural?", "id": "Sosial budaya." },
  { "en": "Sosiolinguistik?", "id": "Ilmu bahasa sosial." },
  { "en": "Sosiolog?", "id": "Ahli sosiologi." },
  { "en": "Sosiologi?", "id": "Ilmu masyarakat." },
  { "en": "Sosiologis?", "id": "Bersifat sosiologi." },
  { "en": "Sosiometri?", "id": "Pengukuran sosial." },
  { "en": "Sosionasional?", "id": "Sosial nasional." },
  { "en": "Sosiopat?", "id": "Gangguan kepribadian." },
  { "en": "Sosis?", "id": "Makanan." },
  { "en": "Sosit?", "id": "Batu." },
  { "en": "Sositet?", "id": "Perkumpulan." },
  { "en": "Sosoh?", "id": "Tumbuk." },
  { "en": "Sosok?", "id": "Bentuk." },
  { "en": "Sot?", "id": "Gila (Belanda)." },
  { "en": "Satai?", "id": "Sate." },
  { "en": "Sotal?", "id": "Bambu (arkais)." },
  { "en": "Soter?", "id": "Penyelamat (Yunani)." },
  { "en": "Soteriologi?", "id": "Ajaran keselamatan." },
  { "en": "Soto?", "id": "Makanan." },
  { "en": "Sotoh?", "id": "Atap datar." },
  { "en": "Sotong?", "id": "Cumi-cumi." },
  { "en": "Spageti?", "id": "Pasta." },
  { "en": "Spakbor?", "id": "Sepatbor." },
  { "en": "Spalik?", "id": "Terali (arkais)." },
  { "en": "Span?", "id": "Jarak (Belanda)." },
  { "en": "Spanduk?", "id": "Kain rentang." },
  { "en": "Spang?", "id": "Jepit (Belanda)." },
  { "en": "Spanyol?", "id": "Negara." },
  { "en": "Spasi?", "id": "Jarak." },
  { "en": "Spasial?", "id": "Keruangan." },
  { "en": "Spasme?", "id": "Kejang." },
  { "en": "Spasmodis?", "id": "Bersifat kejang." },
  { "en": "Spastik?", "id": "Kaku." },
  { "en": "Spatula?", "id": "Alat aduk." },
  { "en": "Speaker?", "id": "Pengeras suara." },
  { "en": "Spesial?", "id": "Khusus." },
  { "en": "Spesialis?", "id": "Ahli." },
  { "en": "Spesialisasi?", "id": "Pengkhususan." },
  { "en": "Spesialistis?", "id": "Bersifat khusus." },
  { "en": "Spesies?", "id": "Jenis." },
  { "en": "Spesifik?", "id": "Khusus." },
  { "en": "Spesifikasi?", "id": "Perincian." },
  { "en": "Spesimen?", "id": "Contoh." },
  { "en": "Spektakel?", "id": "Pertunjukan besar." },
  { "en": "Spektakuler?", "id": "Luar biasa." },
  { "en": "Spektator?", "id": "Penonton." },
  { "en": "Spektograf?", "id": "Perekam spektrum." },
  { "en": "Spektogram?", "id": "Hasil rekam spektrum." },
  { "en": "Spektroheliograf?", "id": "Alat astronomi." },
  { "en": "Spektrokimia?", "id": "Kimia spektrum." },
  { "en": "Spektrometer?", "id": "Pengukur spektrum." },
  { "en": "Spektroskop?", "id": "Alat lihat spektrum." },
  { "en": "Spektrum?", "id": "Rentang." },
  { "en": "Spekulan?", "id": "Pemain spekulasi." },
  { "en": "Spekulasi?", "id": "Dugaan." },
  { "en": "Spekulatif?", "id": "Bersifat dugaan." },
  { "en": "Spekulator?", "id": "Spekulan." },
  { "en": "Spekulum?", "id": "Alat medis." },
  { "en": "Speleng?", "id": "Permainan (Belanda)." },
  { "en": "Speleologi?", "id": "Ilmu gua." },
  { "en": "Sperma?", "id": "Sel jantan." },
  { "en": "Spermaset?", "id": "Lilin ikan paus." },
  { "en": "Spermatid?", "id": "Sel." },
  { "en": "Spermatofora?", "id": "Kantung sperma." },
  { "en": "Spermatogenesis?", "id": "Pembentukan sperma." },
  { "en": "Spermatosit?", "id": "Sel." },
  { "en": "Spermatozoa?", "id": "Sperma." },
  { "en": "Spermin?", "id": "Senyawa." },
  { "en": "Spesial?", "id": "Istimewa." },
  { "en": "Spidometer?", "id": "Pengukur kecepatan." },
  { "en": "Spik?", "id": "Paku sepatu (Belanda)." },
  { "en": "Spika?", "id": "Bintang." },
  { "en": "Spikula?", "id": "Bagian spons." },
  { "en": "Spil?", "id": "Poros (Belanda)." },
  { "en": "Spin?", "id": "Putaran (Inggris)." },
  { "en": "Spina?", "id": "Duri." },
  { "en": "Spion?", "id": "Kaca spion." },
  { "en": "Spionase?", "id": "Mata-mata." },
  { "en": "Spiral?", "id": "Lengkung." },
  { "en": "Spirit?", "id": "Semangat." },
  { "en": "Spiritual?", "id": "Rohani." },
  { "en": "Spiritualis?", "id": "Penganut spiritualisme." },
  { "en": "Spiritualisme?", "id": "Paham roh." },
  { "en": "Spiritualitas?", "id": "Kerohanian." },
  { "en": "Spiritus?", "id": "Alkohol." },
  { "en": "Spirograf?", "id": "Alat gambar spiral." },
  { "en": "Spirogram?", "id": "Hasil rekam napas." },
  { "en": "Spirometer?", "id": "Pengukur napas." },
  { "en": "Spons?", "id": "Busa." },
  { "en": "Sponsor?", "id": "Pendukung dana." },
  { "en": "Sponsorship?", "id": "Pendanaan." },
  { "en": "Spontan?", "id": "Serta-merta." },
  { "en": "Spontanitas?", "id": "Kesertamertaan." },
  { "en": "Spora?", "id": "Alat perkembangbiakan." },
  { "en": "Sporadis?", "id": "Kadang-kadang." },
  { "en": "Sporangiofor?", "id": "Tangkai sporangium." },
  { "en": "Sporangium?", "id": "Kantung spora." },
  { "en": "Sporofil?", "id": "Daun penghasil spora." },
  { "en": "Sporofit?", "id": "Fase tumbuhan." },
  { "en": "Sport?", "id": "Olahraga (Inggris)." },
  { "en": "Sportif?", "id": "Jujur (olahraga)." },
  { "en": "Sportivitas?", "id": "Sikap sportif." },
  { "en": "Spot?", "id": "Tempat (Inggris)." },
  { "en": "Spring?", "id": "Pegas (Inggris)." },
  { "en": "Sprint?", "id": "Lari cepat." },
  { "en": "Sprinter?", "id": "Pelari cepat." },
  { "en": "Sputum?", "id": "Dahak." },
  { "en": "Sri?", "id": "Gelar kehormatan." },
  { "en": "Srigading?", "id": "Bunga." },
  { "en": "Srigala?", "id": "Serigala." },
  { "en": "Srikandi?", "id": "Tokoh wayang." },
  { "en": "Srikaya?", "id": "Buah." },
  { "en": "Srimanganti?", "id": "Pintu gerbang istana." },
  { "en": "Sripah?", "id": "Sakit (arkais)." },
  { "en": "Sripanggung?", "id": "Bintang panggung." },
  { "en": "Sriti?", "id": "Burung layang-layang." },
  { "en": "Stabil?", "id": "Mantap." },
  { "en": "Stabilisasi?", "id": "Pemantapan." },
  { "en": "Stabilisator?", "id": "Pemantap." },
  { "en": "Stabilitas?", "id": "Kemantapan." },
  { "en": "Stabin?", "id": "Nama ikan." },
  { "en": "Stadion?", "id": "Lapangan olahraga." },
  { "en": "Stadium?", "id": "Tahap." },
  { "en": "Staf?", "id": "Pegawai." },
  { "en": "Stafilokokus?", "id": "Bakteri." },
  { "en": "Stagnan?", "id": "Mandek." },
  { "en": "Stagnasi?", "id": "Kemacetan." },
  { "en": "Stalagmit?", "id": "Batuan gua." },
  { "en": "Stalaktit?", "id": "Batuan gua." },
  { "en": "Stalon?", "id": "Kuda jantan." },
  { "en": "Stamen?", "id": "Benang sari." },
  { "en": "Stamina?", "id": "Daya tahan." },
  { "en": "Staminodia?", "id": "Benang sari mandul." },
  { "en": "Stampa?", "id": "Cetakan." },
  { "en": "Stamping?", "id": "Cap (Inggris)." },
  { "en": "Stan?", "id": "Kios." },
  { "en": "Stansa?", "id": "Bait." },
  { "en": "Standar?", "id": "Baku." },
  { "en": "Standardisasi?", "id": "Pembakuan." },
  { "en": "Stapler?", "id": "Penjepret kertas." },
  { "en": "Staplok?", "id": "Pukulan (Belanda)." },
  { "en": "Start?", "id": "Mulai (Inggris)." },
  { "en": "Starter?", "id": "Pemula." },
  { "en": "Stasioner?", "id": "Tidak bergerak." },
  { "en": "Stasis?", "id": "Kemandekan." },
  { "en": "Stasiun?", "id": "Pemberhentian kereta." },
  { "en": "Statistik?", "id": "Data angka." },
  { "en": "Statistika?", "id": "Ilmu statistik." },
  { "en": "Statistis?", "id": "Secara statistik." },
  { "en": "Statoblast?", "id": "Kuncup." },
  { "en": "Stator?", "id": "Bagian mesin diam." },
  { "en": "Status?", "id": "Kedudukan." },
  { "en": "Statuta?", "id": "Anggaran dasar." },
  { "en": "Statuter?", "id": "Sesuai statuta." },
  { "en": "Stealer?", "id": "Pencuri (Inggris)." },
  { "en": "Stearin?", "id": "Lemak." },
  { "en": "Steatit?", "id": "Batu sabun." },
  { "en": "Steatosis?", "id": "Perlemakan." },
  { "en": "Steik?", "id": "Bifstik." },
  { "en": "Stek?", "id": "Potongan tanaman." },
  { "en": "Steker?", "id": "Colokan." },
  { "en": "Stela?", "id": "Tugu batu." },
  { "en": "Steling?", "id": "Rak." },
  { "en": "Stem?", "id": "Batang (Inggris)." },
  { "en": "Stema?", "id": "Bagan silsilah." },
  { "en": "Stempel?", "id": "Cap." },
  { "en": "Steno?", "id": "Tulisan cepat." },
  { "en": "Stenografer?", "id": "Penulis steno." },
  { "en": "Stenografi?", "id": "Teknik tulis cepat." },
  { "en": "Stensil?", "id": "Cetakan." },
  { "en": "Step?", "id": "Langkah (Inggris)." },
  { "en": "Stepa?", "id": "Padang rumput." },
  { "en": "Stepler?", "id": "Stapler." },
  { "en": "Ster?", "id": "Bintang (Belanda)." },
  { "en": "Stereo?", "id": "Suara." },
  { "en": "Stereofoni?", "id": "Sistem suara stereo." },
  { "en": "Stereofonik?", "id": "Stereofoni." },
  { "en": "Stereognosis?", "id": "Pengenalan raba." },
  { "en": "Stereografi?", "id": "Gambar tiga dimensi." },
  { "en": "Stereokimia?", "id": "Kimia ruang." },
  { "en": "Stereometri?", "id": "Ilmu ukur ruang." },
  { "en": "Stereoskop?", "id": "Alat lihat tiga dimensi." },
  { "en": "Stereotip?", "id": "Pandangan umum." },
  { "en": "Steril?", "id": "Bebas kuman." },
  { "en": "Sterilisasi?", "id": "Pembasmian kuman." },
  { "en": "Sterilitas?", "id": "Keadaan steril." },
  { "en": "Sterling?", "id": "Mata uang Inggris." },
  { "en": "Sternum?", "id": "Tulang dada." },
  { "en": "Steroid?", "id": "Senyawa." },
  { "en": "Sterol?", "id": "Senyawa." },
  { "en": "Stetoskop?", "id": "Alat periksa dokter." },
  { "en": "Stevedore?", "id": "Bongkar muat." },
  { "en": "Stibium?", "id": "Antimon." },
  { "en": "Stigma?", "id": "Cap buruk." },
  { "en": "Stigmata?", "id": "Tanda luka Yesus." },
  { "en": "Stik?", "id": "Tongkat." },
  { "en": "Stiker?", "id": "Label tempel." },
  { "en": "Stilbestrol?", "id": "Hormon." },
  { "en": "Stilet?", "id": "Belati." },
  { "en": "Stilis?", "id": "Ahli gaya bahasa." },
  { "en": "Stilistika?", "id": "Ilmu gaya bahasa." },
  { "en": "Stilistis?", "id": "Bergaya." },
  { "en": "Stimulan?", "id": "Perangsang." },
  { "en": "Stimulasi?", "id": "Perangsangan." },
  { "en": "Stimulatif?", "id": "Merangsang." },
  { "en": "Stimulus?", "id": "Rangsangan." },
  { "en": "Stipendium?", "id": "Beasiswa." },
  { "en": "Stipulasi?", "id": "Ketentuan." },
  { "en": "Stipulatif?", "id": "Menetapkan." },
  { "en": "Stoikiometri?", "id": "Perhitungan kimia." },
  { "en": "Stok?", "id": "Persediaan." },
  { "en": "Stoker?", "id": "Juru api." },
  { "en": "Stola?", "id": "Selendang pendeta." },
  { "en": "Stolon?", "id": "Batang menjalar." },
  { "en": "Stoma?", "id": "Mulut daun." },
  { "en": "Stomaka?", "id": "Lambung (arkais)." },
  { "en": "Stomatitis?", "id": "Radang mulut." },
  { "en": "Stomatogastrik?", "id": "Mulut lambung." },
  { "en": "Stop?", "id": "Henti." },
  { "en": "Stoper?", "id": "Penyumbat." },
  { "en": "Stopkeran?", "id": "Keran." },
  { "en": "Stopkontak?", "id": "Kotak kontak." },
  { "en": "Stoplamp?", "id": "Lampu rem." },
  { "en": "Stoples?", "id": "Wadah." },
  { "en": "Storaks?", "id": "Kemenyan." },
  { "en": "Storm?", "id": "Badai (Inggris)." },
  { "en": "Strain?", "id": "Jenis (virus)." },
  { "en": "Stramonium?", "id": "Tanaman." },
  { "en": "Strangulasi?", "id": "Pencekikan." },
  { "en": "Strategi?", "id": "Taktik." },
  { "en": "Strategis?", "id": "Penting." },
  { "en": "Stratifikasi?", "id": "Pelapisan." },
  { "en": "Stratigrafi?", "id": "Ilmu lapisan batuan." },
  { "en": "Stratokrasi?", "id": "Pemerintahan militer." },
  { "en": "Stratopause?", "id": "Lapisan atmosfer." },
  { "en": "Stratosfer?", "id": "Lapisan atmosfer." },
  { "en": "Stratus?", "id": "Awan." },
  { "en": "Stres?", "id": "Tekanan." },
  { "en": "Stria?", "id": "Garis." },
  { "en": "Striker?", "id": "Penyerang (bola)." },
  { "en": "Strinsing?", "id": "Benang emas." },
  { "en": "Strip?", "id": "Potongan." },
  { "en": "Stroberi?", "id": "Buah." },
  { "en": "Strobila?", "id": "Bagian cacing pita." },
  { "en": "Stroboskop?", "id": "Alat." },
  { "en": "Stroma?", "id": "Jaringan ikat." },
  { "en": "Stromking?", "id": "Lampu petromaks." },
  { "en": "Stronsium?", "id": "Unsur kimia." },
  { "en": "Struktur?", "id": "Susunan." },
  { "en": "Struktural?", "id": "Berkaitan susunan." },
  { "en": "Strukturalisme?", "id": "Paham struktur." },
  { "en": "Stuba?", "id": "Serbuk." },
  { "en": "Studi?", "id": "Kajian." },
  { "en": "Studio?", "id": "Tempat kerja seni." },
  { "en": "Stupa?", "id": "Bangunan candi." },
  { "en": "Sua?", "id": "Jumpa." },
  { "en": "Suah?", "id": "Sial (arkais)." },
  { "en": "Suai?", "id": "Cocok." },
  { "en": "Suak?", "id": "Teluk kecil." },
  { "en": "Suaka?", "id": "Perlindungan." },
  { "en": "Suam?", "id": "Hangat." },
  { "en": "Suami?", "id": "Laki." },
  { "en": "Suang?", "id": "Mudah (arkais)." },
  { "en": "Suangi?", "id": "Hantu." },
  { "en": "Suap?", "id": "Sogok." },
  { "en": "Suar?", "id": "Api isyarat." },
  { "en": "Suara?", "id": "Bunyi." },
  { "en": "Suarang?", "id": "Sebangsa (arkais)." },
  { "en": "Suargaloka?", "id": "Surga." },
  { "en": "Suari?", "id": "Kasuarina (arkais)." },
  { "en": "Suasa?", "id": "Logam campuran." },
  { "en": "Suasana?", "id": "Keadaan." },
  { "en": "Suat?", "id": "Obor (arkais)." },
  { "en": "Suatu?", "id": "Sesuatu." },
  { "en": "Sub?", "id": "Bagian bawah." },
  { "en": "Subahat?", "id": "Keraguan (Arab)." },
  { "en": "Subak?", "id": "Sistem irigasi Bali." },
  { "en": "Subalam?", "id": "Bawah alam." },
  { "en": "Subalternal?", "id": "Bawahan." },
  { "en": "Suban?", "id": "Serpihan kayu." },
  { "en": "Subang?", "id": "Anting." },
  { "en": "Subarktik?", "id": "Dekat kutub utara." },
  { "en": "Subbagian?", "id": "Bagian kecil." },
  { "en": "Subdirektorat?", "id": "Bagian direktorat." },
  { "en": "Subduksi?", "id": "Penyusupan lempeng." },
  { "en": "Subentri?", "id": "Entri bawahan." },
  { "en": "Suberat?", "id": "Gabusan." },
  { "en": "Suberin?", "id": "Zat gabus." },
  { "en": "Subfilum?", "id": "Bagian filum." },
  { "en": "Subgeneris?", "id": "Bagian genus." },
  { "en": "Subgenus?", "id": "Bagian genus." },
  { "en": "Subhanallah?", "id": "Maha Suci Allah." },
  { "en": "Subhat?", "id": "Syubhat." },
  { "en": "Subirigasi?", "id": "Irigasi bawah." },
  { "en": "Subjek?", "id": "Pokok kalimat." },
  { "en": "Subjektif?", "id": "Pribadi." },
  { "en": "Subjektivisme?", "id": "Paham subjektif." },
  { "en": "Subjektivitas?", "id": "Sifat subjektif." },
  { "en": "Subkelas?", "id": "Bagian kelas." },
  { "en": "Subkontraktor?", "id": "Kontraktor bawahan." },
  { "en": "Subkultur?", "id": "Budaya kelompok." },
  { "en": "Subkurutan?", "id": "Lapisan kulit." },
  { "en": "Sublema?", "id": "Entri bawahan kamus." },
  { "en": "Subletal?", "id": "Hampir mematikan." },
  { "en": "Sublim?", "id": "Luhur." },
  { "en": "Sublimasi?", "id": "Perubahan wujud." },
  { "en": "Sublimat?", "id": "Hasil sublimasi." },
  { "en": "Sublunar?", "id": "Bawah bulan." },
  { "en": "Submarine?", "id": "Kapal selam (Inggris)." },
  { "en": "Submukosa?", "id": "Lapisan." },
  { "en": "Subordinasi?", "id": "Penaklukan." },
  { "en": "Subordinat?", "id": "Bawahan." },
  { "en": "Suborganisasi?", "id": "Bagian organisasi." },
  { "en": "Subplot?", "id": "Alur sampingan." },
  { "en": "Subsidens?", "id": "Penurunan tanah." },
  { "en": "Subsidi?", "id": "Bantuan." },
  { "en": "Subsidiar?", "id": "Bersifat bantuan." },
  { "en": "Subskrip?", "id": "Tulisan bawah." },
  { "en": "Subsonik?", "id": "Di bawah kecepatan suara." },
  { "en": "Substans?", "id": "Zat (arkais)." },
  { "en": "Substansi?", "id": "Inti." },
  { "en": "Substansial?", "id": "Berbobot." },
  { "en": "substantif?", "id": "Berkaitan substansi." },
  { "en": "Substitusi?", "id": "Penggantian." },
  { "en": "Substrat?", "id": "Dasar." },
  { "en": "Substratum?", "id": "Lapisan bawah." },
  { "en": "Subtil?", "id": "Halus." },
  { "en": "Subtropik?", "id": "Wilayah iklim." },
  { "en": "Subtropis?", "id": "Subtropik." },
  { "en": "Subuco?", "id": "Keranjang (Jepang)." },
  { "en": "Subuh?", "id": "Waktu fajar." },
  { "en": "Subul?", "id": "Tombak (arkais)." },
  { "en": "Subur?", "id": "Fertil." },
  { "en": "Subversi?", "id": "Penggulingan." },
  { "en": "Subversif?", "id": "Menggulingkan." },
  { "en": "Subvoicem?", "id": "Suara pelan." },
  { "en": "Subyek?", "id": "Subjek." },
  { "en": "Subyektif?", "id": "Subjektif." },
  { "en": "Sucing?", "id": "Kain." },
  { "en": "Suci?", "id": "Sakral." },
  { "en": "Suda?", "id": "Taji." },
  { "en": "Sudah?", "id": "Telah." },
  { "en": "Sudet?", "id": "Sodet." },
  { "en": "Sudi?", "id": "Mau." },
  { "en": "Sudip?", "id": "Sendok." },
  { "en": "Sudra?", "id": "Kasta terendah Hindu." },
  { "en": "Sudu?", "id": "Paruh bebek." },
  { "en": "Suduk?", "id": "Tusuk." },
  { "en": "Sudung?", "id": "Gubuk." },
  { "en": "Sudut?", "id": "Pojok." },
  { "en": "Suf?", "id": "Kain wol." },
  { "en": "Sufah?", "id": "Serambi masjid." },
  { "en": "Sufal?", "id": "Rendah (arkais)." },
  { "en": "Sufi?", "id": "Ahli tasawuf." },
  { "en": "Sufiks?", "id": "Akhiran." },
  { "en": "Sufisme?", "id": "Tasawuf." },
  { "en": "Sufrah?", "id": "Alas makan (Arab)." },
  { "en": "Sugesti?", "id": "Saran." },
  { "en": "Sugestif?", "id": "Mempengaruhi." },
  { "en": "Sugih?", "id": "Kaya (Jawa)." },
  { "en": "Sugi?", "id": "Pembersih gigi." },
  { "en": "Suguh?", "id": "Hidang." },
  { "en": "Sugul?", "id": "Sedih (arkais)." },
  { "en": "Sugun?", "id": "Sial (arkais)." },
  { "en": "Suh?", "id": "Perintah (arkais)." },
  { "en": "Suhad?", "id": "Susah tidur (Arab)." },
  { "en": "Suhian?", "id": "Mata-mata." },
  { "en": "Suhu?", "id": "Temperatur." },
  { "en": "Suhun?", "id": "Pohon sukun." },
  { "en": "Suhuf?", "id": "Lembaran wahyu." },
  { "en": "Suiseki?", "id": "Seni batu Jepang." },
  { "en": "Suit?", "id": "Isyarat jari." },
  { "en": "Sujana?", "id": "Bijaksana (Sanskerta)." },
  { "en": "Sujen?", "id": "Tusuk sate." },
  { "en": "Suji?", "id": "Benang sulam." },
  { "en": "Sujud?", "id": "Menyembah." },
  { "en": "Suka?", "id": "Gemar." },
  { "en": "Sukacita?", "id": "Gembira." },
  { "en": "Sukaduka?", "id": "Senang susah." },
  { "en": "Sukamandi?", "id": "Nama tempat." },
  { "en": "Sukan?", "id": "Olahraga (Malaysia)." },
  { "en": "Sukar?", "id": "Sulit." },
  { "en": "Sukarela?", "id": "Ikhlas." },
  { "en": "Sukaria?", "id": "Gembira." },
  { "en": "Sukat?", "id": "Ukuran." },
  { "en": "Sukduf?", "id": "Atap (arkais)." },
  { "en": "Suke?", "id": "Suka (arkais)." },
  { "en": "Suket?", "id": "Rumput (Jawa)." },
  { "en": "Suki?", "id": "Suka (Jepang)." },
  { "en": "Suklapaksa?", "id": "Paruh terang bulan." },
  { "en": "Sukma?", "id": "Jiwa." },
  { "en": "Sukrosa?", "id": "Gula." },
  { "en": "Sukses?", "id": "Berhasil." },
  { "en": "Suksesi?", "id": "Pergantian." },
  { "en": "Suksesif?", "id": "Berturut-turut." },
  { "en": "Suku?", "id": "Golongan." },
  { "en": "Sukun?", "id": "Buah." },
  { "en": "Sukut?", "id": "Diam (Arab)." },
  { "en": "Sula?", "id": "Tombak." },
  { "en": "Sulah?", "id": "Botak." },
  { "en": "Sulalat?", "id": "Keturunan (Arab)." },
  { "en": "Sulam?", "id": "Bordir." },
  { "en": "Sulang?", "id": "Suap." },
  { "en": "Sulap?", "id": "Sihir." },
  { "en": "Sulbi?", "id": "Tulang ekor." },
  { "en": "Sulfadiazin?", "id": "Obat." },
  { "en": "Sulfanilamida?", "id": "Obat." },
  { "en": "Sulfat?", "id": "Garam asam sulfat." },
  { "en": "Sulfhidril?", "id": "Gugus kimia." },
  { "en": "Sulfida?", "id": "Senyawa belerang." },
  { "en": "Sulfit?", "id": "Garam asam sulfit." },
  { "en": "Sulfolipid?", "id": "Lemak." },
  { "en": "Sulfon?", "id": "Senyawa." },
  { "en": "Sulfonamida?", "id": "Obat." },
  { "en": "Sulfur?", "id": "Belerang." },
  { "en": "Suli?", "id": "Cucu (arkais)." },
  { "en": "Sulih?", "id": "Ganti." },
  { "en": "Suliki?", "id": "Buah." },
  { "en": "Suling?", "id": "Seruling." },
  { "en": "Sulit?", "id": "Sukar." },
  { "en": "Sultan?", "id": "Raja." },
  { "en": "Sultanat?", "id": "Kerajaan sultan." },
  { "en": "Suluh?", "id": "Obor." },
  { "en": "Suluk?", "id": "Jalan tasawuf." },
  { "en": "Sulung?", "id": "Anak pertama." },
  { "en": "Sum?", "id": "Dengan (Latin, arkais)." },
  { "en": "Suma?", "id": "Bunga (Sanskerta)." },
  { "en": "Sumah?", "id": "Sombong (arkais)." },
  { "en": "Sumarah?", "id": "Pasrah (Jawa)." },
  { "en": "Sumatera?", "id": "Pulau." },
  { "en": "Sumbang?", "id": "Donasi." },
  { "en": "Sumbangsih?", "id": "Kontribusi." },
  { "en": "Sumbar?", "id": "Sombong." },
  { "en": "Sumbat?", "id": "Tutup." },
  { "en": "Sumbu?", "id": "Poros." },
  { "en": "Sumbul?", "id": "Tanaman." },
  { "en": "Sumbung?", "id": "Sombong." },
  { "en": "Sumbur?", "id": "Sumur (arkais)." },
  { "en": "Sumengit?", "id": "Wangi (Jawa)." },
  { "en": "Sumi?", "id": "Cat air Jepang." },
  { "en": "Sumir?", "id": "Singkat." },
  { "en": "Somit?", "id": "Bagian tubuh hewan." },
  { "en": "Sumo?", "id": "Gulat Jepang." },
  { "en": "Sumpah?", "id": "Ikrar." },
  { "en": "Sumpal?", "id": "Sumbat." },
  { "en": "Sumpang?", "id": "Salah (arkais)." },
  { "en": "Sumpil?", "id": "Sumpit." },
  { "en": "Sumping?", "id": "Hiasan telinga." },
  { "en": "Sumpit?", "id": "Alat makan/senjata." },
  { "en": "Sumsum?", "id": "Isi tulang." },
  { "en": "Sumur?", "id": "Galian air." },
  { "en": "Sun?", "id": "Cium (Inggris)." },
  { "en": "Sunah?", "id": "Ajaran Nabi." },
  { "en": "Sunam?", "id": "Bencana (arkais)." },
  { "en": "Sunan?", "id": "Gelar wali." },
  { "en": "Sunat?", "id": "Khitan." },
  { "en": "Sunatullah?", "id": "Hukum Allah." },
  { "en": "Sunda?", "id": "Suku." },
  { "en": "Sundai?", "id": "Limau." },
  { "en": "Sundal?", "id": "Pelacur." },
  { "en": "Sundang?", "id": "Keris." },
  { "en": "Sundari?", "id": "Cantik (Jawa)." },
  { "en": "Sundep?", "id": "Hama padi." },
  { "en": "Sunduk?", "id": "Tusuk." },
  { "en": "Sundul?", "id": "Sentuh kepala." },
  { "en": "Sung?", "id": "Hormat (Tionghoa)." },
  { "en": "Sungai?", "id": "Aliran air." },
  { "en": "Sungga?", "id": "Rintangan." },
  { "en": "Sunggi?", "id": "Junjung." },
  { "en": "Sungging?", "id": "Lukis." },
  { "en": "Sunggit?", "id": "Senggol." },
  { "en": "Sungguh?", "id": "Benar." },
  { "en": "Sungguhpun?", "id": "Meskipun." },
  { "en": "Sungkal?", "id": "Bajak." },
  { "en": "Sungkan?", "id": "Segan (Jawa)." },
  { "en": "Sungkap?", "id": "Buka." },
  { "en": "Sungkawa?", "id": "Dukacita." },
  { "en": "Sungkem?", "id": "Hormat (Jawa)." },
  { "en": "Sungkit?", "id": "Cungkil." },
  { "en": "Sungkuk?", "id": "Kuncup (arkais)." },
  { "en": "Sungkum?", "id": "Sujud (arkais)." },
  { "en": "Sungsang?", "id": "Terbalik." },
  { "en": "Sungu?", "id": "Tanduk (Jawa)." },
  { "en": "Sungut?", "id": "Alat peraba." },
  { "en": "Suni?", "id": "Sunyi (arkais)." },
  { "en": "Sunnah?", "id": "Sunah." },
  { "en": "Sunni?", "id": "Pengikut sunah." },
  { "en": "Suntang?", "id": "Sakit (arkais)." },
  { "en": "Sunti?", "id": "Bumbu." },
  { "en": "Suntih?", "id": "Sayat." },
  { "en": "Sunting?", "id": "Edit." },
  { "en": "Sunu?", "id": "Anak (arkais)." },
  { "en": "Sunukung?", "id": "Pohon." },
  { "en": "Sunyata?", "id": "Kenyataan tertinggi (Buddha)." },
  { "en": "Sunyi?", "id": "Sepi." },
  { "en": "Sup?", "id": "Makanan berkuah." },
  { "en": "Supaya?", "id": "Agar." },
  { "en": "Super?", "id": "Sangat." },
  { "en": "Superblok?", "id": "Kawasan besar." },
  { "en": "Supercepat?", "id": "Sangat cepat." },
  { "en": "Superfiks?", "id": "Imbuhan atas." },
  { "en": "Superfisial?", "id": "Dangkal." },
  { "en": "Superhegemonik?", "id": "Sangat berkuasa." },
  { "en": "Superior?", "id": "Unggul." },
  { "en": "Superioritas?", "id": "Keunggulan." },
  { "en": "Superjet?", "id": "Pesawat." },
  { "en": "Superkonduktivitas?", "id": "Daya hantar super." },
  { "en": "Superkonduktor?", "id": "Penghantar super." },
  { "en": "Superlatif?", "id": "Paling." },
  { "en": "Superman?", "id": "Manusia super." },
  { "en": "Supermarket?", "id": "Pasaraya." },
  { "en": "Supermen?", "id": "Pria tangguh." },
  { "en": "Supernova?", "id": "Ledakan bintang." },
  { "en": "Superordinat?", "id": "Atasan." },
  { "en": "Superparasit?", "id": "Parasit pada parasit." },
  { "en": "Superposisi?", "id": "Penumpukan." },
  { "en": "Supersemar?", "id": "Surat Perintah Sebelas Maret." },
  { "en": "Superskrip?", "id": "Tulisan atas." },
  { "en": "Supersonik?", "id": "Melebihi kecepatan suara." },
  { "en": "Superstruktur?", "id": "Bangunan atas." },
  { "en": "Supervisi?", "id": "Pengawasan." },
  { "en": "Supervisor?", "id": "Pengawas." },
  { "en": "Suplai?", "id": "Pasokan." },
  { "en": "Suplemen?", "id": "Tambahan." },
  { "en": "Suplementer?", "id": "Bersifat tambahan." },
  { "en": "Suplesi?", "id": "Penggantian bentuk." },
  { "en": "Suplir?", "id": "Tanaman." },
  { "en": "Suporter?", "id": "Pendukung." },
  { "en": "Suportif?", "id": "Mendukung." },
  { "en": "Supra?", "id": "Atas (awalan)." },
  { "en": "Supramolekuler?", "id": "Antar molekul." },
  { "en": "Supranasional?", "id": "Lintas negara." },
  { "en": "Suprareneal?", "id": "Kelenjar adrenal." },
  { "en": "Suprasegmental?", "id": "Unsur bahasa." },
  { "en": "Supremasi?", "id": "Keunggulan." },
  { "en": "Sur?", "id": "Tembok (Arab)." },
  { "en": "Sura?", "id": "Berani (Sanskerta)." },
  { "en": "Surah?", "id": "Bab Al-Quran." },
  { "en": "Surai?", "id": "Rambut kuda." },
  { "en": "Suralaga?", "id": "Medan perang dewa." },
  { "en": "Suralaya?", "id": "Kahyangan." },
  { "en": "Suram?", "id": "Tidak cerah." },
  { "en": "Surat?", "id": "Tulisan." },
  { "en": "Surati?", "id": "Berasal dari Surat." },
  { "en": "Surau?", "id": "Musala." },
  { "en": "Surealis?", "id": "Penganut surealisme." },
  { "en": "Surealisme?", "id": "Aliran seni." },
  { "en": "Surealistis?", "id": "Bersifat surealisme." },
  { "en": "Suren?", "id": "Pohon." },
  { "en": "Surfaktan?", "id": "Zat." },
  { "en": "Surga?", "id": "Firdaus." },
  { "en": "Surgawi?", "id": "Berkaitan surga." },
  { "en": "Suri?", "id": "Sisir." },
  { "en": "Surian?", "id": "Pohon." },
  { "en": "Surih?", "id": "Jejak (arkais)." },
  { "en": "Surili?", "id": "Monyet." },
  { "en": "Surjan?", "id": "Baju Jawa." },
  { "en": "Surplus?", "id": "Kelebihan." },
  { "en": "Suruh?", "id": "Perintah." },
  { "en": "Suruk?", "id": "Sembunyi." },
  { "en": "Surup?", "id": "Terbenam (matahari)." },
  { "en": "Survei?", "id": "Penelitian." },
  { "en": "Surveyor?", "id": "Peneliti." },
  { "en": "Survival?", "id": "Bertahan hidup (Inggris)." },
  { "en": "Surya?", "id": "Matahari." },
  { "en": "Suryakanta?", "id": "Kaca pembesar." },
  { "en": "Sus?", "id": "Susu (Belanda)." },
  { "en": "Susah?", "id": "Sulit." },
  { "en": "Susastera?", "id": "Sastra." },
  { "en": "Suseptibilitas?", "id": "Kerentanan." },
  { "en": "Susila?", "id": "Baik budi." },
  { "en": "Suspens?", "id": "Ketegangan." },
  { "en": "Suspensi?", "id": "Penundaan." },
  { "en": "Susu?", "id": "Cairan putih." },
  { "en": "Susuh?", "id": "Sarang." },
  { "en": "Susuk?", "id": "Jimat." },
  { "en": "Susul?", "id": "Ikut." },
  { "en": "Susun?", "id": "Tata." },
  { "en": "Susunan?", "id": "Komposisi." },
  { "en": "Susup?", "id": "Menyusup." },
  { "en": "Susur?", "id": "Pinggir." },
  { "en": "Susut?", "id": "Berkurang." },
  { "en": "Sut?", "id": "Diam (seruan)." },
  { "en": "Sutan?", "id": "Gelar bangsawan." },
  { "en": "Suten?", "id": "Suit." },
  { "en": "Sutil?", "id": "Alat masak." },
  { "en": "Sutra?", "id": "Kain." },
  { "en": "Sutradara?", "id": "Pengarah film." },
  { "en": "Suul?", "id": "Jahat (Arab)." },
  { "en": "Suun?", "id": "Soun." },
  { "en": "Suuzan?", "id": "Prasangka buruk." },
  { "en": "Suvenir?", "id": "Cenderamata." },
  { "en": "Suwarnabumi?", "id": "Sumatra (Sanskerta)." },
  { "en": "Suwarnadwipa?", "id": "Sumatra." },
  { "en": "Suwir?", "id": "Sobek kecil." },
  { "en": "Swabakar?", "id": "Terbakar sendiri." },
  { "en": "Swabela?", "id": "Bela diri." },
  { "en": "Swadarma?", "id": "Kewajiban sendiri." },
  { "en": "Swadaya?", "id": "Kekuatan sendiri." },
  { "en": "Swadesi?", "id": "Cinta produk dalam negeri." },
  { "en": "Swadidik?", "id": "Belajar sendiri." },
  { "en": "Swadisiplin?", "id": "Disiplin diri." },
  { "en": "Swagatra?", "id": "Tubuh sendiri." },
  { "en": "Swahara?", "id": "Suara sendiri." },
  { "en": "Swaimbas?", "id": "Induksi diri." },
  { "en": "Swak?", "id": "Jenis ikan." },
  { "en": "Swakaji?", "id": "Kajian diri." },
  { "en": "Swakarsa?", "id": "Kemauan sendiri." },
  { "en": "Swakarya?", "id": "Karya sendiri." },
  { "en": "Swakelola?", "id": "Kelola sendiri." },
  { "en": "Swakendali?", "id": "Kendali diri." },
  { "en": "Swakontradiksi?", "id": "Pertentangan diri." },
  { "en": "Swalayan?", "id": "Layan diri." },
  { "en": "Swanggi?", "id": "Sihir." },
  { "en": "Swantis?", "id": "Gesit." },
  { "en": "Swapraja?", "id": "Daerah otonom." },
  { "en": "Swarabakti?", "id": "Pengabdian." },
  { "en": "Swargaloka?", "id": "Surga." },
  { "en": "Swasembada?", "id": "Mencukupi sendiri." },
  { "en": "Swasensor?", "id": "Sensor diri." },
  { "en": "Swasirna?", "id": "Hancur sendiri." },
  { "en": "Swasta?", "id": "Nonpemerintah." },
  { "en": "Swastika?", "id": "Simbol." },
  { "en": "Swatantra?", "id": "Otonom." },
  { "en": "Sweter?", "id": "Baju hangat." },
  { "en": "Swike?", "id": "Makanan katak." },
  { "en": "Swipoa?", "id": "Sempoa." },
  { "en": "Syabah?", "id": "Mirip (Arab)." },
  { "en": "Syabas?", "id": "Sabas." },
  { "en": "Syafaat?", "id": "Pertolongan (akhirat)." },
  { "en": "Syafakat?", "id": "Belas kasihan (Arab)." },
  { "en": "Syafi?", "id": "Penyembuh (Arab)." },
  { "en": "Syafii?", "id": "Pengikut mazhab." },
  { "en": "Syah?", "id": "Sah." },
  { "en": "Syahadat?", "id": "Pengakuan iman Islam." },
  { "en": "Syahbandar?", "id": "Kepala pelabuhan." },
  { "en": "Syahda?", "id": "Mulia (arkais)." },
  { "en": "Syahdan?", "id": "Maka." },
  { "en": "Syahdu?", "id": "Khidmat." },
  { "en": "Syahid?", "id": "Mati membela agama." },
  { "en": "Syahriah?", "id": "Bulanan (Arab)." },
  { "en": "Syahsiah?", "id": "Kepribadian (Arab)." },
  { "en": "Syahwat?", "id": "Nafsu." },
  { "en": "Syair?", "id": "Sajak." },
  { "en": "Syajar?", "id": "Pohon (Arab)." },
  { "en": "Syajarah?", "id": "Silsilah." },
  { "en": "Syak?", "id": "Ragu." },
  { "en": "Syaka?", "id": "Sakit (Arab)." },
  { "en": "Syakar?", "id": "Gula (Arab)." },
  { "en": "Syakir?", "id": "Bersyukur (Arab)." },
  { "en": "Syal?", "id": "Selendang." },
  { "en": "Syala?", "id": "Nama dewa (India)." },
  { "en": "Syaman?", "id": "Dukun." },
  { "en": "Syamanisme?", "id": "Paham perdukunan." },
  { "en": "Syamar?", "id": "Adas." },
  { "en": "Syamsi?", "id": "Matahari (Arab)." },
  { "en": "Syamsiah?", "id": "Tahun matahari." },
  { "en": "Syamsir?", "id": "Pedang." },
  { "en": "Syamsu?", "id": "Matahari." },
  { "en": "Syar?", "id": "Rambut (Arab)." },
  { "en": "Syarab?", "id": "Minuman (Arab)." },
  { "en": "Syarah?", "id": "Penjelasan." },
  { "en": "Syarat?", "id": "Ketentuan." },
  { "en": "Syarbat?", "id": "Minuman." },
  { "en": "Syarekat?", "id": "Serikat." },
  { "en": "Syarf?", "id": "Mulia (Arab)." },
  { "en": "Syariat?", "id": "Hukum Islam." },
  { "en": "Syarif?", "id": "Mulia (keturunan Nabi)." },
  { "en": "Syarifah?", "id": "Wanita mulia." },
  { "en": "Syarik?", "id": "Sekutu (Arab)." },
  { "en": "Syarikat?", "id": "Perusahaan." },
  { "en": "Syarki?", "id": "Timur (Arab)." },
  { "en": "Syatar?", "id": "Tirai (Arab)." },
  { "en": "Syekh?", "id": "Tuan (Arab)." },
  { "en": "Syeti?", "id": "Pedagang (India)." },
  { "en": "Syiah?", "id": "Aliran Islam." },
  { "en": "Syikak?", "id": "Permusuhan (Arab)." },
  { "en": "Syiling?", "id": "Mata uang." },
  { "en": "Syin?", "id": "Huruf Arab." },
  { "en": "Syir?", "id": "Rahasia (Arab)." },
  { "en": "Syirk?", "id": "Menyekutukan Tuhan." },
  { "en": "Syok?", "id": "Kejutan." },
  { "en": "Syubhat?", "id": "Keraguan." },
  { "en": "Syuhada?", "id": "Para syahid." },
  { "en": "Syukur?", "id": "Terima kasih." },
  { "en": "Syur?", "id": "Indah." },
  { "en": "Syura?", "id": "Musyawarah (Arab)." },
  { "en": "Syurga?", "id": "Surga." },
  { "en": "Syuri?", "id": "Musyawarah (arkais)." },
  { "en": "Ta?", "id": "Huruf Arab." },
  { "en": "Taala?", "id": "Maha Tinggi (Allah)." },
  { "en": "Taaruf?", "id": "Perkenalan." },
  { "en": "Taasub?", "id": "Fanatik." },
  { "en": "Taat?", "id": "Patuh." },
  { "en": "Taawun?", "id": "Tolong-menolong (Arab)." },
  { "en": "Tabah?", "id": "Kuat." },
  { "en": "Tabak?", "id": "Nampan." },
  { "en": "Tabal?", "id": "Genderang." },
  { "en": "Tabar?", "id": "Kapak (arkais)." },
  { "en": "Tabarak?", "id": "Maha Suci (Arab)." },
  { "en": "Tabaruk?", "id": "Berkah." },
  { "en": "Tabel?", "id": "Daftar." },
  { "en": "Tabela?", "id": "Tabel." },
  { "en": "Tabelaris?", "id": "Berbentuk tabel." },
  { "en": "Tabernakel?", "id": "Kuil." },
  { "en": "Tabet?", "id": "Peti mati." },
  { "en": "Tabia?", "id": "Watak (Arab)." },
  { "en": "Tabiat?", "id": "Perangai." },
  { "en": "Tabib?", "id": "Dokter." },
  { "en": "Tabii?", "id": "Alami (Arab)." },
  { "en": "Tabik?", "id": "Salam." },
  { "en": "Tabir?", "id": "Tirai." },
  { "en": "Tablet?", "id": "Pil." },
  { "en": "Tablig?", "id": "Penyampaian ajaran." },
  { "en": "Tablo?", "id": "Lukisan." },
  { "en": "Tabloid?", "id": "Surat kabar." },
  { "en": "Tabo?", "id": "Serangga." },
  { "en": "Tabok?", "id": "Tampar." },
  { "en": "Tabrak?", "id": "Bentur." },
  { "en": "Tabu?", "id": "Larangan." },
  { "en": "Tabuh?", "id": "Alat pukul." },
  { "en": "Tabula rasa?", "id": "Jiwa bersih (Latin)." },
  { "en": "Tabulasi?", "id": "Penyusunan tabel." },
  { "en": "Tabulator?", "id": "Mesin hitung." },
  { "en": "Tabun?", "id": "Asap." },
  { "en": "Tabung?", "id": "Silinder." },
  { "en": "Tabur?", "id": "Sebar." },
  { "en": "Tabut?", "id": "Peti." },
  { "en": "Tadabur?", "id": "Perenungan (Arab)." },
  { "en": "Tadah?", "id": "Wadah." },
  { "en": "Tadahan?", "id": "Tampungan." },
  { "en": "Tadarus?", "id": "Membaca Al-Quran bersama." },
  { "en": "Tadi?", "id": "Barusan." },
  { "en": "Tadib?", "id": "Pendidikan (Arab)." },
  { "en": "Tadwin?", "id": "Pencatatan (Arab)." },
  { "en": "Taf?", "id": "Kain tebal." },
  { "en": "Tafadal?", "id": "Silakan (Arab)." },
  { "en": "Tafahus?", "id": "Penyelidikan (Arab)." },
  { "en": "Tafakur?", "id": "Renungan." },
  { "en": "Tafeta?", "id": "Kain." },
  { "en": "Tafsir?", "id": "Penjelasan." },
  { "en": "Tafsiran?", "id": "Interpretasi." },
  { "en": "Tag?", "id": "Label (Inggris)." },
  { "en": "Tagak?", "id": "Tegak (Minang)." },
  { "en": "Tagan?", "id": "Tahan." },
  { "en": "Tagal?", "id": "Karena (Jawa)." },
  { "en": "Tagan?", "id": "Kokoh." },
  { "en": "Tagar?", "id": "Guntur." },
  { "en": "Tagarih?", "id": "Garis (arkais)." },
  { "en": "Tageh?", "id": "Kuat (arkais)." },
  { "en": "Tagih?", "id": "Tuntut." },
  { "en": "Tagihan?", "id": "Tuntutan." },
  { "en": "Tagut?", "id": "Berhala (Arab)." },
  { "en": "Tah?", "id": "Seruan." },
  { "en": "Taha?", "id": "Nama surat Al-Quran." },
  { "en": "Tahajud?", "id": "Salat malam." },
  { "en": "Tahal?", "id": "Selesai ihram." },
  { "en": "Tahalul?", "id": "Tahal." },
  { "en": "Tahan?", "id": "Kuat." },
  { "en": "Tahanan?", "id": "Orang ditahan." },
  { "en": "Tahang?", "id": "Lekuk leher." },
  { "en": "Tahap?", "id": "Tingkat." },
  { "en": "Taharah?", "id": "Bersuci (Arab)." },
  { "en": "Tahi?", "id": "Tinja." },
  { "en": "Tahiat?", "id": "Salam (Arab)." },
  { "en": "Tahil?", "id": "Ukuran berat." },
  { "en": "Tahir?", "id": "Suci (Arab)." },
  { "en": "Tahlil?", "id": "Zikir." },
  { "en": "Tahmid?", "id": "Pujian Allah." },
  { "en": "Tahniah?", "id": "Ucapan selamat." },
  { "en": "Tahrif?", "id": "Perubahan (Arab)." },
  { "en": "Tahsil?", "id": "Hasil (Arab)." },
  { "en": "Tahu?", "id": "Kenal." },
  { "en": "Tahun?", "id": "Masa dua belas bulan." },
  { "en": "Taib?", "id": "Baik (Arab)." },
  { "en": "Taif?", "id": "Kota." },
  { "en": "Taifun?", "id": "Topan." },
  { "en": "Taiga?", "id": "Hutan." },
  { "en": "Taiko?", "id": "Gendang Jepang." },
  { "en": "Taipan?", "id": "Pengusaha besar." },
  { "en": "Tais?", "id": "Kain Timor." },
  { "en": "Taja?", "id": "Mulai (arkais)." },
  { "en": "Tajak?", "id": "Alat pertanian." },
  { "en": "Tajali?", "id": "Penampakan Tuhan." },
  { "en": "Tajam?", "id": "Runcing." },
  { "en": "Tajar?", "id": "Kunjungi (arkais)." },
  { "en": "Tajau?", "id": "Tempayan." },
  { "en": "Taji?", "id": "Susuh ayam." },
  { "en": "Tajin?", "id": "Air rebusan beras." },
  { "en": "Tajnis?", "id": "Gaya bahasa." },
  { "en": "Tajribah?", "id": "Percobaan (Arab)." },
  { "en": "Tajuk?", "id": "Mahkota." },
  { "en": "Tajur?", "id": "Tanjung." },
  { "en": "Tajusalatin?", "id": "Mahkota para sultan." },
  { "en": "Tajwid?", "id": "Ilmu baca Al-Quran." },
  { "en": "Tak?", "id": "Tidak." },
  { "en": "Takabur?", "id": "Sombong." },
  { "en": "Takaddas?", "id": "Suci (Arab)." },
  { "en": "Takaful?", "id": "Asuransi syariah." },
  { "en": "Takak?", "id": "Lekuk." },
  { "en": "Takal?", "id": "Kerekan." },
  { "en": "Takan?", "id": "Tidak akan." },
  { "en": "Takar?", "id": "Ukuran." },
  { "en": "Takaran?", "id": "Dosis." },
  { "en": "Takbir?", "id": "Ucapan Allahu Akbar." },
  { "en": "Takbiratulihram?", "id": "Takbir awal salat." },
  { "en": "Takdim?", "id": "Penghormatan." },
  { "en": "Takdir?", "id": "Nasib." },
  { "en": "Takeh?", "id": "Cerdik (arkais)." },
  { "en": "Takhayul?", "id": "Kepercayaan buta." },
  { "en": "Takhta?", "id": "Singgasana." },
  { "en": "Takik?", "id": "Lekuk." },
  { "en": "Takir?", "id": "Wadah daun." },
  { "en": "Takjil?", "id": "Makanan buka puasa." },
  { "en": "Takjub?", "id": "Heran." },
  { "en": "Taklid?", "id": "Ikut buta." },
  { "en": "Taklif?", "id": "Beban hukum (Islam)." },
  { "en": "Taklik?", "id": "Janji talak." },
  { "en": "Taklim?", "id": "Pengajaran (Arab)." },
  { "en": "Takluk?", "id": "Kalah." },
  { "en": "Takma?", "id": "Baju zirah." },
  { "en": "Takmurni?", "id": "Tidak murni." },
  { "en": "Takometer?", "id": "Pengukur putaran." },
  { "en": "Takraw?", "id": "Bola rotan." },
  { "en": "Takrif?", "id": "Definisi (Arab)." },
  { "en": "Takrim?", "id": "Penghormatan (Arab)." },
  { "en": "Takrir?", "id": "Penetapan (Arab)." },
  { "en": "Taksa?", "id": "Ambigu." },
  { "en": "Taksi?", "id": "Mobil sewa." },
  { "en": "Taksimeter?", "id": "Argometer." },
  { "en": "Taksir?", "id": "Perkiraan." },
  { "en": "Taksologi?", "id": "Ilmu susunan." },
  { "en": "Takson?", "id": "Kelompok taksonomi." },
  { "en": "Taksonomi?", "id": "Ilmu klasifikasi." },
  { "en": "Taktik?", "id": "Strategi." },
  { "en": "Taktil?", "id": "Sentuhan." },
  { "en": "Taktis?", "id": "Secara taktik." },
  { "en": "Takuh?", "id": "Lekuk (arkais)." },
  { "en": "Takuk?", "id": "Takik." },
  { "en": "Takung?", "id": "Tampung." },
  { "en": "Takur?", "id": "Tunduk (arkais)." },
  { "en": "Takut?", "id": "Gentar." },
  { "en": "Takwa?", "id": "Patuh (Tuhan)." },
  { "en": "Takwil?", "id": "Penafsiran." },
  { "en": "Takwim?", "id": "Kalender." },
  { "en": "Takziah?", "id": "Kunjungan duka." },
  { "en": "Takzim?", "id": "Hormat." },
  { "en": "Takzir?", "id": "Hukuman (Islam)." },
  { "en": "Tala?", "id": "Dasar (Sanskerta)." },
  { "en": "Talabiah?", "id": "Ucapan labaik." },
  { "en": "Talabu?", "id": "Pemusnahan (arkais)." },
  { "en": "Talah?", "id": "Telan (arkais)." },
  { "en": "Talak?", "id": "Cerai." },
  { "en": "Talam?", "id": "Nampan." },
  { "en": "Talang?", "id": "Saluran air." },
  { "en": "Talar?", "id": "Telan (arkais)." },
  { "en": "Talas?", "id": "Umbi." },
  { "en": "Talbiah?", "id": "Talabiah." },
  { "en": "Talek?", "id": "Bedak." },
  { "en": "Talempong?", "id": "Alat musik Minang." },
  { "en": "Talen?", "id": "Mata uang." },
  { "en": "Talenan?", "id": "Alas potong." },
  { "en": "Talenta?", "id": "Bakat." },
  { "en": "Tali?", "id": "Tambang." },
  { "en": "Talib?", "id": "Pelajar (Arab)." },
  { "en": "Talibun?", "id": "Puisi lama." },
  { "en": "Talium?", "id": "Unsur kimia." },
  { "en": "Talk?", "id": "Bedak." },
  { "en": "Talofit?", "id": "Tumbuhan talus." },
  { "en": "Talok?", "id": "Kersen." },
  { "en": "Talu?", "id": "Musik pembuka." },
  { "en": "Taluk?", "id": "Takluk." },
  { "en": "Talun?", "id": "Bergema." },
  { "en": "Talupuh?", "id": "Penyakit." },
  { "en": "Talus?", "id": "Badan tumbuhan sederhana." },
  { "en": "Tam?", "id": "Paman (Belanda)." },
  { "en": "Tamadun?", "id": "Peradaban." },
  { "en": "Tamah?", "id": "Ramah." },
  { "en": "Tamak?", "id": "Rakus." },
  { "en": "Tamam?", "id": "Sempurna (Arab)." },
  { "en": "Taman?", "id": "Kebun." },
  { "en": "Tamar?", "id": "Kurma (Arab)." },
  { "en": "Tamarinda?", "id": "Asam." },
  { "en": "Tamas?", "id": "Gelap (Sanskerta)." },
  { "en": "Tamasya?", "id": "Piknik." },
  { "en": "Tamat?", "id": "Selesai." },
  { "en": "Tambah?", "id": "Tambah." },
  { "en": "Tambahan?", "id": "Ekstra." },
  { "en": "Tambak?", "id": "Kolam." },
  { "en": "Tambal?", "id": "Tutup lubang." },
  { "en": "Tamban?", "id": "Ikan." },
  { "en": "Tambang?", "id": "Tali." },
  { "en": "Tambat?", "id": "Ikat." },
  { "en": "Tambi?", "id": "Panggilan India." },
  { "en": "Tambo?", "id": "Sejarah Minang." },
  { "en": "Tamborin?", "id": "Rebana." },
  { "en": "Tambra?", "id": "Ikan." },
  { "en": "Tambuh?", "id": "Minum (arkais)." },
  { "en": "Tambul?", "id": "Makanan ringan." },
  { "en": "Tambun?", "id": "Gemuk." },
  { "en": "Tambung?", "id": "Sombong." },
  { "en": "Tambur?", "id": "Genderang." },
  { "en": "Tameng?", "id": "Perisai." },
  { "en": "Tamimah?", "id": "Jimat (Arab)." },
  { "en": "Tampa?", "id": "Terima (arkais)." },
  { "en": "Tampah?", "id": "Nyiru." },
  { "en": "Tampak?", "id": "Terlihat." },
  { "en": "Tampal?", "id": "Tambal." },
  { "en": "Tampan?", "id": "Gagah." },
  { "en": "Tampang?", "id": "Wajah." },
  { "en": "Tampar?", "id": "Pukul." },
  { "en": "Tampas?", "id": "Potong." },
  { "en": "Tampel?", "id": "Tambal (Jawa)." },
  { "en": "Tampi?", "id": "Ayak." },
  { "en": "Tampil?", "id": "Muncul." },
  { "en": "Tampin?", "id": "Sumbat." },
  { "en": "Tamping?", "id": "Rapat (arkais)." },
  { "en": "Tampo?", "id": "Waktu (arkais)." },
  { "en": "Tampoi?", "id": "Pohon." },
  { "en": "Tampomas?", "id": "Gunung." },
  { "en": "Tampon?", "id": "Sumbat." },
  { "en": "Tampuk?", "id": "Pucuk." },
  { "en": "Tampung?", "id": "Wadah." },
  { "en": "Tamsil?", "id": "Perumpamaan." },
  { "en": "Tamtam?", "id": "Gong." },
  { "en": "Tamu?", "id": "Pengunjung." },
  { "en": "Tamyiz?", "id": "Pembedaan (Arab)." },
  { "en": "Tanah?", "id": "Bumi." },
  { "en": "Tanai?", "id": "Kecil (arkais)." },
  { "en": "Tanak?", "id": "Masak nasi." },
  { "en": "Tanam?", "id": "Menanam." },
  { "en": "Tanaman?", "id": "Tumbuhan." },
  { "en": "Tanau?", "id": "Danau (arkais)." },
  { "en": "Tanazul?", "id": "Turun (Arab)." },
  { "en": "Tancang?", "id": "Ikat (arkais)." },
  { "en": "Tancap?", "id": "Tusuk." },
  { "en": "Tanda?", "id": "Isyarat." },
  { "en": "Tandak?", "id": "Tari." },
  { "en": "Tandan?", "id": "Rangkaian buah." },
  { "en": "Tandang?", "id": "Kunjung." },
  { "en": "Tandas?", "id": "Habis." },
  { "en": "Tandem?", "id": "Berdua." },
  { "en": "Tandik?", "id": "Kutu (arkais)." },
  { "en": "Tanding?", "id": "Lawan." },
  { "en": "Tandon?", "id": "Wadah air." },
  { "en": "Tandts?", "id": "Gigi (Belanda)." },
  { "en": "Tanduk?", "id": "Cula." },
  { "en": "Tandur?", "id": "Tanam padi." },
  { "en": "Tandus?", "id": "Gersang." },
  { "en": "Tangan?", "id": "Bagian tubuh." },
  { "en": "Tangap?", "id": "Sambut." },
  { "en": "Tangar?", "id": "Pohon." },
  { "en": "Tangas?", "id": "Mandi uap." },
  { "en": "Tangga?", "id": "Alat naik." },
  { "en": "Tanggah?", "id": "Tahan (arkais)." },
  { "en": "Tanggal?", "id": "Lepas." },
  { "en": "Tanggam?", "id": "Kait." },
  { "en": "Tanggang?", "id": "Buka." },
  { "en": "Tanggap?", "id": "Respon." },
  { "en": "Tangguk?", "id": "Alat tangkap ikan." },
  { "en": "Tanggul?", "id": "Bendungan." },
  { "en": "Tanggulang?", "id": "Cegah." },
  { "en": "Tanggun?", "id": "Tanggung (arkais)." },
  { "en": "Tanggung?", "id": "Jamin." },
  { "en": "Tangis?", "id": "Ratap." },
  { "en": "Tangkahan?", "id": "Dermaga kecil." },
  { "en": "Tangkai?", "id": "Gagang." },
  { "en": "Tangkal?", "id": "Cegah." },
  { "en": "Tangkap?", "id": "Sergap." },
  { "en": "Tangkar?", "id": "Biak." },
  { "en": "Tangkas?", "id": "Cekatan." },
  { "en": "Tangki?", "id": "Wadah." },
  { "en": "Tangkil?", "id": "Dekat." },
  { "en": "Tangkis?", "id": "Tahan." },
  { "en": "Tangkue?", "id": "Manisan labu." },
  { "en": "Tangkul?", "id": "Jaring." },
  { "en": "Tangkup?", "id": "Katup." },
  { "en": "Tangkur?", "id": "Kuda laut kering." },
  { "en": "Tanglung?", "id": "Lampion." },
  { "en": "Tango?", "id": "Tarian." },
  { "en": "Tangsel?", "id": "Tali perut kuda." },
  { "en": "Tanin?", "id": "Zat." },
  { "en": "Tanji?", "id": "Musik." },
  { "en": "Tanjidor?", "id": "Orkes Betawi." },
  { "en": "Tanjung?", "id": "Daratan menjorok." },
  { "en": "Tanpa?", "id": "Nir." },
  { "en": "Tantang?", "id": "Ajak berkelahi." },
  { "en": "Tante?", "id": "Bibi." },
  { "en": "Tantiem?", "id": "Bonus." },
  { "en": "Tantrisme?", "id": "Ajaran." },
  { "en": "Tanur?", "id": "Tungku." },
  { "en": "Tanwir?", "id": "Penerangan (Arab)." },
  { "en": "Tanya?", "id": "Soal." },
  { "en": "Tao?", "id": "Ajaran Tiongkok." },
  { "en": "Taoci?", "id": "Kain (arkais)." },
  { "en": "Taoco?", "id": "Bumbu." },
  { "en": "Taoge?", "id": "Tauge." },
  { "en": "Taoke?", "id": "Tauke." },
  { "en": "Taosi?", "id": "Sepatu (arkais)." },
  { "en": "Tap?", "id": "Keran (Inggris)." },
  { "en": "Tapa?", "id": "Bertapa." },
  { "en": "Tapai?", "id": "Makanan fermentasi." },
  { "en": "Tapak?", "id": "Jejak." },
  { "en": "Tapal?", "id": "Batas." },
  { "en": "Tapang?", "id": "Pohon." },
  { "en": "Tape?", "id": "Tapai." },
  { "en": "Tapel?", "id": "Bedak perut." },
  { "en": "Taper?", "id": "Lilin." },
  { "en": "Tapi?", "id": "Tetapi." },
  { "en": "Tapih?", "id": "Kain." },
  { "en": "Tapin?", "id": "Kabupaten." },
  { "en": "Tapioka?", "id": "Tepung singkong." },
  { "en": "Tapir?", "id": "Hewan." },
  { "en": "Tapis?", "id": "Saring." },
  { "en": "Taplak?", "id": "Alas meja." },
  { "en": "Taprofit?", "id": "Organisme." },
  { "en": "Taprofita?", "id": "Tumbuhan." },
  { "en": "Taptibau?", "id": "Kadal." },
  { "en": "Tapui?", "id": "Minuman." },
  { "en": "Tapuk?", "id": "Tempel." },
  { "en": "Tapung?", "id": "Kain." },
  { "en": "Tapus?", "id": "Alat." },
  { "en": "Tar?", "id": "Aspal." },
  { "en": "Tara?", "id": "Selisih berat." },
  { "en": "Taraf?", "id": "Tingkat." },
  { "en": "Tarah?", "id": "Rata." },
  { "en": "Tarak?", "id": "Pantang makan." },
  { "en": "Taraksasin?", "id": "Zat." },
  { "en": "Tarantela?", "id": "Tarian Italia." },
  { "en": "Tarantula?", "id": "Laba-laba." },
  { "en": "Tarawih?", "id": "Salat malam Ramadan." },
  { "en": "Tarekat?", "id": "Jalan tasawuf." },
  { "en": "Target?", "id": "Sasaran." },
  { "en": "Tarhim?", "id": "Pujian sebelum azan." },
  { "en": "Tari?", "id": "Gerak berirama." },
  { "en": "Tarif?", "id": "Harga." },
  { "en": "Tarik?", "id": "Seret." },
  { "en": "Tarikh?", "id": "Tanggal." },
  { "en": "Taring?", "id": "Gigi tajam." },
  { "en": "Tarip?", "id": "Tarif (arkais)." },
  { "en": "Taris?", "id": "Ikat (arkais)." },
  { "en": "Tarling?", "id": "Musik." },
  { "en": "Tarmak?", "id": "Aspal bandara." },
  { "en": "Taro?", "id": "Talas." },
  { "en": "Tarpentin?", "id": "Terpentin." },
  { "en": "Tart?", "id": "Kue." },
  { "en": "Tartar?", "id": "Suku." },
  { "en": "Tartil?", "id": "Bacaan Al-Quran." },
  { "en": "Taruh?", "id": "Letak." },
  { "en": "Taruhan?", "id": "Judi." },
  { "en": "Taruk?", "id": "Tunas." },
  { "en": "Tarum?", "id": "Pohon nila." },
  { "en": "Tarung?", "id": "Berkelahi." },
  { "en": "Tarup?", "id": "Dekorasi." },
  { "en": "Tas?", "id": "Kantong." },
  { "en": "Tasalsul?", "id": "Rantai (Arab)." },
  { "en": "Tasamuh?", "id": "Toleransi (Arab)." },
  { "en": "Tasawuf?", "id": "Ilmu kebatinan Islam." },
  { "en": "Tasbih?", "id": "Alat zikir." },
  { "en": "Tasdid?", "id": "Penguatan (Arab)." },
  { "en": "Tasdik?", "id": "Pengesahan (Arab)." },
  { "en": "Taser?", "id": "Alat kejut listrik." },
  { "en": "Tasik?", "id": "Danau." },
  { "en": "Taslim?", "id": "Penyerahan diri (Arab)." },
  { "en": "Tasmik?", "id": "Mendengarkan (Arab)." },
  { "en": "Tasrif?", "id": "Perubahan bentuk kata Arab." },
  { "en": "Tasrih?", "id": "Pernyataan (Arab)." },
  { "en": "Tasyahud?", "id": "Bagian salat." },
  { "en": "Tasyakur?", "id": "Syukuran." },
  { "en": "Tasyayuh?", "id": "Sikap Syiah (Arab)." },
  { "en": "Tata?", "id": "Atur." },
  { "en": "Tatabahasa?", "id": "Gramatika." },
  { "en": "Tatah?", "id": "Ukir." },
  { "en": "Tatak?", "id": "Tahan (arkais)." },
  { "en": "Tatakrama?", "id": "Sopan santun." },
  { "en": "Tatal?", "id": "Serpihan kayu." },
  { "en": "Tatanan?", "id": "Sistem." },
  { "en": "Tatang?", "id": "Angkat (arkais)." },
  { "en": "Tatap?", "id": "Pandang." },
  { "en": "Tatar?", "id": "Suku." },
  { "en": "Tatih?", "id": "Belajar jalan." },
  { "en": "Tatkala?", "id": "Ketika." },
  { "en": "TATO?", "id": "Rajahan kulit." },
  { "en": "Tatu?", "id": "Luka (Jawa)." },
  { "en": "Taubat?", "id": "Tobat." },
  { "en": "Taufan?", "id": "Topan." },
  { "en": "Taufik?", "id": "Pertolongan Tuhan." },
  { "en": "Tauhid?", "id": "Keesaan Tuhan." },
  { "en": "Tauk?", "id": "Tahu (arkais)." },
  { "en": "Tauke?", "id": "Bos." },
  { "en": "Taul?", "id": "Tali (arkais)." },
  { "en": "Tauladan?", "id": "Teladan." },
  { "en": "Taulan?", "id": "Teman." },
  { "en": "Taung?", "id": "Tahun (arkais)." },
  { "en": "Tauran?", "id": "Berkelahi (arkais)." },
  { "en": "Taurat?", "id": "Kitab suci Yahudi." },
  { "en": "Taurus?", "id": "Rasi bintang." },
  { "en": "Tausiah?", "id": "Ceramah agama." },
  { "en": "Taut?", "id": "Kait." },
  { "en": "Tautofoni?", "id": "Pengulangan bunyi." },
  { "en": "Tautologi?", "id": "Pengulangan makna." },
  { "en": "Tautonim?", "id": "Nama ilmiah sama." },
  { "en": "Tawaf?", "id": "Mengelilingi Kakbah." },
  { "en": "Tawajuh?", "id": "Menghadap (Arab)." },
  { "en": "Tawak?", "id": "Lempar (arkais)." },
  { "en": "Tawakal?", "id": "Berserah diri." },
  { "en": "Tawan?", "id": "Tahan." },
  { "en": "Tawanan?", "id": "Orang ditawan." },
  { "en": "Tawar?", "id": "Hambar." },
  { "en": "Tawarik?", "id": "Sejarah (Arab)." },
  { "en": "Tawas?", "id": "Zat kimia." },
  { "en": "Tawadu?", "id": "Rendah hati (Arab)." },
  { "en": "Tawon?", "id": "Serangga." },
  { "en": "Tayamum?", "id": "Bersuci debu." },
  { "en": "Tayang?", "id": "Tampil." },
  { "en": "Tayub?", "id": "Tarian." },
  { "en": "Te?", "id": "Huruf." },
  { "en": "Teater?", "id": "Panggung." },
  { "en": "Teatrikal?", "id": "Bersifat teater." },
  { "en": "Teba?", "id": "Tebang (arkais)." },
  { "en": "Tebah?", "id": "Pukul." },
  { "en": "Tebak?", "id": "Terka." },
  { "en": "Tebal?", "id": "Tidak tipis." },
  { "en": "Tebang?", "id": "Potong pohon." },
  { "en": "Tebar?", "id": "Sebar." },
  { "en": "Tebas?", "id": "Potong." },
  { "en": "Tebat?", "id": "Bendungan." },
  { "en": "Teberau?", "id": "Tanaman." },
  { "en": "Tebu?", "id": "Tanaman." },
  { "en": "Tebuk?", "id": "Lubang." },
  { "en": "Tebung?", "id": "Serbuk (arkais)." },
  { "en": "Tebus?", "id": "Bayar." },
  { "en": "Tedak?", "id": "Turun (Jawa)." },
  { "en": "Tedas?", "id": "Jelas (arkais)." },
  { "en": "Tedeng?", "id": "Tutup." },
  { "en": "Tedong?", "id": "Kerbau (Toraja)." },
  { "en": "Teduh?", "id": "Tidak panas." },
  { "en": "Tegal?", "id": "Tanah lapang." },
  { "en": "Tegalan?", "id": "Ladang." },
  { "en": "Tegang?", "id": "Kaku." },
  { "en": "Tegap?", "id": "Kuat." },
  { "en": "Tegar?", "id": "Keras." },
  { "en": "Tegas?", "id": "Jelas." },
  { "en": "Tegel?", "id": "Ubin." },
  { "en": "Tegen?", "id": "Kuat (Belanda)." },
  { "en": "Teguh?", "id": "Kuat." },
  { "en": "Teguk?", "id": "Minum." },
  { "en": "Tegun?", "id": "Diam (arkais)." },
  { "en": "Tegur?", "id": "Sapa." },
  { "en": "Teh?", "id": "Minuman." },
  { "en": "Teja?", "id": "Cahaya (Sanskerta)." },
  { "en": "Teji?", "id": "Kuda." },
  { "en": "Tekad?", "id": "Niat." },
  { "en": "Tekak?", "id": "Langit-langit mulut." },
  { "en": "Tekalak?", "id": "Tawa (arkais)." },
  { "en": "Tekam?", "id": "Dekap (arkais)." },
  { "en": "Tekan?", "id": "Desak." },
  { "en": "Tekanan?", "id": "Presi." },
  { "en": "Tekap?", "id": "Tutup." },
  { "en": "Tekar?", "id": "Keras (arkais)." },
  { "en": "Tekebur?", "id": "Sombong." },
  { "en": "Tekek?", "id": "Tokek." },
  { "en": "Tekel?", "id": "Ambil bola (sepak bola)." },
  { "en": "Teken?", "id": "Tanda tangan." },
  { "en": "Teker?", "id": "Kelereng (Belanda)." },
  { "en": "Tekidanto?", "id": "Granat (Jepang)." },
  { "en": "Tekik?", "id": "Lekuk." },
  { "en": "Tekis?", "id": "Nyaris." },
  { "en": "Teklek?", "id": "Sandal kayu." },
  { "en": "Tekniawan?", "id": "Teknisi." },
  { "en": "Teknik?", "id": "Cara." },
  { "en": "Teknisi?", "id": "Ahli teknik." },
  { "en": "Teknokrat?", "id": "Ahli teknologi." },
  { "en": "Teknologi?", "id": "Ilmu teknik." },
  { "en": "Teknologis?", "id": "Bersifat teknologi." },
  { "en": "Teko?", "id": "Poci." },
  { "en": "Tekoa?", "id": "Kue." },
  { "en": "Tekok?", "id": "Bengkok (arkais)." },
  { "en": "Tekong?", "id": "Kapten (perahu)." },
  { "en": "Tekor?", "id": "Rugi." },
  { "en": "Teks?", "id": "Tulisan." },
  { "en": "Tekstil?", "id": "Kain." },
  { "en": "Tekstur?", "id": "Permukaan." },
  { "en": "Tekstural?", "id": "Bersifat tekstur." },
  { "en": "Tektonik?", "id": "Gerak lempeng bumi." },
  { "en": "Tektonis?", "id": "Bersifat tektonik." },
  { "en": "Tekua?", "id": "Kedelai hitam." },
  { "en": "Tekuk?", "id": "Lipat." },
  { "en": "Tekukur?", "id": "Burung." },
  { "en": "Tekun?", "id": "Rajin." },
  { "en": "Tekung?", "id": "Bungkuk (arkais)." },
  { "en": "Tekur?", "id": "Tunduk." },
  { "en": "Tela?", "id": "Papan (arkais)." },
  { "en": "Telaah?", "id": "Kajian." },
  { "en": "Teladan?", "id": "Contoh." },
  { "en": "Telaga?", "id": "Danau." },
  { "en": "Telah?", "id": "Sudah." },
  { "en": "Telajak?", "id": "Terlanjur." },
  { "en": "Telak?", "id": "Tepat." },
  { "en": "Telampung?", "id": "Pelampung." },
  { "en": "Telan?", "id": "Masuk kerongkongan." },
  { "en": "Telang?", "id": "Bunga." },
  { "en": "Telangkai?", "id": "Perantara." },
  { "en": "Telangkup?", "id": "Tengkurap." },
  { "en": "Telanjang?", "id": "Tidak berpakaian." },
  { "en": "Telanjur?", "id": "Sudah terlanjur." },
  { "en": "Telantar?", "id": "Terbengkalai." },
  { "en": "Telap?", "id": "Resap." },
  { "en": "Telapak?", "id": "Bagian bawah kaki." },
  { "en": "Telas?", "id": "Habis (Jawa)." },
  { "en": "Telat?", "id": "Terlambat." },
  { "en": "Telatah?", "id": "Tingkah." },
  { "en": "Telau?", "id": "Belang." },
  { "en": "Tele?", "id": "Tidur (arkais)." },
  { "en": "Teledek?", "id": "Penari wanita." },
  { "en": "Telefon?", "id": "Telepon." },
  { "en": "Telegraf?", "id": "Alat kirim pesan." },
  { "en": "Telegrafi?", "id": "Ilmu telegraf." },
  { "en": "Telegrafis?", "id": "Singkat." },
  { "en": "Telegram?", "id": "Surat kawat." },
  { "en": "Telekan?", "id": "Tekan (arkais)." },
  { "en": "Teleks?", "id": "Mesin kirim tulisan." },
  { "en": "Teleku?", "id": "Tunduk (arkais)." },
  { "en": "Telekung?", "id": "Mukena." },
  { "en": "Teleologi?", "id": "Filsafat tujuan." },
  { "en": "Teleost?", "id": "Ikan." },
  { "en": "Telepati?", "id": "Komunikasi pikiran." },
  { "en": "Telepon?", "id": "Alat komunikasi suara." },
  { "en": "Teleprinter?", "id": "Mesin cetak jauh." },
  { "en": "Teleprompter?", "id": "Alat bantu baca pidato." },
  { "en": "Teleskop?", "id": "Teropong bintang." },
  { "en": "Televisi?", "id": "Siaran gambar." },
  { "en": "Telih?", "id": "Jelas (arkais)." },
  { "en": "Telik?", "id": "Mata-mata." },
  { "en": "Telimpuh?", "id": "Duduk bersimpuh." },
  { "en": "Telinga?", "id": "Indra pendengar." },
  { "en": "Telingkah?", "id": "Bertingkah." },
  { "en": "Teliti?", "id": "Cermat." },
  { "en": "Telop?", "id": "Tulisan layar TV." },
  { "en": "Telor?", "id": "Telur." },
  { "en": "Teluh?", "id": "Sihir." },
  { "en": "Teluk?", "id": "Bagian laut menjorok." },
  { "en": "Teluki?", "id": "Anyelir." },
  { "en": "Telungkup?", "id": "Tengkurap." },
  { "en": "Telunjuk?", "id": "Jari." },
  { "en": "Telur?", "id": "Hasil unggas." },
  { "en": "Telus?", "id": "Tembus." },
  { "en": "Telusur?", "id": "Susur." },
  { "en": "Tema?", "id": "Pokok." },
  { "en": "Temabur?", "id": "Bertaburan." },
  { "en": "Temaha?", "id": "Sengaja (arkais)." },
  { "en": "Temahak?", "id": "Rakus." },
  { "en": "Teman?", "id": "Kawan." },
  { "en": "Temang?", "id": "Lirik (arkais)." },
  { "en": "Temaram?", "id": "Redup." },
  { "en": "Temas?", "id": "Emas (arkais)." },
  { "en": "Tembaga?", "id": "Logam." },
  { "en": "Tembak?", "id": "Dor." },
  { "en": "Tembakau?", "id": "Bahan rokok." },
  { "en": "Tembakul?", "id": "Ikan." },
  { "en": "Tembam?", "id": "Bengkak." },
  { "en": "Tembarau?", "id": "Rumput." },
  { "en": "Tembatar?", "id": "Sayap (arkais)." },
  { "en": "Tembelang?", "id": "Busuk (telur)." },
  { "en": "Tembem?", "id": "Gemuk (pipi)." },
  { "en": "Tembesar?", "id": "Besar (arkais)." },
  { "en": "Tembesi?", "id": "Pohon." },
  { "en": "Tembikai?", "id": "Semangka." },
  { "en": "Tembikar?", "id": "Gerabah." },
  { "en": "Tembilang?", "id": "Alat gali." },
  { "en": "Tembok?", "id": "Dinding." },
  { "en": "Tembolok?", "id": "Perut burung." },
  { "en": "Tembosa?", "id": "Kue." },
  { "en": "Tembuh?", "id": "Tembus (arkais)." },
  { "en": "Tembuni?", "id": "Plasenta." },
  { "en": "Temburung?", "id": "Tempurung." },
  { "en": "Tembus?", "id": "Bolong." },
  { "en": "Temengalan?", "id": "Tenggiling." },
  { "en": "Temenggung?", "id": "Pejabat." },
  { "en": "Tempa?", "id": "Bentuk logam." },
  { "en": "Tempah?", "id": "Pesan." },
  { "en": "Tempala?", "id": "Pukulan (arkais)." },
  { "en": "Tempap?", "id": "Tampar (arkais)." },
  { "en": "Tempat?", "id": "Lokasi." },
  { "en": "Tempawan?", "id": "Tukang tempa." },
  { "en": "Tempayak?", "id": "Larva." },
  { "en": "Tempayang?", "id": "Buah." },
  { "en": "Tempaz?", "id": "Batu permata." },
  { "en": "Tempel?", "id": "Lekat." },
  { "en": "Tempelak?", "id": "Cela." },
  { "en": "Tempeleng?", "id": "Tampar." },
  { "en": "Temperamen?", "id": "Watak." },
  { "en": "Temperamental?", "id": "Mudah marah." },
  { "en": "Temperatur?", "id": "Suhu." },
  { "en": "Tempirai?", "id": "Kutu." },
  { "en": "Tempinis?", "id": "Pohon." },
  { "en": "Tempo?", "id": "Waktu." },
  { "en": "Tempoh?", "id": "Tempo." },
  { "en": "Tempoyak?", "id": "Makanan durian." },
  { "en": "Tempua?", "id": "Burung." },
  { "en": "Tempuh?", "id": "Lalui." },
  { "en": "Tempui?", "id": "Pohon." },
  { "en": "Tempuling?", "id": "Tombak ikan." },
  { "en": "Tempunai?", "id": "Emas (arkais)." },
  { "en": "Tempur?", "id": "Berkelahi." },
  { "en": "Tempurung?", "id": "Batok." },
  { "en": "Tempus?", "id": "Waktu (Latin)." },
  { "en": "Tempuyung?", "id": "Tanaman." },
  { "en": "Temu?", "id": "Jumpa." },
  { "en": "Temucut?", "id": "Rumput." },
  { "en": "Temurun?", "id": "Turun-temurun." },
  { "en": "Tenabang?", "id": "Tanah Abang." },
  { "en": "Tenaga?", "id": "Kekuatan." },
  { "en": "Tenam?", "id": "Pohon." },
  { "en": "Tenang?", "id": "Damai." },
  { "en": "Tenar?", "id": "Terkenal." },
  { "en": "Tenda?", "id": "Kemah." },
  { "en": "Tendang?", "id": "Sepak." },
  { "en": "Tendas?", "id": "Kepala (kasar)." },
  { "en": "Tendensi?", "id": "Kecenderungan." },
  { "en": "Tendensius?", "id": "Memihak." },
  { "en": "Tender?", "id": "Tawaran." },
  { "en": "Tendinitis?", "id": "Radang tendon." },
  { "en": "Tendo?", "id": "Otot (arkais)." },
  { "en": "Tendon?", "id": "Urat." },
  { "en": "Tenebrae?", "id": "Ritual Paskah." },
  { "en": "Tenebrikol?", "id": "Hidup di gelap." },
  { "en": "Tenesmus?", "id": "Rasa ingin buang air." },
  { "en": "Teng?", "id": "Lampu (Belanda)." },
  { "en": "Tenga?", "id": "Kelelawar." },
  { "en": "Tengadah?", "id": "Lihat ke atas." },
  { "en": "Tengah?", "id": "Pusat." },
  { "en": "Tengak?", "id": "Lihat (arkais)." },
  { "en": "Tengar?", "id": "Pohon." },
  { "en": "Tengara?", "id": "Tanda." },
  { "en": "Tenggak?", "id": "Minum." },
  { "en": "Tenggala?", "id": "Bajak." },
  { "en": "Tenggalung?", "id": "Musang." },
  { "en": "Tenggan?", "id": "Tahan (arkais)." },
  { "en": "Tenggang?", "id": "Batas waktu." },
  { "en": "Tenggar?", "id": "Sarang lebah." },
  { "en": "Tenggara?", "id": "Arah mata angin." },
  { "en": "Tenggat?", "id": "Batas waktu." },
  { "en": "Tenggayun?", "id": "Burung." },
  { "en": "Tenggehem?", "id": "Deham." },
  { "en": "Tenggek?", "id": "Bertengger." },
  { "en": "Tenggelam?", "id": "Karam." },
  { "en": "Tengger?", "id": "Hinggap." },
  { "en": "Tenggiling?", "id": "Hewan." },
  { "en": "Tenggiri?", "id": "Ikan." },
  { "en": "Tenggok?", "id": "Leher." },
  { "en": "Tenggorok?", "id": "Kerongkongan." },
  { "en": "Tengguli?", "id": "Gula." },
  { "en": "Tengik?", "id": "Bau tak sedap." },
  { "en": "Tengil?", "id": "Nakal." },
  { "en": "Tengkalak?", "id": "Perangkap ikan." },
  { "en": "Tengkalang?", "id": "Lumbung." },
  { "en": "Tengkar?", "id": "Bertengkar." },
  { "en": "Tengkarah?", "id": "Ribut." },
  { "en": "Tengkarap?", "id": "Tengkurap." },
  { "en": "Tengkayan?", "id": "Sungai (arkais)." },
  { "en": "Tengkek?", "id": "Burung." },
  { "en": "Tengker?", "id": "Bertengger." },
  { "en": "Tengking?", "id": "Bentak." },
  { "en": "Tengkolok?", "id": "Ikat kepala." },
  { "en": "Tengkorak?", "id": "Tulang kepala." },
  { "en": "Tengkuk?", "id": "Leher belakang." },
  { "en": "Tengkulak?", "id": "Pedagang perantara." },
  { "en": "Tengkurap?", "id": "Menelungkup." },
  { "en": "Tengu?", "id": "Kutu." },
  { "en": "Tenis?", "id": "Olahraga." },
  { "en": "Tenjet?", "id": "Pijit." },
  { "en": "Tenok?", "id": "Tapir." },
  { "en": "Tenor?", "id": "Suara pria tinggi." },
  { "en": "Tens?", "id": "Tegang (Inggris)." },
  { "en": "Tensi?", "id": "Tekanan." },
  { "en": "Tentakel?", "id": "Lengan hewan." },
  { "en": "Tentamen?", "id": "Ujian." },
  { "en": "Tentang?", "id": "Mengenai." },
  { "en": "Tentara?", "id": "Prajurit." },
  { "en": "Tentatif?", "id": "Sementara." },
  { "en": "Tenteng?", "id": "Jinjing." },
  { "en": "Tentu?", "id": "Pasti." },
  { "en": "Tenturun?", "id": "Hantu." },
  { "en": "Tenuk?", "id": "Tapir." },
  { "en": "Tenun?", "id": "Kain." },
  { "en": "Teodolit?", "id": "Alat ukur tanah." },
  { "en": "Teokrasi?", "id": "Pemerintahan Tuhan." },
  { "en": "Teokratis?", "id": "Bersifat teokrasi." },
  { "en": "Teolog?", "id": "Ahli teologi." },
  { "en": "Teologi?", "id": "Ilmu ketuhanan." },
  { "en": "Teologis?", "id": "Bersifat teologi." },
  { "en": "Teorem?", "id": "Dalil." },
  { "en": "Teorema?", "id": "Teorem." },
  { "en": "Teoretikus?", "id": "Ahli teori." },
  { "en": "Teoretis?", "id": "Berdasarkan teori." },
  { "en": "Teori?", "id": "Pendapat." },
  { "en": "Teosofi?", "id": "Filsafat." },
  { "en": "Tepa?", "id": "Sisi." },
  { "en": "Tepak?", "id": "Pukul." },
  { "en": "Tepam?", "id": "Penuh." },
  { "en": "Tepas?", "id": "Teras." },
  { "en": "Tepat?", "id": "Akurat." },
  { "en": "Tepeh?", "id": "Datar (arkais)." },
  { "en": "Tepekong?", "id": "Dewa Tionghoa." },
  { "en": "Teperam?", "id": "Terperangkap." },
  { "en": "Tepes?", "id": "Sabut kelapa." },
  { "en": "Tepet?", "id": "Rapat." },
  { "en": "Tepi?", "id": "Pinggir." },
  { "en": "Tepian?", "id": "Pinggiran." },
  { "en": "Tepok?", "id": "Pukul." },
  { "en": "Tepo?", "id": "Lelah (Jawa)." },
  { "en": "Tepos?", "id": "Tipis (pantat)." },
  { "en": "Tepuk?", "id": "Pukulan ringan." },
  { "en": "Tepung?", "id": "Bubuk." },
  { "en": "Ter?", "id": "Aspal cair." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 4000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
