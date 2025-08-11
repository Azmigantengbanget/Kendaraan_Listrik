<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
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
        <h1>Pengetahuan Umum Dasar</h1>
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



  { "en": "EV singkatan dari apa?", "id": "Electric Vehicle (Kendaraan Listrik)." },
  { "en": "Sumber tenaga utama EV?", "id": "Listrik dari baterai." },
  { "en": "BEV singkatan dari apa?", "id": "Battery Electric Vehicle." },
  { "en": "BEV sepenuhnya listrik?", "id": "Ya, 100% bertenaga baterai." },
  { "en": "Apa itu mobil hybrid?", "id": "Gabungan mesin bensin dan motor listrik." },
  { "en": "HEV singkatan dari apa?", "id": "Hybrid Electric Vehicle." },
  { "en": "Apakah HEV perlu dicas?", "id": "Tidak, mengisi daya dari mesin." },
  { "en": "PHEV singkatan dari apa?", "id": "Plug-in Hybrid Electric Vehicle." },
  { "en": "Apakah PHEV perlu dicas?", "id": "Ya, bisa diisi daya eksternal." },
  { "en": "PHEV bisa pakai bensin?", "id": "Ya, jika baterai habis." },
  { "en": "Komponen utama EV?", "id": "Baterai, motor listrik, inverter." },
  { "en": "Apa fungsi baterai EV?", "id": "Menyimpan energi listrik." },
  { "en": "Apa fungsi motor listrik?", "id": "Menggerakkan roda mobil." },
  { "en": "Apa fungsi inverter?", "id": "Mengubah arus DC menjadi AC." },
  { "en": "Baterai EV menghasilkan arus?", "id": "Arus searah (DC)." },
  { "en": "Motor listrik EV menggunakan?", "id": "Arus bolak-balik (AC)." },
  { "en": "Keuntungan utama EV?", "id": "Ramah lingkungan, tanpa emisi gas buang." },
  { "en": "EV menghasilkan polusi suara?", "id": "Tidak, suaranya sangat senyap." },
  { "en": "Apa itu 'instant torque'?", "id": "Torsi instan saat pedal diinjak." },
  { "en": "Efek 'instant torque'?", "id": "Akselerasi mobil sangat cepat." },
  { "en": "Biaya perawatan EV?", "id": "Lebih rendah dari mobil bensin." },
  { "en": "Mengapa lebih rendah?", "id": "Lebih sedikit komponen bergerak." },
  { "en": "EV perlu ganti oli?", "id": "Tidak perlu ganti oli mesin." },
  { "en": "Kekurangan utama EV?", "id": "Jarak tempuh terbatas, waktu cas lama." },
  { "en": "Apa itu 'range anxiety'?", "id": "Kekhawatiran kehabisan baterai di jalan." },
  { "en": "Satuan jarak tempuh EV?", "id": "Kilometer (km) per cas penuh." },
  { "en": "Apa itu 'charging station'?", "id": "Stasiun Pengisian Kendaraan Listrik Umum (SPKLU)." },
  { "en": "Di mana mengisi daya EV?", "id": "Di rumah atau SPKLU." },
  { "en": "Apa itu 'AC charging'?", "id": "Pengisian daya arus bolak-balik (lambat)." },
  { "en": "Apa itu 'DC charging'?", "id": "Pengisian daya arus searah (cepat)." },
  { "en": "DC charging disebut juga?", "id": "DC fast charging." },
  { "en": "Mana lebih cepat, AC/DC?", "id": "DC charging jauh lebih cepat." },
  { "en": "Alat cas di rumah?", "id": "Wall charger atau portable charger." },
  { "en": "Jenis baterai paling umum?", "id": "Lithium-ion (Li-ion)." },
  { "en": "Apa itu 'BMS'?", "id": "Battery Management System." },
  { "en": "Fungsi BMS?", "id": "Melindungi dan mengoptimalkan kinerja baterai." },
  { "en": "Satuan kapasitas baterai?", "id": "Kilowatt-hour (kWh)." },
  { "en": "Kapasitas baterai besar berarti?", "id": "Jarak tempuh lebih jauh." },
  { "en": "Apa itu 'regenerative braking'?", "id": "Pengereman yang menghasilkan listrik." },
  { "en": "Bagaimana cara kerjanya?", "id": "Motor listrik menjadi generator saat mengerem." },
  { "en": "Fungsinya?", "id": "Menambah sedikit jarak tempuh." },
  { "en": "Apa itu 'one-pedal driving'?", "id": "Mengemudi hanya dengan pedal akselerator." },
  { "en": "Bagaimana cara mengerem?", "id": "Melepas pedal gas (regenerative braking kuat)." },
  { "en": "Apa itu 'powertrain'?", "id": "Sistem penggerak kendaraan." },
  { "en": "Apa itu 'thermal management'?", "id": "Sistem manajemen suhu baterai." },
  { "en": "Mengapa baterai perlu didinginkan?", "id": "Untuk performa dan keamanan optimal." },
  { "en": "Apa itu 'preconditioning'?", "id": "Menyiapkan suhu baterai sebelum cas/berkendara." },
  { "en": "Apa itu 'frunk'?", "id": "Bagasi depan (front trunk)." },
  { "en": "Mengapa EV punya 'frunk'?", "id": "Karena tidak ada mesin di depan." },
  { "en": "Apa itu 'skateboard platform'?", "id": "Desain sasis EV yang datar." },
  { "en": "Isinya?", "id": "Baterai di lantai, motor di sumbu." },
  { "en": "Keuntungannya?", "id": "Kabin lebih luas, pusat gravitasi rendah." },
  { "en": "Pusat gravitasi rendah membuat?", "id": "Mobil lebih stabil saat menikung." },
  { "en": "Apa itu 'state of charge' (SoC)?", "id": "Tingkat persentase keterisian baterai." },
  { "en": "Apa itu 'depth of discharge' (DoD)?", "id": "Seberapa dalam baterai dikosongkan." },
  { "en": "Umur baterai diukur dalam?", "id": "Siklus pengisian (charge cycles)." },
  { "en": " degradasi baterai?", "id": "Penurunan kapasitas seiring waktu." },
  { "en": "Faktor yang mempengaruhi degradasi?", "id": "Suhu, siklus cas, pengisian cepat." },
  { "en": "Mengisi daya hingga 100%?", "id": "Tidak disarankan untuk penggunaan harian." },
  { "en": "Batas pengisian harian ideal?", "id": "Sekitar 80% hingga 90%." },
  { "en": "Jenis konektor cas di Indonesia?", "id": "Umumnya Type 2 (AC), CCS 2 (DC)." },
  { "en": "Apa itu 'CCS'?", "id": "Combined Charging System." },
  { "en": "Apa itu 'CHAdeMO'?", "id": "Standar konektor cas DC lain." },
  { "en": "Tesla menggunakan konektor apa?", "id": "NACS (North American Charging Standard)." },
  { "en": "Apa itu 'V2L'?", "id": "Vehicle-to-Load." },
  { "en": "Fungsi V2L?", "id": "Mobil menjadi sumber listrik untuk perangkat." },
  { "en": "Apa itu 'V2G'?", "id": "Vehicle-to-Grid." },
  { "en": "Fungsi V2G?", "id": "Mobil menyuplai daya kembali ke jaringan." },
  { "en": "Apa itu 'FCEV'?", "id": "Fuel Cell Electric Vehicle." },
  { "en": "Bahan bakarnya apa?", "id": "Gas hidrogen." },
  { "en": "Emisinya?", "id": "Hanya uap air." },
  { "en": "Apa itu 'fuel cell'?", "id": "Sel bahan bakar." },
  { "en": "Mengubah apa?", "id": "Hidrogen menjadi listrik." },
  { "en": "Tantangan FCEV?", "id": "Infrastruktur pengisian hidrogen." },
  { "en": "Suara buatan pada EV?", "id": "Ya, untuk keamanan pejalan kaki." },
  { "en": "Disebut apa?", "id": "Acoustic Vehicle Alerting System (AVAS)." },
  { "en": "Apa itu 'ICE'?", "id": "Internal Combustion Engine (mesin bensin)." },
  { "en": "Singkatan mobil bensin?", "id": "Kendaraan ICE." },
  { "en": "Apa itu 'MPGe'?", "id": "Miles per gallon equivalent." },
  { "en": "Ukuran efisiensi untuk?", "id": "Kendaraan listrik dan plug-in hybrid." },
  { "en": "Apa itu 'solid-state battery'?", "id": "Baterai dengan elektrolit padat." },
  { "en": "Keunggulannya?", "id": "Lebih aman, kepadatan energi tinggi." },
  { "en": "Masih dalam pengembangan?", "id": "Ya, teknologi masa depan." },
  { "en": "Apa itu 'second-life battery'?", "id": "Baterai EV bekas untuk penggunaan lain." },
  { "en": "Digunakan untuk apa?", "id": "Penyimpanan energi stasioner." },
  { "en": "Pemerintah memberi insentif EV?", "id": "Ya, di banyak negara." },
  { "en": "Contoh insentif?", "id": "Potongan pajak, subsidi pembelian." },
  { "en": "EV lebih berat dari mobil bensin?", "id": "Ya, karena bobot baterai." },
  { "en": "Dampak pada ban?", "id": "Membutuhkan ban khusus EV." },
  { "en": "EV bebas ganjil genap?", "id": "Ya, di beberapa wilayah." },
  { "en": "Apa itu 'over-the-air' (OTA) update?", "id": "Pembaruan perangkat lunak via internet." },
  { "en": "EV modern punya OTA?", "id": "Ya, banyak yang punya." },
  { "en": "Apa itu 'E-mobility'?", "id": "Mobilitas elektronik." },
  { "en": "Apa itu 'LFP'?", "id": "Lithium Iron Phosphate." },
  { "en": "Jenis apa itu?", "id": "Jenis katoda baterai lithium-ion." },
  { "en": "Keunggulan LFP?", "id": "Lebih aman, lebih murah, siklus hidup panjang." },
  { "en": "Apa itu 'NMC'?", "id": "Nickel Manganese Cobalt." },
  { "en": "Keunggulan NMC?", "id": "Kepadatan energi lebih tinggi." },
  { "en": "Apa itu 'Level 1 charging'?", "id": "Pengisian daya lambat di stopkontak biasa." },
  { "en": "Berapa dayanya?", "id": "Sekitar 1-2 kW." },
  { "en": "Berapa lama cas penuh?", "id": "Bisa lebih dari 24 jam." },
  { "en": "Apa itu 'Level 2 charging'?", "id": "Pengisian daya sedang (AC)." },
  { "en": "Ditemukan di mana?", "id": "Rumah (wall charger) dan publik." },
  { "en": "Berapa dayanya?", "id": "Sekitar 3.7 kW hingga 22 kW." },
  { "en": "Berapa lama cas penuh?", "id": "Sekitar 4-8 jam." },
  { "en": "Apa itu 'Level 3 charging'?", "id": "Pengisian daya cepat (DC)." },
  { "en": "Ditemukan di mana?", "id": "SPKLU di jalan tol/umum." },
  { "en": "Berapa dayanya?", "id": "50 kW ke atas." },
  { "en": "Berapa lama cas 80%?", "id": "Sekitar 20-40 menit." },
  { "en": "Mengapa hanya sampai 80%?", "id": "Setelah itu kecepatan menurun drastis." },
  { "en": "Tujuannya?", "id": "Melindungi kesehatan baterai." },
  { "en": "Apa itu 'on-board charger' (OBC)?", "id": "Perangkat di mobil untuk cas AC." },
  { "en": "Fungsinya?", "id": "Mengubah listrik AC menjadi DC." },
  { "en": "Saat cas DC, OBC dipakai?", "id": "Tidak, dayanya langsung ke baterai." },
  { "en": "Apa itu 'ICE-ing'?", "id": "Mobil bensin parkir di tempat cas EV." },
  { "en": "Apa itu 'drivetrain'?", "id": "Sinonim untuk powertrain." },
  { "en": "Apa itu 'motor controller'?", "id": "Otak yang mengatur motor listrik." },
  { "en": "Fungsinya?", "id": "Mengatur kecepatan dan torsi motor." },
  { "en": "Apa itu 'cell'?", "id": "Sel, unit dasar baterai." },
  { "en": "Apa itu 'module'?", "id": "Modul, gabungan beberapa sel baterai." },
  { "en": "Apa itu 'battery pack'?", "id": "Paket baterai, gabungan banyak modul." },
  { "en": "Baterai EV adalah 'battery pack'?", "id": "Ya." },
  { "en": "Apa itu 'energy density'?", "id": "Kepadatan energi." },
  { "en": "Artinya?", "id": "Energi per satuan berat/volume." },
  { "en": "Baterai LFP punya kepadatan energi?", "id": "Lebih rendah dari NMC." },
  { "en": "Apa itu 'power density'?", "id": "Kepadatan daya." },
  { "en": "Artinya?", "id": "Daya per satuan berat/volume." },
  { "en": "Apa itu 'cycle life'?", "id": "Jumlah siklus isi-ulang baterai." },
  { "en": "Baterai LFP punya 'cycle life'?", "id": "Sangat tinggi." },
  { "en": "Apa itu 'thermal runaway'?", "id": "Reaksi berantai panas pada baterai." },
  { "en": "Menyebabkan apa?", "id": "Kebakaran atau ledakan." },
  { "en": "Baterai LFP lebih aman?", "id": "Ya, lebih tahan thermal runaway." },
  { "en": "Apa itu 'range'?", "id": "Jarak tempuh." },
  { "en": "Faktor yang mempengaruhi 'range'?", "id": "Kecepatan, suhu, gaya mengemudi." },
  { "en": "AC/pemanas mengurangi 'range'?", "id": "Ya, mengurangi secara signifikan." },
  { "en": "Suhu dingin mengurangi 'range'?", "id": "Ya." },
  { "en": "Apa itu 'drag coefficient'?", "id": "Ukuran hambatan aerodinamis." },
  { "en": "EV punya 'drag' rendah?", "id": "Ya, untuk meningkatkan efisiensi." },
  { "en": "Apa itu 'rolling resistance'?", "id": "Hambatan dari ban yang berputar." },
  { "en": "Ban EV dirancang untuk?", "id": "Memiliki rolling resistance rendah." },
  { "en": "Listrik untuk EV dari mana?", "id": "Dari jaringan listrik (PLN)." },
  { "en": "Jika listrik dari batu bara?", "id": "EV tetap lebih bersih secara total." },
  { "en": "Mengapa?", "id": "Efisiensi pembangkit vs mesin bensin." },
  { "en": "Apa itu 'well-to-wheel'?", "id": "Analisis emisi dari sumber ke roda." },
  { "en": "Apa itu 'tank-to-wheel'?", "id": "Analisis emisi dari tangki ke roda." },
  { "en": "EV punya emisi 'tank-to-wheel'?", "id": "Tidak, emisinya nol." },
  { "en": "Apa itu 'embodied carbon'?", "id": "Emisi dari proses produksi." },
  { "en": "Produksi baterai punya emisi?", "id": "Ya, cukup signifikan." },
  { "en": "Seiring waktu, EV lebih bersih?", "id": "Ya, setelah melewati titik impas." },
  { "en": "Apa itu 'BEV' vs 'FCEV'?", "id": "Baterai vs hidrogen." },
  { "en": "Mana lebih efisien?", "id": "BEV lebih efisien secara total." },
  { "en": "Apa itu 'idle fee'?", "id": "Biaya jika mobil selesai cas tidak dipindah." },
  { "en": "Tujuannya?", "id": "Agar SPKLU bisa dipakai bergantian." },
  { "en": "Apa itu 'smart charging'?", "id": "Pengisian daya cerdas." },
  { "en": "Maksudnya?", "id": "Mengisi daya saat listrik murah/bersih." },
  { "en": "Apa itu 'AC' vs 'DC'?", "id": "Arus bolak-balik vs arus searah." },
  { "en": "Baterai menyimpan listrik sebagai?", "id": "Arus searah (DC)." },
  { "en": "Jaringan listrik menyuplai?", "id": "Arus bolak-balik (AC)." },
  { "en": "Apa itu 'voltage'?", "id": "Tegangan (Volt)." },
  { "en": "Apa itu 'ampere'?", "id": "Arus (Amp)." },
  { "en": "Apa itu 'watt'?", "id": "Daya (Volt x Amp)." },
  { "en": "Apa itu 'kWh'?", "id": "Kilowatt-hour, satuan energi." },
  { "en": "Tagihan listrik dalam kWh?", "id": "Ya." },
  { "en": "Apa itu 'converter' DC-DC?", "id": "Mengubah tegangan DC ke level lain." },
  { "en": "Digunakan di EV?", "id": "Ya, untuk sistem 12V." },
  { "en": "Aki 12V di EV?", "id": "Ya, untuk lampu, radio, dll." },
  { "en": "Apa itu 'inverter' vs 'converter'?", "id": "Inverter DC-AC, converter DC-DC." },
  { "en": "Apa itu 'MCU'?", "id": "Motor Control Unit." },
  { "en": "Apa itu 'VCU'?", "id": "Vehicle Control Unit." },
  { "en": "VCU adalah otak mobil?", "id": "Ya, otak utama EV." },
  { "en": "Apa itu 'OTA'?", "id": "Over-the-Air update." },
  { "en": "Apa itu 'CAN bus'?", "id": "Jaringan komunikasi di dalam mobil." },
  { "en": "Apa itu 'EVSE'?", "id": "Electric Vehicle Supply Equipment." },
  { "en": "Nama teknis untuk apa?", "id": "Stasiun pengisian daya (charger)." },
  { "en": "Apa itu 'connector'?", "id": "Konektor atau colokan cas." },
  { "en": "Apa itu 'inlet'?", "id": "Soket cas di mobil." },
  { "en": "Apa itu 'range extender'?", "id": "Penambah jarak tempuh." },
  { "en": "Contoh 'range extender'?", "id": "Mesin bensin kecil di beberapa PHEV." },
  { "en": "Apa itu 'mild hybrid' (MHEV)?", "id": "Hybrid dengan bantuan listrik kecil." },
  { "en": "MHEV bisa jalan listrik saja?", "id": "Tidak, hanya membantu mesin bensin." },
  { "en": "Apa itu 'full hybrid'?", "id": "Bisa jalan dengan listrik saja (jarak pendek)." },
  { "en": "Toyota Prius adalah contoh?", "id": "Full hybrid (HEV)." },
  { "en": "Apa itu 'NEV'?", "id": "New Energy Vehicle." },
  { "en": "Istilah yang populer di mana?", "id": "Tiongkok." },
  { "en": "Apa itu 'ZEV'?", "id": "Zero Emission Vehicle." },
  { "en": "BEV dan FCEV termasuk ZEV?", "id": "Ya." },
  { "en": "Apa itu 'micromobility'?", "id": "Kendaraan kecil untuk perjalanan singkat." },
  { "en": "Contohnya?", "id": "Sepeda listrik, skuter listrik." },
  { "en": "Apa itu 'e-bike'?", "id": "Sepeda listrik." },
  { "en": "Apa itu 'e-scooter'?", "id": "Skuter listrik." },
  { "en": "Apa itu 'last mile'?", "id": "Perjalanan terakhir dari hub transportasi." },
  { "en": "Micromobility cocok untuk 'last mile'?", "id": "Ya, sangat cocok." },
  { "en": "Apa itu 'battery swapping station'?", "id": "Stasiun penukaran baterai." },
  { "en": "Populer untuk apa?", "id": "Motor listrik." },
  { "en": "Apa itu 'AC motor' vs 'DC motor'?", "id": "Motor arus AC vs motor arus DC." },
  { "en": "EV modern menggunakan motor?", "id": "Umumnya motor AC." },
  { "en": "Jenis motor AC di EV?", "id": "Induction motor, permanent magnet motor." },
  { "en": "Apa itu 'induction motor'?", "id": "Motor induksi." },
  { "en": "Siapa yang mempopulerkannya di EV?", "id": "Tesla." },
  { "en": "Apa itu 'permanent magnet motor'?", "id": "Motor magnet permanen." },
  { "en": "Keunggulannya?", "id": "Sangat efisien dan bertenaga." },
  { "en": "Menggunakan apa?", "id": "Logam tanah jarang (rare-earth magnets)." },
  { "en": "Apa itu 'B-segment', 'C-segment'?", "id": "Klasifikasi ukuran mobil." },
  { "en": "Wuling Air EV termasuk segmen?", "id": "Segmen A (city car)." },
  { "en": "Hyundai Ioniq 5 termasuk segmen?", "id": "Segmen D (SUV/Crossover)." },
  { "en": "Apa itu 'crossover'?", "id": "Mobil yang menggabungkan desain SUV dan hatchback." },
  { "en": "Apa itu 'SUV'?", "id": "Sport Utility Vehicle." },
  { "en": "Apa itu 'sedan'?", "id": "Mobil penumpang dengan tiga kompartemen." },
  { "en": "Apa itu 'hatchback'?", "id": "Mobil dengan pintu bagasi menyatu kaca." },
  { "en": "Apa itu 'ground clearance'?", "id": "Jarak terendah mobil ke tanah." },
  { "en": "Apa itu 'wheelbase'?", "id": "Jarak antara roda depan dan belakang." },
  { "en": "Wheelbase panjang menghasilkan?", "id": "Kabin lebih lega, pengendaraan lebih stabil." },
  { "en": "Apa itu 'anoda'?", "id": "Elektroda negatif baterai." },
  { "en": "Apa itu 'katoda'?", "id": "Elektroda positif baterai." },
  { "en": "Apa itu 'elektrolit'?", "id": "Cairan penghantar ion di baterai." },
  { "en": "Baterai solid-state menggunakan elektrolit?", "id": "Elektrolit padat." },
  { "en": "Baterai LFP menggunakan katoda apa?", "id": "Lithium Iron Phosphate." },
  { "en": "Baterai NMC menggunakan katoda apa?", "id": "Nickel Manganese Cobalt." },
  { "en": "Anoda EV saat ini terbuat dari?", "id": "Grafit." },
  { "en": "Anoda masa depan?", "id": "Silikon." },
  { "en": "Apa itu 'gigafactory'?", "id": "Pabrik baterai skala sangat besar." },
  { "en": "Siapa yang mempopulerkan istilah ini?", "id": "Tesla." },
  { "en": "Apa itu 'cell-to-pack'?", "id": "Teknologi baterai tanpa modul." },
  { "en": "Keuntungannya?", "id": "Kepadatan energi lebih tinggi." },
  { "en": "Apa itu 'cell-to-chassis'?", "id": "Baterai menjadi bagian dari sasis." },
  { "en": "Keuntungannya?", "id": "Struktur lebih kaku, efisiensi ruang." },
  { "en": "Apa itu 'cooling system'?", "id": "Sistem pendingin." },
  { "en": "Pendingin baterai EV menggunakan apa?", "id": "Cairan (liquid cooling) atau udara." },
  { "en": "Mana yang lebih efektif?", "id": "Pendingin cairan." },
  { "en": "Apa itu 'heat pump'?", "id": "Pompa panas." },
  { "en": "Digunakan di EV untuk apa?", "id": "Pemanas kabin yang sangat efisien." },
  { "en": "Meningkatkan 'range' di cuaca dingin?", "id": "Ya, secara signifikan." },
  { "en": "Apa itu 'phantom drain'?", "id": "Baterai berkurang saat mobil diparkir." },
  { "en": "Disebut juga?", "id": "Vampire drain." },
  { "en": "Disebabkan oleh apa?", "id": "Sistem mobil yang tetap aktif." },
  { "en": "Apa itu 'kilowatt' (kW)?", "id": "Satuan daya (power)." },
  { "en": "Tenaga motor diukur dalam?", "id": "kW atau horsepower." },
  { "en": "Kecepatan cas diukur dalam?", "id": "kW." },
  { "en": "Apa itu 'horsepower' (hp)?", "id": "Satuan daya lain." },
  { "en": "1 kW berapa hp?", "id": "Sekitar 1.34 hp." },
  { "en": "Apa itu 'torque'?", "id": "Torsi atau momen putar." },
  { "en": "Satuannya?", "id": "Newton-meter (Nm)." },
  { "en": "Torsi instan berarti?", "id": "Torsi maksimum sejak 0 RPM." },
  { "en": "Apa itu 'RPM'?", "id": "Rotations Per Minute." },
  { "en": "Apa itu 'FWD', 'RWD', 'AWD'?", "id": "Jenis penggerak roda mobil." },
  { "en": "Apa itu 'FWD'?", "id": "Front-Wheel Drive (penggerak roda depan)." },
  { "en": "Apa itu 'RWD'?", "id": "Rear-Wheel Drive (penggerak roda belakang)." },
  { "en": "Apa itu 'AWD'?", "id": "All-Wheel Drive (penggerak semua roda)." },
  { "en": "EV bisa jadi AWD?", "id": "Ya, dengan dua motor (dual motor)." },
  { "en": "Satu motor di depan, satu di belakang?", "id": "Ya." },
  { "en": "Apa itu 'e-axle'?", "id": "Sistem penggerak terintegrasi (motor, inverter)." },
  { "en": "Apa itu 'charging curve'?", "id": "Grafik kecepatan cas vs SoC." },
  { "en": "Kecepatan cas paling tinggi saat?", "id": "SoC rendah." },
  { "en": "Kecepatan cas menurun saat?", "id": "Baterai hampir penuh." },
  { "en": "Apa itu 'coldgate'?", "id": "Pembatasan cas cepat karena baterai dingin." },
  { "en": "Apa itu 'bidirectional charging'?", "id": "Pengisian daya dua arah (V2L/V2G)." },
  { "en": "Apa itu 'roaming' (charging)?", "id": "Menggunakan jaringan SPKLU yang berbeda." },
  { "en": "Apa itu 'plug and charge'?", "id": "Otentikasi cas otomatis saat dicolok." },
  { "en": "Apa itu 'battery swapping'?", "id": "Menukar baterai kosong dengan yang penuh." },
  { "en": "Lebih cepat dari fast charging?", "id": "Ya, hanya butuh beberapa menit." },
  { "en": "Tantangannya?", "id": "Standarisasi baterai dan biaya infrastruktur." },
  { "en": "Populer untuk motor listrik?", "id": "Ya, sangat populer." },
  { "en": "Apa itu 'suspensi'?", "id": "Sistem peredam kejut mobil." },
  { "en": "EV lebih berat, butuh suspensi?", "id": "Suspensi yang lebih kuat." },
  { "en": "Apa itu 'low-rolling-resistance tires'?", "id": "Ban dengan hambatan gulir rendah." },
  { "en": "Apa itu 'aerodynamic wheels'?", "id": "Pelek aerodinamis." },
  { "en": "Tujuannya?", "id": "Mengurangi hambatan udara, menambah 'range'." },
  { "en": "Apa itu 'TPMS'?", "id": "Tire Pressure Monitoring System." },
  { "en": "Mengukur apa?", "id": "Tekanan angin ban." },
  { "en": "Tekanan ban yang tepat penting?", "id": "Ya, untuk efisiensi dan keamanan." },
  { "en": "Apa itu 'ABS'?", "id": "Anti-lock Braking System." },
  { "en": "Mencegah apa?", "id": "Roda terkunci saat pengereman mendadak." },
  { "en": "Apa itu 'ESP' atau 'ESC'?", "id": "Electronic Stability Program/Control." },
  { "en": "Mencegah apa?", "id": "Mobil tergelincir (oversteer/understeer)." },
  { "en": "EV punya fitur keselamatan canggih?", "id": "Ya, seringkali menjadi standar." },
  { "en": "Apa itu 'ADAS'?", "id": "Advanced Driver-Assistance Systems." },
  { "en": "Contoh ADAS?", "id": "Adaptive cruise control, lane keeping assist." },
  { "en": "Apa itu 'adaptive cruise control'?", "id": "Menjaga jarak aman dengan mobil depan." },
  { "en": "Apa itu 'lane keeping assist'?", "id": "Membantu mobil tetap di jalurnya." },
  { "en": "Apa itu 'autonomous driving'?", "id": "Mengemudi otonom." },
  { "en": "Levelnya?", "id": "Dari 0 hingga 5." },
  { "en": "Level 5 berarti?", "id": "Otonomi penuh, tanpa perlu setir." },
  { "en": "Apa itu 'lithium'?", "id": "Logam ringan, komponen utama baterai." },
  { "en": "Apa itu 'cobalt'?", "id": "Logam yang digunakan di katoda NMC." },
  { "en": "Isu terkait kobalt?", "id": "Penambangan yang tidak etis." },
  { "en": "Baterai LFP bebas kobalt?", "id": "Ya." },
  { "en": "Apa itu 'nickel'?", "id": "Logam lain di katoda NMC." },
  { "en": "Apa itu 'graphite'?", "id": "Grafit, bahan utama anoda." },
  { "en": "Daur ulang baterai penting?", "id": "Ya, untuk memulihkan logam berharga." },
  { "en": "Apa itu 'circular economy'?", "id": "Ekonomi sirkular." },
  { "en": "Apa itu 'TCO'?", "id": "Total Cost of Ownership." },
  { "en": "TCO EV lebih rendah?", "id": "Ya, dalam jangka panjang." },
  { "en": "Karena apa?", "id": "Biaya 'bahan bakar' dan perawatan lebih rendah." },
  { "en": "Apa itu 'depresiasi'?", "id": "Penurunan nilai jual kembali." },
  { "en": "Depresiasi EV saat ini?", "id": "Cenderung lebih tinggi, tapi membaik." },
  { "en": "Apa itu 'resale value'?", "id": "Sinonim untuk nilai jual kembali." },
  { "en": "Listrik untuk EV lebih murah?", "id": "Ya, jauh lebih murah dari bensin." },
  { "en": "Apa itu 'home charging'?", "id": "Mengisi daya di rumah." },
  { "en": "Apa itu 'public charging'?", "id": "Mengisi daya di tempat umum." },
  { "en": "Cas di rumah butuh daya?", "id": "Daya listrik rumah yang cukup." },
  { "en": "Minimal berapa Ampere?", "id": "Disarankan minimal 16A (3500W)." },
  { "en": "Apa itu 'destination charging'?", "id": "Cas di tujuan (hotel, mal)." },
  { "en": "Biasanya jenis cas apa?", "id": "AC charging (Level 2)." },
  { "en": "Apa itu 'opportunity charging'?", "id": "Cas singkat saat ada kesempatan." },
  { "en": "Apa itu 'WLTP'?", "id": "Standar pengukuran jarak tempuh global." },
  { "en": "WLTP singkatan dari?", "id": "Worldwide Harmonised Light Vehicle Test Procedure." },
  { "en": "Lebih akurat dari NEDC?", "id": "Ya, jauh lebih realistis." },
  { "en": "Apa itu 'NEDC'?", "id": "Standar pengukuran yang lebih tua." },
  { "en": "Apa itu 'EPA range'?", "id": "Standar jarak tempuh di AS." },
  { "en": "Dianggap paling akurat?", "id": "Ya, EPA seringkali paling mendekati." },
  { "en": "Apa itu 'phantom braking'?", "id": "Mobil mengerem sendiri tanpa alasan." },
  { "en": "Masalah pada sistem apa?", "id": "Masalah pada sistem ADAS." },
  { "en": "Apa itu 'state of health' (SoH)?", "id": "Kondisi kesehatan baterai." },
  { "en": "SoH menurun seiring waktu?", "id": "Ya, karena degradasi." },
  { "en": "Garansi baterai EV berapa lama?", "id": "Umumnya 8 tahun atau lebih." },
  { "en": "Mencakup apa?", "id": "Jika SoH turun di bawah 70%." },
  { "en": "Apakah EV butuh radiator?", "id": "Ya, untuk mendinginkan baterai/motor." },
  { "en": "Apa itu 'glycol'?", "id": "Cairan pendingin (coolant)." },
  { "en": "Apa itu 'gearbox'?", "id": "Girboks atau transmisi." },
  { "en": "EV punya 'gearbox'?", "id": "Tidak, umumnya transmisi satu percepatan." },
  { "en": "Mengapa?", "id": "Karena rentang RPM motor sangat lebar." },
  { "en": "Beberapa EV performa tinggi punya?", "id": "Transmisi dua percepatan." },
  { "en": "Apa itu 'carbon fiber'?", "id": "Serat karbon." },
  { "en": "Digunakan di EV?", "id": "Ya, untuk mengurangi bobot." },
  { "en": "Mahal?", "id": "Ya, sangat mahal." },
  { "en": "EV pertama kali dibuat kapan?", "id": "Abad ke-19." },
  { "en": "Lebih dulu dari mobil bensin?", "id": "Ya, sekitar waktu yang sama." },
  { "en": "Mengapa dulu tidak populer?", "id": "Keterbatasan baterai dan penemuan minyak." },
  { "en": "Siapa yang mempopulerkan EV modern?", "id": "Tesla." },
  { "en": "Model Tesla pertama?", "id": "Tesla Roadster (2008)." },
  { "en": "Apa itu 'TCO parity'?", "id": "Saat TCO EV sama dengan mobil bensin." },
  { "en": "Apa itu 'price parity'?", "id": "Saat harga beli EV sama." },
  { "en": "Kapan 'price parity' diprediksi?", "id": "Sekitar tahun 2025-2030." },
  { "en": "Apa itu 'giga casting'?", "id": "Mencetak bagian sasis besar." },
  { "en": "Siapa yang mempopulerkannya?", "id": "Tesla." },
  { "en": "Keuntungannya?", "id": "Mengurangi jumlah komponen dan biaya." },
  { "en": "Apa itu 'battery recycling'?", "id": "Daur ulang baterai." },
  { "en": "Prosesnya?", "id": "Pyrometallurgy dan hydrometallurgy." },
  { "en": "Apa itu 'pyrometallurgy'?", "id": "Daur ulang menggunakan panas tinggi." },
  { "en": "Apa itu 'hydrometallurgy'?", "id": "Daur ulang menggunakan larutan kimia." },
  { "en": "Mana yang lebih ramah lingkungan?", "id": "Hydrometallurgy, jika dikelola baik." },
  { "en": "Apa itu 'urban mining'?", "id": "Menambang logam dari sampah elektronik." },
  { "en": "Apa itu 'critical minerals'?", "id": "Mineral penting untuk transisi energi." },
  { "en": "Contohnya?", "id": "Lithium, kobalt, nikel, grafit." },
  { "en": "Apa itu 'ethical sourcing'?", "id": "Pengadaan bahan baku yang etis." },
  { "en": "Apa itu 'supply chain'?", "id": "Rantai pasok." },
  { "en": "Tantangan rantai pasok baterai?", "id": "Terkonsentrasi di beberapa negara." },
  { "en": "Negara produsen lithium terbesar?", "id": "Australia, Chili, Tiongkok." },
  { "en": "Negara produsen kobalt terbesar?", "id": "Republik Demokratik Kongo." },
  { "en": "Apa itu 'geopolitik'?", "id": "Politik yang dipengaruhi oleh geografi." },
  { "en": "EV mengubah geopolitik energi?", "id": "Ya, dari minyak ke mineral." },
  { "en": "Apa itu 'OPEC'?", "id": "Organisasi negara pengekspor minyak." },
  { "en": "Apa itu 'kartel'?", "id": "Kelompok produsen yang mengatur harga." },
  { "en": "Apa itu 'range' di musim dingin?", "id": "Berkurang karena suhu dingin." },
  { "en": "Mengapa?", "id": "Reaksi kimia baterai melambat." },
  { "en": "Apa itu 'AC' di mobil?", "id": "Air Conditioner (pendingin)." },
  { "en": "Apa itu 'HVAC'?", "id": "Heating, Ventilation, and Air Conditioning." },
  { "en": "HVAC mengurangi 'range'?", "id": "Ya, terutama pemanas (heating)." },
  { "en": "Pemanas EV menggunakan apa?", "id": "Pemanas resistif atau pompa panas." },
  { "en": "Mana lebih efisien?", "id": "Pompa panas (heat pump)." },
  { "en": "Apa itu 'heated seats'?", "id": "Kursi dengan pemanas." },
  { "en": "Lebih efisien dari pemanas kabin?", "id": "Ya, jauh lebih efisien." },
  { "en": "Apa itu 'infotainment system'?", "id": "Sistem informasi dan hiburan di mobil." },
  { "en": "EV punya layar besar?", "id": "Ya, menjadi ciri khas." },
  { "en": "Apa itu 'user interface' (UI)?", "id": "Antarmuka pengguna." },
  { "en": "Apa itu 'user experience' (UX)?", "id": "Pengalaman pengguna." },
  { "en": "EV butuh 'charging planner'?", "id": "Ya, untuk perjalanan jauh." },
  { "en": "Fungsinya?", "id": "Merencanakan pemberhentian cas secara otomatis." },
  { "en": "Mempertimbangkan apa?", "id": "Topografi, suhu, dan ketersediaan SPKLU." },
  { "en": "Apa itu 'topography'?", "id": "Bentuk permukaan daratan (naik/turun)." },
  { "en": "Jalan menanjak boros baterai?", "id": "Ya." },
  { "en": "Jalan menurun?", "id": "Bisa mengisi baterai (regenerasi)." },
  { "en": "Apa itu 'coefficient of performance' (COP)?", "id": "Ukuran efisiensi pompa panas." },
  { "en": "Apa itu 'plug-in grant'?", "id": "Subsidi atau hibah untuk pembelian EV." },
  { "en": "Apa itu 'tax credit'?", "id": "Kredit atau potongan pajak." },
  { "en": "Apa itu 'congestion charge'?", "id": "Biaya kemacetan di pusat kota." },
  { "en": "EV seringkali bebas dari ini?", "id": "Ya." },
  { "en": "Apa itu 'low emission zone' (LEZ)?", "id": "Zona rendah emisi." },
  { "en": "EV boleh masuk?", "id": "Ya, karena emisinya nol." },
  { "en": "Apa itu 'phase-out'?", "id": "Penghentian secara bertahap." },
  { "en": "Banyak negara merencanakan 'phase-out'?", "id": "Ya, untuk mobil bensin/diesel." },
  { "en": "Kapan targetnya?", "id": "Bervariasi, antara 2030 hingga 2040." },
  { "en": "Apa itu 'grid stability'?", "id": "Stabilitas jaringan listrik." },
  { "en": "EV bisa membantu atau mengganggu?", "id": "Bisa keduanya, tergantung manajemen." },
  { "en": "Smart charging membantu?", "id": "Ya, sangat membantu." },
  { "en": "Apa itu 'duck curve'?", "id": "Grafik permintaan listrik." },
  { "en": "Bentuknya seperti bebek?", "id": "Ya." },
  { "en": "EV bisa membantu 'duck curve'?", "id": "Ya, dengan mengisi daya di siang hari." },
  { "en": "Apa itu 'right-sizing' a battery?", "id": "Memilih ukuran baterai yang tepat." },
  { "en": "Baterai lebih besar selalu lebih baik?", "id": "Tidak selalu, menambah bobot dan biaya." },
  { "en": "Apa itu 'EV conversion'?", "id": "Mengubah mobil bensin menjadi listrik." },
  { "en": "Disebut juga?", "id": "Retrofitting." },
  { "en": "Apakah legal?", "id": "Tergantung regulasi di setiap negara." },
  { "en": "Apa itu 'crate motor'?", "id": "Motor listrik siap pasang." },
  { "en": "Untuk apa?", "id": "Memudahkan proses konversi EV." },
  { "en": "Apa itu 'battery fire'?", "id": "Kebakaran baterai." },
  { "en": "Sulit dipadamkan?", "id": "Ya, sangat sulit." },
  { "en": "Mengapa?", "id": "Karena ada 'thermal runaway'." },
  { "en": "Butuh air banyak?", "id": "Ya, untuk mendinginkan baterai." },
  { "en": "Apa itu 'submersion tank'?", "id": "Tangki untuk merendam EV terbakar." },
  { "en": "Apa itu 'kabel oranye'?", "id": "Kabel tegangan tinggi di EV." },
  { "en": "Berbahaya disentuh?", "id": "Ya, sangat berbahaya." },
  { "en": "Apa itu 'service disconnect'?", "id": "Pemutus darurat sistem tegangan tinggi." },
  { "en": "Apa itu 'sound symposer'?", "id": "Sistem suara buatan di dalam kabin." },
  { "en": "Tujuannya?", "id": "Memberi sensasi suara mesin." },
  { "en": "Apa itu 'active noise cancellation'?", "id": "Mengurangi kebisingan jalan secara aktif." },
  { "en": "Bagaimana cara kerjanya?", "id": "Menghasilkan gelombang suara terbalik." },
  { "en": "Apa itu 'battery pack' vs 'fuel tank'?", "id": "Penyimpan listrik vs penyimpan bensin." },
  { "en": "Mana lebih berat?", "id": "Battery pack jauh lebih berat." },
  { "en": "Apa itu 'charging port'?", "id": "Port pengisian daya." },
  { "en": "Apa itu 'AC' vs 'DC' charger?", "id": "Charger AC vs charger DC." },
  { "en": "On-board charger untuk cas?", "id": "AC charging." },
  { "en": "Apa itu 'swappable battery'?", "id": "Baterai yang bisa ditukar." },
  { "en": "Apa itu 'kW' vs 'kWh'?", "id": "Daya (power) vs energi." },
  { "en": "Efisiensi EV diukur dalam?", "id": "kWh per 100 km." },
  { "en": "Nilai lebih rendah berarti?", "id": "Mobil lebih efisien." },
  { "en": "Apa itu 'rolling chassis'?", "id": "Sasis lengkap dengan roda dan powertrain." },
  { "en": "Apa itu 'body-on-frame'?", "id": "Konstruksi sasis dan bodi terpisah." },
  { "en": "Apa itu 'unibody'?", "id": "Konstruksi sasis dan bodi menyatu." },
  { "en": "EV modern menggunakan konstruksi?", "id": "Unibody atau skateboard." },
  { "en": "Apa itu 'crash structure'?", "id": "Struktur mobil untuk menyerap benturan." },
  { "en": "Baterai bagian dari struktur?", "id": "Ya, di desain skateboard." },
  { "en": "Apa itu 'crumple zone'?", "id": "Zona remuk untuk menyerap energi." },
  { "en": "Apa itu 'occupant safety'?", "id": "Keselamatan penumpang." },
  { "en": "EV cenderung aman?", "id": "Ya, karena pusat gravitasi rendah." },
  { "en": "Apa itu 'rollover risk'?", "id": "Risiko mobil terguling." },
  { "en": "EV punya risiko ini?", "id": "Sangat rendah." },
  { "en": "Apa itu 'lithium triangle'?", "id": "Wilayah kaya lithium di Amerika Selatan." },
  { "en": "Negaranya?", "id": "Argentina, Bolivia, Chili." },
  { "en": "Apa itu 'brine extraction'?", "id": "Ekstraksi lithium dari air garam." },
  { "en": "Apa itu 'hard-rock mining'?", "id": "Penambangan lithium dari batuan." },
  { "en": "Mana lebih boros air?", "id": "Brine extraction." },
  { "en": "Apa itu 'direct lithium extraction' (DLE)?", "id": "Teknologi ekstraksi lithium baru." },
  { "en": "Tujuannya?", "id": "Lebih cepat dan ramah lingkungan." },
  { "en": "Apa itu 'cobalt-free battery'?", "id": "Baterai tanpa kobalt." },
  { "en": "Baterai LFP termasuk?", "id": "Ya, baterai LFP bebas kobalt." },
  { "en": "Apa itu 'sodium-ion battery'?", "id": "Baterai berbasis natrium (sodium)." },
  { "en": "Potensinya?", "id": "Sangat murah dan melimpah." },
  { "en": "Kekurangannya?", "id": "Kepadatan energi lebih rendah." },
  { "en": "Apa itu 'wireless charging'?", "id": "Pengisian daya nirkabel." },
  { "en": "Disebut juga?", "id": "Inductive charging." },
  { "en": "Bagaimana cara kerjanya?", "id": "Menggunakan medan magnet." },
  { "en": "Sudah umum untuk EV?", "id": "Belum, masih dalam pengembangan." },
  { "en": "Apa itu 'dynamic wireless charging'?", "id": "Mengisi daya saat mobil bergerak." },
  { "en": "Contohnya?", "id": "Jalan raya yang bisa mengecas." },
  { "en": "Masih dalam tahap riset?", "id": "Ya, sangat futuristik." },
  { "en": "Apa itu 'solar car'?", "id": "Mobil yang ditenagai panel surya." },
  { "en": "Bisa jadi sumber daya utama?", "id": "Belum, hanya sebagai penambah 'range'." },
  { "en": "Apa itu 'kerb weight'?", "id": "Bobot kosong kendaraan." },
  { "en": "Apa itu 'payload'?", "id": "Kapasitas angkut maksimum." },
  { "en": "Apa itu 'GVWR'?", "id": "Gross Vehicle Weight Rating." },
  { "en": "Artinya?", "id": "Total bobot maksimum yang diizinkan." },
  { "en": "Apa itu 'towing capacity'?", "id": "Kapasitas menarik (gandeng)." },
  { "en": "EV bisa menarik beban berat?", "id": "Ya, torsi instan sangat membantu." },
  { "en": "Apa itu 'electric truck'?", "id": "Truk listrik." },
  { "en": "Contohnya?", "id": "Tesla Semi." },
  { "en": "Apa itu 'electric bus'?", "id": "Bus listrik." },
  { "en": "Sudah beroperasi di Jakarta?", "id": "Ya, sebagai TransJakarta." },
  { "en": "Apa itu 'electric ferry'?", "id": "Kapal feri listrik." },
  { "en": "Apa itu 'electric airplane'?", "id": "Pesawat listrik." },
  { "en": "Tantangan utamanya?", "id": "Kepadatan energi baterai." },
  { "en": "Apa itu 'eVTOL'?", "id": "Electric Vertical Take-Off and Landing." },
  { "en": "Disebut juga?", "id": "Taksi terbang atau mobil terbang." },
  { "en": "Apa itu 'cold weather package'?", "id": "Fitur tambahan untuk cuaca dingin." },
  { "en": "Contohnya?", "id": "Heated seats, heat pump." },
  { "en": "Apa itu 'charging etiquette'?", "id": "Etiket menggunakan SPKLU." },
  { "en": "Contohnya?", "id": "Jangan 'ICE-ing', jangan 'idle'." },
  { "en": "Apa itu 'phantom drain' vs 'idle drain'?", "id": "Sama, artinya baterai berkurang." },
  { "en": "Apa itu 'Sentry Mode'?", "id": "Fitur keamanan Tesla." },
  { "en": "Mengonsumsi banyak daya?", "id": "Ya, bisa mengurangi baterai signifikan." },
  { "en": "Apa itu 'dog mode'?", "id": "Fitur Tesla untuk hewan peliharaan." },
  { "en": "Fungsinya?", "id": "Menjaga suhu kabin tetap nyaman." },
  { "en": "Apa itu 'Camp Mode'?", "id": "Fitur Tesla untuk berkemah." },
  { "en": "Fungsinya?", "id": "Menjaga aliran udara, suhu, dan daya." },
  { "en": "Apa itu 'Supercharger'?", "id": "Jaringan DC fast charging Tesla." },
  { "en": "Apa itu 'Destination Charger'?", "id": "Charger AC Level 2 Tesla." },
  { "en": "Jaringan Tesla terbuka untuk umum?", "id": "Mulai terbuka di beberapa wilayah." },
  { "en": "Apa itu 'GOM'?", "id": "Guess-O-Meter." },
  { "en": "Menunjukkan apa?", "id": "Estimasi sisa jarak tempuh." },
  { "en": "Apakah selalu akurat?", "id": "Tidak, hanya sebuah estimasi." },
  { "en": "Apa itu 'battery buffer'?", "id": "Kapasitas baterai yang tidak bisa diakses." },
  { "en": "Tujuannya?", "id": "Melindungi baterai dari kerusakan." },
  { "en": "Kapasitas 'usable' vs 'total'?", "id": "Total dikurangi buffer adalah usable." },
  { "en": "Apa itu 'bricking' a battery?", "id": "Baterai menjadi rusak total." },
  { "en": "Penyebabnya?", "id": "Dibiarkan kosong total terlalu lama." },
  { "en": "Apa itu 'balance charging'?", "id": "Menyeimbangkan tegangan di setiap sel." },
  { "en": "Penting untuk kesehatan baterai?", "id": "Ya, sangat penting." },
  { "en": "Dilakukan oleh siapa?", "id": "BMS (Battery Management System)." },
  { "en": "Apa itu 'anode' vs 'cathode'?", "id": "Anoda vs katoda." },
  { "en": "Saat 'discharging', elektron mengalir?", "id": "Dari anoda ke katoda." },
  { "en": "Saat 'charging', elektron mengalir?", "id": "Dari katoda ke anoda." },
  { "en": "Ion lithium bergerak ke arah mana?", "id": "Mengikuti aliran elektron." },
  { "en": "Apa itu 'C-rate'?", "id": "Ukuran kecepatan cas/decas baterai." },
  { "en": "1C berarti cas penuh dalam?", "id": "Satu jam." },
  { "en": "2C berarti cas penuh dalam?", "id": "Setengah jam (30 menit)." },
  { "en": "Fast charging menggunakan C-rate?", "id": "Tinggi, misalnya 1C atau lebih." },
  { "en": "Apa itu 'single-phase' power?", "id": "Daya listrik satu fasa." },
  { "en": "Digunakan di mana?", "id": "Rumah tangga pada umumnya." },
  { "en": "Apa itu 'three-phase' power?", "id": "Daya listrik tiga fasa." },
  { "en": "Digunakan di mana?", "id": "Industri dan beberapa SPKLU." },
  { "en": "Mana yang bisa menyuplai daya lebih besar?", "id": "Tiga fasa." },
  { "en": "Apa itu 'alternator'?", "id": "Generator AC di mobil bensin." },
  { "en": "Fungsinya?", "id": "Mengisi aki dan menyuplai listrik." },
  { "en": "EV punya alternator?", "id": "Tidak, digantikan oleh DC-DC converter." },
  { "en": "Apa itu 'drivetrain losses'?", "id": "Energi yang hilang di sistem penggerak." },
  { "en": "EV punya 'drivetrain losses'?", "id": "Jauh lebih rendah dari mobil bensin." },
  { "en": "Apa itu 'efficiency' (efisiensi)?", "id": "Rasio output energi terhadap input." },
  { "en": "Efisiensi motor listrik?", "id": "Sangat tinggi (sekitar 90-95%)." },
  { "en": "Efisiensi mesin bensin?", "id": "Sangat rendah (sekitar 20-30%)." },
  { "en": "Apa itu 'cooling loop'?", "id": "Sirkuit cairan pendingin." },
  { "en": "EV bisa punya beberapa 'cooling loop'?", "id": "Ya, untuk baterai, motor, dan kabin." },
  { "en": "Apa itu 'chiller'?", "id": "Unit pendingin." },
  { "en": "Apa itu 'cabin filter'?", "id": "Filter udara kabin." },
  { "en": "Apa itu 'HEPA filter'?", "id": "Filter udara efisiensi tinggi." },
  { "en": "Beberapa EV punya HEPA filter?", "id": "Ya, contohnya Tesla." },
  { "en": "Apa itu 'Bioweapon Defense Mode'?", "id": "Fitur HEPA filter Tesla." },
  { "en": "Apa itu 'pedestrian warning sound'?", "id": "Suara peringatan untuk pejalan kaki." },
  { "en": "Sama dengan AVAS?", "id": "Ya, itu nama lainnya." },
  { "en": "Aktif pada kecepatan rendah?", "id": "Ya, di bawah 20-30 km/jam." },
  { "en": "Apa itu 'bonnet'?", "id": "Kap mesin." },
  { "en": "Apa itu 'boot'?", "id": "Bagasi belakang." },
  { "en": "Apa itu 'tailpipe'?", "id": "Pipa knalpot." },
  { "en": "EV punya 'tailpipe'?", "id": "Tidak." },
  { "en": "Apa itu 'emissions'?", "id": "Emisi gas buang." },
  { "en": "Apa itu 'smog'?", "id": "Kabut asap polusi." },
  { "en": "EV membantu mengurangi 'smog'?", "id": "Ya, di perkotaan." },
  { "en": "Apa itu 'legacy automaker'?", "id": "Pabrikan mobil tradisional." },
  { "en": "Contohnya?", "id": "Toyota, Ford, Volkswagen." },
  { "en": "Apa itu 'EV startup'?", "id": "Perusahaan baru yang fokus pada EV." },
  { "en": "Contohnya?", "id": "Rivian, Lucid." },
  { "en": "Apa itu 'direct-to-consumer' sales?", "id": "Model penjualan langsung ke konsumen." },
  { "en": "Siapa yang menggunakan model ini?", "id": "Tesla." },
  { "en": "Tanpa melalui dealer?", "id": "Ya, tanpa jaringan dealer." },
  { "en": "Apa itu 'service center'?", "id": "Pusat servis atau bengkel resmi." },
  { "en": "Apa itu 'mobile service'?", "id": "Layanan servis yang datang ke rumah." },
  { "en": "Tesla punya 'mobile service'?", "id": "Ya." },
  { "en": "Apa itu 'software-defined vehicle'?", "id": "Mobil yang fiturnya didefinisikan software." },
  { "en": "EV modern termasuk ini?", "id": "Ya, sangat." },
  { "en": "Apa itu 'subscription' (langganan)?", "id": "Membayar biaya bulanan untuk fitur." },
  { "en": "Contoh fitur langganan?", "id": "Heated seats, self-driving." },
  { "en": "Kontroversial?", "id": "Ya, sangat kontroversial." },
  { "en": "Apa itu 'total addressable market' (TAM)?", "id": "Total pasar yang bisa dijangkau." },
  { "en": "Apa itu 'market share'?", "id": "Pangsa pasar." },
  { "en": "Apa itu 'early adopter'?", "id": "Pengguna awal sebuah teknologi." },
  { "en": "Apa itu 'mainstream adoption'?", "id": "Saat teknologi diadopsi oleh mayoritas." },
  { "en": "EV saat ini di tahap mana?", "id": "Transisi dari early adopter ke mainstream." },
  { "en": "Apa itu 'S-curve' adopsi?", "id": "Grafik adopsi teknologi dari waktu ke waktu." },
  { "en": "Apa itu 'tipping point'?", "id": "Titik di mana perubahan menjadi cepat." },
  { "en": "Adopsi EV sudah mencapai 'tipping point'?", "id": "Di beberapa negara, ya." },
  { "en": "Negara dengan adopsi EV tertinggi?", "id": "Norwegia." },
  { "en": "Apa itu 'ICE ban'?", "id": "Larangan penjualan mobil bensin/diesel." },
  { "en": "Apa itu 'right-hand drive' (RHD)?", "id": "Setir kanan." },
  { "en": "Indonesia menggunakan RHD?", "id": "Ya." },
  { "en": "Apa itu 'left-hand drive' (LHD)?", "id": "Setir kiri." },
  { "en": "Apa itu 'homologation'?", "id": "Proses sertifikasi kendaraan agar legal." },
  { "en": "Apa itu 'crash test'?", "id": "Uji tabrak." },
  { "en": "Lembaga uji tabrak?", "id": "NCAP (New Car Assessment Programme)." },
  { "en": "Apa itu 'Euro NCAP'?", "id": "Lembaga NCAP untuk Eropa." },
  { "en": "Ratingnya?", "id": "Bintang, dari 1 hingga 5." },
  { "en": "EV biasanya dapat rating bagus?", "id": "Ya, karena strukturnya yang kokoh." },
  { "en": "Apa itu 'pedestrian safety'?", "id": "Keselamatan pejalan kaki." },
  { "en": "AVAS meningkatkan 'pedestrian safety'?", "id": "Ya, itu tujuannya." },
  { "en": "Apa itu 'battery pack intrusion'?", "id": "Benda asing menembus paket baterai." },
  { "en": "Sangat berbahaya?", "id": "Ya, bisa menyebabkan kebakaran." },
  { "en": "EV punya pelindung bawah baterai?", "id": "Ya, biasanya sangat kuat." },
  { "en": "Apa itu 'class action lawsuit'?", "id": "Gugatan hukum oleh sekelompok orang." },
  { "en": "Apa itu 'recall'?", "id": "Penarikan kembali produk untuk perbaikan." },
  { "en": "Recall bisa dilakukan via OTA?", "id": "Ya, untuk masalah perangkat lunak." },
  { "en": "Apa itu 'warranty'?", "id": "Garansi." },
  { "en": "Garansi baterai terpisah?", "id": "Ya, biasanya terpisah dari garansi mobil." },
  { "en": "Apa itu 'residual value'?", "id": "Nilai sisa kendaraan setelah beberapa tahun." },
  { "en": "Sama dengan 'resale value'?", "id": "Ya, sering digunakan bergantian." },
  { "en": "Apa itu 'leasing'?", "id": "Sewa jangka panjang." },
  { "en": "Apa itu 'financing'?", "id": "Pembiayaan atau kredit." },
  { "en": "Apa itu 'down payment'?", "id": "Uang muka." },
  { "en": "Apa itu 'amortization'?", "id": "Proses pelunasan utang dengan cicilan." },
  { "en": "Apa itu 'interest rate'?", "id": "Suku bunga." },
  { "en": "Apa itu 'insurance'?", "id": "Asuransi." },
  { "en": "Asuransi EV lebih mahal?", "id": "Terkadang, karena biaya perbaikan tinggi." },
  { "en": "Apa itu 'spare part'?", "id": "Suku cadang." },
  { "en": "Ketersediaan suku cadang EV?", "id": "Masih menjadi tantangan di beberapa daerah." },
  { "en": "Apa itu 'aftermarket'?", "id": "Pasar suku cadang/aksesoris non-orisinil." },
  { "en": "Apa itu 'modding'?", "id": "Modifikasi kendaraan." },
  { "en": "EV bisa dimodifikasi?", "id": "Bisa, tapi lebih kompleks." },
  { "en": "Apa itu 'DIY'?", "id": "Do It Yourself." },
  { "en": "Perbaikan EV bisa DIY?", "id": "Tidak disarankan karena tegangan tinggi." },
  { "en": "Apa itu 'high-voltage certified technician'?", "id": "Mekanik bersertifikat untuk tegangan tinggi." },
  { "en": "Apa itu 'AC motor'?", "id": "Motor arus bolak-balik." },
  { "en": "Apa itu 'DC motor'?", "id": "Motor arus searah." },
  { "en": "Motor listrik konversi seringkali?", "id": "Motor DC." },
  { "en": "Apa itu 'brushed DC motor'?", "id": "Motor DC dengan sikat." },
  { "en": "Apa itu 'brushless DC motor' (BLDC)?", "id": "Motor DC tanpa sikat." },
  { "en": "Mana lebih baik?", "id": "BLDC, lebih efisien dan andal." },
  { "en": "Digunakan di mana?", "id": "Sepeda listrik, skuter, drone." },
  { "en": "Apa itu 'IP rating'?", "id": "Peringkat ketahanan terhadap debu dan air." },
  { "en": "IP67 berarti?", "id": "Tahan debu total, tahan air sementara." },
  { "en": "Penting untuk baterai EV?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'liquid cooling'?", "id": "Pendingin cairan." },
  { "en": "Apa itu 'air cooling'?", "id": "Pendingin udara." },
  { "en": "Apa itu 'passive cooling'?", "id": "Pendinginan pasif (tanpa kipas/pompa)." },
  { "en": "Apa itu 'active cooling'?", "id": "Pendinginan aktif (dengan kipas/pompa)." },
  { "en": "Baterai EV menggunakan pendingin?", "id": "Umumnya pendingin cairan aktif." },
  { "en": "Apa itu 'charging loss'?", "id": "Energi yang hilang saat mengisi daya." },
  { "en": "Berapa besar 'charging loss'?", "id": "Sekitar 10-15%." },
  { "en": "Penyebabnya?", "id": "Panas pada baterai dan komponen." },
  { "en": "Cas lebih lambat lebih efisien?", "id": "Ya, umumnya." },
  { "en": "Apa itu 'Vampire drain'?", "id": "Baterai habis saat mobil tidak dipakai." },
  { "en": "Apa itu 'kilowatt-hour'?", "id": "kWh, satuan energi." },
  { "en": "Apa itu 'ampere-hour' (Ah)?", "id": "Satuan kapasitas muatan listrik." },
  { "en": "Kapasitas baterai bisa diukur Ah?", "id": "Ya, tapi kWh lebih umum." },
  { "en": "Apa itu 'Coulomb counting'?", "id": "Metode BMS mengukur SoC." },
  { "en": "Apa itu 'cell balancing'?", "id": "Proses menyeimbangkan tegangan sel." },
  { "en": "Jenis 'cell balancing'?", "id": "Pasif dan aktif." },
  { "en": "Apa itu 'passive balancing'?", "id": "Membuang energi berlebih sebagai panas." },
  { "en": "Apa itu 'active balancing'?", "id": "Memindahkan energi dari sel penuh ke kosong." },
  { "en": "Mana lebih efisien?", "id": "Active balancing." },
  { "en": "Apa itu 'lithium plating'?", "id": "Fenomena perusak baterai." },
  { "en": "Terjadi saat apa?", "id": "Cas cepat pada suhu dingin." },
  { "en": "BMS mencegah ini?", "id": "Ya, dengan membatasi kecepatan cas." },
  { "en": "Apa itu 'dendrite'?", "id": "Formasi kristal lithium di baterai." },
  { "en": "Berbahaya?", "id": "Ya, bisa menyebabkan korsleting." },
  { "en": "Baterai solid-state mengurangi risiko?", "id": "Ya, itu salah satu janjinya." },
  { "en": "Apa itu 'lithium-sulfur battery'?", "id": "Jenis baterai masa depan." },
  { "en": "Potensinya?", "id": "Kepadatan energi sangat tinggi." },
  { "en": "Apa itu 'lithium-air battery'?", "id": "Jenis baterai masa depan lain." },
  { "en": "Apa itu 'supercapacitor'?", "id": "Penyimpan energi dengan daya tinggi." },
  { "en": "Bisa dicas sangat cepat?", "id": "Ya, dalam hitungan detik." },
  { "en": "Kekurangannya?", "id": "Kepadatan energi sangat rendah." },
  { "en": "Bisa digabung dengan baterai?", "id": "Ya, dalam sistem hybrid." },
  { "en": "Apa itu 'regenerative' vs 'friction' brakes?", "id": "Pengereman motor vs pengereman mekanis." },
  { "en": "Kapan 'friction brakes' dipakai?", "id": "Saat pengereman mendadak atau berhenti total." },
  { "en": "Apa itu 'brake blending'?", "id": "Transisi mulus antara dua jenis rem." },
  { "en": "Apa itu 'go-kart effect'?", "id": "Sensasi berkendara EV yang responsif." },
  { "en": "Karena apa?", "id": "Pusat gravitasi rendah, torsi instan." },
  { "en": "Apa itu 'vin'?", "id": "Vehicle Identification Number." },
  { "en": "Apa itu 'monroney sticker'?", "id": "Stiker informasi mobil baru di AS." },
  { "en": "Apa itu 'build quality'?", "id": "Kualitas rakitan mobil." },
  { "en": "Apa itu 'panel gap'?", "id": "Celah antar panel bodi mobil." },
  { "en": "Apa itu 'infotainment'?", "id": "Sistem informasi dan hiburan." },
  { "en": "Apa itu 'Android Auto'?", "id": "Platform infotainment dari Google." },
  { "en": "Apa itu 'Apple CarPlay'?", "id": "Platform infotainment dari Apple." },
  { "en": "Apa itu 'Telematics'?", "id": "Sistem yang mengirim data kendaraan." },
  { "en": "Contoh data?", "id": "Lokasi, status baterai, gaya mengemudi." },
  { "en": "Digunakan untuk apa?", "id": "Asuransi, manajemen armada." },
  { "en": "Apa itu 'fleet management'?", "id": "Manajemen armada kendaraan." },
  { "en": "EV cocok untuk armada?", "id": "Ya, karena biaya operasional rendah." },
  { "en": "Contoh armada?", "id": "Taksi, mobil sewaan, mobil pengiriman." },
  { "en": "Apa itu 'charging network'?", "id": "Jaringan stasiun pengisian daya." },
  { "en": "Apa itu 'interoperability'?", "id": "Kemampuan sistem berbeda bekerja sama." },
  { "en": "Penting untuk jaringan cas?", "id": "Ya, agar bisa cas di mana saja." },
  { "en": "Apa itu 'roaming agreement'?", "id": "Perjanjian antar jaringan cas." },
  { "en": "Apa itu 'app-based' charging?", "id": "Memulai cas menggunakan aplikasi." },
  { "en": "Apa itu 'RFID card'?", "id": "Kartu untuk otentikasi di SPKLU." },
  { "en": "Apa itu 'credit card terminal'?", "id": "Terminal kartu kredit di SPKLU." },
  { "en": "Apa itu 'plug type'?", "id": "Jenis colokan atau konektor." },
  { "en": "Eropa menggunakan apa?", "id": "Type 2 dan CCS2." },
  { "en": "Amerika Utara menggunakan apa?", "id": "Type 1 dan CCS1." },
  { "en": "Jepang menggunakan apa?", "id": "Type 1 dan CHAdeMO." },
  { "en": "Tiongkok menggunakan apa?", "id": "Standar GB/T." },
  { "en": "Apa itu 'AC' vs 'DC' fast charging?", "id": "DC jauh lebih cepat." },
  { "en": "Apa itu 'charging speed curve'?", "id": "Sinonim untuk charging curve." },
  { "en": "Apa itu 'tapering'?", "id": "Penurunan kecepatan cas saat baterai penuh." },
  { "en": "Penting menjaga suhu baterai?", "id": "Ya, untuk kecepatan cas optimal." },
  { "en": "Apa itu 'battery heater'?", "id": "Pemanas baterai." },
  { "en": "Digunakan di cuaca dingin?", "id": "Ya." },
  { "en": "Apa itu 'insulation resistance'?", "id": "Ketahanan isolasi." },
  { "en": "Apa itu 'short circuit'?", "id": "Korsleting atau hubungan pendek." },
  { "en": "Apa itu 'fuse'?", "id": "Sekering." },
  { "en": "Fungsinya?", "id": "Melindungi dari arus berlebih." },
  { "en": "Apa itu 'circuit breaker'?", "id": "Pemutus sirkuit." },
  { "en": "Bisa direset?", "id": "Ya." },
  { "en": "Apa itu 'contactor'?", "id": "Relay besar untuk daya tinggi." },
  { "en": "Digunakan di EV?", "id": "Ya, untuk menyambung/memutus baterai." },
  { "en": "Apa itu 'inrush current'?", "id": "Arus awal yang sangat tinggi." },
  { "en": "Apa itu 'pre-charge circuit'?", "id": "Sirkuit untuk membatasi inrush current." },
  { "en": "Apa itu 'State of Function' (SOF)?", "id": "Kemampuan baterai bekerja saat ini." },
  { "en": "Apa itu 'State of Power' (SOP)?", "id": "Kemampuan baterai menyuplai daya." },
  { "en": "BMS memonitor semua ini?", "id": "Ya." },
  { "en": "Apa itu 'data logger'?", "id": "Perekam data." },
  { "en": "Bisa dipasang di EV?", "id": "Ya, untuk analisis mendalam." },
  { "en": "Apa itu 'OBD-II port'?", "id": "Port diagnostik standar di mobil." },
  { "en": "EV punya OBD-II?", "id": "Ya, banyak yang punya." },
  { "en": "Apa itu 'right to repair'?", "id": "Hak untuk memperbaiki barang sendiri." },
  { "en": "Isu untuk EV?", "id": "Ya, karena sistemnya yang tertutup." },
  { "en": "Apa itu 'proprietary' software?", "id": "Perangkat lunak dengan sumber tertutup." },
  { "en": "Apa itu 'open source'?", "id": "Perangkat lunak dengan sumber terbuka." },
  { "en": "Apa itu 'glamping'?", "id": "Glamorous camping." },
  { "en": "V2L cocok untuk glamping?", "id": "Ya, sangat cocok." },
  { "en": "Apa itu 'overlanding'?", "id": "Berpetualang ke lokasi terpencil." },
  { "en": "EV untuk overlanding?", "id": "Tantangannya adalah pengisian daya." },
  { "en": "Solusinya?", "id": "Panel surya portabel, power station." },
  { "en": "Apa itu 'power station' portabel?", "id": "Baterai besar portabel." },
  { "en": "Apa itu 'jerry can'?", "id": "Jeriken (untuk bensin)." },
  { "en": "Padanan untuk EV?", "id": "Power station portabel." },
  { "en": "Apa itu 'range' vs 'efficiency'?", "id": "Jarak tempuh vs konsumsi energi." },
  { "en": "Mobil efisien punya range jauh?", "id": "Belum tentu, tergantung ukuran baterai." },
  { "en": "Apa itu 'hypermiling'?", "id": "Teknik mengemudi untuk efisiensi maksimal." },
  { "en": "Contoh teknik hypermiling?", "id": "Akselerasi halus, menjaga momentum." },
  { "en": "Menggunakan 'regenerative braking' maksimal?", "id": "Ya." },
  { "en": "Apa itu 'drafting' atau 'slipstreaming'?", "id": "Mengemudi di belakang kendaraan besar." },
  { "en": "Mengurangi apa?", "id": "Hambatan udara (drag)." },
  { "en": "Apakah aman?", "id": "Tidak, sangat tidak disarankan." },
  { "en": "Apa itu 'EV grin'?", "id": "Senyum saat merasakan torsi instan." },
  { "en": "Apa itu 'silent killer'?", "id": "Julukan bahaya EV bagi pejalan kaki." },
  { "en": "Solusinya?", "id": "AVAS (suara buatan)." },
  { "en": "Apa itu 'kwh' vs 'kw'?", "id": "Energi vs Daya." },
  { "en": "Kapasitas baterai diukur dalam?", "id": "kWh." },
  { "en": "Daya motor diukur dalam?", "id": "kW." },
  { "en": "Kecepatan cas diukur dalam?", "id": "kW." },
  { "en": "Apa itu 'c-rate'?", "id": "Ukuran kecepatan pengisian baterai." },
  { "en": "Apa itu 'single motor'?", "id": "Satu motor penggerak." },
  { "en": "Apa itu 'dual motor'?", "id": "Dua motor (depan dan belakang)." },
  { "en": "Dual motor biasanya AWD?", "id": "Ya." },
  { "en": "Apa itu 'tri-motor'?", "id": "Tiga motor penggerak." },
  { "en": "Apa itu 'quad-motor'?", "id": "Empat motor, satu per roda." },
  { "en": "Keuntungannya?", "id": "Kontrol traksi yang superior." },
  { "en": "Disebut juga?", "id": "Torque vectoring." },
  { "en": "Apa itu 'tank turn'?", "id": "Berputar di tempat seperti tank." },
  { "en": "Mobil apa yang bisa?", "id": "EV quad-motor seperti Rivian." },
  { "en": "Apa itu 'curb weight' vs 'gross weight'?", "id": "Bobot kosong vs bobot total." },
  { "en": "Apa itu 'unsprung weight'?", "id": "Bobot komponen di bawah suspensi." },
  { "en": "Contohnya?", "id": "Roda, ban, rem." },
  { "en": "Mempengaruhi apa?", "id": "Kenyamanan dan handling." },
  { "en": "Apa itu 'in-wheel motor'?", "id": "Motor yang terpasang di dalam roda." },
  { "en": "Meningkatkan 'unsprung weight'?", "id": "Ya, itu salah satu kelemahannya." },
  { "en": "Apa itu 'carbon neutral'?", "id": "Karbon netral." },
  { "en": "Apa itu 'net-zero'?", "id": "Nol-bersih (emisi)." },
  { "en": "Apa itu 'life cycle assessment' (LCA)?", "id": "Analisis dampak lingkungan seumur hidup." },
  { "en": "LCA untuk EV vs ICE?", "id": "Membandingkan total emisi keduanya." },
  { "en": "Kesimpulannya?", "id": "EV lebih bersih dalam jangka panjang." },
  { "en": "Apa itu 'break-even point'?", "id": "Titik impas emisi." },
  { "en": "Berapa km?", "id": "Bervariasi, tergantung sumber listrik." },
  { "en": "Apa itu 'grid mix'?", "id": "Bauran sumber energi jaringan listrik." },
  { "en": "Grid 'hijau' membuat EV?", "id": "Semakin bersih." },
  { "en": "Apa itu 'renewable energy certificate' (REC)?", "id": "Sertifikat untuk energi terbarukan." },
  { "en": "Bisa untuk mengklaim cas 'hijau'?", "id": "Ya." },
  { "en": "Apa itu 'carbon offsetting'?", "id": "Menyeimbangkan emisi dengan proyek hijau." },
  { "en": "Apa itu 'EV tax'?", "id": "Pajak kendaraan listrik." },
  { "en": "Biasanya lebih rendah?", "id": "Ya, seringkali mendapat insentif." },
  { "en": "Apa itu 'luxury tax'?", "id": "Pajak barang mewah." },
  { "en": "EV mewah kena pajak ini?", "id": "Ya, biasanya tetap kena." },
  { "en": "Apa itu 'road tax'?", "id": "Pajak jalan." },
  { "en": "Bagaimana EV akan membayar pajak jalan?", "id": "Masih menjadi perdebatan." },
  { "en": "Berdasarkan jarak tempuh?", "id": "Itu salah satu opsinya." },
  { "en": "Apa itu 'electrify america'?", "id": "Jaringan pengisian daya di AS." },
  { "en": "Apa itu 'ionity'?", "id": "Jaringan pengisian daya di Eropa." },
  { "en": "Dibentuk oleh siapa?", "id": "Konsorsium pabrikan mobil." },
  { "en": "Apa itu 'DIN'?", "id": "Standar industri dari Jerman." },
  { "en": "Apa itu 'SAE'?", "id": "Society of Automotive Engineers." },
  { "en": "Apa itu 'ISO'?", "id": "International Organization for Standardization." },
  { "en": "Apa itu 'smart city'?", "id": "Kota pintar." },
  { "en": "EV adalah bagian dari 'smart city'?", "id": "Ya, melalui V2G dan smart charging." },
  { "en": "Apa itu 'urban air mobility' (UAM)?", "id": "Mobilitas udara perkotaan." },
  { "en": "eVTOL bagian dari UAM?", "id": "Ya." },
  { "en": "Apa itu 'autonomous taxi'?", "id": "Taksi otonom (tanpa sopir)." },
  { "en": "Disebut juga?", "id": "Robotaxi." },
  { "en": "Sudah beroperasi?", "id": "Ya, di beberapa kota terbatas." },
  { "en": "Apa itu 'geofencing'?", "id": "Batas virtual geografis." },
  { "en": "Digunakan untuk robotaxi?", "id": "Ya, untuk membatasi area operasi." },
  { "en": "Apa itu 'LiDAR'?", "id": "Sensor berbasis laser." },
  { "en": "Apa itu 'RADAR'?", "id": "Sensor berbasis gelombang radio." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari berbagai sensor." },
  { "en": "Penting untuk mobil otonom?", "id": "Ya, sangat krusial." },
  { "en": "Tesla menggunakan LiDAR?", "id": "Tidak, mengandalkan visi kamera ('Tesla Vision')." },
  { "en": "Apa itu 'neural network'?", "id": "Jaringan syaraf tiruan." },
  { "en": "Sistem visi otonom menggunakan?", "id": "Neural network." },
  { "en": "Apa itu 'OTA' vs 'recall'?", "id": "Update software vs perbaikan fisik." },
  { "en": "Apa itu 'dealer markup'?", "id": "Dealer menaikkan harga di atas MSRP." },
  { "en": "Apa itu 'MSRP'?", "id": "Harga eceran yang disarankan pabrikan." },
  { "en": "Model penjualan langsung menghindari ini?", "id": "Ya." },
  { "en": "Apa itu 'build to order'?", "id": "Mobil dibuat setelah ada pesanan." },
  { "en": "Apa itu 'inventory'?", "id": "Stok mobil yang tersedia." },
  { "en": "Apa itu 'lead time'?", "id": "Waktu tunggu dari pesan hingga pengiriman." },
  { "en": "Lead time EV bisa lama?", "id": "Ya, karena permintaan tinggi." },
  { "en": "Apa itu 'semiconductor shortage'?", "id": "Kekurangan pasokan semikonduktor." },
  { "en": "Mempengaruhi produksi EV?", "id": "Ya, sangat mempengaruhi." },
  { "en": "Apa itu 'vertical integration'?", "id": "Perusahaan mengontrol banyak bagian rantai pasok." },
  { "en": "Tesla sangat 'vertically integrated'?", "id": "Ya." },
  { "en": "Apa itu 'battery passport'?", "id": "Paspor digital untuk baterai." },
  { "en": "Isinya?", "id": "Informasi asal, komposisi, dan kesehatan." },
  { "en": "Tujuannya?", "id": "Meningkatkan transparansi dan daur ulang." },
  { "en": "Apa itu 'E-waste'?", "id": "Sampah elektronik." },
  { "en": "Baterai EV termasuk 'e-waste'?", "id": "Ya, pada akhir masa pakainya." },
  { "en": "Apa itu 'urban mobility'?", "id": "Mobilitas perkotaan." },
  { "en": "EV mengubah mobilitas perkotaan?", "id": "Ya, dengan mengurangi polusi." },
  { "en": "Apa itu 'traffic congestion'?", "id": "Kemacetan lalu lintas." },
  { "en": "Apa itu 'road pricing'?", "id": "Sistem jalan berbayar." },
  { "en": "Apa itu 'car sharing'?", "id": "Berbagi mobil." },
  { "en": "EV cocok untuk 'car sharing'?", "id": "Ya, karena biaya operasional rendah." },
  { "en": "Apa itu 'ride-hailing'?", "id": "Layanan pemesanan tumpangan (Gojek/Grab)." },
  { "en": "Banyak driver 'ride-hailing' pakai EV?", "id": "Ya, semakin banyak." },
  { "en": "Apa itu 'gig economy'?", "id": "Ekonomi berbasis pekerjaan lepas." },
  { "en": "Apa itu 'last-mile delivery'?", "id": "Pengiriman tahap akhir ke pelanggan." },
  { "en": "Motor listrik populer untuk ini?", "id": "Ya, sangat." },
  { "en": "Apa itu 'three-wheeler'?", "id": "Kendaraan roda tiga." },
  { "en": "Contoh 'three-wheeler' listrik?", "id": "Bajaj listrik." },
  { "en": "Apa itu 'quadricycle'?", "id": "Kendaraan roda empat sangat ringan." },
  { "en": "Wuling Air EV termasuk?", "id": "Mirip konsepnya, tapi mobil penuh." },
  { "en": "Apa itu 'kei car'?", "id": "Mobil kecil standar Jepang." },
  { "en": "Apa itu 'amortization schedule'?", "id": "Jadwal pembayaran pinjaman." },
  { "en": "Apa itu 'balloon payment'?", "id": "Pembayaran besar di akhir cicilan." },
  { "en": "Apa itu 'interest'?", "id": "Bunga pinjaman." },
  { "en": "Apa itu 'principal'?", "id": "Pokok pinjaman." },
  { "en": "Apa itu 'asset'?", "id": "Aset." },
  { "en": "Mobil adalah aset depresiasi?", "id": "Ya." },
  { "en": "Apa itu 'depreciation'?", "id": "Penyusutan nilai." },
  { "en": "EV klasik punya nilai?", "id": "Ya, bisa menjadi barang kolektor." },
  { "en": "Apa itu 'resto-mod'?", "id": "Restorasi dengan modifikasi modern." },
  { "en": "Contohnya?", "id": "Mobil klasik dikonversi jadi listrik." },
  { "en": "Apa itu 'drag race'?", "id": "Balap trek lurus." },
  { "en": "EV sangat cepat di 'drag race'?", "id": "Ya, karena torsi instan." },
  { "en": "0-100 km/jam adalah ukuran?", "id": "Akselerasi." },
  { "en": "Apa itu 'top speed'?", "id": "Kecepatan tertinggi." },
  { "en": "EV punya 'top speed' tinggi?", "id": "Bisa, tapi menguras baterai cepat." },
  { "en": "Apa itu 'braking distance'?", "id": "Jarak pengereman." },
  { "en": "EV lebih berat, 'braking distance'?", "id": "Bisa sedikit lebih panjang." },
  { "en": "Apa itu 'brake fade'?", "id": "Performa rem menurun karena panas." },
  { "en": "Regenerative braking membantu mengurangi?", "id": "Ya, mengurangi beban rem mekanis." },
  { "en": "Apa itu 'brake fluid'?", "id": "Cairan rem." },
  { "en": "EV masih butuh 'brake fluid'?", "id": "Ya, untuk rem mekanisnya." },
  { "en": "Apa itu 'caliper'?", "id": "Kaliper rem." },
  { "en": "Apa itu 'brake pad'?", "id": "Kampas rem." },
  { "en": "Kampas rem EV lebih awet?", "id": "Ya, karena dibantu regenerasi." },
  { "en": "Apa itu 'chassis'?", "id": "Sasis atau rangka mobil." },
  { "en": "Apa itu 'subframe'?", "id": "Sub-rangka." },
  { "en": "Apa itu 'body panel'?", "id": "Panel bodi." },
  { "en": "Bisa terbuat dari aluminium?", "id": "Ya, untuk mengurangi bobot." },
  { "en": "Atau dari baja?", "id": "Ya, untuk kekuatan dan biaya." },
  { "en": "Apa itu 'high-strength steel'?", "id": "Baja berkekuatan tinggi." },
  { "en": "Digunakan di mana?", "id": "Struktur keselamatan kabin." },
  { "en": "Apa itu 'A-pillar', 'B-pillar', 'C-pillar'?", "id": "Pilar-pilar penyangga atap mobil." },
  { "en": "Apa itu 'windshield'?", "id": "Kaca depan." },
  { "en": "Apa itu 'wiper'?", "id": "Wiper atau penyeka kaca." },
  { "en": "Apa itu 'headlight'?", "id": "Lampu depan." },
  { "en": "Apa itu 'taillight'?", "id": "Lampu belakang." },
  { "en": "EV menggunakan lampu LED?", "id": "Ya, untuk efisiensi energi." },
  { "en": "Apa itu 'DRL'?", "id": "Daytime Running Light." },
  { "en": "Apa itu 'climate control'?", "id": "Sistem pengatur suhu kabin." },
  { "en": "Otomatis atau manual?", "id": "Bisa keduanya." },
  { "en": "Apa itu 'air conditioning' (AC)?", "id": "Pendingin udara." },
  { "en": "Kompresor AC EV ditenagai?", "id": "Listrik tegangan tinggi dari baterai." },
  { "en": "Apa itu 'refrigerant'?", "id": "Zat pendingin dalam sistem AC." },
  { "en": "Apa itu 'heated steering wheel'?", "id": "Setir dengan pemanas." },
  { "en": "Lebih efisien dari pemanas kabin?", "id": "Ya." },
  { "en": "Apa itu 'EVSE' vs 'charger'?", "id": "EVSE adalah 'colokan', charger di mobil." },
  { "en": "Untuk cas AC?", "id": "Ya." },
  { "en": "Untuk cas DC?", "id": "Charger ada di luar mobil." },
  { "en": "Apa itu 'charging session'?", "id": "Sesi pengisian daya." },
  { "en": "Apa itu 'range meter'?", "id": "Penunjuk sisa jarak tempuh." },
  { "en": "Apa itu 'state-of-charge' meter?", "id": "Penunjuk persentase baterai." },
  { "en": "Mana lebih akurat?", "id": "State-of-charge meter." },
  { "en": "Apa itu 'bricked' EV?", "id": "EV yang mati total." },
  { "en": "Apa itu 'limp mode'?", "id": "Mode darurat dengan tenaga terbatas." },
  { "en": "Tujuannya?", "id": "Melindungi komponen dan memungkinkan ke bengkel." },
  { "en": "Apa itu 'EVSE handshake'?", "id": "Komunikasi antara mobil dan charger." },
  { "en": "Terjadi sebelum cas dimulai?", "id": "Ya." },
  { "en": "Untuk apa?", "id": "Menentukan kecepatan cas yang aman." },
  { "en": "Apa itu 'smart grid'?", "id": "Jaringan listrik pintar." },
  { "en": "V2G adalah bagian dari 'smart grid'?", "id": "Ya, komponen penting." },
  { "en": "Apa itu 'demand response'?", "id": "Merespons sinyal dari jaringan listrik." },
  { "en": "Smart charging adalah bentuknya?", "id": "Ya." },
  { "en": "Apa itu 'peak shaving'?", "id": "Mengurangi penggunaan listrik saat beban puncak." },
  { "en": "V2G bisa membantu 'peak shaving'?", "id": "Ya, dengan menyuplai daya." },
  { "en": "Apa itu 'grid stability'?", "id": "Stabilitas jaringan." },
  { "en": "Apa itu 'frequency regulation'?", "id": "Mengatur frekuensi jaringan." },
  { "en": "EV bisa membantu ini?", "id": "Ya, dengan V2G." },
  { "en": "Apa itu 'behind-the-meter'?", "id": "Aset energi di sisi pelanggan." },
  { "en": "Baterai rumah termasuk?", "id": "Ya." },
  { "en": "Apa itu 'virtual power plant' (VPP)?", "id": "Agregasi sumber daya energi terdistribusi." },
  { "en": "Armada EV bisa jadi VPP?", "id": "Ya, itu salah satu visinya." },
  { "en": "Apa itu 'decarbonization'?", "id": "Dekarbonisasi, mengurangi emisi karbon." },
  { "en": "Elektrifikasi transportasi adalah kuncinya?", "id": "Ya, kunci utama." },
  { "en": "Apa itu 'sector coupling'?", "id": "Menghubungkan sektor listrik dengan sektor lain." },
  { "en": "Contohnya?", "id": "Listrik untuk transportasi dan pemanasan." },
  { "en": "Apa itu 'Power-to-X'?", "id": "Mengubah listrik menjadi produk lain." },
  { "en": "Contoh 'X'?", "id": "Gas (hidrogen) atau bahan bakar cair." },
  { "en": "Apa itu 'e-fuels'?", "id": "Bahan bakar sintetis dari listrik." },
  { "en": "Bisa digunakan di mobil bensin?", "id": "Ya, secara teori." },
  { "en": "Efisien?", "id": "Tidak, banyak energi hilang." },
  { "en": "Lebih efisien mana?", "id": "Langsung menggunakan listrik di BEV." },
  { "en": "Apa itu 'electromobility'?", "id": "Sinonim untuk e-mobility." },
  { "en": "Apa itu 'powertrain' vs 'drivetrain'?", "id": "Powertrain mencakup sumber tenaga." },
  { "en": "Motor listrik bagian dari powertrain?", "id": "Ya." },
  { "en": "Apa itu 'axle'?", "id": "Sumbu atau poros roda." },
  { "en": "Apa itu 'differential'?", "id": "Gardan, mengatur putaran roda." },
  { "en": "EV punya 'differential'?", "id": "Ya, pada konfigurasi satu motor." },
  { "en": "EV quad-motor punya 'differential'?", "id": "Tidak, setiap roda dikontrol sendiri." },
  { "en": "Apa itu 'open differential'?", "id": "Gardan standar." },
  { "en": "Apa itu 'limited-slip differential' (LSD)?", "id": "Gardan dengan selip terbatas." },
  { "en": "Apa itu 'locking differential'?", "id": "Gardan yang bisa dikunci." },
  { "en": "Digunakan untuk apa?", "id": "Off-road." },
  { "en": "Apa itu 'traction control system' (TCS)?", "id": "Mencegah roda selip saat akselerasi." },
  { "en": "EV punya TCS?", "id": "Ya, dan sangat responsif." },
  { "en": "Mengapa lebih responsif?", "id": "Karena kontrol motor listrik presisi." },
  { "en": "Apa itu 'motorcycle'?", "id": "Sepeda motor." },
  { "en": "Ada motor listrik?", "id": "Ya, sangat banyak." },
  { "en": "Apa itu 'electric scooter'?", "id": "Skuter listrik." },
  { "en": "Apa itu 'electric bicycle'?", "id": "Sepeda listrik." },
  { "en": "Disebut juga 'e-bike'?", "id": "Ya." },
  { "en": "Apa itu 'pedal assist'?", "id": "Motor membantu saat pedal dikayuh." },
  { "en": "Apa itu 'throttle'?", "id": "Tuas gas." },
  { "en": "E-bike punya 'throttle'?", "id": "Beberapa punya, beberapa tidak." },
  { "en": "Apa itu 'hub motor'?", "id": "Motor di pusat roda." },
  { "en": "Digunakan di e-bike?", "id": "Ya, sangat umum." },
  { "en": "Apa itu 'mid-drive motor'?", "id": "Motor di tengah frame sepeda." },
  { "en": "Mana yang lebih baik?", "id": "Mid-drive, terasa lebih alami." },
  { "en": "Apa itu 'battery pack' vs 'battery cell'?", "id": "Paket vs sel tunggal." },
  { "en": "Bentuk sel baterai?", "id": "Silinder, prismatic, pouch." },
  { "en": "Sel 18650 artinya?", "id": "Diameter 18mm, panjang 65mm." },
  { "en": "Bentuknya?", "id": "Silinder." },
  { "en": "Sel 2170 artinya?", "id": "Diameter 21mm, panjang 70mm." },
  { "en": "Sel 4680 artinya?", "id": "Diameter 46mm, panjang 80mm." },
  { "en": "Siapa yang mengembangkannya?", "id": "Tesla." },
  { "en": "Sel lebih besar berarti?", "id": "Lebih sedikit sel, biaya lebih rendah." },
  { "en": "Apa itu 'prismatic cell'?", "id": "Sel berbentuk kotak pipih." },
  { "en": "Apa itu 'pouch cell'?", "id": "Sel dalam kantong fleksibel." },
  { "en": "Mana paling umum di EV?", "id": "Prismatic dan pouch." },
  { "en": "Apa itu 'busbar'?", "id": "Pelat konduktor untuk menghubungkan sel." },
  { "en": "Apa itu 'wire bonding'?", "id": "Menghubungkan sel dengan kawat kecil." },
  { "en": "Apa itu 'spot welding'?", "id": "Pengelasan titik." },
  { "en": "Apa itu 'lithium' vs 'Li-ion'?", "id": "Elemen vs teknologi baterai." },
  { "en": "Apa itu 'electrolyte'?", "id": "Elektrolit." },
  { "en": "Apa itu 'separator'?", "id": "Separator, pemisah anoda dan katoda." },
  { "en": "Jika separator rusak?", "id": "Bisa terjadi korsleting." },
  { "en": "Apa itu 'body in white' (BIW)?", "id": "Rangka mobil sebelum dicat." },
  { "en": "Apa itu 'paint shop'?", "id": "Area pengecatan di pabrik mobil." },
  { "en": "Apa itu 'assembly line'?", "id": "Lini perakitan." },
  { "en": "Siapa yang mempopulerkannya?", "id": "Henry Ford." },
  { "en": "Apa itu 'just-in-time' (JIT) manufacturing?", "id": "Manufaktur tepat waktu." },
  { "en": "Tujuannya?", "id": "Mengurangi biaya penyimpanan inventaris." },
  { "en": "Apa itu 'lean manufacturing'?", "id": "Manufaktur ramping, mengurangi pemborosan." },
  { "en": "Apa itu 'quality control' (QC)?", "id": "Kontrol kualitas." },
  { "en": "Apa itu 'quality assurance' (QA)?", "id": "Jaminan kualitas." },
  { "en": "Apa itu 'Six Sigma'?", "id": "Metodologi untuk meningkatkan kualitas." },
  { "en": "Apa itu 'ISO 9001'?", "id": "Standar internasional untuk manajemen mutu." },
  { "en": "Apa itu 'ergonomics'?", "id": "Ilmu tentang desain tempat kerja." },
  { "en": "Apa itu 'user-centered design'?", "id": "Desain yang berpusat pada pengguna." },
  { "en": "Apa itu 'HMI'?", "id": "Human-Machine Interface." },
  { "en": "Layar sentuh EV adalah HMI?", "id": "Ya." },
  { "en": "Apa itu 'haptic feedback'?", "id": "Umpan balik getaran." },
  { "en": "Digunakan di layar sentuh?", "id": "Ya, beberapa mobil menggunakannya." },
  { "en": "Apa itu 'voice command'?", "id": "Perintah suara." },
  { "en": "Apa itu 'gesture control'?", "id": "Kontrol dengan gerakan tangan." },
  { "en": "Apa itu 'heads-up display' (HUD)?", "id": "Menampilkan info di kaca depan." },
  { "en": "Apa itu 'augmented reality' (AR)?", "id": "Realitas tertambah." },
  { "en": "AR HUD menampilkan apa?", "id": "Navigasi yang seolah di jalan." },
  { "en": "Apa itu 'digital cockpit'?", "id": "Panel instrumen digital." },
  { "en": "Apa itu 'steering wheel'?", "id": "Roda kemudi atau setir." },
  { "en": "Apa itu 'yoke steering'?", "id": "Setir seperti tuas pesawat." },
  { "en": "Siapa yang mempopulerkannya?", "id": "Tesla." },
  { "en": "Apa itu 'steer-by-wire'?", "id": "Kemudi tanpa koneksi mekanis." },
  { "en": "Apa itu 'brake-by-wire'?", "id": "Pengereman tanpa koneksi mekanis." },
  { "en": "Apa itu 'redundancy'?", "id": "Sistem cadangan untuk keamanan." },
  { "en": "Sistem 'by-wire' butuh redundansi?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'fail-safe'?", "id": "Sistem yang aman saat gagal." },
  { "en": "Apa itu 'fail-operational'?", "id": "Sistem tetap beroperasi saat gagal." },
  { "en": "Sistem otonom harus 'fail-operational'?", "id": "Ya." },
  { "en": "Apa itu 'black box'?", "id": "Perekam data kejadian." },
  { "en": "Mobil punya 'black box'?", "id": "Ya, disebut Event Data Recorder (EDR)." },
  { "en": "Apa itu 'E-call'?", "id": "Panggilan darurat otomatis saat kecelakaan." },
  { "en": "Wajib di Eropa?", "id": "Ya, untuk mobil baru." },
  { "en": "Apa itu 'Global Navigation Satellite System' (GNSS)?", "id": "Sistem satelit navigasi global." },
  { "en": "GPS adalah contoh GNSS?", "id": "Ya." },
  { "en": "Apa itu 'dead reckoning'?", "id": "Estimasi posisi tanpa sinyal GPS." },
  { "en": "Menggunakan apa?", "id": "Sensor gerak (IMU) dan kecepatan roda." },
  { "en": "Apa itu 'IMU'?", "id": "Inertial Measurement Unit." },
  { "en": "Isinya?", "id": "Akselerometer dan giroskop." },
  { "en": "Apa itu 'accelerometer'?", "id": "Mengukur percepatan." },
  { "en": "Apa itu 'gyroscope'?", "id": "Mengukur orientasi dan kecepatan sudut." },
  { "en": "Ponsel punya IMU?", "id": "Ya." },
  { "en": "EV otonom punya IMU?", "id": "Ya, yang sangat canggih." },
  { "en": "Apa itu 'calibration'?", "id": "Kalibrasi." },
  { "en": "Sensor perlu dikalibrasi?", "id": "Ya, untuk akurasi." },
  { "en": "Apa itu 'firmware'?", "id": "Perangkat lunak level rendah." },
  { "en": "Bisa di-update via OTA?", "id": "Ya." },
  { "en": "Apa itu 'ECU'?", "id": "Electronic Control Unit." },
  { "en": "Mobil modern punya banyak ECU?", "id": "Ya, bisa puluhan hingga ratusan." },
  { "en": "Apa itu 'domain controller'?", "id": "Komputer terpusat yang menggantikan ECU." },
  { "en": "Arsitektur masa depan EV?", "id": "Menggunakan domain controller." },
  { "en": "Apa itu 'zonal architecture'?", "id": "Arsitektur kelistrikan berbasis zona." },
  { "en": "Apa itu 'automotive-grade'?", "id": "Komponen yang memenuhi standar otomotif." },
  { "en": "Standarnya lebih ketat?", "id": "Ya, untuk suhu dan getaran." },
  { "en": "Apa itu 'AEC-Q'?", "id": "Standar kualifikasi komponen elektronik otomotif." },
  { "en": "Apa itu 'ISO 26262'?", "id": "Standar keamanan fungsional otomotif." },
  { "en": "Apa itu 'ASIL'?", "id": "Automotive Safety Integrity Level." },
  { "en": "Ukuran apa?", "id": "Tingkat risiko dan persyaratan keamanan." },
  { "en": "Level ASIL tertinggi?", "id": "ASIL D." },
  { "en": "Sistem krusial butuh ASIL D?", "id": "Ya, seperti pengereman dan kemudi." },
  { "en": "Apa itu 'cybersecurity'?", "id": "Keamanan siber." },
  { "en": "Penting untuk EV modern?", "id": "Ya, sangat penting." },
  { "en": "Mengapa?", "id": "Karena mobil terhubung ke internet." },
  { "en": "Apa itu 'hacking'?", "id": "Peretasan." },
  { "en": "EV bisa diretas?", "id": "Ya, secara teoretis bisa." },
  { "en": "Apa itu 'firewall'?", "id": "Dinding api (keamanan jaringan)." },
  { "en": "Apa itu 'encryption'?", "id": "Enkripsi." },
  { "en": "Melindungi apa?", "id": "Data dari akses tidak sah." },
  { "en": "Apa itu 'keyless entry'?", "id": "Masuk mobil tanpa kunci fisik." },
  { "en": "Menggunakan apa?", "id": "Sinyal radio (fob) atau ponsel." },
  { "en": "Apa itu 'relay attack'?", "id": "Serangan untuk mencuri sinyal kunci." },
  { "en": "Apa itu 'faraday pouch'?", "id": "Kantong untuk memblokir sinyal kunci." },
  { "en": "Apa itu 'NFC'?", "id": "Near Field Communication." },
  { "en": "Digunakan untuk kunci mobil?", "id": "Ya, dalam bentuk kartu." },
  { "en": "Apa itu 'Bluetooth Low Energy' (BLE)?", "id": "Bluetooth hemat energi." },
  { "en": "Digunakan untuk 'phone as a key'?", "id": "Ya." },
  { "en": "Apa itu 'digital key'?", "id": "Kunci digital." },
  { "en": "Bisa dibagikan ke orang lain?", "id": "Ya, melalui aplikasi." },
  { "en": "Apa itu 'valet mode'?", "id": "Mode untuk membatasi fungsi mobil." },
  { "en": "Untuk siapa?", "id": "Petugas parkir valet." },
  { "en": "Membatasi apa?", "id": "Kecepatan, akses glove box." },
  { "en": "Apa itu 'eco mode'?", "id": "Mode berkendara paling efisien." },
  { "en": "Apa itu 'sport mode'?", "id": "Mode berkendara paling responsif." },
  { "en": "Apa itu 'drive modes'?", "id": "Mode berkendara (eco, normal, sport)." },
  { "en": "Mengubah apa?", "id": "Respons pedal, bobot setir." },
  { "en": "Apa itu 'adjustable suspension'?", "id": "Suspensi yang bisa diatur." },
  { "en": "Jenisnya?", "id": "Suspensi udara (air suspension)." },
  { "en": "Bisa menaikkan/menurunkan mobil?", "id": "Ya." },
  { "en": "Apa itu 'skid plate'?", "id": "Pelat pelindung di bawah mobil." },
  { "en": "Melindungi apa?", "id": "Baterai dan komponen penting lainnya." },
  { "en": "Apa itu 'off-roading'?", "id": "Berkendara di luar jalan raya." },
  { "en": "EV bisa 'off-road'?", "id": "Ya, yang dirancang khusus." },
  { "en": "Contohnya?", "id": "Rivian R1T, Hummer EV." },
  { "en": "Apa itu 'wading depth'?", "id": "Kedalaman air maksimum yang bisa dilewati." },
  { "en": "EV punya 'wading depth'?", "id": "Ya, dan seringkali sangat baik." },
  { "en": "Mengapa?", "id": "Karena tidak ada intake udara mesin." },
  { "en": "Apa itu 'battery pack sealing'?", "id": "Penyegelan paket baterai agar kedap." },
  { "en": "Apa itu 'motor sealing'?", "id": "Penyegelan motor listrik." },
  { "en": "Apa itu 'rust proofing'?", "id": "Perlindungan anti karat." },
  { "en": "Apa itu 'underbody coating'?", "id": "Lapisan pelindung di bawah mobil." },
  { "en": "Apa itu 'road noise'?", "id": "Kebisingan dari ban dan jalan." },
  { "en": "Apa itu 'wind noise'?", "id": "Kebisingan dari angin." },
  { "en": "EV lebih senyap, jadi noise ini?", "id": "Menjadi lebih terdengar." },
  { "en": "Solusinya?", "id": "Isolasi kabin, kaca akustik." },
  { "en": "Apa itu 'acoustic glass'?", "id": "Kaca yang dirancang untuk meredam suara." },
  { "en": "Apa itu 'NVH'?", "id": "Noise, Vibration, and Harshness." },
  { "en": "Insinyur EV fokus pada NVH?", "id": "Ya, sangat." },
  { "en": "Apa itu 'EVSE' vs 'charging cable'?", "id": "EVSE adalah unit, kabel bagiannya." },
  { "en": "Apa itu 'J1772'?", "id": "Standar konektor AC Type 1." },
  { "en": "Digunakan di mana?", "id": "Amerika Utara dan Jepang." },
  { "en": "Apa itu 'Type 2' connector?", "id": "Standar konektor AC di Eropa/Indonesia." },
  { "en": "Disebut juga 'Mennekes'?", "id": "Ya." },
  { "en": "Apa itu 'CCS1' vs 'CCS2'?", "id": "CCS dengan basis Type 1/Type 2." },
  { "en": "Apa itu 'PLC'?", "id": "Power Line Communication." },
  { "en": "Digunakan untuk komunikasi cas?", "id": "Ya, di standar CCS." },
  { "en": "Apa itu 'handshake protocol'?", "id": "Protokol komunikasi awal." },
  { "en": "Apa itu 'megawatt charging system' (MCS)?", "id": "Standar cas daya sangat tinggi." },
  { "en": "Untuk apa?", "id": "Truk dan bus listrik." },
  { "en": "Berapa dayanya?", "id": "Bisa lebih dari 1 MW." },
  { "en": "Apa itu 'pantograph charger'?", "id": "Charger seperti lengan pantograf." },
  { "en": "Digunakan untuk apa?", "id": "Bus listrik di halte." },
  { "en": "Apa itu 'opportunity charging' untuk bus?", "id": "Cas singkat di setiap pemberhentian." },
  { "en": "Apa itu 'depot charging'?", "id": "Cas semalaman di depo bus." },
  { "en": "Apa itu 'fleet'?", "id": "Armada." },
  { "en": "Apa itu 'EV fleet management'?", "id": "Manajemen armada kendaraan listrik." },
  { "en": "Meliputi apa saja?", "id": "Monitoring, penjadwalan cas, pemeliharaan." },
  { "en": "Apa itu 'total cost of ownership' (TCO)?", "id": "Total biaya kepemilikan." },
  { "en": "Lebih penting untuk armada?", "id": "Ya, sangat." },
  { "en": "Apa itu 'uptime'?", "id": "Waktu operasional kendaraan." },
  { "en": "Apa itu 'downtime'?", "id": "Waktu henti (tidak operasional)." },
  { "en": "Tujuan manajemen armada?", "id": "Memaksimalkan uptime, meminimalkan downtime." },
  { "en": "Apa itu 'predictive maintenance'?", "id": "Memprediksi kapan perawatan dibutuhkan." },
  { "en": "Menggunakan data telematika?", "id": "Ya." },
  { "en": "Apa itu 'EVSE load balancing'?", "id": "Membagi daya secara cerdas antar charger." },
  { "en": "Tujuannya?", "id": "Menghindari kelebihan beban daya listrik." },
  { "en": "Penting untuk depo bus/truk?", "id": "Ya, sangat penting." },
  { "en": "Apa itu 'time-of-use' (TOU) tariff?", "id": "Tarif listrik yang berbeda berdasarkan waktu." },
  { "en": "Smart charging memanfaatkan TOU?", "id": "Ya, mengisi saat tarif murah." },
  { "en": "Apa itu 'demand charge'?", "id": "Biaya berdasarkan permintaan daya puncak." },
  { "en": "Berlaku untuk siapa?", "id": "Pelanggan komersial dan industri." },
  { "en": "Manajemen cas bisa mengurangi ini?", "id": "Ya." },
  { "en": "Apa itu 'behind-the-meter' storage?", "id": "Penyimpanan energi di sisi pelanggan." },
  { "en": "Bisa mengurangi 'demand charge'?", "id": "Ya, dengan 'peak shaving'." },
  { "en": "Apa itu 'solar car park'?", "id": "Tempat parkir dengan atap panel surya." },
  { "en": "Bisa untuk mengecas EV?", "id": "Ya, kombinasi yang ideal." },
  { "en": "Apa itu 'green building'?", "id": "Bangunan hijau." },
  { "en": "Menyediakan cas EV?", "id": "Ya, sering menjadi salah satu fitur." },
  { "en": "Apa itu 'LEED'?", "id": "Sertifikasi untuk bangunan hijau." },
  { "en": "Apa itu 'range' sebenarnya vs 'rated'?", "id": "Jarak nyata vs jarak resmi." },
  { "en": "Seringkali berbeda?", "id": "Ya, tergantung banyak faktor." }





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
            }, 7000);
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
