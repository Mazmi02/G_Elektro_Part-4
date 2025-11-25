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


  { "en": "Apa Itu Modular Multilevel Converter (MMC)?", "id": "Inverter Tegangan Tinggi Modul Seri." },
  { "en": "Apa Itu Submodule Pada MMC?", "id": "Blok Kapasitor Dan Switch Dasar." },
  { "en": "Apa Itu Circulating Current Suppression?", "id": "Meredam Arus Putar Antar Fasa." },
  { "en": "Apa Itu Nearest Level Control?", "id": "Modulasi Inverter Tangga Sederhana." },
  { "en": "Apa Itu Phase Shifted Carrier PWM?", "id": "Teknik Modulasi Pembawa Geser Fasa." },
  { "en": "Apa Itu Level Shifted Carrier PWM?", "id": "Teknik Modulasi Pembawa Tumpuk Level." },
  { "en": "Apa Itu Cascaded H-Bridge Inverter?", "id": "Inverter H-Bridge Seri Bertingkat." },
  { "en": "Apa Itu Flying Capacitor Balancing?", "id": "Menjaga Tegangan Kapasitor Tetap Sama." },
  { "en": "Apa Itu Neutral Point Balancing?", "id": "Menjaga Titik Tengah DC Stabil." },
  { "en": "Apa Itu Common Mode Voltage Reduction?", "id": "Mengurangi Tegangan Pada Bearing Motor." },
  { "en": "Apa Itu Matrix Converter?", "id": "Konverter AC Ke AC Langsung." },
  { "en": "Apa Kelebihan Matrix Converter?", "id": "Tanpa Kapasitor DC Link Besar." },
  { "en": "Apa Itu Current Source Inverter (CSI)?", "id": "Inverter Dengan Sumber Arus Konstan." },
  { "en": "Apa Fungsi Induktor DC Link CSI?", "id": "Menjaga Arus Input Tetap Rata." },
  { "en": "Apa Itu Reverse Blocking IGBT?", "id": "IGBT Menahan Tegangan Balik." },
  { "en": "Apa Itu Reverse Conducting IGBT?", "id": "IGBT Dengan Dioda Body Terintegrasi." },
  { "en": "Apa Itu GaN HEMT Transistor?", "id": "Transistor Gallium Nitride Cepat." },
  { "en": "Kelebihan GaN Dibanding SiC Adalah?", "id": "Switching Lebih Cepat Di Frekuensi Tinggi." },
  { "en": "Kelebihan SiC Dibanding GaN Adalah?", "id": "Tahan Tegangan Dan Panas Tinggi." },
  { "en": "Apa Itu Gate Driver Desaturation?", "id": "Proteksi Hubung Singkat IGBT Aktif." },
  { "en": "Apa Itu Active Clamping Driver?", "id": "Membatasi Lonjakan Tegangan Saat Mati." },
  { "en": "Apa Itu Soft Turn Off?", "id": "Mematikan IGBT Perlahan Saat Fault." },
  { "en": "Apa Itu Rogowski Current Sensor?", "id": "Koil Udara Ukur Arus AC." },
  { "en": "Apa Itu Fluxgate Current Sensor?", "id": "Sensor Arus DC Presisi Tinggi." },
  { "en": "Apa Itu Hall Effect Closed Loop?", "id": "Sensor Arus Umpan Balik Nol." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Sinyal Terpisah Galvanis." },
  { "en": "Apa Itu Delta Sigma Modulator?", "id": "Konversi Analog Digital Bit Stream." },
  { "en": "Apa Itu Sinc Filter Digital?", "id": "Filter Output ADC Delta Sigma." },
  { "en": "Apa Itu Decimation Filter?", "id": "Menurunkan Kecepatan Sampel Data." },
  { "en": "Apa Itu Oversampling ADC?", "id": "Sampling Jauh Di Atas Nyquist." },
  { "en": "Manfaat Oversampling Adalah?", "id": "Meningkatkan Resolusi Dan Menurunkan Noise." },
  { "en": "Apa Itu Noise Shaping?", "id": "Menggeser Noise Ke Frekuensi Tinggi." },
  { "en": "Apa Itu SAR ADC?", "id": "Successive Approximation Register ADC." },
  { "en": "Kelebihan SAR ADC Adalah?", "id": "Efisien Daya Dan Akurasi Menengah." },
  { "en": "Apa Itu Flash ADC?", "id": "ADC Paralel Paling Cepat." },
  { "en": "Apa Itu Pipeline ADC?", "id": "Kombinasi Kecepatan Dan Resolusi." },
  { "en": "Apa Itu ENOB (Effective Number Of Bits)?", "id": "Resolusi Nyata Sebuah ADC." },
  { "en": "Apa Itu SFDR (Spurious Free Dynamic Range)?", "id": "Jarak Sinyal Ke Noise Tertinggi." },
  { "en": "Apa Itu SINAD (Signal To Noise Distortion)?", "id": "Rasio Sinyal Total Ke Gangguan." },
  { "en": "Apa Itu IMD (Intermodulation Distortion)?", "id": "Cacat Akibat Campuran Dua Frekuensi." },
  { "en": "Apa Itu IP3 (Third Order Intercept)?", "id": "Titik Ukur Linieritas Penguat RF." },
  { "en": "Apa Itu P1dB Compression Point?", "id": "Titik Penguat Mulai Jenuh." },
  { "en": "Apa Itu LNA (Low Noise Amplifier)?", "id": "Penguat Awal Penerima Sangat Sensitif." },
  { "en": "Apa Itu PA (Power Amplifier) RF?", "id": "Penguat Akhir Pemancar Daya Besar." },
  { "en": "Apa Itu Doherty Amplifier?", "id": "Penguat RF Efisiensi Tinggi." },
  { "en": "Apa Itu Envelope Tracking?", "id": "Modulasi Tegangan Suplai Mengikuti Sinyal." },
  { "en": "Apa Itu Digital Pre Distortion (DPD)?", "id": "Kompensasi Cacat PA Secara Digital." },
  { "en": "Apa Itu ALC (Automatic Level Control)?", "id": "Menjaga Daya Output RF Konstan." },
  { "en": "Apa Itu Circulator RF?", "id": "Mengarahkan Sinyal Putar Satu Arah." },
  { "en": "Apa Itu Isolator RF?", "id": "Mencegah Sinyal Balik Ke Sumber." },
  { "en": "Apa Itu Directional Coupler?", "id": "Mengambil Sampel Daya RF." },
  { "en": "Apa Itu Hybrid Coupler 90 Derajat?", "id": "Membagi Sinyal Beda Fasa Kuadratur." },
  { "en": "Apa Itu Wilkinson Power Divider?", "id": "Pembagi Daya RF Sama Rata." },
  { "en": "Apa Itu Bias Tee?", "id": "Menyuntikkan Tegangan DC Ke RF." },
  { "en": "Apa Itu DC Block RF?", "id": "Kapasitor Penahan Arus DC." },
  { "en": "Apa Itu PIN Diode Switch?", "id": "Sakelar RF Cepat Tanpa Mekanik." },
  { "en": "Apa Itu YIG Oscillator?", "id": "Osilator Bola Magnetik Tuning Lebar." },
  { "en": "Apa Itu DRO (Dielectric Resonator Oscillator)?", "id": "Osilator Stabil Keramik Frekuensi Tinggi." },
  { "en": "Apa Itu DDS (Direct Digital Synthesis)?", "id": "Pembangkit Gelombang Murni Digital." },
  { "en": "Apa Itu Phase Noise Oscillator?", "id": "Jitter Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Allan Variance?", "id": "Ukuran Stabilitas Frekuensi Jangka Panjang." },
  { "en": "Apa Itu Rubidium Atomic Clock?", "id": "Jam Atom Kompak Presisi Tinggi." },
  { "en": "Apa Itu GPSDO (GPS Disciplined Oscillator)?", "id": "Osilator Dikoreksi Sinyal GPS." },
  { "en": "Apa Itu NTP (Network Time Protocol)?", "id": "Sinkronisasi Waktu Via Internet." },
  { "en": "Apa Itu PTP (Precision Time Protocol)?", "id": "Sinkronisasi Waktu Mikro Detik LAN." },
  { "en": "Apa Itu Grandmaster Clock?", "id": "Sumber Waktu Utama Jaringan PTP." },
  { "en": "Apa Itu Transparent Clock?", "id": "Switch PTP Mengukur Waktu Tunda." },
  { "en": "Apa Itu IRIG-B Time Code?", "id": "Standar Waktu Analog Gardu Induk." },
  { "en": "Apa Itu PPS Signal?", "id": "Denyut Satu Detik Sangat Akurat." },
  { "en": "Apa Itu TOU (Time Of Use) Tariff?", "id": "Harga Listrik Berdasar Waktu Pakai." },
  { "en": "Apa Itu CPP (Critical Peak Pricing)?", "id": "Harga Sangat Mahal Saat Kritis." },
  { "en": "Apa Itu RTP (Real Time Pricing)?", "id": "Harga Listrik Mengikuti Pasar Spot." },
  { "en": "Apa Itu Demand Charge?", "id": "Biaya Beban Puncak Tertinggi Bulanan." },
  { "en": "Apa Itu Power Factor Penalty?", "id": "Denda Jika Faktor Daya Rendah." },
  { "en": "Apa Itu Load Profiling?", "id": "Merekam Pola Pemakaian Listrik Pelanggan." },
  { "en": "Apa Itu Interval Data Recorder?", "id": "Meteran Perekam Data Per Menit." },
  { "en": "Apa Itu AMI (Advanced Metering Infrastructure)?", "id": "Sistem Meter Cerdas Dua Arah." },
  { "en": "Apa Itu MDM (Meter Data Management)?", "id": "Pusat Pengolahan Data Meter Cerdas." },
  { "en": "Apa Itu HES (Head End System)?", "id": "Gerbang Komunikasi Meter Ke Server." },
  { "en": "Apa Itu HAN (Home Area Network)?", "id": "Jaringan Meter Ke Perangkat Rumah." },
  { "en": "Apa Itu NAN (Neighborhood Area Network)?", "id": "Jaringan Meter Ke Pengumpul Data." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Pengumpul Ke Pusat Data." },
  { "en": "Apa Itu Broadband PLC?", "id": "Komunikasi Data Cepat Kabel Listrik." },
  { "en": "Apa Itu Narrowband PLC?", "id": "Komunikasi Data Lambat Jarak Jauh." },
  { "en": "Apa Itu G3-PLC Standard?", "id": "Standar PLC Tahan Gangguan Noise." },
  { "en": "Apa Itu RF Mesh Metering?", "id": "Meter Saling Oper Data Radio." },
  { "en": "Apa Itu DCU (Data Concentrator Unit)?", "id": "Alat Pengumpul Data Banyak Meter." },
  { "en": "Apa Itu Characteristic Impedance Zo?", "id": "Impedansi Saluran Tanpa Pantulan." },
  { "en": "Apa Itu Microstrip Differential Impedance?", "id": "Impedansi Pasangan Jalur Di Permukaan." },
  { "en": "Apa Itu Stripline Differential Impedance?", "id": "Impedansi Pasangan Jalur Di Dalam." },
  { "en": "Apa Itu LEO Satellite?", "id": "Low Earth Orbit Satelit Rendah." },
  { "en": "Apa Itu GEO Satellite?", "id": "Geostationary Earth Orbit Satelit Tetap." },
  { "en": "Apa Itu MEO Satellite?", "id": "Medium Earth Orbit Satelit Menengah." },
  { "en": "Apa Itu Ka Band Frequency?", "id": "Frekuensi 26 Hingga 40 GHz." },
  { "en": "Apa Itu Ku Band Frequency?", "id": "Frekuensi 12 Hingga 18 GHz." },
  { "en": "Apa Itu C Band Frequency?", "id": "Frekuensi 4 Hingga 8 GHz." },
  { "en": "Apa Itu Rain Fade?", "id": "Pelemahan Sinyal Akibat Hujan." },
  { "en": "Apa Itu Look Angle Satelit?", "id": "Sudut Antena Ke Satelit." },
  { "en": "Apa Itu Satellite Footprint?", "id": "Area Cakupan Sinyal Di Bumi." },
  { "en": "Apa Itu VSAT Hub?", "id": "Stasiun Pusat Pengendali Jaringan Satelit." },
  { "en": "Apa Itu SCPC (Single Channel Per Carrier)?", "id": "Satu Saluran Satu Frekuensi Pembawa." },
  { "en": "Apa Itu TDMA Satellite Access?", "id": "Berbagi Waktu Akses Transponder." },
  { "en": "Apa Itu Gain To Noise Temperature (G/T)?", "id": "Ukuran Kualitas Penerimaan Stasiun Bumi." },
  { "en": "Apa Itu EIRP Satelit?", "id": "Daya Pancar Efektif Isotropik." },
  { "en": "Apa Itu Slotted Waveguide Antenna?", "id": "Antena Radar Celah Pipa Logam." },
  { "en": "Apa Itu Parabolic Dish Reflector?", "id": "Pemantul Sinyal Fokus Satu Titik." },
  { "en": "Apa Itu Cassegrain Antenna?", "id": "Antena Parabola Dua Reflektor." },
  { "en": "Apa Itu Gregorian Antenna?", "id": "Mirip Cassegrain Tapi Subreflektor Cekung." },
  { "en": "Apa Itu Offset Dish?", "id": "Fokus Tidak Menghalangi Sinyal Datang." },
  { "en": "Apa Itu Prime Focus Dish?", "id": "LNB Tepat Di Tengah Parabola." },
  { "en": "Apa Itu Horn Feed?", "id": "Corong Pengumpan Sinyal Ke Dish." },
  { "en": "Apa Itu Orthomode Transducer (OMT)?", "id": "Pemisah Polarisasi Vertikal Horizontal." },
  { "en": "Apa Itu Waveguide Flange?", "id": "Sambungan Pipa Gelombang Mikro." },
  { "en": "Apa Itu Smith Chart Admittance?", "id": "Peta Admitansi Kebalikan Impedansi." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Tegangan Maksimum Minimum." },
  { "en": "Apa Itu Reflection Coefficient Gamma?", "id": "Rasio Gelombang Pantul Dan Maju." },
  { "en": "Apa Itu Transmission Coefficient?", "id": "Rasio Gelombang Terus Dan Maju." },
  { "en": "Apa Itu Friis Transmission Formula?", "id": "Rumus Daya Terima Ruang Hampa." },
  { "en": "Apa Itu Path Loss Exponent?", "id": "Faktor Pelemahan Lingkungan Spesifik." },
  { "en": "Apa Itu Link Margin?", "id": "Cadangan Daya Agar Sinyal Aman." },
  { "en": "Apa Itu Noise Figure (NF)?", "id": "Penurunan SNR Akibat Alat." },
  { "en": "Apa Itu Noise Temperature?", "id": "Suhu Ekuivalen Daya Noise." },
  { "en": "Apa Itu Low Noise Block (LNB)?", "id": "Penguat Dan Penurun Frekuensi Satelit." },
  { "en": "Apa Itu Block Up Converter (BUC)?", "id": "Penguat Dan Penaik Frekuensi Satelit." },
  { "en": "Apa Itu Solid State Power Amplifier?", "id": "Penguat Daya Transistor Semikonduktor." },
  { "en": "Apa Itu Traveling Wave Tube (TWT)?", "id": "Tabung Penguat Daya Satelit Lama." },
  { "en": "Apa Itu Klystron Amplifier?", "id": "Tabung Penguat Daya Tinggi Radar." },
  { "en": "Apa Itu Magnetron Oscillator?", "id": "Tabung Pembangkit Gelombang Mikro Oven." },
  { "en": "Apa Itu Cross Field Amplifier?", "id": "Penguat Gelombang Mikro Medan Silang." },
  { "en": "Apa Itu Gyrotron?", "id": "Pembangkit Gelombang Millimeter Daya Tinggi." },
  { "en": "Apa Itu Impatt Diode?", "id": "Dioda Osilator Gelombang Mikro Avalanche." },
  { "en": "Apa Itu Trapatt Diode?", "id": "Dioda Plasma Avalanche Transit Time." },
  { "en": "Apa Itu Baritt Diode?", "id": "Barrier Injection Transit Time Diode." },
  { "en": "Apa Itu MMIC (Monolithic Microwave IC)?", "id": "Sirkuit Terpadu Gelombang Mikro." },
  { "en": "Apa Itu Microstrip Patch Antenna?", "id": "Antena Cetak PCB Tipis." },
  { "en": "Apa Itu Fractal Antenna?", "id": "Antena Geometri Fraktal Lebar Pita." },
  { "en": "Apa Itu Log Periodic Antenna?", "id": "Antena Lebar Pita Susunan Dipole." },
  { "en": "Apa Itu Helical Antenna?", "id": "Antena Spiral Polarisasi Melingkar." },
  { "en": "Apa Itu Dielectric Resonator Antenna?", "id": "Antena Keramik Efisiensi Tinggi." },
  { "en": "Apa Itu Phased Array Radar?", "id": "Radar Tanpa Putaran Mekanik." },
  { "en": "Apa Itu Active Electronically Scanned Array?", "id": "Radar Modul Transmit Receive Banyak." },
  { "en": "Apa Itu Passive Electronically Scanned Array?", "id": "Radar Satu Sumber Banyak Pemancar." },
  { "en": "Apa Itu Synthetic Aperture Radar (SAR)?", "id": "Radar Citra Resolusi Tinggi Bergerak." },
  { "en": "Apa Itu Ground Penetrating Radar (GPR)?", "id": "Radar Deteksi Benda Bawah Tanah." },
  { "en": "Apa Itu Radar Jamming?", "id": "Mengganggu Sinyal Radar Lawan." },
  { "en": "Apa Itu Stealth Technology?", "id": "Menyerap Atau Memantulkan Radar." },
  { "en": "Apa Itu Radar Cross Section (RCS)?", "id": "Ukuran Pantulan Radar Target." },
  { "en": "Apa Itu Doppler Shift Radar?", "id": "Perubahan Frekuensi Akibat Kecepatan Objek." },
  { "en": "Apa Itu FMCW Radar?", "id": "Radar Gelombang Kontinyu Modulasi Frekuensi." },
  { "en": "Apa Itu Pulse Radar?", "id": "Radar Kirim Pulsa Pendek Kuat." },
  { "en": "Apa Itu Range Resolution?", "id": "Kemampuan Membedakan Jarak Dua Objek." },
  { "en": "Apa Itu Azimuth Resolution?", "id": "Kemampuan Membedakan Arah Dua Objek." },
  { "en": "Apa Itu Blind Speed Radar?", "id": "Kecepatan Target Tidak Terdeteksi Doppler." },
  { "en": "Apa Itu MTI (Moving Target Indication)?", "id": "Filter Membuang Objek Diam Clutter." },
  { "en": "Apa Itu Clutter Radar?", "id": "Pantulan Sinyal Pengganggu Non Target." },
  { "en": "Apa Itu Sea Clutter?", "id": "Pantulan Gelombang Laut Pada Radar." },
  { "en": "Apa Itu Rain Clutter?", "id": "Pantulan Butiran Hujan Pada Radar." },
  { "en": "Apa Itu Chaff Countermeasure?", "id": "Serpihan Logam Pengecoh Radar." },
  { "en": "Apa Itu Electronic Warfare (EW)?", "id": "Perang Spektrum Elektromagnetik." },
  { "en": "Apa Itu ESM (Electronic Support Measures)?", "id": "Deteksi Dan Analisis Sinyal Musuh." },
  { "en": "Apa Itu ECM (Electronic Counter Measures)?", "id": "Serangan Jamming Sinyal Musuh." },
  { "en": "Apa Itu ECCM (Electronic Counter Counter)?", "id": "Teknik Anti Jamming Radar." },
  { "en": "Apa Itu FHSS (Frequency Hopping)?", "id": "Pindah Frekuensi Cepat Hindari Jamming." },
  { "en": "Apa Itu DSSS (Direct Sequence Spread)?", "id": "Sebar Sinyal Dengan Kode Noise." },
  { "en": "Apa Itu Chirp Signal?", "id": "Sinyal Frekuensi Naik Turun Cepat." },
  { "en": "Apa Itu Pulse Compression?", "id": "Memadatkan Pulsa Radar Panjang." },
  { "en": "Apa Itu Barker Code?", "id": "Kode Biner Korelasi Sinyal Radar." },
  { "en": "Apa Itu Ambiguity Function?", "id": "Analisis Sinyal Radar Waktu Frekuensi." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Deteksi Sinyal Noise." },
  { "en": "Apa Itu Constant False Alarm Rate?", "id": "Ambang Batas Adaptif Deteksi Radar." },
  { "en": "Apa Itu Monopulse Radar?", "id": "Lacak Sudut Target Satu Pulsa." },
  { "en": "Apa Itu Conical Scan Radar?", "id": "Lacak Target Dengan Putar Beam." },
  { "en": "Apa Itu Track While Scan (TWS)?", "id": "Melacak Target Sambil Terus Scanning." },
  { "en": "Apa Itu Over The Horizon Radar?", "id": "Radar Pantulan Ionosfer Jarak Jauh." },
  { "en": "Apa Itu Bistatic Radar?", "id": "Pemancar Dan Penerima Terpisah Jauh." },
  { "en": "Apa Itu Multistatic Radar?", "id": "Banyak Pemancar Penerima Tersebar." },
  { "en": "Apa Itu Secondary Surveillance Radar?", "id": "Radar Tanya Jawab Transponder Pesawat." },
  { "en": "Apa Itu IFF (Identification Friend Foe)?", "id": "Sistem Identifikasi Teman Atau Lawan." },
  { "en": "Apa Itu ADS-B (Automatic Dependent Surveillance)?", "id": "Pesawat Lapor Posisi GPS Sendiri." },
  { "en": "Apa Itu TCAS (Traffic Collision Avoidance)?", "id": "Sistem Anti Tabrakan Pesawat Udara." },
  { "en": "Apa Itu GPWS (Ground Proximity Warning)?", "id": "Peringatan Pesawat Dekat Tanah." },
  { "en": "Apa Itu Radar Altimeter?", "id": "Ukur Tinggi Pesawat Dari Tanah." },
  { "en": "Apa Itu ILS (Instrument Landing System)?", "id": "Panduan Pendaratan Pesawat Presisi." },
  { "en": "Apa Itu Localizer ILS?", "id": "Panduan Lurus Tengah Landasan." },
  { "en": "Apa Itu Glideslope ILS?", "id": "Panduan Sudut Turun Pendaratan." },
  { "en": "Apa Itu VOR (VHF Omnidirectional Range)?", "id": "Navigasi Arah Radio Pesawat." },
  { "en": "Apa Itu DME (Distance Measuring Equipment)?", "id": "Ukur Jarak Pesawat Ke Stasiun." },
  { "en": "Apa Itu RTOS (Real Time Operating System)?", "id": "Sistem Operasi Waktu Nyata Presisi." },
  { "en": "Apa Itu Hard Real Time?", "id": "Wajib Tepat Waktu Tanpa Toleransi." },
  { "en": "Apa Itu Soft Real Time?", "id": "Toleransi Keterlambatan Masih Diterima." },
  { "en": "Apa Itu Task Scheduler?", "id": "Pengatur Urutan Eksekusi Program." },
  { "en": "Apa Itu Preemptive Scheduling?", "id": "Tugas Prioritas Menghentikan Tugas Rendah." },
  { "en": "Apa Itu Round Robin Scheduling?", "id": "Bergiliran Waktu Sama Rata." },
  { "en": "Apa Itu Context Switch?", "id": "Penyimpanan Status Saat Pindah Tugas." },
  { "en": "Apa Itu Latency Interrupt?", "id": "Waktu Tunda Respon Sinyal Luar." },
  { "en": "Apa Itu Semaphore?", "id": "Sinyal Izin Akses Sumber Daya." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Kunci Eksklusif Satu Proses." },
  { "en": "Apa Itu Deadlock?", "id": "Dua Proses Saling Tunggu Macet." },
  { "en": "Apa Itu Priority Inversion?", "id": "Prioritas Rendah Menghambat Prioritas Tinggi." },
  { "en": "Apa Itu Race Condition?", "id": "Hasil Tergantung Urutan Waktu Eksekusi." },
  { "en": "Apa Itu Stack Overflow?", "id": "Memori Tumpukan Penuh Meluap." },
  { "en": "Apa Itu Heap Fragmentation?", "id": "Memori Dinamis Terpecah Pecah." },
  { "en": "Apa Itu Deterministic System?", "id": "Waktu Respon Selalu Konsisten Pasti." },
  { "en": "Apa Itu Jitter Sinyal?", "id": "Variasi Waktu Kedatangan Sinyal." },
  { "en": "Apa Itu Watchdog Timer Hardware?", "id": "Reset Chip Jika Program Macet." },
  { "en": "Apa Itu Bootloader?", "id": "Program Awal Sebelum Sistem Operasi." },
  { "en": "Apa Itu Firmware OTA?", "id": "Update Perangkat Lunak Nirkabel." },
  { "en": "Apa Itu Protokol CANopen?", "id": "Standar Komunikasi Berbasis CAN Bus." },
  { "en": "Apa Itu DeviceNet?", "id": "Jaringan Industri Berbasis CAN." },
  { "en": "Apa Itu Profibus DP?", "id": "Decentralized Peripherals Kecepatan Tinggi." },
  { "en": "Apa Itu Profibus PA?", "id": "Process Automation Area Berbahaya." },
  { "en": "Apa Itu Foundation Fieldbus H1?", "id": "Komunikasi Digital 31,25 kbit/s." },
  { "en": "Apa Itu HSE (High Speed Ethernet)?", "id": "Backbone Cepat Foundation Fieldbus." },
  { "en": "Apa Itu AS-Interface (AS-i)?", "id": "Kabel Pipih Sensor Aktuator Sederhana." },
  { "en": "Apa Itu IO-Link?", "id": "Komunikasi Titik Ke Titik Sensor." },
  { "en": "Apa Itu HART Protocol?", "id": "Data Digital Tumpangan Analog 4-20mA." },
  { "en": "Apa Itu WirelessHART?", "id": "Jaringan Mesh Sensor Nirkabel." },
  { "en": "Apa Itu ISA100.11a?", "id": "Standar Otomasi Nirkabel Industri." },
  { "en": "Apa Itu Zigbee Pro?", "id": "Jaringan Mesh Hemat Daya." },
  { "en": "Apa Itu LoRaWAN?", "id": "Jaringan Luas Daya Rendah." },
  { "en": "Apa Itu MQTT Broker?", "id": "Server Pengelola Pesan IoT." },
  { "en": "Apa Itu OPC DA (Data Access)?", "id": "Standar Lama Berbasis Windows COM." },
  { "en": "Apa Itu OPC UA (Unified Architecture)?", "id": "Standar Baru Lintas Platform Aman." },
  { "en": "Apa Itu Gateway Industri?", "id": "Penerjemah Antar Protokol Berbeda." },
  { "en": "Apa Itu Switch Managed?", "id": "Switch Jaringan Bisa Dikonfigurasi." },
  { "en": "Apa Itu Switch Unmanaged?", "id": "Switch Plug And Play Sederhana." },
  { "en": "Apa Itu VLAN (Virtual LAN)?", "id": "Memisahkan Jaringan Secara Logika." },
  { "en": "Apa Itu Redundancy Ring?", "id": "Topologi Cincin Tahan Putus." },
  { "en": "Apa Itu STP (Spanning Tree Protocol)?", "id": "Mencegah Loop Pada Ethernet." },
  { "en": "Apa Itu QoS (Quality Of Service)?", "id": "Prioritas Paket Data Penting." },
  { "en": "Apa Itu VPN Tunnel?", "id": "Jalur Aman Lewat Internet Publik." },
  { "en": "Apa Itu Firewall Deep Packet Inspection?", "id": "Memeriksa Isi Data Protokol Industri." },
  { "en": "Apa Itu Air Gap Security?", "id": "Isolasi Fisik Jaringan Total." },
  { "en": "Apa Itu Safety PLC?", "id": "PLC Khusus Fungsi Keselamatan." },
  { "en": "Apa Warna Modul Safety PLC?", "id": "Biasanya Berwarna Kuning Atau Merah." },
  { "en": "Apa Itu Dual Channel Input?", "id": "Dua Sensor Untuk Satu Fungsi." },
  { "en": "Apa Itu Discrepancy Time?", "id": "Toleransi Beda Waktu Dua Kanal." },
  { "en": "Apa Itu Proof Test Interval?", "id": "Jadwal Uji Fungsi Safety." },
  { "en": "Apa Itu SIL (Safety Integrity Level)?", "id": "Tingkat Keandalan Pengurangan Risiko." },
  { "en": "Apa Itu PL (Performance Level)?", "id": "Standar Keamanan Mesin ISO 13849." },
  { "en": "Apa Itu Emergency Stop Category 0?", "id": "Memutus Daya Seketika Stop." },
  { "en": "Apa Itu Emergency Stop Category 1?", "id": "Pengereman Terkendali Lalu Putus." },
  { "en": "Apa Itu Light Curtain?", "id": "Tirai Sensor Cahaya Pengaman." },
  { "en": "Apa Itu Laser Scanner Safety?", "id": "Deteksi Area Bahaya Dengan Laser." },
  { "en": "Apa Itu Two Hand Control?", "id": "Wajib Tekan Dua Tombol Bersamaan." },
  { "en": "Apa Itu Safety Relay?", "id": "Relay Ganda Dengan Cross Monitoring." },
  { "en": "Apa Itu Feedback Loop Monitoring?", "id": "Memastikan Kontaktor Benar Terbuka." },
  { "en": "Apa Itu Lockout Tagout (LOTO)?", "id": "Prosedur Kunci Sumber Energi." },
  { "en": "Apa Itu Arc Flash Suit?", "id": "Pakaian Tahan Ledakan Listrik." },
  { "en": "Apa Itu Insulated Tools?", "id": "Alat Kerja Lapis Isolator 1000V." },
  { "en": "Apa Itu Grounding Stick?", "id": "Tongkat Pembuang Muatan Sisa." },
  { "en": "Apa Itu Hot Stick?", "id": "Tongkat Kerja Bertegangan Tinggi." },
  { "en": "Apa Itu Step Voltage?", "id": "Tegangan Antara Dua Kaki." },
  { "en": "Apa Itu Touch Voltage?", "id": "Tegangan Antara Tangan Dan Kaki." },
  { "en": "Apa Itu Equipotential Bonding?", "id": "Menyamakan Tegangan Semua Logam." },
  { "en": "Apa Itu GFCI (Ground Fault Interrupter)?", "id": "Pemutus Arus Bocor Ke Tanah." },
  { "en": "Apa Itu AFCI (Arc Fault Interrupter)?", "id": "Pemutus Deteksi Percikan Api." },
  { "en": "Apa Itu Class 1 Division 1?", "id": "Area Gas Bahaya Selalu Ada." },
  { "en": "Apa Itu Class 1 Division 2?", "id": "Area Gas Bahaya Jarang Ada." },
  { "en": "Apa Itu Intrinsic Safety Barrier?", "id": "Pembatas Energi Ke Area Bahaya." },
  { "en": "Apa Itu Explosion Proof Enclosure?", "id": "Box Tahan Ledakan Dari Dalam." },
  { "en": "Apa Itu Purged Enclosure?", "id": "Box Bertekanan Udara Bersih." },
  { "en": "Apa Itu IP69K?", "id": "Tahan Semprotan Air Panas Tekanan." },
  { "en": "Apa Itu NEMA 4X?", "id": "Tahan Air Dan Korosi Outdoor." },
  { "en": "Apa Itu Motion Profile?", "id": "Grafik Kecepatan Terhadap Waktu." },
  { "en": "Apa Itu Jerk Dalam Gerakan?", "id": "Perubahan Percepatan Per Detik." },
  { "en": "Apa Itu S-Curve Ramp?", "id": "Akselerasi Halus Bentuk S." },
  { "en": "Apa Itu Electronic Gearing?", "id": "Sinkronisasi Rasio Putaran Digital." },
  { "en": "Apa Itu Camming Profile?", "id": "Gerakan Mengikuti Pola Non Linier." },
  { "en": "Apa Itu Homing Sequence?", "id": "Proses Mencari Titik Nol Mesin." },
  { "en": "Apa Itu Limit Switch Hard?", "id": "Pembatas Gerak Sakelar Fisik." },
  { "en": "Apa Itu Limit Switch Soft?", "id": "Pembatas Gerak Angka Software." },
  { "en": "Apa Itu Following Error?", "id": "Selisih Posisi Target Dan Aktual." },
  { "en": "Apa Itu Encoder Resolution?", "id": "Jumlah Pulsa Per Putaran." },
  { "en": "Apa Itu Quadrature Encoder?", "id": "Dua Sinyal Beda Fasa 90." },
  { "en": "Apa Itu Absolute Encoder?", "id": "Posisi Tetap Tahu Walau Mati." },
  { "en": "Apa Itu SSI (Synchronous Serial Interface)?", "id": "Protokol Data Encoder Absolut." },
  { "en": "Apa Itu BiSS Interface?", "id": "Protokol Encoder Digital Terbuka." },
  { "en": "Apa Itu Resolver?", "id": "Sensor Posisi Tahan Lingkungan Keras." },
  { "en": "Apa Itu Torque Mode Control?", "id": "Mengatur Kekuatan Putar Motor." },
  { "en": "Apa Itu Velocity Mode Control?", "id": "Mengatur Kecepatan Putar Motor." },
  { "en": "Apa Itu Position Mode Control?", "id": "Mengatur Sudut Lokasi Motor." },
  { "en": "Apa Itu PID Autotuning?", "id": "Mencari Parameter Kontrol Otomatis." },
  { "en": "Apa Itu Feedforward Gain?", "id": "Prediksi Output Tanpa Menunggu Error." },
  { "en": "Apa Itu Konfigurasi One And Half Breaker?", "id": "3 Breaker Untuk 2 Sirkuit." },
  { "en": "Apa Kelebihan One And Half Breaker?", "id": "Keandalan Tinggi Fleksibel Perawatan." },
  { "en": "Apa Itu Konfigurasi Ring Bus?", "id": "Breaker Disusun Melingkar Hemat Biaya." },
  { "en": "Apa Kelemahan Ring Bus?", "id": "Sulit Perluasan Sirkuit Baru." },
  { "en": "Apa Itu Konfigurasi Double Bus Single Breaker?", "id": "Dua Busbar Satu Pemutus Sirkuit." },
  { "en": "Apa Itu Bus Coupler Breaker?", "id": "Penghubung Antara Dua Busbar Utama." },
  { "en": "Apa Itu Transfer Bus?", "id": "Busbar Cadangan Saat Pemeliharaan Breaker." },
  { "en": "Apa Itu Mesh Corner Substation?", "id": "Gardu Tanpa Busbar Utama Konvensional." },
  { "en": "Apa Itu GIL (Gas Insulated Line)?", "id": "Pipa Transmisi Daya Berisi Gas." },
  { "en": "Gas Apa Yang Digunakan Dalam GIL?", "id": "Campuran SF6 Dan Nitrogen." },
  { "en": "Apa Keunggulan GIL Dibanding Kabel Tanah?", "id": "Kapasitas Arus Jauh Lebih Besar." },
  { "en": "Apa Itu Ferroresonance Pada Gardu Induk?", "id": "Resonansi Non Linier Trafo Kapasitor." },
  { "en": "Penyebab Utama Ferroresonance Adalah?", "id": "Switching Beban Ringan Trafo Tegangan." },
  { "en": "Apa Itu Damping Resistor VT?", "id": "Peredam Osilasi Ferroresonance Trafo." },
  { "en": "Apa Itu TRV (Transient Recovery Voltage)?", "id": "Tegangan Balik Kontak Saat Memutus." },
  { "en": "Apa Itu RRRV (Rate of Rise of Recovery Voltage)?", "id": "Kecepatan Naiknya Tegangan Balik." },
  { "en": "Breaker Gagal Memutus Jika?", "id": "RRRV Lebih Cepat Dari Dielektrik." },
  { "en": "Apa Itu First Pole To Clear Factor?", "id": "Tegangan Lebih Pada Kutub Pertama." },
  { "en": "Nilai Faktor Kutub Pertama Standar?", "id": "1,3 Atau 1,5 Kali Tegangan." },
  { "en": "Apa Itu Pre-Insertion Resistor Breaker?", "id": "Resistor Peredam Saat Menutup Kontak." },
  { "en": "Apa Itu Point On Wave Switching?", "id": "Menutup Kontak Saat Fasa Tepat." },
  { "en": "Tujuan Point On Wave Adalah?", "id": "Mengurangi Inrush Current Trafo." },
  { "en": "Apa Itu Sampled Values IEC 61850?", "id": "Data Digital Arus Tegangan Ethernet." },
  { "en": "Apa Itu Merging Unit (MU)?", "id": "Pengubah Analog Ke Sampled Values." },
  { "en": "Apa Itu Process Bus?", "id": "Jaringan Komunikasi Level Sensor." },
  { "en": "Apa Itu Station Bus?", "id": "Jaringan Komunikasi Level Kontrol." },
  { "en": "Apa Itu GOOSE Message?", "id": "Sinyal Event Cepat Antar Relay." },
  { "en": "Apa Itu Time Synchronization PTP?", "id": "Sinkronisasi Waktu Presisi Mikro Detik." },
  { "en": "Apa Itu Redundancy Protocol PRP?", "id": "Parallel Redundancy Protocol Tanpa Jeda." },
  { "en": "Apa Itu Redundancy Protocol HSR?", "id": "High Availability Seamless Redundancy." },
  { "en": "Apa Itu AFE (Active Front End)?", "id": "Penyearah VFD Dengan IGBT." },
  { "en": "Apa Kelebihan AFE VFD?", "id": "Harmonisa Rendah Dan Bisa Regeneratif." },
  { "en": "Apa Itu Flux Braking?", "id": "Pengereman Dengan Overshoot Fluks Motor." },
  { "en": "Apa Itu High Slip Braking?", "id": "Pengereman Dengan Mengatur Slip Motor." },
  { "en": "Apa Itu DC Link Choke?", "id": "Induktor Perata Arus Bus DC." },
  { "en": "Apa Itu AC Line Reactor?", "id": "Induktor Peredam Harmonisa Input." },
  { "en": "Apa Itu Sine Wave Filter VFD?", "id": "Filter Output Menjadi Sinus Murni." },
  { "en": "Kapan Sine Wave Filter Wajib?", "id": "Kabel Motor Sangat Panjang." },
  { "en": "Apa Itu Top Balancing Baterai?", "id": "Menyeimbangkan Sel Saat Penuh." },
  { "en": "Apa Itu Bottom Balancing Baterai?", "id": "Menyeimbangkan Sel Saat Kosong." },
  { "en": "Apa Itu Capacity Test Baterai?", "id": "Uji Nyata Kapasitas Simpan Energi." },
  { "en": "Apa Itu Impedance Track Technology?", "id": "Algoritma Estimasi Sisa Baterai." },
  { "en": "Apa Itu Battery Grading?", "id": "Klasifikasi Kualitas Sel Pabrik." },
  { "en": "Apa Itu Repurposed Battery?", "id": "Baterai Bekas Mobil Untuk Rumah." },
  { "en": "Apa Itu PV Optimizer?", "id": "MPPT Terpisah Tiap Panel Surya." },
  { "en": "Apa Itu Rapid Shutdown PV?", "id": "Mematikan Tegangan Atap Saat Darurat." },
  { "en": "Apa Itu Arc Fault Detection Unit?", "id": "Deteksi Percikan Api Kabel DC." },
  { "en": "Apa Itu Ground Fault Detection Interrupter?", "id": "Deteksi Bocor Arus Sisi DC." },
  { "en": "Apa Itu Solenoid Valve Pilot Operated?", "id": "Menggunakan Tekanan Pipa Untuk Buka." },
  { "en": "Apa Itu Solenoid Valve Direct Acting?", "id": "Kekuatan Magnet Langsung Buka Katup." },
  { "en": "Direct Acting Valve Cocok Untuk?", "id": "Tekanan Rendah Atau Vakum." },
  { "en": "Pilot Operated Valve Cocok Untuk?", "id": "Tekanan Tinggi Pipa Besar." },
  { "en": "Apa Itu Water Hammer Arrester?", "id": "Tabung Udara Peredam Kejut Pipa." },
  { "en": "Apa Itu Check Valve Swing?", "id": "Katup Satu Arah Ayun." },
  { "en": "Apa Itu Check Valve Spring?", "id": "Katup Satu Arah Pegas." },
  { "en": "Apa Itu Globe Valve?", "id": "Katup Pengatur Debit Presisi." },
  { "en": "Apa Itu Gate Valve?", "id": "Katup Buka Tutup Penuh Saja." },
  { "en": "Apa Itu Ball Valve?", "id": "Katup Bola Putar Cepat." },
  { "en": "Apa Itu Butterfly Valve Wafer?", "id": "Katup Kupu Kupu Tipis." },
  { "en": "Apa Itu Actuator Pneumatic Single Acting?", "id": "Kembali Posisi Dengan Pegas." },
  { "en": "Apa Itu Actuator Pneumatic Double Acting?", "id": "Buka Tutup Dengan Tekanan Angin." },
  { "en": "Apa Itu Namur Interface?", "id": "Standar Dudukan Solenoid Valve." },
  { "en": "Apa Itu Limit Switch Box?", "id": "Kotak Indikator Posisi Valve." },
  { "en": "Apa Itu Electro-Pneumatic Positioner?", "id": "Pengatur Bukaan Valve Sinyal Listrik." },
  { "en": "Apa Itu Industrial Ethernet Switch?", "id": "Switch Tahan Suhu Dan Getaran." },
  { "en": "Apa Itu PoE Mode A?", "id": "Daya Lewat Kabel Data Langsung." },
  { "en": "Apa Itu PoE Mode B?", "id": "Daya Lewat Kabel Cadangan." },
  { "en": "Apa Itu Fiber Optic Splicing Tray?", "id": "Wadah Pelindung Sambungan Fiber." },
  { "en": "Apa Itu Optical Splitter PLC?", "id": "Planar Lightwave Circuit Splitter." },
  { "en": "Apa Itu Optical Splitter FBT?", "id": "Fused Biconical Taper Splitter." },
  { "en": "Splitter PLC Lebih Baik Untuk?", "id": "Rasio Pembagian Banyak Kanal." },
  { "en": "Apa Itu Dark Fiber Leasing?", "id": "Sewa Kabel Optik Non Aktif." },
  { "en": "Apa Itu Metro Ethernet?", "id": "Jaringan Ethernet Skala Kota." },
  { "en": "Apa Itu MPLS Network?", "id": "Multi Protocol Label Switching." },
  { "en": "Apa Itu SD-WAN?", "id": "Software Defined Wide Area Network." },
  { "en": "Apa Itu VSAT Ku-Band?", "id": "Internet Satelit Piringan Kecil." },
  { "en": "Apa Itu VSAT C-Band?", "id": "Internet Satelit Tahan Hujan." },
  { "en": "Apa Itu LNB LO Frequency?", "id": "Frekuensi Osilator Lokal LNB." },
  { "en": "Apa Itu Cross-Pol Isolation?", "id": "Pemisahan Sinyal Vertikal Horizontal." },
  { "en": "Apa Itu Sun Outage Satelit?", "id": "Gangguan Sinyal Saat Matahari Segaris." },
  { "en": "Kapan Sun Outage Terjadi?", "id": "Sekitar Ekuinoks Maret Dan September." },
  { "en": "Apa Itu Azimuth Calculator?", "id": "Hitung Arah Kompas Antena." },
  { "en": "Apa Itu Elevation Angle?", "id": "Sudut Dongak Antena Ke Langit." },
  { "en": "Apa Itu Skew Angle LNB?", "id": "Sudut Putar LNB Terhadap Horizon." },
  { "en": "Apa Itu BUC Power?", "id": "Daya Pancar Block Up Converter." },
  { "en": "Satuan Daya BUC Adalah?", "id": "Watt." },
  { "en": "Apa Itu Modem Satelit?", "id": "Modulator Demodulator Sinyal Satelit." },
  { "en": "Apa Itu TDMA vs SCPC?", "id": "Berbagi Waktu vs Satu Kanal." },
  { "en": "Apa Itu Teleport Station?", "id": "Stasiun Bumi Hub Satelit Besar." },
  { "en": "Apa Itu Latency Geostationary?", "id": "Sekitar 500 Sampai 600 Milidetik." },
  { "en": "Apa Itu Starlink LEO?", "id": "Internet Satelit Orbit Rendah Cepat." },
  { "en": "Latency Satelit LEO Adalah?", "id": "Sekitar 20 Sampai 40 Milidetik." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Antena Datar Tanpa Gerak Mekanik." },
  { "en": "Phased Array Mengarahkan Sinyal Dengan?", "id": "Menggeser Fasa Elemen Antena." },
  { "en": "Apa Itu Beam Steering?", "id": "Mengarahkan Sinyal Secara Elektronik." },
  { "en": "Apa Itu Frequency Reuse?", "id": "Menggunakan Frekuensi Sama Di Area Beda." },
  { "en": "Apa Itu Spot Beam Satellite?", "id": "Pancaran Fokus Area Kecil Kuat." },
  { "en": "Apa Itu Logical Node IEC 61850?", "id": "Blok Fungsi Virtual Dalam IED." },
  { "en": "Apa Kepanjangan SCL File?", "id": "Substation Configuration Language." },
  { "en": "Apa Itu ICD File IEC 61850?", "id": "Deskripsi Kemampuan IED Bawaan Pabrik." },
  { "en": "Apa Itu CID File IEC 61850?", "id": "File Konfigurasi IED Siap Pakai." },
  { "en": "Apa Itu Report Control Block?", "id": "Pengaturan Pengiriman Data Event." },
  { "en": "Apa Itu Buffered Report Control Block?", "id": "Simpan Data Saat Koneksi Putus." },
  { "en": "Apa Itu Unbuffered Report Control Block?", "id": "Data Hilang Saat Koneksi Putus." },
  { "en": "Apa Itu Client Server IEC 61850?", "id": "Komunikasi Vertikal IED Ke SCADA." },
  { "en": "Apa Itu Publisher Subscriber IEC 61850?", "id": "Komunikasi Horizontal Antar IED." },
  { "en": "Apa Itu Merit Order Effect?", "id": "Pembangkit Murah Geser Yang Mahal." },
  { "en": "Apa Itu Capacity Payment?", "id": "Bayaran Untuk Ketersediaan Daya." },
  { "en": "Apa Itu Energy Payment?", "id": "Bayaran Untuk Energi Yang Dihasilkan." },
  { "en": "Apa Itu Ramp Rate Limit?", "id": "Batas Kecepatan Naik Turun Daya." },
  { "en": "Apa Itu AGC (Automatic Generation Control)?", "id": "Pengatur Frekuensi Terpusat Otomatis." },
  { "en": "Apa Itu Tie Line Flow Control?", "id": "Mengatur Aliran Daya Antar Area." },
  { "en": "Apa Itu Islanding Detection Method?", "id": "Cara Deteksi Lepas Dari Grid." },
  { "en": "Apa Itu Passive Islanding Detection?", "id": "Pantau Tegangan Frekuensi Pasif." },
  { "en": "Apa Itu Active Islanding Detection?", "id": "Injeksi Gangguan Kecil Ke Grid." },
  { "en": "Apa Itu ROCOF Relay?", "id": "Rate Of Change Of Frequency." },
  { "en": "Apa Itu Vector Shift Relay?", "id": "Deteksi Geseran Fasa Tiba Tiba." },
  { "en": "Apa Itu Bushing Trafo?", "id": "Isolator Terminal Tembus Tangki." },
  { "en": "Apa Itu Bushing OIP?", "id": "Oil Impregnated Paper Bushing." },
  { "en": "Apa Itu Bushing RIP?", "id": "Resin Impregnated Paper Bushing." },
  { "en": "Apa Itu Tan Delta Bushing?", "id": "Ukuran Kesehatan Isolasi Bushing." },
  { "en": "Apa Itu Tap Test Bushing?", "id": "Cek Kapasitansi Terminal Tap." },
  { "en": "Apa Itu Grading Ring Bushing?", "id": "Cincin Perata Medan Bawah." },
  { "en": "Apa Itu Corona Shielding?", "id": "Pelindung Sudut Tajam HV." },
  { "en": "Apa Itu Bumpless Transfer Control?", "id": "Pindah Mode Halus Tanpa Kejut." },
  { "en": "Apa Itu Split Range Control?", "id": "Satu Output Dua Valve Berurutan." },
  { "en": "Apa Itu Ratio Control System?", "id": "Jaga Perbandingan Dua Variabel." },
  { "en": "Apa Itu Cascade Control Loop?", "id": "Loop Dalam Dan Loop Luar." },
  { "en": "Apa Itu Dead Time Kompensator?", "id": "Mengatasi Tunda Waktu Proses Lama." },
  { "en": "Apa Itu Smith Predictor?", "id": "Algoritma Kontrol Dead Time Dominan." },
  { "en": "Apa Itu Gain Scheduling PID?", "id": "Parameter Berubah Sesuai Kondisi." },
  { "en": "Apa Itu Adaptive Tuning?", "id": "PID Menyesuaikan Diri Otomatis." },
  { "en": "Apa Itu Feedforward Control?", "id": "Kompensasi Gangguan Sebelum Error." },
  { "en": "Apa Itu Feedback Control?", "id": "Koreksi Berdasarkan Error Output." },
  { "en": "Apa Itu XLPE Cross Linking?", "id": "Ikatan Silang Molekul Polimer." },
  { "en": "Apa Itu Water Treeing XLPE?", "id": "Cacat Isolasi Akibat Air." },
  { "en": "Apa Itu Electrical Treeing XLPE?", "id": "Cacat Isolasi Akibat Partial Discharge." },
  { "en": "Apa Itu Semiconductive Screen Kabel?", "id": "Lapisan Hitam Perata Medan." },
  { "en": "Apa Itu Copper Tape Shield?", "id": "Pita Tembaga Pelindung Kabel." },
  { "en": "Apa Itu Wire Screen Shield?", "id": "Kawat Tembaga Pelindung Kabel." },
  { "en": "Apa Itu Lead Sheath Kabel?", "id": "Pelindung Timbal Tahan Air." },
  { "en": "Apa Itu Corrugated Aluminum Sheath?", "id": "Pelindung Aluminium Bergelombang Ringan." },
  { "en": "Apa Itu Milliken Conductor?", "id": "Konduktor Segmen Kurangi Skin Effect." },
  { "en": "Apa Itu Segmental Conductor?", "id": "Nama Lain Milliken Conductor." },
  { "en": "Apa Itu Oil Filled Cable?", "id": "Kabel Minyak Tekanan Tinggi Kuno." },
  { "en": "Apa Itu Submarine Cable?", "id": "Kabel Laut Bawah Air." },
  { "en": "Apa Itu Double Armour Cable?", "id": "Dua Lapis Kawat Baja Pelindung." },
  { "en": "Apa Itu Fiber Optic Sensing Kabel?", "id": "Kabel Power Berisi Sensor Suhu." },
  { "en": "Apa Itu DTS Sensing?", "id": "Ukur Suhu Sepanjang Kabel Fiber." },
  { "en": "Apa Itu DAS Sensing?", "id": "Deteksi Getaran Sepanjang Kabel." },
  { "en": "Apa Itu RTD 2 Wire?", "id": "Tidak Akurat Kabel Panjang." },
  { "en": "Apa Itu RTD 3 Wire?", "id": "Kompensasi Kabel Paling Umum." },
  { "en": "Apa Itu RTD 4 Wire?", "id": "Paling Akurat Laboratorium." },
  { "en": "Apa Itu Thermocouple Extension Wire?", "id": "Kabel Perpanjangan Material Sama." },
  { "en": "Apa Itu Thermocouple Compensating Cable?", "id": "Kabel Material Beda Lebih Murah." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Koreksi Suhu Sambungan Referensi." },
  { "en": "Apa Itu Transmitter Head Mount?", "id": "Pemancar Sinyal Di Kepala Sensor." },
  { "en": "Apa Itu Transmitter DIN Rail?", "id": "Pemancar Sinyal Di Panel." },
  { "en": "Apa Itu Isolator Galvanis Sinyal?", "id": "Pemisah Ground Loop Instrumen." },
  { "en": "Apa Itu Signal Splitter?", "id": "Satu Sinyal Jadi Dua Output." },
  { "en": "Apa Itu Intrinsic Safety Barrier?", "id": "Pembatas Energi Area Ledakan." },
  { "en": "Apa Itu HART Communicator?", "id": "Alat Setting Transmitter HART." },
  { "en": "Apa Itu Damping Pada Transmitter?", "id": "Peredam Fluktuasi Sinyal Pembacaan." },
  { "en": "Apa Itu Upper Range Value (URV)?", "id": "Nilai Maksimal Skala Ukur." },
  { "en": "Apa Itu Lower Range Value (LRV)?", "id": "Nilai Minimal Skala Ukur." },
  { "en": "Apa Itu Span Adjustment?", "id": "Kalibrasi Rentang Atas Alat." },
  { "en": "Apa Itu Zero Adjustment?", "id": "Kalibrasi Titik Nol Alat." },
  { "en": "Apa Itu Wet Leg Level?", "id": "Pipa Referensi Berisi Cairan." },
  { "en": "Apa Itu Dry Leg Level?", "id": "Pipa Referensi Berisi Gas." },
  { "en": "Apa Itu Capillary Tube Sensor?", "id": "Pipa Kapiler Minyak Silikon." },
  { "en": "Apa Itu Diaphragm Seal?", "id": "Membran Pemisah Cairan Proses." },
  { "en": "Apa Itu Flush Diaphragm?", "id": "Membran Rata Dinding Tangki." },
  { "en": "Apa Itu Extended Diaphragm?", "id": "Membran Menonjol Ke Dalam." },
  { "en": "Apa Itu Ultrasonic Blind Distance?", "id": "Jarak Dekat Tak Terbaca Sensor." },
  { "en": "Apa Itu Radar Guided Wave?", "id": "Radar Lewat Kawat Pemandu." },
  { "en": "Apa Itu Non Contact Radar?", "id": "Radar Lewat Udara Bebas." },
  { "en": "Apa Itu Dielectric Constant Cairan?", "id": "Faktor Pantulan Sinyal Radar." },
  { "en": "Apa Itu Coriolis Mass Flow?", "id": "Ukur Berat Cairan Langsung." },
  { "en": "Apa Itu Magnetic Flow Meter Liner?", "id": "Lapisan Dalam Pipa Sensor." },
  { "en": "Apa Itu Electrode Magmeter?", "id": "Titik Kontak Cairan Sensor." },
  { "en": "Apa Itu Empty Pipe Detection?", "id": "Alarm Pipa Kosong Magmeter." },
  { "en": "Apa Itu Low Flow Cutoff?", "id": "Mengenolkan Aliran Sangat Kecil." },
  { "en": "Apa Itu Totalizer Flow?", "id": "Penghitung Jumlah Volume Total." },
  { "en": "Apa Itu Pulse Output Flow?", "id": "Sinyal Denyut Per Volume." },
  { "en": "Apa Itu Analog Output Flow?", "id": "Sinyal Arus Sesuai Debit." },
  { "en": "Apa Itu Orifice Plate Beta Ratio?", "id": "Rasio Diameter Lubang Dan Pipa." },
  { "en": "Apa Itu Vena Contracta?", "id": "Titik Aliran Tersempit Setelah Orifice." },
  { "en": "Apa Itu Permanent Pressure Loss?", "id": "Tekanan Hilang Akibat Sensor." },
  { "en": "Apa Itu Turndown Ratio Flow?", "id": "Rentang Ukur Maksimal Banding Minimal." },
  { "en": "Apa Itu Rotameter Float?", "id": "Benda Apung Penunjuk Aliran." },
  { "en": "Apa Itu Variable Area Flowmeter?", "id": "Nama Lain Dari Rotameter." },
  { "en": "Apa Itu Thermal Mass Flowmeter?", "id": "Ukur Aliran Berdasarkan Pendinginan." },
  { "en": "Apa Itu Vortex Shedding?", "id": "Pusaran Di Belakang Benda Halang." },
  { "en": "Apa Itu Bluff Body Vortex?", "id": "Batang Pembuat Pusaran Aliran." },
  { "en": "Apa Itu K-Factor Flowmeter?", "id": "Jumlah Pulsa Per Liter." },
  { "en": "Apa Itu Prover Tank?", "id": "Tangki Kalibrasi Flowmeter." },
  { "en": "Apa Itu Master Meter?", "id": "Meter Standar Acuan Kalibrasi." },
  { "en": "Apa Itu Murray Loop Test?", "id": "Metode Jembatan Cari Lokasi Ground." },
  { "en": "Apa Itu Varley Loop Test?", "id": "Metode Jembatan Cari Lokasi Short." },
  { "en": "Apa Itu Cable Thumper?", "id": "Pembangkit Dentuman Tegangan Tinggi." },
  { "en": "Prinsip Kerja Cable Thumper?", "id": "Suara Ledakan Di Titik Gangguan." },
  { "en": "Apa Itu Arc Reflection Method?", "id": "Gabungan TDR Dan Thumper." },
  { "en": "Apa Kelebihan Arc Reflection?", "id": "Tidak Merusak Isolasi Kabel." },
  { "en": "Apa Itu TDR Pulse Width?", "id": "Lebar Pulsa Sinyal Radar Kabel." },
  { "en": "Pulsa TDR Lebar Digunakan Untuk?", "id": "Melihat Kabel Jarak Jauh." },
  { "en": "Pulsa TDR Sempit Digunakan Untuk?", "id": "Resolusi Tinggi Jarak Dekat." },
  { "en": "Apa Itu VLF Cable Testing?", "id": "Uji Tegangan Frekuensi 0,1 Hz." },
  { "en": "Apa Itu Sheath Fault Locator?", "id": "Alat Cari Bocor Kulit Kabel." },
  { "en": "Apa Itu Step Voltage Method?", "id": "Deteksi Tegangan Tanah Titik Bocor." },
  { "en": "Apa Itu Tone Generator Tracer?", "id": "Pelacak Kabel Putus Dengan Suara." },
  { "en": "Apa Itu Rolling Sphere Method?", "id": "Desain Proteksi Petir Bola Gelinding." },
  { "en": "Radius Bola Gelinding Kelas 1?", "id": "20 Meter." },
  { "en": "Radius Bola Gelinding Kelas 4?", "id": "60 Meter." },
  { "en": "Apa Itu Protection Angle Method?", "id": "Sudut Lindung Batang Air Terminal." },
  { "en": "Apa Itu Mesh Method Lightning?", "id": "Jaring Konduktor Di Atap Datar." },
  { "en": "Ukuran Mesh Kelas 1 Adalah?", "id": "5 Kali 5 Meter." },
  { "en": "Apa Itu Down Conductor Petir?", "id": "Kabel Penurun Arus Ke Tanah." },
  { "en": "Jarak Antar Down Conductor Kelas 1?", "id": "10 Meter." },
  { "en": "Apa Itu Natural Component Down Conductor?", "id": "Tiang Beton Bertulang Gedung." },
  { "en": "Apa Itu Equipotential Bonding Bar?", "id": "Busbar Penyatuan Semua Grounding." },
  { "en": "Apa Itu Spark Gap Bonding?", "id": "Penyatu Ground Terpisah Saat Petir." },
  { "en": "Apa Itu Surge Protective Device (SPD)?", "id": "Alat Pelindung Tegangan Surja." },
  { "en": "SPD Tipe 1 Dipasang Di?", "id": "Panel Utama Main Distribution Panel." },
  { "en": "SPD Tipe 2 Dipasang Di?", "id": "Panel Bagi Sub Distribution Panel." },
  { "en": "SPD Tipe 3 Dipasang Di?", "id": "Dekat Peralatan Elektronik Sensitif." },
  { "en": "Apa Itu Op-Amp Slew Rate?", "id": "Kecepatan Perubahan Tegangan Output." },
  { "en": "Apa Itu Unity Gain Bandwidth?", "id": "Frekuensi Saat Penguatan Satu Kali." },
  { "en": "Apa Itu Common Mode Rejection Ratio?", "id": "Kemampuan Menolak Sinyal Bersama." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Error Input Saat Nol." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Masuk Kaki Basis Op-Amp." },
  { "en": "Apa Itu Virtual Ground Op-Amp?", "id": "Titik Referensi Semu Non Ground." },
  { "en": "Apa Itu Rail To Rail Output?", "id": "Tegangan Output Mencapai Suplai." },
  { "en": "Apa Itu Schmitt Trigger Op-Amp?", "id": "Komparator Dengan Histeresis Noise." },
  { "en": "Apa Itu Precision Rectifier?", "id": "Penyearah Aktif Dioda Ideal." },
  { "en": "Apa Itu Logarithmic Amplifier?", "id": "Output Logaritma Dari Input." },
  { "en": "Apa Itu Transimpedance Amplifier?", "id": "Pengubah Arus Menjadi Tegangan." },
  { "en": "Aplikasi Transimpedance Amplifier Adalah?", "id": "Penguat Sensor Photodiode." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Beda Impedansi Tinggi." },
  { "en": "Apa Itu PID Instruction PLC?", "id": "Blok Fungsi Kontrol Proses." },
  { "en": "Apa Itu Scaling Instruction?", "id": "Konversi Nilai Analog Ke Engineering." },
  { "en": "Apa Itu Move Instruction (MOV)?", "id": "Menyalin Data Antar Register." },
  { "en": "Apa Itu Compare Instruction?", "id": "Membandingkan Dua Nilai Data." },
  { "en": "Apa Itu Jump Instruction (JMP)?", "id": "Lompat Ke Label Program Lain." },
  { "en": "Apa Itu Subroutine Call?", "id": "Memanggil Program Bagian Terpisah." },
  { "en": "Apa Itu One Shot Rising (OSR)?", "id": "Pulsa Satu Scan Saat Naik." },
  { "en": "Apa Itu One Shot Falling (OSF)?", "id": "Pulsa Satu Scan Saat Turun." },
  { "en": "Apa Itu First Scan Bit?", "id": "Bit Aktif Saat PLC Nyala." },
  { "en": "Apa Itu Always On Bit?", "id": "Bit Selalu Bernilai Satu." },
  { "en": "Apa Itu Always Off Bit?", "id": "Bit Selalu Bernilai Nol." },
  { "en": "Apa Itu FIFO Instruction?", "id": "First In First Out Antrian." },
  { "en": "Apa Itu LIFO Instruction?", "id": "Last In First Out Tumpukan." },
  { "en": "Apa Itu Shift Register Bit?", "id": "Geser Posisi Bit Kiri Kanan." },
  { "en": "Apa Itu Sequencer Instruction?", "id": "Pengendali Langkah Berurutan." },
  { "en": "Apa Itu Real Time Clock (RTC)?", "id": "Jam Internal PLC." },
  { "en": "Apa Itu High Speed Counter (HSC)?", "id": "Penghitung Pulsa Cepat Encoder." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Output Pulsa Lebar Variabel." },
  { "en": "Apa Itu Pulse Train Output (PTO)?", "id": "Output Pulsa Frekuensi Variabel." },
  { "en": "PTO Digunakan Untuk Mengendalikan?", "id": "Motor Stepper Dan Servo." },
  { "en": "Apa Itu IEC 61131-3 Standard?", "id": "Standar Bahasa Pemrograman PLC." },
  { "en": "Apa Itu Instruction List (IL)?", "id": "Bahasa Teks Mirip Assembly." },
  { "en": "Apa Itu Structured Text (ST)?", "id": "Bahasa Teks Mirip Pascal C." },
  { "en": "Apa Itu Ladder Diagram (LD)?", "id": "Bahasa Grafis Kontak Relay." },
  { "en": "Apa Itu Function Block Diagram (FBD)?", "id": "Bahasa Grafis Blok Logika." },
  { "en": "Apa Itu Sequential Function Chart (SFC)?", "id": "Bahasa Alur Proses Step." },
  { "en": "Apa Itu Data Type BOOL?", "id": "Boolean Satu Bit 0 1." },
  { "en": "Apa Itu Data Type INT?", "id": "Integer Bilangan Bulat 16 Bit." },
  { "en": "Apa Itu Data Type DINT?", "id": "Double Integer 32 Bit." },
  { "en": "Apa Itu Data Type REAL?", "id": "Floating Point Bilangan Pecahan." },
  { "en": "Apa Itu Data Type TIME?", "id": "Durasi Waktu Milidetik." },
  { "en": "Apa Itu Data Type STRING?", "id": "Kumpulan Karakter Teks." },
  { "en": "Apa Itu Data Type ARRAY?", "id": "Kumpulan Data Sejenis Berurutan." },
  { "en": "Apa Itu Data Type STRUCT?", "id": "Kumpulan Data Berbeda Jenis." },
  { "en": "Apa Itu User Defined Data Type?", "id": "Tipe Data Buatan Pengguna." },
  { "en": "Apa Itu Retentive Memory?", "id": "Memori Tahan Saat Listrik Mati." },
  { "en": "Apa Itu Temporary Memory?", "id": "Memori Sementara Dalam Fungsi." },
  { "en": "Apa Itu Global Variable?", "id": "Variabel Diakses Semua Program." },
  { "en": "Apa Itu Local Variable?", "id": "Variabel Hanya Dalam Satu Program." },
  { "en": "Apa Itu Firmware Update PLC?", "id": "Pembaruan Sistem Operasi PLC." },
  { "en": "Apa Itu Online Editing?", "id": "Ubah Program Saat PLC Run." },
  { "en": "Apa Itu Upload Program?", "id": "Ambil Program Dari PLC." },
  { "en": "Apa Itu Download Program?", "id": "Kirim Program Ke PLC." },
  { "en": "Apa Itu Force I/O?", "id": "Paksa Nilai Input Output." },
  { "en": "Apa Itu Cross Reference?", "id": "Daftar Penggunaan Alamat Memori." },
  { "en": "Apa Itu Rung Comment?", "id": "Catatan Penjelas Baris Ladder." },
  { "en": "Apa Itu Tag Name?", "id": "Nama Simbolis Alamat Memori." },
  { "en": "Apa Itu Alias Tag?", "id": "Nama Lain Untuk Tag Sama." },
  { "en": "Apa Itu Produced Consumed Tag?", "id": "Pertukaran Data Antar PLC." },
  { "en": "Apa Itu Add On Instruction (AOI)?", "id": "Blok Fungsi Buatan Sendiri." },
  { "en": "Apa Itu Safety PLC?", "id": "PLC Standar Keamanan Tinggi." },
  { "en": "Apa Itu Dual Channel Input?", "id": "Dua Sensor Satu Fungsi Safety." },
  { "en": "Apa Itu Pulse Test Output?", "id": "Cek Kabel Putus Secara Otomatis." },
  { "en": "Apa Itu Scan Cycle PLC?", "id": "Waktu Eksekusi Program Satu Putaran." },
  { "en": "Apa Itu Tag Database HMI?", "id": "Penyimpanan Variabel Data HMI." },
  { "en": "Apa Itu Historian Server?", "id": "Database Riwayat Data Jangka Panjang." },
  { "en": "Apa Itu Alarm Management?", "id": "Pengelolaan Notifikasi Gangguan Sistem." },
  { "en": "Apa Itu Trend Display?", "id": "Grafik Data Terhadap Waktu." },
  { "en": "Apa Itu Faceplate HMI?", "id": "Jendela Pop Up Kontrol Detail." },
  { "en": "Apa Itu Industrial Ethernet?", "id": "Ethernet Tahan Lingkungan Keras." },
  { "en": "Apa Itu Managed Switch?", "id": "Switch Bisa Dikonfigurasi IP." },
  { "en": "Apa Itu Unmanaged Switch?", "id": "Switch Plug And Play Sederhana." },
  { "en": "Apa Itu VLAN Tagging?", "id": "Pemisahan Jaringan Secara Logika." },
  { "en": "Apa Itu Port Mirroring?", "id": "Duplikasi Trafik Untuk Monitoring." },
  { "en": "Apa Itu MAC Address Filtering?", "id": "Keamanan Berbasis Alamat Fisik." },
  { "en": "Apa Itu Modbus TCP/IP?", "id": "Modbus Lewat Kabel Jaringan." },
  { "en": "Apa Itu DNP3 Protocol?", "id": "Protokol Telemetri Gardu Induk." },
  { "en": "Apa Itu IEC 60870-5-104?", "id": "Telecontrol Berbasis IP Network." },
  { "en": "Apa Itu OPC Data Access?", "id": "Standar Pertukaran Data Windows." },
  { "en": "Apa Itu OPC Unified Architecture?", "id": "Standar Data Lintas Platform." },
  { "en": "Apa Itu MQTT Protocol?", "id": "Protokol IoT Ringan Publish Subscribe." },
  { "en": "Apa Itu Broker MQTT?", "id": "Server Pengatur Pesan IoT." },
  { "en": "Apa Itu Topic MQTT?", "id": "Alamat Kategori Pesan Data." },
  { "en": "Apa Itu QoS Level 0?", "id": "Kirim Sekali Tanpa Garansi." },
  { "en": "Apa Itu QoS Level 1?", "id": "Minimal Satu Kali Sampai." },
  { "en": "Apa Itu QoS Level 2?", "id": "Tepat Satu Kali Sampai." },
  { "en": "Apa Itu JSON Format?", "id": "Format Data Teks Terstruktur." },
  { "en": "Apa Itu XML Format?", "id": "Format Data Tag Terstruktur." },
  { "en": "Apa Itu Cyber Attack Vector?", "id": "Jalur Masuk Serangan Siber." },
  { "en": "Apa Itu Denial Of Service?", "id": "Membanjiri Jaringan Sampai Lumpuh." },
  { "en": "Apa Itu Man In The Middle?", "id": "Penyadapan Komunikasi Di Tengah." },
  { "en": "Apa Itu Firewall Rules?", "id": "Aturan Izin Lewat Paket." },
  { "en": "Apa Itu VPN Tunneling?", "id": "Jalur Enkripsi Jarak Jauh." },
  { "en": "Apa Itu Two Factor Authentication?", "id": "Dua Langkah Verifikasi Login." },
  { "en": "Apa Itu Air Gap Network?", "id": "Jaringan Terputus Fisik Internet." },
  { "en": "Apa Itu Intrusion Detection System?", "id": "Deteksi Pola Serangan Jaringan." },
  { "en": "Apa Itu Safety Instrumented Function?", "id": "Fungsi Keselamatan Otomatis Spesifik." },
  { "en": "Apa Itu Safety Integrity Level?", "id": "Tingkat Keandalan Pengurangan Risiko." },
  { "en": "Apa Itu Probability Of Failure?", "id": "Kemungkinan Gagal Saat Diminta." },
  { "en": "Apa Itu Risk Reduction Factor?", "id": "Faktor Penurunan Risiko Bahaya." },
  { "en": "Apa Itu Safe Failure Fraction?", "id": "Persentase Kegagalan Ke Aman." },
  { "en": "Apa Itu Diagnostic Coverage?", "id": "Kemampuan Deteksi Kerusakan Sendiri." },
  { "en": "Apa Itu Redundancy N+1?", "id": "Satu Cadangan Untuk N Alat." },
  { "en": "Apa Itu Voting Logic 1oo2?", "id": "Satu Dari Dua Trip." },
  { "en": "Apa Itu Voting Logic 2oo3?", "id": "Dua Dari Tiga Trip." },
  { "en": "Apa Itu Emergency Shutdown System?", "id": "Sistem Matikan Pabrik Darurat." },
  { "en": "Apa Itu Fire And Gas System?", "id": "Deteksi Api Dan Gas Bocor." },
  { "en": "Apa Itu Partial Stroke Test?", "id": "Uji Gerak Valve Sebagian." },
  { "en": "Apa Itu Solenoid Valve SIL?", "id": "Valve Sertifikasi Safety Level." },
  { "en": "Apa Itu Motor Protection Relay?", "id": "Relay Khusus Pengaman Motor." },
  { "en": "Apa Itu Thermal Overload Relay?", "id": "Proteksi Panas Berlebih Motor." },
  { "en": "Apa Itu Locked Rotor Protection?", "id": "Proteksi Saat Rotor Macet." },
  { "en": "Apa Itu Jamming Protection?", "id": "Proteksi Beban Macet Tiba Tiba." },
  { "en": "Apa Itu Unbalance Protection?", "id": "Proteksi Arus Tidak Seimbang." },
  { "en": "Apa Itu Phase Reversal Protection?", "id": "Proteksi Urutan Fasa Terbalik." },
  { "en": "Apa Itu Undercurrent Protection?", "id": "Proteksi Beban Hilang Pompa." },
  { "en": "Apa Itu Dry Run Protection?", "id": "Mencegah Pompa Jalan Tanpa Air." },
  { "en": "Apa Itu Restart Inhibition?", "id": "Mencegah Start Ulang Terlalu Cepat." },
  { "en": "Apa Itu Motor Heating Element?", "id": "Pemanas Anti Embun Motor." },
  { "en": "Apa Itu Bearing Temp Sensor?", "id": "Sensor Suhu Bantalan Motor." },
  { "en": "Apa Itu Winding Temp Sensor?", "id": "Sensor Suhu Gulungan Motor." },
  { "en": "Apa Itu Vibration Monitoring?", "id": "Pemantauan Getaran Mesin." },
  { "en": "Apa Itu Partial Discharge Motor?", "id": "Deteksi Isolasi Bocor Dini." },
  { "en": "Apa Itu Soft Starter Bypass?", "id": "Kontaktor Pintas Setelah Start." },
  { "en": "Apa Itu VFD Bypass Mode?", "id": "Jalan Langsung Tanpa Inverter." },
  { "en": "Apa Itu Harmonic Mitigation?", "id": "Pengurangan Kandungan Harmonisa." },
  { "en": "Apa Itu Line Reactor VFD?", "id": "Induktor Input Peredam Harmonisa." },
  { "en": "Apa Itu Load Reactor VFD?", "id": "Induktor Output Pelindung Motor." },
  { "en": "Apa Itu dV/dt Filter?", "id": "Filter Lonjakan Tegangan Output." },
  { "en": "Apa Itu Sinewave Filter?", "id": "Filter Output Gelombang Sinus." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Peredam Arus Bocor Bearing." },
  { "en": "Apa Itu Shaft Grounding Ring?", "id": "Sikat Grounding Poros Motor." },
  { "en": "Apa Itu Insulated Bearing?", "id": "Bantalan Isolasi Listrik." },
  { "en": "Apa Itu Ceramic Bearing?", "id": "Bantalan Bola Keramik Isolator." },
  { "en": "Apa Itu Vector Control VFD?", "id": "Kontrol Torsi Presisi Tinggi." },
  { "en": "Apa Itu V/Hz Control VFD?", "id": "Kontrol Standar Pompa Kipas." },
  { "en": "Apa Itu Torque Limit VFD?", "id": "Batas Maksimal Torsi Motor." },
  { "en": "Apa Itu Speed Reference?", "id": "Sinyal Setpoint Kecepatan." },
  { "en": "Apa Itu Ramp Up Time?", "id": "Waktu Akselerasi Kecepatan." },
  { "en": "Apa Itu Ramp Down Time?", "id": "Waktu Deselerasi Kecepatan." },
  { "en": "Apa Itu DC Injection Braking?", "id": "Pengereman Suntik Arus DC." },
  { "en": "Apa Itu Dynamic Braking Resistor?", "id": "Resistor Buang Energi Rem." },
  { "en": "Apa Itu Regenerative Unit?", "id": "Kembalikan Energi Rem Ke Grid." },
  { "en": "Apa Itu Multi Speed Motor?", "id": "Motor Dua Kecepatan Dahlander." },
  { "en": "Apa Itu Slip Ring Motor?", "id": "Motor Rotor Belitan Cincin." },
  { "en": "Apa Itu Liquid Resistance Starter?", "id": "Starter Tahanan Cairan Elektrolit." },
  { "en": "Apa Itu Star Delta Starter?", "id": "Starter Dua Tahap Tegangan." },
  { "en": "Apa Itu DOL Starter?", "id": "Starter Langsung Tegangan Penuh." },
  { "en": "Apa Itu Soft Starter?", "id": "Starter Tegangan Naik Bertahap." },
  { "en": "Apa Itu Vacuum Contactor?", "id": "Kontaktor Pemutus Ruang Hampa." },
  { "en": "Apa Itu Arc Chute Contactor?", "id": "Peredam Busur Api Udara." },
  { "en": "Apa Itu Auxiliary Contact?", "id": "Kontak Bantu Status Sakelar." },
  { "en": "Apa Itu Mechanical Interlock?", "id": "Pencegah Dua Kontaktor Masuk." },
  { "en": "Apa Itu Electrical Interlock?", "id": "Kunci Logika Rangkaian Kontrol." },
  { "en": "Apa Itu Push Button Station?", "id": "Kotak Tombol Kendali Lapangan." },
  { "en": "Apa Itu Pilot Light Tester?", "id": "Tombol Cek Lampu Panel." },
  { "en": "Apa Itu Cable Entry Gland?", "id": "Pengikat Kabel Masuk Panel." },
  { "en": "Apa Itu Busbar Support Insulator?", "id": "Isolator Penyangga Rel Tembaga." },
  { "en": "Apa Itu Neutral Link?", "id": "Terminal Sambung Kabel Netral." },
  { "en": "Apa Itu Earth Bar?", "id": "Terminal Sambung Kabel Ground." },
  { "en": "Apa Itu CCS Combo 1?", "id": "Konektor Standar Amerika 1 Fasa." },
  { "en": "Apa Itu CCS Combo 2?", "id": "Konektor Standar Eropa 3 Fasa." },
  { "en": "Apa Itu Liquid Cooled Cable?", "id": "Kabel Charger Pendingin Cairan." },
  { "en": "Apa Itu Megawatt Charging System?", "id": "Standar Charging Truk Listrik." },
  { "en": "Berapa Arus Maksimal CCS 2?", "id": "500 Ampere Pada Kabel Dingin." },
  { "en": "Apa Itu Plug And Charge?", "id": "Otentikasi Otomatis Tanpa Aplikasi." },
  { "en": "Apa Itu ISO 15118-20?", "id": "Standar V2G Dua Arah." },
  { "en": "Apa Itu Wireless Power Transfer?", "id": "Pengisian Induktif Tanpa Kabel." },
  { "en": "Efisiensi Wireless Charging EV?", "id": "Sekitar 90 Sampai 93 Persen." },
  { "en": "Apa Itu Pantograph Charging Bus?", "id": "Pengisian Cepat Bus Di Atap." },
  { "en": "Apa Itu Locational Marginal Pricing?", "id": "Harga Listrik Di Titik Tertentu." },
  { "en": "Apa Itu Congestion Cost?", "id": "Biaya Akibat Kemacetan Transmisi." },
  { "en": "Apa Itu Day Ahead Market?", "id": "Pasar Listrik Untuk Besok." },
  { "en": "Apa Itu Real Time Market?", "id": "Pasar Listrik Saat Ini." },
  { "en": "Apa Itu Capacity Market?", "id": "Pasar Ketersediaan Daya Jangka Panjang." },
  { "en": "Apa Itu Arbitrage Listrik?", "id": "Beli Murah Jual Mahal Baterai." },
  { "en": "Apa Itu Levelized Cost Of Storage?", "id": "Biaya Simpan Energi Per kWh." },
  { "en": "Apa Itu Grid Parity?", "id": "Biaya EBT Sama Dengan Fosil." },
  { "en": "Apa Itu Nodal Analysis?", "id": "Analisis Tegangan Titik Simpul." },
  { "en": "Apa Itu Mesh Analysis?", "id": "Analisis Arus Loop Tertutup." },
  { "en": "Apa Itu Supernode Analysis?", "id": "Sumber Tegangan Antara Dua Node." },
  { "en": "Apa Itu Supermesh Analysis?", "id": "Sumber Arus Antara Dua Mesh." },
  { "en": "Apa Itu Transient Response?", "id": "Respon Sesaat Perubahan Status." },
  { "en": "Apa Itu Overdamped Response?", "id": "Kembali Stabil Tanpa Osilasi." },
  { "en": "Apa Itu Underdamped Response?", "id": "Kembali Stabil Dengan Osilasi." },
  { "en": "Apa Itu Critically Damped?", "id": "Kembali Stabil Tercepat Tanpa Osilasi." },
  { "en": "Apa Itu Time Constant Tau?", "id": "Waktu Respon 63,2 Persen." },
  { "en": "Apa Itu Steady State Response?", "id": "Respon Setelah Waktu Lama." },
  { "en": "Apa Itu Harmonic Resonance?", "id": "Impedansi L Dan C Meniadakan." },
  { "en": "Apa Itu Parallel Resonance?", "id": "Impedansi Tinggi Arus Nol." },
  { "en": "Apa Itu Series Resonance?", "id": "Impedansi Rendah Arus Tinggi." },
  { "en": "Bahaya Series Resonance Adalah?", "id": "Arus Harmonisa Sangat Besar." },
  { "en": "Apa Itu Interharmonics?", "id": "Frekuensi Bukan Kelipatan Bulat." },
  { "en": "Sumber Interharmonics Adalah?", "id": "Cycloconverter Dan Tungku Busur." },
  { "en": "Apa Itu Flicker Pst?", "id": "Short Term Flicker Severity." },
  { "en": "Batas Pst Standar Adalah?", "id": "Maksimal 1,0." },
  { "en": "Apa Itu Voltage Unbalance?", "id": "Ketidakseimbangan Tegangan Tiga Fasa." },
  { "en": "Rumus Voltage Unbalance NEMA?", "id": "Deviasi Maksimum Bagi Rata Rata." },
  { "en": "Apa Itu Superconductor Cable?", "id": "Kabel Tanpa Hambatan Suhu Rendah." },
  { "en": "Apa Itu HTS Cable?", "id": "High Temperature Superconductor." },
  { "en": "Pendingin Kabel HTS Adalah?", "id": "Nitrogen Cair." },
  { "en": "Apa Itu Fault Current Limiter?", "id": "Pembatas Arus Gangguan Superkonduktor." },
  { "en": "Apa Itu Solid State Transformer?", "id": "Trafo Berbasis Elektronika Daya." },
  { "en": "Kelebihan Solid State Transformer?", "id": "Ukuran Kecil Dan Kontrol Penuh." },
  { "en": "Apa Itu Digital Substation?", "id": "Gardu Induk Full Fiber Optik." },
  { "en": "Apa Itu NCIT Sensor?", "id": "Non Conventional Instrument Transformer." },
  { "en": "Apa Itu Rogowski Coil Accuracy?", "id": "Linearitas Tinggi Tanpa Saturasi." },
  { "en": "Apa Itu Optical Voltage Transducer?", "id": "Sensor Tegangan Efek Pockels." },
  { "en": "Apa Itu Process Bus IEC 61850-9-2?", "id": "Bus Data Sampled Values." },
  { "en": "Apa Itu Station Bus IEC 61850-8-1?", "id": "Bus Data MMS Dan GOOSE." },
  { "en": "Apa Itu Parallel Redundancy Protocol?", "id": "Duplikasi Paket Data Tanpa Jeda." },
  { "en": "Apa Itu HSR Ring Protocol?", "id": "Redundansi Cincin Tanpa Switch." },
  { "en": "Apa Itu Precision Time Protocol?", "id": "Sinkronisasi Waktu Bawah Mikrodetik." },
  { "en": "Apa Itu Ethernet Switch IEEE 1588?", "id": "Switch Dukungan Hardware Timestamping." },
  { "en": "Apa Itu Transient Stability?", "id": "Kestabilan Saat Gangguan Besar." },
  { "en": "Apa Itu Dynamic Stability?", "id": "Kestabilan Saat Gangguan Kecil." },
  { "en": "Apa Itu Voltage Stability?", "id": "Kemampuan Menjaga Tegangan Bus." },
  { "en": "Apa Itu Frequency Stability?", "id": "Kemampuan Menjaga Frekuensi Sistem." },
  { "en": "Apa Itu Rotor Angle Stability?", "id": "Sinkronisasi Sudut Rotor Generator." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Alat Peredam Osilasi Frekuensi Rendah." },
  { "en": "Apa Itu FACTS Device?", "id": "Flexible AC Transmission System." },
  { "en": "Apa Itu STATCOM?", "id": "Kompensator Statis Berbasis Inverter." },
  { "en": "Apa Itu SVC (Static Var Compensator)?", "id": "Kompensator Pasif Berbasis Thyristor." },
  { "en": "Apa Itu UPFC?", "id": "Unified Power Flow Controller." },
  { "en": "Apa Itu TCSC?", "id": "Thyristor Controlled Series Capacitor." },
  { "en": "Apa Itu Phase Shifting Transformer?", "id": "Trafo Pengatur Aliran Daya Aktif." },
  { "en": "Apa Itu Grid Forming Control?", "id": "Inverter Membentuk Tegangan Sendiri." },
  { "en": "Apa Itu Grid Following Control?", "id": "Inverter Mengikuti Fasa Grid." },
  { "en": "Apa Itu Virtual Synchronous Machine?", "id": "Inverter Meniru Karakteristik Generator." },
  { "en": "Apa Itu Droop Control Inverter?", "id": "Berbagi Beban Tanpa Komunikasi." },
  { "en": "Apa Itu Synthetic Inertia?", "id": "Inersia Buatan Dari Baterai." },
  { "en": "Apa Itu Microgrid Controller?", "id": "Otak Pengendali Jaringan Mandiri." },
  { "en": "Apa Itu Islanding Mode?", "id": "Operasi Terpisah Dari Grid Utama." },
  { "en": "Apa Itu Resynchronization?", "id": "Menyambung Kembali Ke Grid Utama." },
  { "en": "Apa Itu Black Start Microgrid?", "id": "Menyalakan Microgrid Dari Mati Total." },
  { "en": "Apa Itu Distributed Energy Resource?", "id": "Sumber Energi Tersebar Kecil." },
  { "en": "Apa Itu Virtual Power Plant?", "id": "Agregasi Pembangkit Kecil Terpadu." },
  { "en": "Apa Itu Demand Response Event?", "id": "Perintah Turunkan Beban Pelanggan." },
  { "en": "Apa Itu Aggregator?", "id": "Pengumpul Partisipasi Beban Kecil." },
  { "en": "Apa Itu Transactive Energy?", "id": "Pasar Energi Peer To Peer." },
  { "en": "Apa Itu Blockchain Energy?", "id": "Ledger Transaksi Energi Terdesentralisasi." },
  { "en": "Apa Itu Smart Meter Gateway?", "id": "Gerbang Komunikasi Meter Cerdas." },
  { "en": "Apa Itu Home Energy Management?", "id": "Sistem Pengelola Listrik Rumah." },
  { "en": "Apa Itu Load Profiling?", "id": "Analisis Pola Pemakaian Listrik." },
  { "en": "Apa Itu Peak Load Shifting?", "id": "Geser Beban Ke Luar Puncak." },
  { "en": "Apa Itu Base Load Plant?", "id": "Pembangkit Beban Dasar Murah." },
  { "en": "Apa Itu Peaker Plant?", "id": "Pembangkit Beban Puncak Cepat." },
  { "en": "Apa Itu Load Factor?", "id": "Rasio Beban Rata Rata Puncak." },
  { "en": "Apa Itu Capacity Factor?", "id": "Rasio Output Nyata Maksimal." },
  { "en": "Apa Itu Heat Rate?", "id": "Efisiensi Bahan Bakar Pembangkit." },
  { "en": "Apa Itu Spark Spread?", "id": "Selisih Harga Listrik Dan Gas." },
  { "en": "Apa Itu Skin Depth Tembaga 50Hz?", "id": "Sekitar 8,5 mm." },
  { "en": "Apa Itu Proximity Effect Kabel?", "id": "Arus Terdesak Akibat Medan Tetangga." },
  { "en": "Apa Fungsi Lapisan Semikonduktor Kabel?", "id": "Meratakan Medan Listrik Isolasi." },
  { "en": "Apa Itu Water Treeing?", "id": "Kerusakan Isolasi Akibat Kelembaban." },
  { "en": "Apa Itu Electrical Treeing?", "id": "Jalur Karbon Akibat Partial Discharge." },
  { "en": "Apa Itu Sheath Voltage Limiter?", "id": "Proteksi Tegangan Lebih Layar Kabel." },
  { "en": "Apa Itu Cross Bonding Kabel?", "id": "Teknik Grounding Mengurangi Arus Sirkulasi." },
  { "en": "Apa Itu Link Box Kabel?", "id": "Kotak Sambungan Grounding Sheath." },
  { "en": "Apa Itu Transposisi Kabel?", "id": "Menukar Posisi Fasa Seimbangkan Impedansi." },
  { "en": "Apa Itu Corona Inception Voltage?", "id": "Tegangan Mulai Muncul Korona." },
  { "en": "Apa Itu Visual Corona?", "id": "Cahaya Ungu Di Sekitar Konduktor." },
  { "en": "Apa Itu Audible Noise Corona?", "id": "Suara Desis Akibat Pelepasan Muatan." },
  { "en": "Apa Itu Radio Interference Voltage?", "id": "Gangguan Frekuensi Radio Akibat Korona." },
  { "en": "Apa Fungsi Grading Ring?", "id": "Mengurangi Stres Medan Listrik Ujung." },
  { "en": "Apa Itu Dry Band Arcing?", "id": "Loncat Api Di Isolator Basah." },
  { "en": "Apa Itu Leakage Distance Isolator?", "id": "Jarak Bocor Arus Permukaan." },
  { "en": "Apa Itu Pollution Severity Level?", "id": "Tingkat Kekotoran Lingkungan Isolator." },
  { "en": "Apa Itu Hydrophobicity Class?", "id": "Kemampuan Isolator Menolak Air." },
  { "en": "Apa Itu ESDD (Equivalent Salt Density)?", "id": "Ukuran Polusi Garam Permukaan." },
  { "en": "Apa Itu Constant Torque Region?", "id": "Area Kendali Torsi Tetap." },
  { "en": "Apa Itu Constant Power Region?", "id": "Area Kendali Daya Tetap." },
  { "en": "Apa Itu Field Weakening Point?", "id": "Titik Mulai Pelemahan Medan." },
  { "en": "Apa Itu Base Speed Motor?", "id": "Kecepatan Nominal Pada Tegangan Penuh." },
  { "en": "Apa Itu Maximum Speed Motor?", "id": "Batas Kecepatan Mekanis Dan Elektrik." },
  { "en": "Apa Itu Stall Torque?", "id": "Torsi Saat Motor Ditahan Diam." },
  { "en": "Apa Itu Cogging Torque?", "id": "Torsi Denyut Akibat Gigi Stator." },
  { "en": "Apa Itu Torque Ripple?", "id": "Fluktuasi Torsi Saat Berputar." },
  { "en": "Apa Itu Back EMF Constant?", "id": "Tegangan Balik Per RPM." },
  { "en": "Apa Itu Torque Constant Kt?", "id": "Torsi Per Ampere Arus." },
  { "en": "Apa Itu SAS (Substation Automation System)?", "id": "Sistem Otomasi Gardu Induk." },
  { "en": "Apa Itu IED (Intelligent Electronic Device)?", "id": "Relay Proteksi Berbasis Mikroprosesor." },
  { "en": "Apa Itu Merging Unit IEC 61850?", "id": "Pengubah Analog Ke Digital Sampled." },
  { "en": "Apa Itu GOOSE Message?", "id": "Sinyal Event Cepat Antar IED." },
  { "en": "Apa Itu MMS Protocol?", "id": "Komunikasi Data Client Server SCADA." },
  { "en": "Apa Itu Sampled Values (SV)?", "id": "Data Gelombang Arus Tegangan Digital." },
  { "en": "Apa Itu CID File?", "id": "File Konfigurasi IED Spesifik." },
  { "en": "Apa Itu SCD File?", "id": "File Konfigurasi Seluruh Gardu." },
  { "en": "Apa Itu Interoperability IEC 61850?", "id": "Kemampuan Kerja Sama Beda Merek." },
  { "en": "Apa Itu Time Synchronization PTP?", "id": "Sinkronisasi Waktu Presisi Jaringan." },
  { "en": "Apa Itu PID Derivative Kick?", "id": "Lonjakan Output Saat Setpoint Berubah." },
  { "en": "Cara Mengatasi Derivative Kick?", "id": "Turunan Pada Process Variable Saja." },
  { "en": "Apa Itu PID Reset Windup?", "id": "Saturasi Integral Akibat Error Lama." },
  { "en": "Apa Itu Bumpless Transfer?", "id": "Pindah Manual Auto Tanpa Kejut." },
  { "en": "Apa Itu Deadtime Process?", "id": "Waktu Tunda Respon Sistem." },
  { "en": "Apa Itu Gain Scheduling?", "id": "Parameter PID Berubah Sesuai Kondisi." },
  { "en": "Apa Itu Cascade Control?", "id": "Output Master Jadi Setpoint Slave." },
  { "en": "Apa Itu Split Range Control?", "id": "Satu Output Mengatur Dua Valve." },
  { "en": "Apa Itu Ratio Control?", "id": "Menjaga Perbandingan Dua Aliran." },
  { "en": "Apa Itu Fuse Cutout (FCO)?", "id": "Sekering Tegangan Menengah Trafo." },
  { "en": "Apa Itu Fuse Link Type K?", "id": "Sekering Tipe Cepat." },
  { "en": "Apa Itu Fuse Link Type T?", "id": "Sekering Tipe Lambat." },
  { "en": "Apa Itu Melting Time Fuse?", "id": "Waktu Elemen Meleleh." },
  { "en": "Apa Itu Clearing Time Fuse?", "id": "Total Waktu Sampai Arus Putus." },
  { "en": "Apa Itu Breaking Capacity Fuse?", "id": "Arus Gangguan Maksimal Bisa Diputus." },
  { "en": "Apa Itu HRC Fuse?", "id": "High Rupturing Capacity Fuse." },
  { "en": "Apa Fungsi Pasir Kuarsa Fuse?", "id": "Memadamkan Busur Api Dan Dinginkan." },
  { "en": "Apa Itu Striker Pin Fuse?", "id": "Pin Penonjok Mekanik Saat Putus." },
  { "en": "Apa Itu Semiconductor Fuse?", "id": "Sekering Sangat Cepat Lindungi SCR." },
  { "en": "Apa Itu CT Saturation?", "id": "Inti CT Jenuh Tidak Linier." },
  { "en": "Apa Itu Knee Point Voltage?", "id": "Titik Belok Kurva Saturasi CT." },
  { "en": "Apa Itu Accuracy Class 5P20?", "id": "Error 5 Persen Saat 20xIn." },
  { "en": "Apa Itu Instrument Security Factor?", "id": "Batas Arus Aman Meter CT." },
  { "en": "Apa Itu CT Polarity Dot?", "id": "Tanda Arah Arus Primer Sekunder." },
  { "en": "Apa Itu Burden Resistor CT?", "id": "Beban Sekunder Trafo Arus." },
  { "en": "Bahaya Sekunder CT Terbuka?", "id": "Tegangan Tinggi Bisa Meledak." },
  { "en": "Apa Itu Zero Sequence CT?", "id": "CT Lingkaran Besar Tiga Kabel." },
  { "en": "Apa Itu Interposing CT?", "id": "CT Tambahan Penyesuai Rasio." },
  { "en": "Apa Itu Summation CT?", "id": "Menjumlahkan Arus Beberapa Sirkuit." },
  { "en": "Apa Itu Battery Internal Resistance?", "id": "Hambatan Dalam Sel Baterai." },
  { "en": "Apa Itu Battery Sulfation?", "id": "Kristal Timbal Sulfat Pada Pelat." },
  { "en": "Apa Itu Equalizing Charge?", "id": "Cas Tegangan Tinggi Ratakan Sel." },
  { "en": "Apa Itu Float Charging?", "id": "Cas Tegangan Konstan Jaga Penuh." },
  { "en": "Apa Itu Battery Stratification?", "id": "Asam Berat Mengendap Di Bawah." },
  { "en": "Apa Itu Thermal Runaway Battery?", "id": "Panas Hasilkan Panas Lebih Besar." },
  { "en": "Apa Itu Peukert Law?", "id": "Kapasitas Turun Jika Arus Besar." },
  { "en": "Apa Itu DoD (Depth Of Discharge)?", "id": "Persentase Kapasitas Terpakai." },
  { "en": "Apa Itu Battery Cycle Life?", "id": "Jumlah Siklus Cas Hingga Rusak." },
  { "en": "Apa Itu BMS Balancing?", "id": "Menyamakan Tegangan Tiap Sel." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Peredam Frekuensi Tinggi." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Kembar Redam Noise Bersama." },
  { "en": "Apa Itu Differential Mode Noise?", "id": "Gangguan Antara Kabel Fasa Netral." },
  { "en": "Apa Itu X-Capacitor?", "id": "Kapasitor Antar Fasa Dan Netral." },
  { "en": "Apa Itu Y-Capacitor?", "id": "Kapasitor Antara Fasa Dan Ground." },
  { "en": "Apa Itu EMI Filter?", "id": "Penyaring Gangguan Elektromagnetik." },
  { "en": "Apa Itu Varistor MOV?", "id": "Resistor Berubah Sesuai Tegangan." },
  { "en": "Apa Itu Gas Discharge Tube?", "id": "Tabung Gas Pelindung Petir." },
  { "en": "Apa Itu TVS Diode?", "id": "Dioda Pelindung Lonjakan Cepat." },
  { "en": "Apa Itu Polyfuse PTC?", "id": "Sekering Reset Otomatis Saat Dingin." },
  { "en": "Apa Itu Relay Latching?", "id": "Relay Mengunci Posisi Tanpa Listrik." },
  { "en": "Apa Itu Relay Solid State?", "id": "Relay Tanpa Kontak Mekanik." },
  { "en": "Apa Itu Contact Bounce?", "id": "Getaran Kontak Saat Menutup." },
  { "en": "Apa Itu Arc Suppression?", "id": "Pemadaman Busur Api Kontak." },
  { "en": "Apa Itu Wetting Current?", "id": "Arus Minimal Bersihkan Oksida Kontak." },
  { "en": "Apa Itu Rogers Ratio Method?", "id": "Metode Analisis Gas Terlarut Trafo." },
  { "en": "Apa Itu Duval Triangle Method?", "id": "Segitiga Diagnosa Gas Minyak Trafo." },
  { "en": "Apa Itu Key Gas Method?", "id": "Diagnosa Berdasarkan Gas Dominan." },
  { "en": "Gas Asetilen C2H2 Menandakan Apa?", "id": "Busur Api Listrik Energi Tinggi." },
  { "en": "Gas Hidrogen H2 Menandakan Apa?", "id": "Partial Discharge Atau Corona." },
  { "en": "Gas Metana CH4 Menandakan Apa?", "id": "Pemanasan Minyak Suhu Rendah." },
  { "en": "Gas Etilen C2H4 Menandakan Apa?", "id": "Pemanasan Minyak Suhu Tinggi." },
  { "en": "Gas Etana C2H6 Menandakan Apa?", "id": "Pemanasan Minyak Suhu Menengah." },
  { "en": "Gas Karbon Monoksida CO Menandakan Apa?", "id": "Degradasi Kertas Isolasi Selulosa." },
  { "en": "Apa Itu Degree Of Polymerization (DP)?", "id": "Ukuran Kekuatan Kertas Isolasi Trafo." },
  { "en": "Nilai DP Kertas Baru Adalah?", "id": "Sekitar 1000 Sampai 1200." },
  { "en": "Nilai DP Kertas Rusak Adalah?", "id": "Di Bawah 200." },
  { "en": "Apa Itu Furan Analysis?", "id": "Uji Kimia Sisa Kertas Minyak." },
  { "en": "Apa Itu Touch Voltage IEEE 80?", "id": "Tegangan Sentuh Aman Manusia." },
  { "en": "Apa Itu Step Voltage IEEE 80?", "id": "Tegangan Langkah Aman Manusia." },
  { "en": "Apa Itu Ground Potential Rise (GPR)?", "id": "Kenaikan Tegangan Tanah Saat Gangguan." },
  { "en": "Apa Fungsi Lapisan Kerikil Gardu?", "id": "Meningkatkan Tahanan Kontak Kaki." },
  { "en": "Tebal Lapisan Kerikil Standar Adalah?", "id": "10 cm Sampai 15 cm." },
  { "en": "Apa Itu Reflection Coefficient?", "id": "Faktor Pantulan Pada Ujung Saluran." },
  { "en": "Apa Itu Bewley Lattice Diagram?", "id": "Grafik Pantulan Gelombang Berjalan." },
  { "en": "Apa Itu Surge Impedance Loading (SIL)?", "id": "Daya Alami Saluran Transmisi." },
  { "en": "Apa Itu IV Curve Tracer?", "id": "Alat Ukur Karakteristik Panel Surya." },
  { "en": "Apa Itu Anti PID Box?", "id": "Alat Pemulihan Degradasi Panel Surya." },
  { "en": "Apa Itu Bypass Diode Failure?", "id": "Dioda Rusak Menyebabkan Hotspot." },
  { "en": "Apa Itu DC Combiner Box?", "id": "Kotak Pertemuan Kabel String Surya." },
  { "en": "Apa Itu String Fusing?", "id": "Sekering Pelindung Paralel String." },
  { "en": "Apa Itu PV Array Insulation Resistance?", "id": "Tahanan Isolasi Kabel Surya Tanah." },
  { "en": "Apa Itu Earth Fault Alarm PV?", "id": "Deteksi Kebocoran Arus Ke Tanah." },
  { "en": "Apa Itu Global Tilted Irradiance (GTI)?", "id": "Radiasi Matahari Pada Bidang Miring." },
  { "en": "Apa Itu Performance Ratio (PR)?", "id": "Efisiensi Nyata Sistem Panel Surya." },
  { "en": "Apa Itu Triplen Harmonics?", "id": "Harmonisa Kelipatan Tiga." },
  { "en": "Apa Itu Zero Sequence Blocking Transformer?", "id": "Trafo Pemblokir Arus Urutan Nol." },
  { "en": "Apa Itu Subharmonics?", "id": "Frekuensi Di Bawah Frekuensi Dasar." },
  { "en": "Apa Itu Flicker Pst?", "id": "Tingkat Kedipan Jangka Pendek." },
  { "en": "Apa Itu Flicker Plt?", "id": "Tingkat Kedipan Jangka Panjang." },
  { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Ketahanan Panas Harmonisa." },
  { "en": "Apa Itu Displacement Power Factor?", "id": "Faktor Daya Gelombang Dasar." },
  { "en": "Apa Itu Distortion Power Factor?", "id": "Faktor Daya Akibat Harmonisa." },
  { "en": "Apa Itu True Power Factor?", "id": "Total Faktor Daya Termasuk Harmonisa." },
  { "en": "Apa Itu VLAN Trunking?", "id": "Satu Kabel Bawa Banyak VLAN." },
  { "en": "Apa Itu Native VLAN?", "id": "VLAN Tanpa Tag Di Trunk." },
  { "en": "Apa Itu Spanning Tree Protocol (STP)?", "id": "Mencegah Loop Jaringan Ethernet." },
  { "en": "Apa Itu Rapid Spanning Tree (RSTP)?", "id": "Pemulihan Jalur Jaringan Cepat." },
  { "en": "Apa Itu Broadcast Storm?", "id": "Banjir Paket Data Melumpuhkan Jaringan." },
  { "en": "Apa Itu IGMP Snooping?", "id": "Mengatur Lalu Lintas Multicast." },
  { "en": "Apa Itu DHCP Snooping?", "id": "Mencegah Server DHCP Palsu." },
  { "en": "Apa Itu Port Security Switch?", "id": "Batasi Mac Address Per Port." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Daftar Izin Akses Jaringan." },
  { "en": "Apa Itu Network Address Translation (NAT)?", "id": "Menerjemahkan IP Lokal Ke Publik." },
  { "en": "Apa Itu Flux Vector Control?", "id": "Kendali Torsi Motor Presisi." },
  { "en": "Apa Itu Sensorless Vector Control?", "id": "Vector Control Tanpa Encoder." },
  { "en": "Apa Itu Slip Compensation?", "id": "Menambah Frekuensi Saat Beban Berat." },
  { "en": "Apa Itu Droop Control Speed?", "id": "Kecepatan Turun Saat Torsi Naik." },
  { "en": "Apa Itu Torque Limit?", "id": "Batas Maksimal Tenaga Motor." },
  { "en": "Apa Itu Flying Start?", "id": "Menjalankan Motor Yang Sedang Berputar." },
  { "en": "Apa Itu Flux Braking?", "id": "Pengereman Dengan Rugi Besi Motor." },
  { "en": "Apa Itu Regenerative Unit?", "id": "Kembalikan Energi Rem Ke Jala." },
  { "en": "Apa Itu Safe Torque Off (STO)?", "id": "Fungsi Safety Memutus Torsi." },
  { "en": "Apa Itu Safe Stop 1 (SS1)?", "id": "Berhenti Terkendali Lalu Putus Torsi." },
  { "en": "Apa Itu Safe Limited Speed (SLS)?", "id": "Membatasi Kecepatan Maksimal Aman." },
  { "en": "Apa Itu Safe Brake Control (SBC)?", "id": "Kendali Rem Mekanik Aman." },
  { "en": "Apa Itu Light Curtain Muting?", "id": "Mematikan Sensor Saat Barang Lewat." },
  { "en": "Apa Itu Enabling Switch?", "id": "Tombol Izin Operasi Manual." },
  { "en": "Apa Itu Emergency Stop Category 0?", "id": "Stop Seketika Putus Daya." },
  { "en": "Apa Itu Emergency Stop Category 1?", "id": "Stop Terkendali Lalu Putus Daya." },
  { "en": "Apa Itu SIL 3 Rating?", "id": "Tingkat Keandalan Safety Tinggi." },
  { "en": "Apa Itu PL e Rating?", "id": "Performance Level Tertinggi ISO 13849." },
  { "en": "Apa Itu Current Transformer Class X?", "id": "CT Khusus Proteksi Diferensial." },
  { "en": "Apa Itu Knee Point Voltage CT?", "id": "Titik Jenuh Inti CT." },
  { "en": "Apa Itu CT Saturation?", "id": "Inti CT Penuh Fluks Magnet." },
  { "en": "Apa Itu Remanence Flux?", "id": "Sisa Magnet Di Inti CT." },
  { "en": "Apa Itu Instrument Security Factor (ISF)?", "id": "Batas Arus Aman Meter Analog." },
  { "en": "Apa Itu Burden Resistor?", "id": "Beban Resistif Sekunder CT." },
  { "en": "Apa Itu Composite Error CT?", "id": "Total Kesalahan Arus Dan Fasa." },
  { "en": "Apa Itu Thermal Rating Factor?", "id": "Kemampuan Arus Lebih Kontinyu CT." },
  { "en": "Apa Itu Rogowski Coil?", "id": "Sensor Arus Tanpa Inti Besi." },
  { "en": "Apa Itu Optical Current Transformer?", "id": "Sensor Arus Berbasis Cahaya." },
  { "en": "Apa Itu Buchholz Relay?", "id": "Proteksi Gas Trafo Minyak." },
  { "en": "Apa Itu Sudden Pressure Relay?", "id": "Deteksi Lonjakan Tekanan Cepat." },
  { "en": "Apa Itu Pressure Relief Device (PRD)?", "id": "Katup Buang Tekanan Darurat." },
  { "en": "Apa Itu Oil Level Indicator?", "id": "Penunjuk Tinggi Minyak Konservator." },
  { "en": "Apa Itu Winding Temperature Indicator?", "id": "Termometer Suhu Gulungan Trafo." },
  { "en": "Apa Itu Silica Gel Breather?", "id": "Penyerap Kelembaban Udara Trafo." },
  { "en": "Warna Silica Gel Kering?", "id": "Biru Atau Oranye." },
  { "en": "Warna Silica Gel Basah?", "id": "Merah Muda Atau Hijau." },
  { "en": "Apa Itu Conservator Tank?", "id": "Tangki Cadangan Minyak Trafo." },
  { "en": "Apa Itu Radiator Cooling Fin?", "id": "Sirip Pendingin Minyak Trafo." },
  { "en": "Apa Itu On Load Tap Changer?", "id": "Pindah Tap Saat Berbeban." },
  { "en": "Apa Itu Off Circuit Tap Changer?", "id": "Pindah Tap Saat Mati." },
  { "en": "Apa Itu Transformer Turns Ratio Test?", "id": "Uji Perbandingan Jumlah Lilitan." },
  { "en": "Toleransi Hasil Uji TTR Adalah?", "id": "0,5 Persen." },
  { "en": "Apa Itu Excitation Current Test?", "id": "Ukur Arus Magnetisasi Tanpa Beban." },
  { "en": "Apa Itu Magnetic Balance Test?", "id": "Cek Keseimbangan Fluks Inti Trafo." },
  { "en": "Apa Itu Short Circuit Impedance Test?", "id": "Uji Impedansi Hubung Singkat Trafo." },
  { "en": "Apa Itu Winding Resistance Test?", "id": "Ukur Hambatan Tembaga Gulungan." },
  { "en": "Apa Itu Vector Group Verification?", "id": "Memastikan Hubungan Fasa Lilitan." },
  { "en": "Apa Itu Insulation Resistance Test?", "id": "Uji Tahanan Isolasi Megger." },
  { "en": "Apa Itu Polarization Index Test?", "id": "Rasio Megger 10 Menit 1 Menit." },
  { "en": "Apa Itu Sweep Frequency Response Analysis?", "id": "Deteksi Pergeseran Mekanik Lilitan." },
  { "en": "Apa Itu Dielectric Frequency Response?", "id": "Ukur Kadar Air Isolasi Kertas." },
  { "en": "Apa Itu Z-Transform Control?", "id": "Transformasi Laplace Untuk Sinyal Diskrit." },
  { "en": "Apa Itu Region Of Convergence?", "id": "Area Kestabilan Dalam Bidang Z." },
  { "en": "Sistem Stabil Jika Pole Berada?", "id": "Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Bilinear Transformation?", "id": "Konversi S-Domain Ke Z-Domain." },
  { "en": "Apa Itu Zero Order Hold?", "id": "Menahan Nilai Sinyal Antar Sampel." },
  { "en": "Apa Itu Quantization Error?", "id": "Kesalahan Pembulatan Nilai Digital." },
  { "en": "Apa Itu Limit Cycle Oscillation?", "id": "Osilasi Akibat Kuantisasi Digital." },
  { "en": "Apa Itu Digital PID Controller?", "id": "Implementasi PID Dalam Software." },
  { "en": "Apa Itu Anti Windup Digital?", "id": "Cegah Saturasi Integral Software." },
  { "en": "Apa Itu IGBT Tail Current?", "id": "Arus Sisa Saat Turn Off." },
  { "en": "Apa Itu MOSFET Miller Plateau?", "id": "Tegangan Gate Datar Saat Switching." },
  { "en": "Apa Itu Gate Charge Qg?", "id": "Muatan Total Untuk Menyalakan Gate." },
  { "en": "Apa Itu Thermal Resistance RthJC?", "id": "Hambatan Panas Junction Ke Case." },
  { "en": "Apa Itu Safe Operating Area?", "id": "Kurva Batas Aman Tegangan Arus." },
  { "en": "Apa Itu Short Circuit Withstand Time?", "id": "Waktu Tahan Hubung Singkat IGBT." },
  { "en": "Apa Itu Desaturation Detection?", "id": "Deteksi Overcurrent Lewat Tegangan Vce." },
  { "en": "Apa Itu Active Miller Clamp?", "id": "Cegah Gate Nyala Sendiri Parasitik." },
  { "en": "Apa Itu Gate Resistor Effect?", "id": "Mengatur Kecepatan Switching Transistor." },
  { "en": "Apa Itu VSWR 1.5 Artinya?", "id": "4 Persen Daya Terpantul." },
  { "en": "Apa Itu Return Loss -20dB?", "id": "1 Persen Daya Terpantul." },
  { "en": "Apa Itu Antenna Beamwidth?", "id": "Sudut Pancaran Daya Setengah." },
  { "en": "Apa Itu Polarization Mismatch Loss?", "id": "Rugi Akibat Beda Arah Antena." },
  { "en": "Apa Itu Path Loss Exponent?", "id": "Faktor Redaman Lingkungan Propagasi." },
  { "en": "Apa Itu Diversity Technique?", "id": "Menggunakan Banyak Antena Perbaiki Sinyal." },
  { "en": "Apa Itu Spatial Diversity?", "id": "Antena Diberi Jarak Fisik." },
  { "en": "Apa Itu Frequency Diversity?", "id": "Kirim Data Di Frekuensi Beda." },
  { "en": "Apa Itu MIMO Capacity Gain?", "id": "Peningkatan Data Rate Multiplexing." },
  { "en": "Apa Itu Beamforming Gain?", "id": "Peningkatan Sinyal Arah Tertentu." },
  { "en": "Apa Itu Inverter Clipping?", "id": "Memotong Daya Lebih Kapasitas." },
  { "en": "Apa Itu MPPT Efficiency?", "id": "Akurasi Pelacakan Daya Maksimum." },
  { "en": "Apa Itu Euro Efficiency?", "id": "Efisiensi Rata Rata Beban Parsial." },
  { "en": "Apa Itu CEC Efficiency?", "id": "Standar Efisiensi Inverter California." },
  { "en": "Apa Itu PID Recovery?", "id": "Memulihkan Degradasi Panel Malam Hari." },
  { "en": "Apa Itu AFCI PV System?", "id": "Pemutus Percikan Api Arus DC." },
  { "en": "Apa Itu Rapid Shutdown NEC?", "id": "Matikan Tegangan Atap Saat Darurat." },
  { "en": "Apa Itu Temperature Coefficient Pmax?", "id": "Penurunan Daya Saat Panas." },
  { "en": "Apa Itu NOCT Test Condition?", "id": "Uji Suhu Operasi Normal Sel." },
  { "en": "Apa Itu Cut In Wind Speed?", "id": "Kecepatan Angin Mulai Produksi." },
  { "en": "Apa Itu Rated Wind Speed?", "id": "Kecepatan Angin Daya Penuh." },
  { "en": "Apa Itu Betz Limit 59.3%?", "id": "Efisiensi Teoritis Maksimal Turbin Angin." },
  { "en": "Apa Itu Tip Speed Ratio?", "id": "Rasio Kecepatan Ujung Dan Angin." },
  { "en": "Apa Itu Yaw Mechanism?", "id": "Pemutar Turbin Menghadap Angin." },
  { "en": "Apa Itu Pitch Mechanism?", "id": "Pengatur Sudut Serang Baling." },
  { "en": "Apa Itu DFIG Rotor Converter?", "id": "Inverter Sisi Rotor Generator." },
  { "en": "Apa Itu DFIG Grid Converter?", "id": "Inverter Sisi Jala Jala." },
  { "en": "Apa Itu Gearbox Failure Mode?", "id": "Kerusakan Gigi Akibat Beban Kejut." },
  { "en": "Apa Itu CMS Wind Turbine?", "id": "Sistem Pemantauan Kondisi Getaran." },
  { "en": "Apa Itu IEC 61400-25?", "id": "Protokol Komunikasi Turbin Angin." },
  { "en": "Apa Itu IEC 61850 Substation?", "id": "Standar Komunikasi Gardu Digital." },
  { "en": "Apa Itu GOOSE Priority?", "id": "Prioritas Tinggi Untuk Trip Cepat." },
  { "en": "Apa Itu Sampled Values Rate?", "id": "4000 Sampel Per Detik 50Hz." },
  { "en": "Apa Itu PTP IEEE 1588?", "id": "Sinkronisasi Waktu Presisi Ethernet." },
  { "en": "Apa Itu Redundancy PRP?", "id": "Parallel Redundancy Protocol Tanpa Jeda." },
  { "en": "Apa Itu Redundancy HSR?", "id": "High Availability Seamless Redundancy Ring." },
  { "en": "Apa Itu RBAC Cybersecurity?", "id": "Kontrol Akses Berbasis Peran." },
  { "en": "Apa Itu LPZ 0 Protection?", "id": "Zona Luar Terkena Petir Langsung." },
  { "en": "Apa Itu LPZ 1 Protection?", "id": "Zona Dalam Terlindung Surge Arrester." },
  { "en": "Apa Itu SPD Type 1?", "id": "Penahan Arus Petir Langsung." },
  { "en": "Apa Itu SPD Type 2?", "id": "Penahan Tegangan Induksi Petir." },
  { "en": "Apa Itu SPD Type 3?", "id": "Pelindung Akhir Alat Sensitif." },
  { "en": "Apa Itu Let Through Voltage?", "id": "Tegangan Sisa Setelah SPD Aktif." },
  { "en": "Apa Itu Uc Arrester?", "id": "Tegangan Operasi Kontinyu Maksimal." },
  { "en": "Apa Itu In Discharge Current?", "id": "Arus Nominal Uji 8/20us." },
  { "en": "Apa Itu Iimp Discharge Current?", "id": "Arus Impuls Uji 10/350us." },
  { "en": "Apa Itu Electrochemical Impedance Spectroscopy?", "id": "Analisis Impedansi Baterai AC." },
  { "en": "Apa Itu Kalman Filter SOC?", "id": "Estimasi Sisa Baterai Akurat." },
  { "en": "Apa Itu Passive Cell Balancing?", "id": "Buang Energi Resistor Panas." },
  { "en": "Apa Itu Active Cell Balancing?", "id": "Transfer Energi Induktor Kapasitor." },
  { "en": "Apa Itu Contactor Welding?", "id": "Kontak Lengket Akibat Arus Besar." },
  { "en": "Apa Itu HVIL (High Voltage Interlock)?", "id": "Loop Keamanan Konektor Tegangan Tinggi." },
  { "en": "Apa Itu IMD (Insulation Monitoring)?", "id": "Deteksi Kebocoran Sasis EV." },
  { "en": "Apa Itu Isolation Resistance EV?", "id": "Minimal 500 Ohm Per Volt." },
  { "en": "Apa Itu Y Capacitance Limit?", "id": "Batas Kapasitor Bocor Ke Sasis." },
  { "en": "Apa Itu Nanocrystalline Core?", "id": "Inti Magnet Rugi Rendah." },
  { "en": "Apa Itu Amorphous Core?", "id": "Inti Trafo Efisiensi Tinggi." },
  { "en": "Apa Itu Litz Wire Benefit?", "id": "Mengurangi Skin Effect Frekuensi Tinggi." },
  { "en": "Apa Itu Leakage Inductance Model?", "id": "Induktansi Seri Tidak Terkopling." },
  { "en": "Apa Itu Field Weakening Control Motor?", "id": "Kendali Kecepatan Di Atas Nominal." },
  { "en": "Apa Itu MTPA (Maximum Torque Per Ampere)?", "id": "Efisiensi Torsi Maksimal Arus Minimal." },
  { "en": "Apa Itu Space Vector Modulation (SVM)?", "id": "Teknik Modulasi Inverter Vektor Ruang." },
  { "en": "Apa Itu Dead Time Compensation?", "id": "Koreksi Distorsi Tegangan Inverter." },
  { "en": "Apa Itu Regenerative Converter?", "id": "Inverter Kembalikan Energi Ke Grid." },
  { "en": "Apa Itu DC Link Capacitor Sizing?", "id": "Hitung Kapasitas Penyangga Bus DC." },
  { "en": "Apa Itu SOH Estimation Battery?", "id": "Perkiraan Kesehatan Sisa Umur Baterai." },
  { "en": "Apa Itu SOC Estimation Coulomb Counting?", "id": "Hitung Arus Masuk Keluar Baterai." },
  { "en": "Apa Itu Active Cell Balancing?", "id": "Transfer Energi Antar Sel Baterai." },
  { "en": "Apa Itu Passive Cell Balancing?", "id": "Buang Energi Sel Ke Resistor." },
  { "en": "Apa Itu Thermal Management System EV?", "id": "Pengatur Suhu Baterai Dan Motor." },
  { "en": "Apa Itu On Board Charger (OBC)?", "id": "Pengisi Baterai AC Ke DC Mobil." },
  { "en": "Apa Itu EVSE Control Pilot?", "id": "Sinyal Komunikasi Charger Dan Mobil." },
  { "en": "Apa Itu Proximity Pilot Signal?", "id": "Deteksi Kabel Terhubung Aman." },
  { "en": "Apa Itu Bidirectional Charging (V2G)?", "id": "Mobil Suplai Listrik Ke Grid." },
  { "en": "Apa Itu Wireless Charging Resonance?", "id": "Pengisian Induktif Frekuensi Resonansi." },
  { "en": "Apa Itu Dynamic Wireless Charging?", "id": "Pengisian Saat Mobil Bergerak." },
  { "en": "Apa Itu Pantograph Charging Bus?", "id": "Pengisian Cepat Bus Lewat Atap." },
  { "en": "Apa Itu Battery Swap Station?", "id": "Stasiun Ganti Baterai Cepat." },
  { "en": "Apa Itu Traction Inverter Efficiency?", "id": "Efisiensi Pengubah DC Ke AC." },
  { "en": "Apa Itu Relay Differential Trafo 87T?", "id": "Bandingkan Arus Primer Sekunder Trafo." },
  { "en": "Apa Itu Relay Restricted Earth Fault?", "id": "Proteksi Gangguan Tanah Terbatas." },
  { "en": "Apa Itu Relay Negative Sequence 46?", "id": "Proteksi Beban Tidak Seimbang." },
  { "en": "Apa Itu Relay Thermal Overload 49?", "id": "Proteksi Panas Berlebih Motor Trafo." },
  { "en": "Apa Itu Relay Over Excitation 24?", "id": "Proteksi Fluks Berlebih Volts Hertz." },
  { "en": "Apa Itu Relay Reverse Power 32?", "id": "Mencegah Generator Jadi Motor." },
  { "en": "Apa Itu Relay Loss Of Field 40?", "id": "Proteksi Hilang Medan Penguat Generator." },
  { "en": "Apa Itu Relay Breaker Failure 50BF?", "id": "Trip Cadangan Jika Breaker Macet." },
  { "en": "Apa Itu Relay Trip Circuit Supervision?", "id": "Pantau Kesiapan Koil Pemutus." },
  { "en": "Apa Itu Relay Synchrocheck 25?", "id": "Cek Syarat Sinkronisasi Paralel." },
  { "en": "Apa Itu Relay Under Frequency 81U?", "id": "Proteksi Frekuensi Turun Load Shedding." },
  { "en": "Apa Itu Relay Rate Of Change Frequency?", "id": "Deteksi Perubahan Frekuensi Cepat." },
  { "en": "Apa Itu Relay Vector Shift?", "id": "Deteksi Pergeseran Sudut Fasa." },
  { "en": "Apa Itu Arc Flash Detection Relay?", "id": "Deteksi Kilatan Cahaya Busur Api." },
  { "en": "Apa Itu Busbar Protection 87B?", "id": "Proteksi Diferensial Rel Busbar." },
  { "en": "Apa Itu High Impedance Differential?", "id": "Stabil Saat CT Jenuh Eksternal." },
  { "en": "Apa Itu Low Impedance Differential?", "id": "Algoritma Digital Bandingkan Arus." },
  { "en": "Apa Itu Zone Selective Interlocking?", "id": "Koordinasi Cepat Antar Relay." },
  { "en": "Apa Itu PLC Scan Time?", "id": "Waktu Eksekusi Satu Siklus Program." },
  { "en": "Apa Itu PLC Watchdog Timer?", "id": "Reset Sistem Jika Program Hang." },
  { "en": "Apa Itu PLC Retentive Memory?", "id": "Data Tersimpan Saat Listrik Mati." },
  { "en": "Apa Itu PLC Analog Input Resolution?", "id": "Ketelitian Konversi Sinyal Analog." },
  { "en": "Apa Itu PLC High Speed Counter?", "id": "Hitung Pulsa Cepat Encoder." },
  { "en": "Apa Itu PLC Pulse Width Modulation?", "id": "Output Pulsa Lebar Variabel." },
  { "en": "Apa Itu PLC PID Loop?", "id": "Blok Fungsi Kontrol Proses." },
  { "en": "Apa Itu PLC Ladder Diagram?", "id": "Bahasa Pemrograman Logika Relay." },
  { "en": "Apa Itu PLC Structured Text?", "id": "Bahasa Pemrograman Mirip Pascal." },
  { "en": "Apa Itu PLC Function Block Diagram?", "id": "Bahasa Pemrograman Blok Fungsi." },
  { "en": "Apa Itu PLC Sequential Function Chart?", "id": "Bahasa Pemrograman Alur Proses." },
  { "en": "Apa Itu SCADA Master Station?", "id": "Pusat Pengendali Dan Monitor Data." },
  { "en": "Apa Itu SCADA RTU?", "id": "Unit Terminal Jarak Jauh." },
  { "en": "Apa Itu SCADA HMI?", "id": "Antarmuka Grafis Operator Mesin." },
  { "en": "Apa Itu SCADA Historian?", "id": "Database Riwayat Data Jangka Panjang." },
  { "en": "Apa Itu Modbus Function Code?", "id": "Kode Perintah Baca Tulis Data." },
  { "en": "Apa Itu Modbus Exception Code?", "id": "Kode Error Balasan Modbus." },
  { "en": "Apa Itu DNP3 Object Library?", "id": "Struktur Data Protokol DNP3." },
  { "en": "Apa Itu Protocol IEC 60870-5-104?", "id": "Standar Telemetri Listrik Eropa IP." },
  { "en": "Apa Itu SCL File IEC 61850?", "id": "File Konfigurasi Gardu Digital." },
  { "en": "Apa Itu GOOSE Message IEC 61850?", "id": "Pesan Cepat Antar Relay." },
  { "en": "Apa Itu Sampled Values IEC 61850?", "id": "Data Digital Gelombang Arus Tegangan." },
  { "en": "Apa Itu MQTT QoS Level?", "id": "Kualitas Layanan Pengiriman Pesan." },
  { "en": "Apa Itu OPC UA Security?", "id": "Enkripsi Dan Otentikasi Data." },
  { "en": "Apa Itu Profinet Protocol?", "id": "Ethernet Industri Real Time Siemens." },
  { "en": "Apa Itu Ethernet/IP Protocol?", "id": "Ethernet Industri Rockwell Automation." },
  { "en": "Apa Itu EtherCAT Protocol?", "id": "Ethernet Industri Sangat Cepat." },
  { "en": "Apa Itu IO-Link Sensor?", "id": "Komunikasi Digital Sensor Pintar." },
  { "en": "Apa Itu HART Communication?", "id": "Data Digital Di Atas 4-20mA." },
  { "en": "Apa Itu WirelessHART?", "id": "Jaringan Mesh Sensor Nirkabel Industri." },
  { "en": "Apa Itu ISA100.11a Wireless?", "id": "Standar Otomasi Nirkabel Industri." },
  { "en": "Apa Itu LoRaWAN IoT?", "id": "Jaringan Luas Daya Rendah." },
  { "en": "Apa Itu NB-IoT Cellular?", "id": "IoT Menggunakan Jaringan Seluler." },
  { "en": "Apa Itu Edge Computing IoT?", "id": "Proses Data Di Lokasi Alat." },
  { "en": "Apa Itu Cloud Computing IoT?", "id": "Proses Data Di Server Internet." },
  { "en": "Apa Itu Cyber Security OT?", "id": "Keamanan Jaringan Operasional Teknologi." },
  { "en": "Apa Itu Industrial Firewall?", "id": "Penyaring Paket Data Jaringan Pabrik." },
  { "en": "Apa Itu Air Gap Network?", "id": "Isolasi Fisik Jaringan Aman." },
  { "en": "Apa Itu VPN Secure Access?", "id": "Jalur Enkripsi Akses Jarak Jauh." },
  { "en": "Apa Itu Digital Twin?", "id": "Replika Digital Aset Fisik." },
  { "en": "Apa Itu Predictive Maintenance?", "id": "Perawatan Berdasarkan Prediksi Kondisi." },
  { "en": "Apa Itu FFT Vibration Analysis?", "id": "Analisis Spektrum Frekuensi Getaran." },
  { "en": "Apa Itu Thermography Inspection?", "id": "Inspeksi Panas Tanpa Sentuh." },
  { "en": "Apa Itu Oil Analysis DGA?", "id": "Analisis Gas Terlarut Minyak Trafo." },
  { "en": "Apa Itu Motor Current Signature Analysis?", "id": "Diagnosa Motor Dari Gelombang Arus." },
  { "en": "Apa Itu Partial Discharge Analysis?", "id": "Deteksi Cacat Isolasi Dini." },
  { "en": "Apa Itu Ultrasound Inspection?", "id": "Deteksi Kebocoran Gas Atau Arcing." },
  { "en": "Apa Itu Class A Power Quality?", "id": "Standar Alat Ukur Paling Akurat." },
  { "en": "Apa Itu Class S Power Quality?", "id": "Standar Alat Ukur Survei Statistik." },
  { "en": "Apa Itu Voltage Sag Dip?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Voltage Swell?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Harmonic Distortion?", "id": "Cacat Gelombang Akibat Beban Nonlinier." },
  { "en": "Apa Itu Flicker Voltage?", "id": "Kedipan Tegangan Berulang Ulang." },
  { "en": "Apa Itu Unbalance Voltage?", "id": "Ketidakseimbangan Tegangan Tiga Fasa." },
  { "en": "Apa Itu Frequency Deviation?", "id": "Penyimpangan Frekuensi Dari Nominal." },
  { "en": "Apa Itu Power Factor Displacement?", "id": "Faktor Daya Gelombang Dasar." },
  { "en": "Apa Itu Power Factor Distortion?", "id": "Faktor Daya Akibat Harmonisa." },
  { "en": "Apa Itu Power Factor True?", "id": "Total Faktor Daya Nyata." },
  { "en": "Apa Itu Active Harmonic Filter?", "id": "Kompensasi Harmonisa Dengan Inverter." },
  { "en": "Apa Itu Passive Harmonic Filter?", "id": "Kompensasi Harmonisa Dengan LC." },
  { "en": "Apa Itu Detuned Reactor Capacitor?", "id": "Mencegah Resonansi Kapasitor Harmonisa." },
  { "en": "Apa Itu SVG (Static Var Generator)?", "id": "Kompensator Daya Reaktif Stepless." },
  { "en": "Apa Itu SCRAM Pada PLTN?", "id": "Pematian Darurat Reaktor Nuklir." },
  { "en": "Apa Fungsi Moderator Reaktor Nuklir?", "id": "Memperlambat Gerak Neutron Cepat." },
  { "en": "Bahan Batang Kendali Nuklir Adalah?", "id": "Boron Atau Kadmium Penyerap Neutron." },
  { "en": "Apa Itu PWR (Pressurized Water Reactor)?", "id": "Reaktor Air Tekanan Tinggi." },
  { "en": "Apa Itu BWR (Boiling Water Reactor)?", "id": "Reaktor Air Mendidih Langsung." },
  { "en": "Apa Itu Fuel Rod Nuklir?", "id": "Tabung Berisi Pelet Uranium." },
  { "en": "Apa Itu Containment Building?", "id": "Gedung Beton Penahan Radiasi." },
  { "en": "Apa Itu Geiger Counter?", "id": "Alat Deteksi Radiasi Radioaktif." },
  { "en": "Apa Itu Solder Paste Stencil?", "id": "Cetakan Oles Pasta Solder PCB." },
  { "en": "Apa Itu Pick And Place Machine?", "id": "Robot Pasang Komponen SMD." },
  { "en": "Apa Itu Reflow Oven?", "id": "Pemanas Pasta Solder SMD." },
  { "en": "Apa Itu Wave Soldering?", "id": "Solder Gelombang Komponen THT." },
  { "en": "Apa Itu Selective Soldering?", "id": "Solder Robotik Titik Tertentu." },
  { "en": "Apa Itu Conformal Coating?", "id": "Pelapis PCB Anti Lembab." },
  { "en": "Apa Itu Flying Probe Test?", "id": "Uji PCB Dengan Jarum Robot." },
  { "en": "Apa Itu Bed Of Nails Test?", "id": "Uji PCB Dengan Banyak Jarum." },
  { "en": "Apa Itu Modbus Function Code 03?", "id": "Baca Data Holding Register." },
  { "en": "Apa Itu Modbus Function Code 06?", "id": "Tulis Data Single Register." },
  { "en": "Apa Itu Modbus Function Code 16?", "id": "Tulis Data Multiple Register." },
  { "en": "Apa Itu Modbus Exception Error?", "id": "Balasan Error Dari Slave." },
  { "en": "Apa Itu DNP3 Class 0 Data?", "id": "Data Statis Nilai Saat Ini." },
  { "en": "Apa Itu DNP3 Class 1 Data?", "id": "Data Event Prioritas Tinggi." },
  { "en": "Apa Itu Unsolicited Response DNP3?", "id": "Slave Kirim Data Tanpa Tanya." },
  { "en": "Apa Itu Buck-Boost Converter Non-Inverting?", "id": "Tegangan Naik Turun Polaritas Sama." },
  { "en": "Apa Itu Flyback Converter?", "id": "Konverter Terisolasi Trafo Celah Udara." },
  { "en": "Apa Itu Forward Converter?", "id": "Konverter Terisolasi Tanpa Celah Udara." },
  { "en": "Apa Itu Active Clamp Forward?", "id": "Forward Converter Efisiensi Tinggi." },
  { "en": "Apa Itu Synchronous Buck Converter?", "id": "Ganti Dioda Dengan MOSFET." },
  { "en": "Apa Itu Interleaved Buck Converter?", "id": "Dua Buck Paralel Beda Fasa." },
  { "en": "Apa Itu Voltage Multiplier Circuit?", "id": "Rangkaian Dioda Kapasitor Pengganda." },
  { "en": "Apa Itu Cockcroft-Walton Generator?", "id": "Pembangkit Tegangan Tinggi DC Bertingkat." },
  { "en": "Apa Itu Marx Generator?", "id": "Pembangkit Pulsa Tegangan Impuls." },
  { "en": "Apa Itu Tesla Coil Resonant?", "id": "Trafo Udara Resonansi Ganda." },
  { "en": "Apa Itu Skin Effect Loss?", "id": "Rugi Tahanan Akibat Frekuensi Tinggi." },
  { "en": "Apa Itu Proximity Effect Loss?", "id": "Rugi Akibat Medan Kabel Tetangga." },
  { "en": "Kabel Litz Wire Mengurangi Apa?", "id": "Skin Effect Dan Proximity Effect." },
  { "en": "Apa Itu Eddy Current Brake?", "id": "Pengereman Magnetik Tanpa Gesekan." },
  { "en": "Apa Itu Hysteresis Brake?", "id": "Pengereman Magnetik Sangat Halus." },
  { "en": "Apa Itu Magnetic Coupling?", "id": "Kopling Poros Tanpa Kontak Fisik." },
  { "en": "Apa Itu Resolver Motor Sensor?", "id": "Sensor Posisi Tahan Panas Getaran." },
  { "en": "Apa Itu LVDT Displacement Sensor?", "id": "Sensor Jarak Linier Induktif." },
  { "en": "Apa Itu Strain Gauge Bridge?", "id": "Rangkaian Sensor Berat Jembatan." },
  { "en": "Apa Itu Thermocouple Cold Junction?", "id": "Titik Sambungan Referensi Suhu." },
  { "en": "Apa Itu RTD 3-Wire Connection?", "id": "Kompensasi Panjang Kabel Sensor." },
  { "en": "Apa Itu 4-20mA Current Loop?", "id": "Standar Sinyal Analog Industri." },
  { "en": "Apa Itu HART Superimposed Signal?", "id": "Data Digital Di Atas Analog." },
  { "en": "Apa Itu Fieldbus Foundation H1?", "id": "Jaringan Instrumen Digital Penuh." },
  { "en": "Apa Itu Profibus PA Profile?", "id": "Profil Standar Instrumen Proses." },
  { "en": "Apa Itu Safety Barrier IS?", "id": "Pembatas Energi Intrinsically Safe." },
  { "en": "Apa Itu Zener Barrier?", "id": "Barrier Menggunakan Dioda Zener." },
  { "en": "Apa Itu Galvanic Isolator?", "id": "Pemisah Ground Loop Sinyal." },
  { "en": "Apa Itu Signal Conditioner?", "id": "Pengolah Sinyal Sensor Mentah." },
  { "en": "Apa Itu SCADA Historian?", "id": "Database Perekam Data Jangka Panjang." },
  { "en": "Apa Itu Alarm Annunciator?", "id": "Panel Lampu Indikator Bahaya." },
  { "en": "Apa Itu Sequence Of Events (SOE)?", "id": "Rekaman Kejadian Resolusi Milidetik." },
  { "en": "Apa Itu Watchdog Timer PLC?", "id": "Reset PLC Jika Program Macet." },
  { "en": "Apa Itu Force Value PLC?", "id": "Memaksa Nilai Input Output." },
  { "en": "Apa Itu Online Edit PLC?", "id": "Ubah Program Tanpa Stop Mesin." },
  { "en": "Apa Itu Upload Program PLC?", "id": "Ambil Program Dari Memori PLC." },
  { "en": "Apa Itu Download Program PLC?", "id": "Kirim Program Ke Memori PLC." },
  { "en": "Apa Itu Retentive Tag PLC?", "id": "Data Tersimpan Saat Listrik Mati." },
  { "en": "Apa Itu Floating Point Math?", "id": "Perhitungan Bilangan Desimal Koma." },
  { "en": "Apa Itu PID Integral Windup?", "id": "Saturasi Akumulasi Error PID." },
  { "en": "Apa Itu Feedforward Control?", "id": "Antisipasi Gangguan Sebelum Error." },
  { "en": "Apa Itu Cascade Control Loop?", "id": "Satu Kontroler Mengatur Kontroler Lain." },
  { "en": "Apa Itu Ratio Control?", "id": "Menjaga Perbandingan Dua Variabel." },
  { "en": "Apa Itu Split Range Control?", "id": "Satu Output Dua Katup." },
  { "en": "Apa Itu Valve Positioner?", "id": "Pengatur Bukaan Katup Presisi." },
  { "en": "Apa Itu Solenoid Valve NAMUR?", "id": "Standar Dudukan Katup Pneumatik." },
  { "en": "Apa Itu Limit Switch Box?", "id": "Kotak Indikator Posisi Katup." },
  { "en": "Apa Itu Air Filter Regulator?", "id": "Penyaring Dan Pengatur Tekanan Angin." },
  { "en": "Apa Itu Partial Stroke Test?", "id": "Uji Gerak Katup Sebagian." },
  { "en": "Apa Itu Safety Integrity Level (SIL)?", "id": "Tingkat Keandalan Fungsi Safety." },
  { "en": "Apa Itu Emergency Shutdown (ESD)?", "id": "Sistem Pematian Darurat Pabrik." },
  { "en": "Apa Itu Fire And Gas System?", "id": "Sistem Deteksi Api Dan Gas." },
  { "en": "Apa Itu High Integrity Pressure Protection?", "id": "Sistem Proteksi Tekanan Tinggi Khusus." },
  { "en": "Apa Itu Burner Management System?", "id": "Sistem Kendali Pembakaran Boiler." },
  { "en": "Apa Itu Vibration Monitoring System?", "id": "Pantau Getaran Mesin Berputar." },
  { "en": "Apa Itu Orbit Plot Vibration?", "id": "Grafik Gerakan Poros Mesin." },
  { "en": "Apa Itu Bode Plot Vibration?", "id": "Respon Getaran Terhadap Kecepatan." },
  { "en": "Apa Itu Critical Speed Mesin?", "id": "Kecepatan Resonansi Alami Poros." },
  { "en": "Apa Itu Oil Analysis Tribology?", "id": "Analisis Kandungan Logam Oli." },
  { "en": "Apa Itu Acoustic Emission Testing?", "id": "Deteksi Suara Retakan Material." },
  { "en": "Apa Itu Magnetic Particle Inspection?", "id": "Deteksi Retak Permukaan Logam." },
  { "en": "Apa Itu Dye Penetrant Inspection?", "id": "Deteksi Retak Dengan Cairan Warna." },
  { "en": "Apa Itu Ultrasonic Thickness Gauge?", "id": "Ukur Tebal Plat Dengan Suara." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Alat Analisis Kualitas Listrik." },
  { "en": "Apa Itu Voltage Sag/Dip?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Transient Impulse?", "id": "Lonjakan Tegangan Sangat Cepat." },
  { "en": "Apa Itu Flicker Pst?", "id": "Indeks Kedipan Jangka Pendek." },
  { "en": "Apa Itu Total Harmonic Distortion?", "id": "Persentase Cacat Gelombang Harmonisa." },
  { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Tahan Panas Harmonisa." },
  { "en": "Apa Itu Passive Harmonic Filter?", "id": "Filter LC Peredam Harmonisa." },
  { "en": "Apa Itu Active Harmonic Filter?", "id": "Inverter Penyuntik Arus Kompensasi." },
  { "en": "Apa Itu Detuned Reactor?", "id": "Cegah Resonansi Kapasitor Bank." },
  { "en": "Apa Itu Capacitor Bank Step?", "id": "Tahapan Nilai Kapasitor Otomatis." },
  { "en": "Apa Itu Hydrogen Cooling Generator?", "id": "Pendingin Gas Hidrogen Generator Besar." },
  { "en": "Mengapa Hidrogen Dipakai Pendingin Generator?", "id": "Konduktivitas Panas Tinggi Gesekan Rendah." },
  { "en": "Apa Itu Seal Oil System?", "id": "Sistem Penyegel Gas Hidrogen Generator." },
  { "en": "Apa Itu Stator Water Cooling?", "id": "Pendingin Air Murni Lilitan Stator." },
  { "en": "Apa Itu Hollow Conductor Stator?", "id": "Kawat Stator Berlubang Aliran Air." },
  { "en": "Apa Itu Excitation System Static?", "id": "Eksitasi Menggunakan Trafo Dan Rectifier." },
  { "en": "Apa Itu Brushless Excitation System?", "id": "Eksitasi Tanpa Sikat Arang Rotor." },
  { "en": "Apa Itu Pilot Exciter?", "id": "Generator Magnet Permanen Awal." },
  { "en": "Apa Itu Rotating Diode Wheel?", "id": "Roda Dioda Penyearah Di Poros." },
  { "en": "Apa Itu Field Breaker Generator?", "id": "Pemutus Arus Medan Eksitasi." },
  { "en": "Apa Itu Field Discharge Resistor?", "id": "Pembuang Energi Medan Saat Trip." },
  { "en": "Apa Itu Generator Capability Curve?", "id": "Grafik Batas Operasi Aman Generator." },
  { "en": "Apa Itu Under Excited Limit?", "id": "Batas Minimal Eksitasi Mencegah Lepas." },
  { "en": "Apa Itu Over Excited Limit?", "id": "Batas Maksimal Eksitasi Mencegah Panas." },
  { "en": "Apa Itu Negative Sequence Heating?", "id": "Panas Rotor Akibat Beban Unbalance." },
  { "en": "Apa Itu Profibus DP Address?", "id": "Alamat Unik Perangkat 0 Sampai 126." },
  { "en": "Apa Itu GSD File Profibus?", "id": "File Identitas Perangkat Profibus." },
  { "en": "Apa Itu Termination Resistor Profibus?", "id": "Resistor Ujung Kabel Mencegah Pantulan." },
  { "en": "Apa Itu Bus Connector Profibus?", "id": "Konektor DB9 Dengan Sakelar Terminasi." },
  { "en": "Apa Warna Kabel Profibus Standar?", "id": "Warna Ungu." },
  { "en": "Apa Itu Profibus Segment Length?", "id": "Panjang Maksimal Kabel Per Segmen." },
  { "en": "Apa Itu Profibus Repeater?", "id": "Penguat Sinyal Perpanjangan Jarak." },
  { "en": "Apa Itu Profibus Baud Rate?", "id": "Kecepatan Data 9,6k Sampai 12M." },
  { "en": "Apa Itu Token Passing Profibus?", "id": "Giliran Hak Kirim Data Master." },
  { "en": "Apa Itu Wenner 4 Point Method?", "id": "Metode Ukur Tahanan Jenis Tanah." },
  { "en": "Apa Itu Schlumberger Method?", "id": "Metode Ukur Tanah Elektroda Beda." },
  { "en": "Apa Itu Fall Of Potential Method?", "id": "Metode 3 Titik Ukur Grounding." },
  { "en": "Apa Itu Clamp On Earth Tester?", "id": "Ukur Grounding Tanpa Lepas Kabel." },
  { "en": "Apa Itu Soil Resistivity Rho?", "id": "Tahanan Spesifik Tanah Ohm Meter." },
  { "en": "Apa Itu Earth Mat Grid?", "id": "Jaring Kawat Tembaga Bawah Tanah." },
  { "en": "Apa Itu Grounding Rod Copper Bond?", "id": "Besi Lapis Tembaga Hemat Biaya." },
  { "en": "Apa Itu Exothermic Welding?", "id": "Las Kimia Sambungan Kabel Ground." },
  { "en": "Apa Itu Bentonite Clay?", "id": "Tanah Liat Penurun Tahanan Ground." },
  { "en": "Apa Itu Thermal Paste Heatsink?", "id": "Senyawa Penghantar Panas Celah Mikro." },
  { "en": "Apa Itu Thermal Resistance Rth?", "id": "Hambatan Aliran Panas Derajat Watt." },
  { "en": "Apa Itu Junction Temperature Tj?", "id": "Suhu Inti Chip Semikonduktor." },
  { "en": "Apa Itu Case Temperature Tc?", "id": "Suhu Casing Luar Komponen." },
  { "en": "Apa Itu Ambient Temperature Ta?", "id": "Suhu Udara Lingkungan Sekitar." },
  { "en": "Apa Itu Heat Pipe Cooling?", "id": "Pipa Tembaga Berisi Cairan Uap." },
  { "en": "Apa Itu Thermoelectric Cooler (TEC)?", "id": "Pendingin Peltier Sisi Dingin Panas." },
  { "en": "Apa Itu Forced Air Cooling?", "id": "Pendinginan Menggunakan Kipas Angin." },
  { "en": "Apa Itu Liquid Cold Plate?", "id": "Blok Pendingin Dialiri Air." },
  { "en": "Apa Itu Thermal Pad Silicone?", "id": "Lembaran Karet Penghantar Panas." },
  { "en": "Apa Itu Mica Insulator?", "id": "Isolator Listrik Tahan Panas Transistor." },
  { "en": "Apa Itu Snubber Capacitor?", "id": "Kapasitor Peredam Paku Tegangan." },
  { "en": "Apa Itu Bleeder Resistor?", "id": "Resistor Penguras Muatan Kapasitor." },
  { "en": "Apa Itu Crowbar Circuit?", "id": "Proteksi Tegangan Lebih Hubung Singkat." },
  { "en": "Apa Itu Galvanic Isolation?", "id": "Pemisahan Listrik Input Dan Output." },
  { "en": "Apa Itu Creepage Distance PCB?", "id": "Jarak Rambat Arus Permukaan PCB." },
  { "en": "Apa Itu Clearance Distance PCB?", "id": "Jarak Bebas Udara Antar Jalur." },
  { "en": "Apa Itu Comparative Tracking Index (CTI)?", "id": "Ketahanan Bahan PCB Terhadap Arus." },
  { "en": "Apa Itu Solder Mask Opening?", "id": "Area Terbuka Tanpa Cat Hijau." },
  { "en": "Apa Itu Stencil Aperture?", "id": "Lubang Cetakan Pasta Solder." },
  { "en": "Apa Itu Reflow Profile Ramp?", "id": "Laju Kenaikan Suhu Oven Solder." },
  { "en": "Apa Itu Reflow Profile Soak?", "id": "Waktu Tahan Suhu Aktivasi Flux." },
  { "en": "Apa Itu Reflow Profile Peak?", "id": "Suhu Maksimal Cair Timah." },
  { "en": "Apa Itu Wave Solder Fluxing?", "id": "Penyemprotan Flux Sebelum Masuk Timah." },
  { "en": "Apa Itu Dross Timah Solder?", "id": "Kotoran Oksidasi Di Permukaan Timah." },
  { "en": "Apa Itu Desoldering Wick?", "id": "Pita Tembaga Penyerap Timah Cair." },
  { "en": "Apa Itu Solder Sucker Pump?", "id": "Pompa Sedot Timah Manual." },
  { "en": "Apa Itu Hot Air Rework Station?", "id": "Alat Uap Panas Lepas IC." },
  { "en": "Apa Itu BGA Reballing?", "id": "Memasang Ulang Bola Timah IC." },
  { "en": "Apa Itu Digital Multimeter Counts?", "id": "Resolusi Tampilan Angka Multimeter." },
  { "en": "Apa Itu True RMS Meter?", "id": "Akurat Ukur Gelombang Non Sinus." },
  { "en": "Apa Itu Crest Factor Meter?", "id": "Kemampuan Ukur Puncak Gelombang." },
  { "en": "Apa Itu Input Impedance Meter?", "id": "Hambatan Masukan Alat Ukur." },
  { "en": "Apa Itu Burden Voltage Meter?", "id": "Tegangan Jatuh Saat Ukur Arus." },
  { "en": "Apa Itu Category Rating CAT III?", "id": "Aman Untuk Instalasi Gedung." },
  { "en": "Apa Itu Category Rating CAT IV?", "id": "Aman Untuk Sumber Listrik Luar." },
  { "en": "Apa Itu Calibration Traceability?", "id": "Ketelusuran Ke Standar Nasional." },
  { "en": "Apa Itu Test Uncertainty Ratio?", "id": "Rasio Akurasi Alat Dan Standar." },
  { "en": "Apa Itu Dielectric Absorption Ratio?", "id": "Rasio Megger 60 Detik 30 Detik." },
  { "en": "Apa Itu Polarization Index PI?", "id": "Rasio Megger 10 Menit 1 Menit." },
  { "en": "Apa Itu Hipot Test DC?", "id": "Uji Tegangan Tinggi Arus Searah." },
  { "en": "Apa Itu Hipot Test AC?", "id": "Uji Tegangan Tinggi Arus Bolak Balik." },
  { "en": "Apa Itu Tan Delta Test Cable?", "id": "Uji Kualitas Isolasi Kabel Menengah." },
  { "en": "Apa Itu VLF Cable Test?", "id": "Uji Kabel Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Partial Discharge Scan?", "id": "Pindai Kebocoran Isolasi Dini." },
  { "en": "Apa Itu Ultrasonic Leak Detector?", "id": "Deteksi Suara Kebocoran Gas Arcing." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Analisis Harmonisa Dan Gangguan Listrik." },
  { "en": "Apa Itu Flicker Measurement Pst?", "id": "Ukur Kedipan Lampu Jangka Pendek." },
  { "en": "Apa Itu Transient Recorder?", "id": "Rekam Lonjakan Tegangan Cepat." },
  { "en": "Apa Itu Sequence Component Analyzer?", "id": "Analisis Komponen Positif Negatif Nol." },
  { "en": "Apa Itu Battery Impedance Tester?", "id": "Ukur Hambatan Dalam Baterai." },
  { "en": "Apa Itu Lux Meter Cosine?", "id": "Ukur Cahaya Sudut Miring Akurat." },
  { "en": "Apa Itu Tachometer Laser?", "id": "Ukur Putaran Tanpa Kontak." },
  { "en": "Apa Itu Vibration Pen?", "id": "Alat Ukur Getaran Sederhana." },
  { "en": "Apa Itu FFT Vibration Analyzer?", "id": "Analisis Spektrum Frekuensi Getaran." },
  { "en": "Apa Itu Oil Quality Analyzer?", "id": "Ukur Tegangan Tembus Minyak Trafo." },
  { "en": "Apa Itu Dew Point Meter?", "id": "Ukur Titik Embun Gas SF6." },
  { "en": "Apa Itu SF6 Purity Analyzer?", "id": "Ukur Kemurnian Gas SF6." },
  { "en": "Apa Itu Circuit Breaker Timer?", "id": "Ukur Waktu Buka Tutup Kontak." },
  { "en": "Apa Itu Contact Resistance Microhmmeter?", "id": "Ukur Hambatan Kontak Sangat Kecil." },
  { "en": "Apa Itu Relay Test Set?", "id": "Alat Injeksi Arus Uji Relay." },
  { "en": "Apa Itu Secondary Injection Test?", "id": "Uji Relay Tanpa Arus Primer." },
  { "en": "Apa Itu Primary Injection Test?", "id": "Uji Sistem Proteksi Arus Penuh." },
  { "en": "Apa Itu Graetz Bridge?", "id": "Jembatan Penyearah 6 Pulsa." },
  { "en": "Apa Itu 12 Pulse Rectifier?", "id": "Dua Jembatan 6 Pulsa Seri." },
  { "en": "Keunggulan 12 Pulse Rectifier Adalah?", "id": "Harmonisa Lebih Rendah Dari 6 Pulsa." },
  { "en": "Apa Itu Commutation Notch?", "id": "Cacat Tegangan Saat Pindah Fasa." },
  { "en": "Apa Itu Smoothing Reactor HVDC?", "id": "Induktor Besar Perata Arus DC." },
  { "en": "Apa Itu Valve Hall?", "id": "Ruangan Berisi Tumpukan Thyristor HVDC." },
  { "en": "Apa Itu Back To Back HVDC?", "id": "Hubungan Dua Grid Beda Frekuensi." },
  { "en": "Apa Itu Monopolar Link?", "id": "Satu Konduktor Dengan Return Tanah." },
  { "en": "Apa Itu Bipolar Link?", "id": "Dua Konduktor Positif Dan Negatif." },
  { "en": "Apa Itu Ground Return Electrode?", "id": "Elektroda Tanah Arus Balik HVDC." },
  { "en": "Apa Itu Furan Analysis Trafo?", "id": "Uji Penuaan Kertas Isolasi." },
  { "en": "Senyawa 2-Furaldehyde Menandakan Apa?", "id": "Degradasi Kertas Isolasi Selulosa." },
  { "en": "Apa Itu Interfacial Tension Minyak?", "id": "Tegangan Permukaan Minyak Dan Air." },
  { "en": "Apa Itu Neutralization Number?", "id": "Ukuran Keasaman Minyak Trafo." },
  { "en": "Apa Itu Corrosive Sulfur?", "id": "Belerang Penyebab Karat Tembaga." },
  { "en": "Apa Itu Passivator Minyak?", "id": "Zat Pencegah Korosi Tembaga." },
  { "en": "Apa Itu Sludge Minyak?", "id": "Endapan Lumpur Oksidasi Minyak." },
  { "en": "Apa Itu Inhibited Oil?", "id": "Minyak Dengan Anti Oksidan." },
  { "en": "Apa Itu Uninhibited Oil?", "id": "Minyak Murni Tanpa Aditif." },
  { "en": "Apa Itu Stray Gassing?", "id": "Gas Terbentuk Suhu Rendah." },
  { "en": "Apa Itu Overshoot Pada Kontrol?", "id": "Lonjakan Melebihi Setpoint." },
  { "en": "Apa Itu Undershoot?", "id": "Lembah Di Bawah Setpoint." },
  { "en": "Apa Itu Rise Time?", "id": "Waktu Sinyal Naik 10-90 Persen." },
  { "en": "Apa Itu Fall Time?", "id": "Waktu Sinyal Turun 90-10 Persen." },
  { "en": "Apa Itu Settling Time?", "id": "Waktu Stabil Dalam Batas Error." },
  { "en": "Apa Itu Steady State Error?", "id": "Selisih Akhir Target Dan Aktual." },
  { "en": "Apa Itu Gain Margin?", "id": "Batas Penguatan Sebelum Tidak Stabil." },
  { "en": "Apa Itu Phase Margin?", "id": "Batas Fasa Sebelum Osilasi." },
  { "en": "Apa Itu Dead Time Proses?", "id": "Waktu Tunda Respon Sistem." },
  { "en": "Apa Itu Time Constant Tau?", "id": "Waktu Respon Mencapai 63 Persen." },
  { "en": "Apa Itu Efek Hall?", "id": "Beda Potensial Akibat Medan Magnet." },
  { "en": "Apa Itu Lorentz Force?", "id": "Gaya Pada Kawat Berarus." },
  { "en": "Apa Itu Eddy Current?", "id": "Arus Putar Dalam Inti Besi." },
  { "en": "Apa Itu Hysteresis Loss?", "id": "Panas Akibat Gesekan Molekul Magnet." },
  { "en": "Apa Itu Skin Depth?", "id": "Kedalaman Arus Pada Konduktor." },
  { "en": "Apa Itu Proximity Effect?", "id": "Arus Terdesak Kabel Tetangga." },
  { "en": "Apa Itu Permeabilitas?", "id": "Kemampuan Bahan Mengalirkan Fluks." },
  { "en": "Apa Itu Permitivitas?", "id": "Kemampuan Bahan Menyimpan Medan Listrik." },
  { "en": "Apa Itu Dielectric Loss?", "id": "Panas Pada Isolator AC." },
  { "en": "Apa Itu Partial Discharge?", "id": "Loncatan Listrik Kecil Dalam Isolasi." },
  { "en": "Apa Itu Fiber Bragg Grating?", "id": "Sensor Suhu Berbasis Serat Optik." },
  { "en": "Apa Itu OTDR Trace?", "id": "Grafik Pantulan Cahaya Kabel Optik." },
  { "en": "Apa Itu Macro Bend Loss?", "id": "Rugi Cahaya Akibat Tekukan Kabel." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Penyambungan Kabel Optik Dengan Las." },
  { "en": "Apa Itu Mechanical Splicing?", "id": "Sambungan Optik Tanpa Pemanasan." },
  { "en": "Apa Itu Optical Splitter?", "id": "Pembagi Sinyal Cahaya Pasif." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Banyak Warna Cahaya Satu Kabel." },
  { "en": "Apa Itu Dark Fiber?", "id": "Kabel Optik Terpasang Belum Aktif." },
  { "en": "Apa Itu Numerical Aperture?", "id": "Sudut Penerimaan Cahaya Fiber." },
  { "en": "Apa Itu Cladding Mode?", "id": "Cahaya Bocor Ke Lapisan Luar." },
  { "en": "Apa Itu Brushless DC Motor?", "id": "Motor DC Tanpa Sikat Arang." },
  { "en": "Apa Itu Back EMF?", "id": "Tegangan Lawan Putaran Motor." },
  { "en": "Apa Itu Field Weakening?", "id": "Mengurangi Fluks Agar Putaran Naik." },
  { "en": "Apa Itu Slip Ring Motor?", "id": "Motor Rotor Belitan Cincin Gesek." },
  { "en": "Apa Itu Commutator Segment?", "id": "Lempeng Tembaga Pada Rotor DC." },
  { "en": "Apa Itu Carbon Brush Grade?", "id": "Kekerasan Dan Komposisi Sikat Arang." },
  { "en": "Apa Itu Interpole?", "id": "Kutub Bantu Mencegah Percikan Api." },
  { "en": "Apa Itu Armature Reaction?", "id": "Distorsi Medan Magnet Utama." },
  { "en": "Apa Itu Locked Rotor Current?", "id": "Arus Saat Motor Ditahan Diam." },
  { "en": "Apa Itu Relay Differential Slope?", "id": "Garis Bias Mencegah Trip Salah." },
  { "en": "Apa Itu Restraint Current?", "id": "Arus Penahan Trip Diferensial." },
  { "en": "Apa Itu Operate Current?", "id": "Arus Penyebab Trip Diferensial." },
  { "en": "Apa Itu CT Saturation?", "id": "Inti Trafo Arus Jenuh." },
  { "en": "Apa Itu Knee Point Voltage?", "id": "Titik Jenuh Kurva CT." },
  { "en": "Apa Itu Remanence Flux?", "id": "Sisa Magnet Dalam Inti CT." },
  { "en": "Apa Itu Instrument Security Factor?", "id": "Batas Arus Aman Meter." },
  { "en": "Apa Itu Accuracy Limit Factor?", "id": "Batas Akurasi CT Proteksi." },
  { "en": "Apa Itu Class X CT?", "id": "CT Khusus Proteksi Diferensial." },
  { "en": "Apa Itu Luminous Flux?", "id": "Total Energi Cahaya Lampu." },
  { "en": "Apa Itu Luminous Intensity?", "id": "Kuat Cahaya Ke Arah Tertentu." },
  { "en": "Apa Itu Illuminance?", "id": "Kuat Cahaya Pada Bidang Kerja." },
  { "en": "Apa Itu Luminance?", "id": "Kecerahan Cahaya Dilihat Mata." },
  { "en": "Apa Itu Efficacy Lampu?", "id": "Lumen Per Watt." },
  { "en": "Apa Itu Color Rendering Index?", "id": "Kualitas Warna Cahaya Lampu." },
  { "en": "Apa Itu Correlated Color Temperature?", "id": "Warna Cahaya Kelvin." },
  { "en": "Apa Itu Beam Angle?", "id": "Sudut Sebaran Cahaya." },
  { "en": "Apa Itu Glare Rating?", "id": "Indeks Kesilauan Ruangan." },
  { "en": "Apa Itu Maintenance Factor?", "id": "Faktor Penurunan Cahaya Seiring Waktu." },
  { "en": "Apa Itu Peukert's Law?", "id": "Kapasitas Aki Turun Jika Cepat." },
  { "en": "Apa Itu Sulfation?", "id": "Kristal Sulfat Menutup Pelat Aki." },
  { "en": "Apa Itu Equalizing Charge?", "id": "Cas Tegangan Tinggi Meratakan Sel." },
  { "en": "Apa Itu Float Charge?", "id": "Cas Tegangan Konstan Jaga Penuh." },
  { "en": "Apa Itu Battery Cycling?", "id": "Proses Cas Dan Pakai." },
  { "en": "Apa Itu Depth Of Discharge?", "id": "Persentase Kapasitas Terpakai." },
  { "en": "Apa Itu State Of Health?", "id": "Sisa Umur Baterai Dibanding Baru." },
  { "en": "Apa Itu Internal Resistance?", "id": "Hambatan Dalam Baterai." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Panas Baterai Tak Terkendali." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Bandwidth Pada Penguatan Satu." },
  { "en": "Apa Itu CMRR?", "id": "Penolakan Sinyal Mode Bersama." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Error Input Op-Amp." },
  { "en": "Apa Itu Virtual Ground?", "id": "Titik Referensi Semu Op-Amp." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat Penyangga Gain Satu." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Beda Presisi Tinggi." },
  { "en": "Apa Itu Active Filter?", "id": "Filter Menggunakan Op-Amp." },
  { "en": "Apa Itu Critical Clearing Time?", "id": "Waktu Maksimal Pemutusan Gangguan." },
  { "en": "Apa Itu Transient Stability Limit?", "id": "Batas Daya Sebelum Hilang Sinkron." },
  { "en": "Apa Itu Voltage Stability Margin?", "id": "Jarak Operasi Ke Titik Runtuh." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Peredam Osilasi Elektromekanik Generator." },
  { "en": "Apa Itu Inter-Area Oscillation?", "id": "Ayunan Daya Antar Wilayah Grid." },
  { "en": "Apa Itu Small Signal Stability?", "id": "Kestabilan Terhadap Gangguan Kecil." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis Kestabilan Transien." },
  { "en": "Apa Itu Swing Equation?", "id": "Persamaan Gerak Rotor Generator." },
  { "en": "Apa Itu Inertia Constant H?", "id": "Energi Kinetik Rotor Per MVA." },
  { "en": "Apa Itu Damping Torque?", "id": "Torsi Peredam Osilasi Rotor." },
  { "en": "Apa Itu Synchronizing Torque?", "id": "Torsi Penjaga Posisi Rotor." },
  { "en": "Apa Itu Load Frequency Control?", "id": "Menjaga Frekuensi Dengan Atur Daya." },
  { "en": "Apa Itu Automatic Generation Control?", "id": "Pengaturan Daya Pembangkit Terpusat." },
  { "en": "Apa Itu Spinning Reserve?", "id": "Cadangan Daya Generator Berputar." },
  { "en": "Apa Itu Cold Reserve?", "id": "Cadangan Daya Generator Mati." },
  { "en": "Apa Itu Black Start Unit?", "id": "Generator Start Tanpa Listrik Luar." },
  { "en": "Apa Itu Islanding Operation?", "id": "Operasi Terpisah Dari Grid Utama." },
  { "en": "Apa Itu Load Shedding?", "id": "Pelepasan Beban Saat Frekuensi Turun." },
  { "en": "Apa Itu Under Frequency Relay?", "id": "Relay Pemicu Load Shedding." },
  { "en": "Apa Itu Rate Of Change Frequency?", "id": "Deteksi Cepat Perubahan Frekuensi." },
  { "en": "Apa Itu Reverse Power Relay?", "id": "Mencegah Generator Menjadi Motor." },
  { "en": "Kode ANSI Reverse Power Adalah?", "id": "32." },
  { "en": "Apa Itu Loss Of Field Relay?", "id": "Proteksi Hilang Arus Eksitasi." },
  { "en": "Kode ANSI Loss Of Field Adalah?", "id": "40." },
  { "en": "Apa Itu Negative Sequence Relay?", "id": "Proteksi Beban Tidak Seimbang." },
  { "en": "Kode ANSI Negative Sequence Adalah?", "id": "46." },
  { "en": "Apa Itu Inadvertent Energization?", "id": "Generator Mati Tiba Tiba Bertegangan." },
  { "en": "Kode ANSI Inadvertent Energization?", "id": "50/27." },
  { "en": "Apa Itu Stator Earth Fault?", "id": "Gangguan Tanah Lilitan Stator." },
  { "en": "Kode ANSI Stator Earth Fault?", "id": "64G." },
  { "en": "Apa Itu Rotor Earth Fault?", "id": "Gangguan Tanah Lilitan Rotor." },
  { "en": "Kode ANSI Rotor Earth Fault?", "id": "64R." },
  { "en": "Apa Itu Volts Per Hertz Relay?", "id": "Proteksi Overfluxing Trafo Generator." },
  { "en": "Kode ANSI Volts Per Hertz?", "id": "24." },
  { "en": "Apa Itu Generator Differential Relay?", "id": "Proteksi Hubung Singkat Internal." },
  { "en": "Kode ANSI Generator Differential?", "id": "87G." },
  { "en": "Apa Itu Overall Differential Relay?", "id": "Proteksi Generator Dan Trafo Utama." },
  { "en": "Apa Itu Wi-SUN Protocol?", "id": "Jaringan Nirkabel Meter Cerdas." },
  { "en": "Apa Itu Zigbee Smart Energy?", "id": "Profil Zigbee Untuk Manajemen Energi." },
  { "en": "Apa Itu G3-PLC Technology?", "id": "Komunikasi Kabel Listrik Tahan Noise." },
  { "en": "Apa Itu PRIME PLC Standard?", "id": "Standar PLC Meteran Listrik Eropa." },
  { "en": "Apa Itu Data Concentrator Unit?", "id": "Pengumpul Data Dari Banyak Meter." },
  { "en": "Apa Itu Head End System?", "id": "Pusat Manajemen Data Meter." },
  { "en": "Apa Itu Meter Data Management?", "id": "Sistem Validasi Tagihan Listrik." },
  { "en": "Apa Itu DLMS/COSEM Protocol?", "id": "Bahasa Standar Meter Elektronik." },
  { "en": "Apa Itu OBIS Code?", "id": "Kode Identifikasi Data Meter." },
  { "en": "Apa Itu Optical Port Probe?", "id": "Alat Baca Meter Inframerah." },
  { "en": "Apa Itu Tamper Detection?", "id": "Deteksi Usaha Mencuri Listrik." },
  { "en": "Apa Itu Summing Amplifier?", "id": "Op-Amp Penjumlah Sinyal Input." },
  { "en": "Apa Itu Differential Amplifier?", "id": "Op-Amp Penguat Selisih Input." },
  { "en": "Apa Itu Integrator Amplifier?", "id": "Output Integral Dari Input." },
  { "en": "Apa Itu Differentiator Amplifier?", "id": "Output Turunan Dari Input." },
  { "en": "Apa Itu Exponential Amplifier?", "id": "Kebalikan Dari Logarithmic Amplifier." },
  { "en": "Apa Itu Precision Rectifier?", "id": "Penyearah Sinyal Kecil Akurat." },
  { "en": "Apa Itu Peak Detector?", "id": "Menahan Nilai Puncak Sinyal." },
  { "en": "Apa Itu Sample And Hold?", "id": "Mengambil Dan Menahan Tegangan." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Terpisah Secara Galvanis." },
  { "en": "Apa Itu Voltage Follower?", "id": "Buffer Penguat Arus Gain 1." },
  { "en": "Apa Itu Single Ended Input?", "id": "Satu Sinyal Referensi Ground." },
  { "en": "Apa Itu Differential Input?", "id": "Dua Sinyal Saling Berlawanan." },
  { "en": "Apa Itu Common Mode Signal?", "id": "Sinyal Sama Di Kedua Input." },
  { "en": "Apa Itu CMRR Op-Amp?", "id": "Kemampuan Menolak Common Mode." },
  { "en": "Apa Itu Slew Rate Limiting?", "id": "Batas Kecepatan Perubahan Output." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Error Saat Input Nol." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Masuk Basis Op-Amp." },
  { "en": "Apa Itu Virtual Ground?", "id": "Titik Semu Tegangan Nol." },
  { "en": "Apa Itu Passive Filter?", "id": "Filter Hanya R L C." },
  { "en": "Apa Itu Low Pass Filter?", "id": "Loloskan Frekuensi Rendah." },
  { "en": "Apa Itu High Pass Filter?", "id": "Loloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Band Pass Filter?", "id": "Loloskan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Band Stop Filter?", "id": "Tolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Notch Filter?", "id": "Band Stop Sangat Sempit." },
  { "en": "Apa Itu Butterworth Filter?", "id": "Respon Frekuensi Datar Maksimal." },
  { "en": "Apa Itu Chebyshev Filter?", "id": "Cut-off Curam Ada Ripple." },
  { "en": "Apa Itu Bessel Filter?", "id": "Respon Fasa Paling Linier." },
  { "en": "Apa Itu Elliptic Filter?", "id": "Cut-off Paling Curam." },
  { "en": "Apa Itu Order Filter?", "id": "Tingkat Kecuraman Penurunan Gain." },
  { "en": "Filter Orde 1 Turun Sebesar?", "id": "20 dB Per Dekade." },
  { "en": "Filter Orde 2 Turun Sebesar?", "id": "40 dB Per Dekade." },
  { "en": "Apa Itu Cut-Off Frequency?", "id": "Titik Daya Turun Setengah." },
  { "en": "Apa Itu 3dB Point?", "id": "Istilah Lain Cut-Off Frequency." },
  { "en": "Apa Itu Quality Factor Q?", "id": "Ukuran Selektivitas Filter." },
  { "en": "Apa Itu Damping Factor?", "id": "Kebalikan Dari Quality Factor." },
  { "en": "Apa Itu Oscillation Condition?", "id": "Kriteria Barkhausen Terpenuhi." },
  { "en": "Apa Itu Phase Shift Oscillator?", "id": "Osilator Geser Fasa RC." },
  { "en": "Apa Itu Wien Bridge Oscillator?", "id": "Osilator Sinus Audio Presisi." },
  { "en": "Apa Itu Colpitts Oscillator?", "id": "Osilator Pembagi Tegangan Kapasitif." },
  { "en": "Apa Itu IC Code Motor?", "id": "International Cooling Code Standard." },
  { "en": "Apa Itu Motor IC 411?", "id": "Pendingin Kipas Sendiri Tertutup." },
  { "en": "Apa Itu Motor IC 416?", "id": "Pendingin Kipas Listrik Terpisah." },
  { "en": "Apa Itu Motor IC 01?", "id": "Terbuka Kipas Internal ODP." },
  { "en": "Apa Itu NEMA Design B?", "id": "Torsi Normal Arus Start Normal." },
  { "en": "Apa Itu NEMA Design C?", "id": "Torsi Start Tinggi Arus Normal." },
  { "en": "Apa Itu NEMA Design D?", "id": "Torsi Start Sangat Tinggi Slip Tinggi." },
  { "en": "Apa Itu Service Factor Motor?", "id": "Kemampuan Beban Lebih Sesaat." },
  { "en": "Service Factor 1.15 Artinya?", "id": "Bisa Lebih Beban 15 Persen." },
  { "en": "Apa Itu Locked Rotor Code?", "id": "Kode KVA Per HP Saat Start." },
  { "en": "Apa Itu Motor Slide Base?", "id": "Dudukan Geser Pengencang Belt." },
  { "en": "Apa Itu Gearbox Service Factor?", "id": "Faktor Keamanan Beban Kejut." },
  { "en": "Apa Itu Backlash Gearbox?", "id": "Jarak Main Antar Gigi." },
  { "en": "Apa Itu Planetary Gearbox?", "id": "Gearbox Ringkas Torsi Besar." },
  { "en": "Apa Itu Worm Gearbox?", "id": "Gearbox Cacing Output Siku." },
  { "en": "Apa Itu Shaft Grounding Ring?", "id": "Sikat Arang Buang Arus Poros." },
  { "en": "Apa Itu Bearing Fluting?", "id": "Kerusakan Bearing Akibat Arus EDM." },
  { "en": "Apa Itu Common Mode Voltage?", "id": "Tegangan Poros Akibat PWM VFD." },
  { "en": "Apa Itu Encoder Index Channel?", "id": "Sinyal Referensi Satu Putaran." },
  { "en": "Apa Itu Encoder Differential Signal?", "id": "Sinyal A A-Not B B-Not." },
  { "en": "Apa Itu Resolver Excitation?", "id": "Sinyal AC Pemicu Rotor Resolver." },
  { "en": "Apa Itu Hall Sensor Commutation?", "id": "Deteksi Posisi Kutub BLDC." },
  { "en": "Apa Itu Core Balance CT?", "id": "Trafo Arus Lingkar Kabel Tiga Fasa." },
  { "en": "Fungsi Core Balance CT Adalah?", "id": "Deteksi Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Residual Connection CT?", "id": "Jumlah Vektor Tiga CT Fasa." },
  { "en": "Apa Itu Open Delta PT?", "id": "Deteksi Tegangan Urutan Nol." },
  { "en": "Apa Itu Ferroresonance Damping Resistor?", "id": "Resistor Beban Sekunder Open Delta." },
  { "en": "Apa Itu Primary Injection Test?", "id": "Uji Sistem CT Dan Relay." },
  { "en": "Apa Itu Contact Resistance Test?", "id": "Ukur Hambatan Kontak Breaker." },
  { "en": "Satuan Contact Resistance Adalah?", "id": "Mikro Ohm." },
  { "en": "Apa Itu Hot Collar Test?", "id": "Uji Kebocoran Bushing Trafo." },
  { "en": "Apa Itu C1 Capacitance Bushing?", "id": "Kapasitansi Utama Isolasi Bushing." },
  { "en": "Apa Itu C2 Capacitance Bushing?", "id": "Kapasitansi Tap Ke Flange." },
  { "en": "Apa Itu Breakdown Voltage Oil?", "id": "Tegangan Tembus Minyak Trafo." },
  { "en": "Apa Itu Water Content Oil?", "id": "Kadar Air Satuan PPM." },
  { "en": "Apa Itu Acidity Number Oil?", "id": "Tingkat Keasaman Oksidasi Minyak." },
  { "en": "Apa Itu Interfacial Tension?", "id": "Tegangan Antarmuka Minyak Air." },
  { "en": "Apa Itu Sweep Frequency Response?", "id": "Deteksi Pergeseran Lilitan Trafo." },
  { "en": "Apa Itu Excitation Current Test?", "id": "Uji Arus Magnetisasi Inti Trafo." },
  { "en": "Apa Itu Magnetic Balance Test?", "id": "Cek Keseimbangan Fluks Fasa." },
  { "en": "Apa Itu Battery Load Tester?", "id": "Uji Kapasitas Aki Dengan Beban." },
  { "en": "Apa Itu Battery Impedance Tester?", "id": "Uji Kesehatan Aki Tanpa Buang." },
  { "en": "Apa Itu Specific Gravity Electrolyte?", "id": "Berat Jenis Air Aki Basah." },
  { "en": "Apa Itu Intercell Connector Resistance?", "id": "Hambatan Sambungan Antar Sel." },
  { "en": "Apa Itu Hydrogen Gas Monitor?", "id": "Deteksi Gas Bahaya Ruang Baterai." },
  { "en": "Apa Itu Spill Containment Battery?", "id": "Bak Penampung Tumpahan Asam." },
  { "en": "Apa Itu Eye Wash Station?", "id": "Pencuci Mata Darurat Kimia." },
  { "en": "Apa Itu Voltage Detector Probe?", "id": "Alat Cek Tegangan Sebelum Kerja." },
  { "en": "Apa Itu Proving Unit?", "id": "Alat Tes Fungsi Voltage Detector." },
  { "en": "Apa Itu Grounding Cluster?", "id": "Kabel Grounding Sementara Portable." },
  { "en": "Apa Itu Rescue Hook Stick?", "id": "Tongkat Tarik Korban Setrum." },
  { "en": "Apa Itu Confined Space Entry?", "id": "Izin Masuk Tangki Trafo." },
  { "en": "Apa Itu Fire Extinguisher CO2?", "id": "Pemadam Api Listrik Gas Dingin." },
  { "en": "Apa Itu Fire Extinguisher Powder?", "id": "Pemadam Serbuk Kimia Kering." },
  { "en": "Kelemahan Powder Extinguisher Adalah?", "id": "Kotor Dan Korosif." },
  { "en": "Apa Itu FM200 Gas System?", "id": "Pemadam Gas Ruang Server." },
  { "en": "Apa Itu Inert Gas Suppression?", "id": "Pemadam Gas Argon Nitrogen." },
  { "en": "Apa Itu Aspirating Smoke Detector?", "id": "Detektor Asap Hisap Pipa." },
  { "en": "Apa Itu Linear Heat Detector?", "id": "Kabel Deteksi Panas Sepanjang Jalur." },
  { "en": "Apa Itu Cable Tray Fire Stop?", "id": "Penutup Lubang Tembus Dinding." },
  { "en": "Apa Itu Intumescent Paint?", "id": "Cat Mengembang Saat Terbakar." },
  { "en": "Apa Itu Emergency Lighting Unit?", "id": "Lampu Darurat Baterai." },
  { "en": "Apa Itu Exit Sign Lamp?", "id": "Lampu Penunjuk Jalur Keluar." },
  { "en": "Apa Itu Panic Bar Door?", "id": "Pintu Dorong Keluar Cepat." },
  { "en": "Apa Itu Muster Point?", "id": "Titik Kumpul Evakuasi." },
  { "en": "Apa Itu Safety Induction?", "id": "Pengarahan Keselamatan Tamu." },
  { "en": "Apa Itu Permit To Work?", "id": "Izin Kerja Resmi Tertulis." },
  { "en": "Apa Itu Job Safety Analysis?", "id": "Analisis Bahaya Sebelum Kerja." },
  { "en": "Apa Itu Toolbox Meeting?", "id": "Briefing Singkat Sebelum Mulai." },
  { "en": "Apa Itu Lockout Box?", "id": "Kotak Kunci Gembok Bersama." },
  { "en": "Apa Itu Tagout Label?", "id": "Label Peringatan Jangan Dioperasikan." },
  { "en": "Apa Itu Arc Rated Clothing?", "id": "Pakaian Tahan Panas Busur Api." },
  { "en": "Apa Itu Face Shield Electrical?", "id": "Pelindung Wajah Anti UV Panas." },
  { "en": "Apa Itu Insulated Matting?", "id": "Karpet Karet Isolasi Lantai." },
  { "en": "Apa Itu Insulated Gloves Testing?", "id": "Uji Tiup Dan Tegangan Sarung Tangan." },
  { "en": "Apa Itu Leather Protector Gloves?", "id": "Sarung Kulit Pelindung Karet." },
  { "en": "Apa Itu Balaclava FR?", "id": "Tutup Kepala Tahan Api." },
  { "en": "Apa Itu Incident Energy Analysis?", "id": "Hitung Potensi Energi Ledakan." },
  { "en": "Satuan Incident Energy Adalah?", "id": "Kalori Per Sentimeter Persegi." },
  { "en": "Apa Itu Flash Protection Boundary?", "id": "Jarak Batas Luka Bakar." },
  { "en": "Apa Itu Limited Approach Boundary?", "id": "Jarak Batas Orang Awam." },
  { "en": "Apa Itu Restricted Approach Boundary?", "id": "Jarak Batas Teknisi Terlatih." },
  { "en": "Apa Itu Single Line Diagram Update?", "id": "Pembaruan Gambar Skema Listrik." },
  { "en": "Apa Itu Electrical Hazard Sign?", "id": "Rambu Segitiga Petir Kuning." },
  { "en": "Apa Itu High Voltage Sign?", "id": "Rambu Awas Tegangan Tinggi." },
  { "en": "Apa Itu Multiple Earth Neutral (MEN)?", "id": "Sistem Grounding Netral Gabung." },
  { "en": "Apa Itu Equipotential Bonding?", "id": "Menghubungkan Semua Logam Ke Ground." },
  { "en": "Tujuan Equipotential Bonding Adalah?", "id": "Mencegah Beda Potensial Sentuh." },
  { "en": "Apa Itu Clean Earth Ground?", "id": "Ground Khusus Alat Elektronik." },
  { "en": "Apa Itu Dirty Earth Ground?", "id": "Ground Untuk Daya Listrik." },
  { "en": "Apa Itu Signal Ground Reference?", "id": "Titik Nol Sinyal Data." },
  { "en": "Apa Itu Chassis Ground?", "id": "Koneksi Ke Rangka Logam." },
  { "en": "Apa Itu Star Grounding Topology?", "id": "Satu Titik Pusat Grounding." },
  { "en": "Apa Itu Daisy Chain Grounding?", "id": "Grounding Seri (Tidak Disarankan)." },
  { "en": "Apa Itu Ground Loop Noise?", "id": "Gangguan Arus Putar Ground." },
  { "en": "Cara Mengatasi Ground Loop?", "id": "Gunakan Isolator Galvanis." },
  { "en": "Apa Itu Ferrite Clamp On?", "id": "Peredam Noise Kabel Terpasang." },
  { "en": "Apa Itu MEMS (Micro Electro Mechanical System)?", "id": "Sistem Mekanik Ukuran Skala Mikro." },
  { "en": "Apa Itu MEMS Accelerometer?", "id": "Sensor Percepatan Berbasis Kapasitansi Mikro." },
  { "en": "Apa Itu MEMS Gyroscope?", "id": "Sensor Sudut Putar Efek Coriolis." },
  { "en": "Apa Itu Coriolis Force?", "id": "Gaya Semu Akibat Rotasi Benda." },
  { "en": "Apa Itu MEMS Microphone?", "id": "Mikrofon Chip Silikon Sangat Kecil." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Tekanan Mekanik Menjadi Tegangan Listrik." },
  { "en": "Apa Itu Inverse Piezoelectric Effect?", "id": "Tegangan Listrik Menjadi Gerakan Mekanik." },
  { "en": "Apa Itu PZT Material?", "id": "Lead Zirconate Titanate Keramik Piezo." },
  { "en": "Apa Itu Strain Gauge Factor?", "id": "Sensitivitas Perubahan Resistansi Terhadap Regangan." },
  { "en": "Apa Itu Wheatstone Bridge Quarter?", "id": "Menggunakan 1 Strain Gauge Aktif." },
  { "en": "Apa Itu Wheatstone Bridge Half?", "id": "Menggunakan 2 Strain Gauge Aktif." },
  { "en": "Apa Itu Wheatstone Bridge Full?", "id": "Menggunakan 4 Strain Gauge Aktif." },
  { "en": "Apa Itu Load Cell Creep?", "id": "Perubahan Sinyal Saat Beban Tetap." },
  { "en": "Apa Itu Load Cell Hysteresis?", "id": "Beda Output Saat Beban Naik Turun." },
  { "en": "Apa Itu Magnetic Hysteresis Loop?", "id": "Kurva B-H Magnetisasi Dan Demagnetisasi." },
  { "en": "Apa Itu Remanence (Br)?", "id": "Magnet Tersisa Saat Medan Nol." },
  { "en": "Apa Itu Coercivity (Hc)?", "id": "Medan Untuk Menghilangkan Magnet Sisa." },
  { "en": "Apa Itu Hard Magnetic Material?", "id": "Koersivitas Tinggi Susah Hilang Magnet." },
  { "en": "Apa Itu Soft Magnetic Material?", "id": "Koersivitas Rendah Mudah Hilang Magnet." },
  { "en": "Inti Trafo Menggunakan Material Apa?", "id": "Soft Magnetic Material." },
  { "en": "Magnet Permanen Menggunakan Material Apa?", "id": "Hard Magnetic Material." },
  { "en": "Apa Itu Curie Temperature?", "id": "Suhu Material Kehilangan Sifat Magnet." },
  { "en": "Apa Itu Saturation Flux Density?", "id": "Batas Maksimal Kepadatan Garis Gaya." },
  { "en": "Apa Itu Bang-Bang Control?", "id": "Kontrol On Off Histeresis Sederhana." },
  { "en": "Apa Itu Sliding Mode Control?", "id": "Kontrol Non Linier Switching Cepat." },
  { "en": "Apa Itu Chatter Sliding Mode?", "id": "Getaran Sinyal Kontrol Frekuensi Tinggi." },
  { "en": "Apa Itu Deadbeat Control?", "id": "Mencapai Target Dalam Langkah Terbatas." },
  { "en": "Apa Itu Robust Control H-Infinity?", "id": "Tahan Terhadap Gangguan Dan Ketidakpastian." },
  { "en": "Apa Itu Fuzzy Membership Function?", "id": "Kurva Derajat Keanggotaan Logika Samar." },
  { "en": "Apa Itu Defuzzification Center Of Gravity?", "id": "Metode Ubah Fuzzy Ke Tegas." },
  { "en": "Apa Itu Neural Network Hidden Layer?", "id": "Lapisan Pemroses Antara Input Output." },
  { "en": "Apa Itu Backpropagation Training?", "id": "Algoritma Pembelajaran Koreksi Error Mundur." },
  { "en": "Apa Itu Genetic Algorithm?", "id": "Optimasi Meniru Evolusi Seleksi Alam." },
  { "en": "Apa Itu Particle Swarm Optimization?", "id": "Optimasi Meniru Pergerakan Kelompok Burung." },
  { "en": "Apa Itu Conducted Emission EMC?", "id": "Gangguan Elektromagnetik Lewat Kabel." },
  { "en": "Apa Itu Radiated Emission EMC?", "id": "Gangguan Elektromagnetik Lewat Udara." },
  { "en": "Apa Itu LISN (Line Impedance Network)?", "id": "Alat Uji Emisi Konduksi Standar." },
  { "en": "Apa Itu Anechoic Chamber?", "id": "Ruang Kedap Pantulan Gelombang Radio." },
  { "en": "Apa Itu EMI Filter Insertion Loss?", "id": "Besar Redaman Filter Dalam dB." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Ganda Redam Noise Bersama." },
  { "en": "Apa Itu Differential Mode Choke?", "id": "Induktor Tunggal Redam Noise Fasa." },
  { "en": "Apa Itu X-Capacitor Class X2?", "id": "Kapasitor Antar Line Tahan Impuls." },
  { "en": "Apa Itu Y-Capacitor Class Y2?", "id": "Kapasitor Line Ke Ground Aman." },
  { "en": "Apa Itu Shielding Effectiveness?", "id": "Kemampuan Perisai Menahan Gelombang." },
  { "en": "Apa Itu Skin Depth Shielding?", "id": "Tebal Bahan Untuk Redam Sinyal." },
  { "en": "Apa Itu Ground Loop Noise?", "id": "Arus Putar Akibat Beda Potensial." },
  { "en": "Cara Mengatasi Ground Loop?", "id": "Gunakan Isolator Galvanis Atau Optocoupler." },
  { "en": "Apa Itu Ferrite Bead Impedance?", "id": "Resistansi Tinggi Pada Frekuensi Tinggi." },
  { "en": "Apa Itu AS-Interface (AS-i)?", "id": "Bus Sensor Aktuator Kabel Pipih." },
  { "en": "Bentuk Kabel AS-i Adalah?", "id": "Kuning Pipih Profil Trapesium." },
  { "en": "Apa Itu Manchester Encoding?", "id": "Kode Digital Tanpa Level DC." },
  { "en": "Apa Itu IO-Link Master?", "id": "Gerbang Komunikasi Sensor Cerdas." },
  { "en": "Apa Itu IODD File?", "id": "Deskripsi Perangkat IO-Link Digital." },
  { "en": "Apa Itu CAN Bus Arbitration?", "id": "Penentuan Prioritas Pesan Tanpa Tabrakan." },
  { "en": "Apa Itu CAN ID Priority?", "id": "ID Lebih Kecil Prioritas Tinggi." },
  { "en": "Resistor Terminasi CAN Bus?", "id": "120 Ohm Di Kedua Ujung." },
  { "en": "Apa Itu Profibus GSD File?", "id": "File Konfigurasi Perangkat Profibus." },
  { "en": "Apa Itu Modbus Function Code 04?", "id": "Membaca Input Register Analog." },
  { "en": "Apa Itu Modbus Function Code 05?", "id": "Menulis Single Coil Digital." },
  { "en": "Apa Itu Hart Communicator?", "id": "Alat Setting Transmitter 4-20mA." },
  { "en": "Apa Itu Foundation Fieldbus H1?", "id": "Bus Instrumen Digital 31,25 kbps." },
  { "en": "Apa Itu PID Integral Windup?", "id": "Akumulasi Error Saat Aktuator Mentok." },
  { "en": "Apa Itu Current Transformer Knee Point?", "id": "Titik Jenuh Inti Magnet CT." },
  { "en": "Apa Itu CT Accuracy Class 0.5S?", "id": "Akurat Dari Arus Rendah Sekali." },
  { "en": "Apa Itu Rogowski Coil Integrator?", "id": "Rangkaian Pengolah Sinyal Koil Udara." },
  { "en": "Apa Itu CVT Ferroresonance?", "id": "Osilasi Tegangan Pada Trafo Kapasitif." },
  { "en": "Apa Itu Battery Peukert Exponent?", "id": "Konstanta Efisiensi Arus Baterai." },
  { "en": "Nilai Peukert Baterai Lithium?", "id": "Mendekati 1 Sangat Efisien." },
  { "en": "Apa Itu Battery Self Discharge?", "id": "Daya Berkurang Saat Disimpan." },
  { "en": "Apa Itu Battery Cycle Life?", "id": "Jumlah Cas Habis Hingga Rusak." },
  { "en": "Apa Itu Depth Of Discharge (DOD)?", "id": "Persen Kapasitas Yang Terpakai." },
  { "en": "Apa Itu Supercapacitor ESR?", "id": "Hambatan Seri Internal Kapasitor." },
  { "en": "Apa Itu Leakage Current Supercap?", "id": "Arus Bocor Menurunkan Tegangan." },
  { "en": "Apa Itu Active Cell Balancing?", "id": "Transfer Energi Antar Sel." },
  { "en": "Apa Itu BMS Mosfet Switch?", "id": "Sakelar Pemutus Baterai Elektronik." },
  { "en": "Apa Itu NTC Thermistor Beta?", "id": "Konstanta Kurva Suhu Resistor." },
  { "en": "Apa Itu RTD Self Heating?", "id": "Panas Akibat Arus Ukur Sensor." },
  { "en": "Apa Itu Thermocouple Cold Junction?", "id": "Sambungan Referensi Di Alat Ukur." },
  { "en": "Apa Itu Infrared Emissivity?", "id": "Kemampuan Benda Memancar Panas." },
  { "en": "Nilai Emisivitas Benda Hitam?", "id": "1,0 Sempurna." },
  { "en": "Nilai Emisivitas Aluminium Mengkilap?", "id": "Sangat Rendah Di Bawah 0,1." },
  { "en": "Apa Itu Ultrasonic Blind Zone?", "id": "Jarak Dekat Tak Terbaca Sensor." },
  { "en": "Apa Itu Radar Level Dielectric?", "id": "Konstanta Cairan Pantulkan Radar." },
  { "en": "Apa Itu Coriolis Mass Flow?", "id": "Ukur Massa Cairan Langsung." },
  { "en": "Apa Itu Vortex Shedding Bar?", "id": "Batang Pembuat Pusaran Aliran." },
  { "en": "Apa Itu Orifice Plate Beta Ratio?", "id": "Perbandingan Diameter Lubang Dan Pipa." },
  { "en": "Apa Itu pH Buffer Solution?", "id": "Cairan Kalibrasi pH Meter." },
  { "en": "Elektroda pH Harus Disimpan Di?", "id": "Cairan Larutan KCL." },
  { "en": "Apa Itu Conductivity Cell Constant?", "id": "Faktor Geometri Elektroda Konduktivitas." },
  { "en": "Apa Itu Turbidity NTU?", "id": "Satuan Kekeruhan Air." },
  { "en": "Apa Itu Dissolved Oxygen Probe?", "id": "Sensor Oksigen Terlarut Air." },
  { "en": "Apa Itu LEL Gas Detector?", "id": "Lower Explosive Limit Gas." },
  { "en": "Apa Itu PID VOC Sensor?", "id": "Photoionization Detector Gas Organik." },
  { "en": "Apa Itu Economizer Boiler?", "id": "Pemanas Air Umpan Sebelum Boiler." },
  { "en": "Apa Fungsi Air Preheater?", "id": "Memanaskan Udara Masuk Ruang Bakar." },
  { "en": "Apa Itu Superheater Pembangkit?", "id": "Mengubah Uap Basah Jadi Kering." },
  { "en": "Apa Itu Reheater Turbine?", "id": "Memanaskan Ulang Uap Turbin." },
  { "en": "Apa Itu Deaerator?", "id": "Pembuang Oksigen Dari Air Umpan." },
  { "en": "Mengapa Oksigen Harus Dibuang?", "id": "Mencegah Korosi Pipa Boiler." },
  { "en": "Apa Itu Pulverizer Mill?", "id": "Mesin Penggiling Batubara Jadi Bubuk." },
  { "en": "Apa Itu Electrostatic Precipitator (ESP)?", "id": "Penyaring Debu Asap Muatan Listrik." },
  { "en": "Apa Itu Flue Gas Desulfurization?", "id": "Penyaring Gas Sulfur Buang." },
  { "en": "Apa Itu Cooling Tower Drift Loss?", "id": "Air Hilang Terbawa Angin." },
  { "en": "Apa Itu Catenary Stagger?", "id": "Zigzag Kabel Kontak Kereta." },
  { "en": "Tujuan Stagger Kabel Kontak?", "id": "Agar Pantograf Aus Merata." },
  { "en": "Apa Itu Dropper Wire?", "id": "Kawat Penggantung Kabel Kontak." },
  { "en": "Apa Itu Tensioning Device?", "id": "Penjaga Kekencangan Kabel LAA." },
  { "en": "Apa Itu Neutral Section Kereta?", "id": "Area Rel Tanpa Listrik Pemisah." },
  { "en": "Apa Itu Mast LAA?", "id": "Tiang Penyangga Kabel Kereta." },
  { "en": "Apa Itu Return Conductor?", "id": "Kabel Arus Balik Ke Gardu." },
  { "en": "Apa Itu Stray Current Corrosion?", "id": "Karat Pipa Akibat Arus Liar." },
  { "en": "Apa Itu Rail Bond?", "id": "Kabel Sambungan Antar Batang Rel." },
  { "en": "Apa Itu Impedance Bond?", "id": "Pemisah Sinyal Dan Arus Traksi." },
  { "en": "Apa Itu FFT Windowing?", "id": "Teknik Potong Sinyal Sebelum FFT." },
  { "en": "Apa Itu Hamming Window?", "id": "Jendela Filter Redam Sidelobe." },
  { "en": "Apa Itu Spectral Leakage?", "id": "Energi Bocor Ke Frekuensi Sebelah." },
  { "en": "Apa Itu Quantization Noise?", "id": "Error Akibat Resolusi Bit Terbatas." },
  { "en": "Apa Itu Dithering ADC?", "id": "Menambah Noise Putih Tingkatkan Akurasi." },
  { "en": "Apa Itu Oversampling 4x?", "id": "Sampling 4 Kali Frekuensi Nyquist." },
  { "en": "Apa Itu Decimation?", "id": "Mengurangi Sample Rate Data Digital." },
  { "en": "Apa Itu Interpolation DSP?", "id": "Menambah Sample Rate Data Digital." },
  { "en": "Apa Itu Butterfly Operation?", "id": "Perhitungan Dasar Algoritma FFT." },
  { "en": "Apa Itu Fixed Point DSP?", "id": "Prosesor Hitung Bilangan Bulat." },
  { "en": "Apa Itu Floating Point DSP?", "id": "Prosesor Hitung Bilangan Desimal." },
  { "en": "Apa Itu BGA Ball Pitch?", "id": "Jarak Pusat Bola Timah IC." },
  { "en": "Apa Itu Underfill Epoxy?", "id": "Lem Penguat Bawah Chip BGA." },
  { "en": "Apa Itu Wire Bonding?", "id": "Koneksi Kawat Emas Ke Chip." },
  { "en": "Apa Itu Leadframe?", "id": "Kerangka Logam Kaki IC." },
  { "en": "Apa Itu Molding Compound?", "id": "Plastik Hitam Penutup IC." },
  { "en": "Apa Itu Die Attach Paste?", "id": "Perekat Chip Ke Substrat." },
  { "en": "Apa Itu Wafer Sawing?", "id": "Proses Potong Wafer Jadi Chip." },
  { "en": "Apa Itu Flip Chip?", "id": "Chip Terbalik Tanpa Kawat." },
  { "en": "Apa Itu System In Package (SiP)?", "id": "Gabungan Beberapa Chip Satu Kemasan." },
  { "en": "Apa Itu Through Silicon Via (TSV)?", "id": "Jalur Tembus Vertikal Chip." },
  { "en": "Apa Itu QFN Package?", "id": "Quad Flat No-Lead." },
  { "en": "Kelebihan QFN Adalah?", "id": "Ukuran Kecil Dan Padinginan Baik." },
  { "en": "Apa Itu Thermal Pad QFN?", "id": "Logam Dasar Buang Panas." },
  { "en": "Apa Itu Moisture Sensitivity Level?", "id": "Ketahanan Komponen Terhadap Udara Lembab." },
  { "en": "Apa Itu Popcorn Effect?", "id": "IC Pecah Saat Dioven Solder." },
  { "en": "Apa Itu Dry Pack?", "id": "Kemasan Vakum Dengan Silica Gel." },
  { "en": "Apa Itu HIC Card?", "id": "Kertas Indikator Kelembaban Kemasan." },
  { "en": "Apa Itu ESD Tray?", "id": "Wadah Chip Anti Statis." },
  { "en": "Apa Itu Tape And Reel?", "id": "Kemasan Gulungan Untuk Mesin SMT." },
  { "en": "Apa Itu Pick And Place?", "id": "Mesin Pasang Komponen Otomatis." },
  { "en": "Apa Itu Solder Paste?", "id": "Campuran Butiran Timah Dan Flux." },
  { "en": "Apa Itu Stencil Thickness?", "id": "Ketebalan Plat Cetak Solder." },
  { "en": "Apa Itu Reflow Profile?", "id": "Grafik Suhu Pemanasan Oven." },
  { "en": "Zona Soak Reflow Berfungsi?", "id": "Mengaktifkan Flux Dan Ratakan Suhu." },
  { "en": "Apa Itu Liquidus Temperature?", "id": "Suhu Timah Mencair Sempurna." },
  { "en": "Apa Itu Tombstoning?", "id": "Komponen Berdiri Saat Disolder." },
  { "en": "Apa Itu Solder Bridge?", "id": "Hubung Singkat Antar Kaki." },
  { "en": "Apa Itu Voiding?", "id": "Rongga Udara Dalam Timah." },
  { "en": "Apa Itu AOI Inspection?", "id": "Inspeksi Optik Otomatis PCB." },
  { "en": "Apa Itu AXI Inspection?", "id": "Inspeksi Sinar X PCB." },
  { "en": "Apa Itu ICT Test?", "id": "In Circuit Test Jarum." },
  { "en": "Apa Itu Functional Test?", "id": "Uji Fungsi Alat Menyala." },
  { "en": "Apa Itu Burn In Test?", "id": "Uji Nyala Durasi Lama." },
  { "en": "Apa Itu Conformity Assessment?", "id": "Penilaian Kesesuaian Standar." },
  { "en": "Apa Itu CE Marking?", "id": "Tanda Standar Keamanan Eropa." },
  { "en": "Apa Itu UL Listing?", "id": "Sertifikasi Keamanan Amerika." },
  { "en": "Apa Itu RoHS Compliance?", "id": "Bebas Bahan Berbahaya Timbal." },
  { "en": "Apa Itu REACH Regulation?", "id": "Regulasi Bahan Kimia Eropa." },
  { "en": "Apa Itu WEEE Directive?", "id": "Aturan Limbah Elektronik." },
  { "en": "Apa Itu MTBF Calculator?", "id": "Hitung Rata Rata Waktu Rusak." },
  { "en": "Apa Itu FMEA?", "id": "Failure Mode Effects Analysis." },
  { "en": "Apa Itu Root Cause Analysis?", "id": "Mencari Akar Penyebab Masalah." },
  { "en": "Apa Itu Pareto Chart?", "id": "Grafik Prioritas Masalah Terbanyak." },
  { "en": "Apa Itu Six Sigma?", "id": "Metode Perbaikan Kualitas Proses." },
  { "en": "Apa Itu Cpk Index?", "id": "Indeks Kemampuan Proses Produksi." },
  { "en": "Apa Itu Yield Rate?", "id": "Persentase Produk Bagus." },
  { "en": "Apa Itu Scrap Rate?", "id": "Persentase Produk Buang." },
  { "en": "Apa Itu Rework Station?", "id": "Tempat Perbaikan PCB Cacat." },
  { "en": "Apa Itu De-soldering Pump?", "id": "Pompa Penyedot Timah Manual." },
  { "en": "Apa Itu Solder Wick?", "id": "Pita Tembaga Serap Timah." },
  { "en": "Apa Itu Flux Pen?", "id": "Spidol Cairan Flux." },
  { "en": "Apa Itu Tip Tinner?", "id": "Pembersih Mata Solder Oksidasi." },
  { "en": "Apa Itu Smoke Absorber?", "id": "Penyedot Asap Solder." },
  { "en": "Apa Itu ESD Mat?", "id": "Alas Meja Anti Statis." },
  { "en": "Apa Itu Wrist Strap?", "id": "Gelang Grounding Tangan." },
  { "en": "Apa Itu Heel Strap?", "id": "Grounding Sepatu Ke Lantai." },
  { "en": "Apa Itu Ionizer Fan?", "id": "Kipas Netralisir Muatan Udara." },
  { "en": "Apa Itu Surface Resistance Meter?", "id": "Alat Ukur Tahanan ESD." },
  { "en": "Apa Itu Grounding Point?", "id": "Titik Sambung Kabel Tanah." },
  { "en": "Apa Itu Faraday Cage?", "id": "Sangkar Pelindung Medan Listrik." },
  { "en": "Apa Itu Shielding Effectiveness?", "id": "Kemampuan Redam Sinyal RF." },
  { "en": "Apa Itu EMI Gasket?", "id": "Karet Konduktif Celah Casing." },
  { "en": "Apa Itu Conductive Paint?", "id": "Cat Pelapis Anti EMI." },
  { "en": "Apa Itu Ferrite Core?", "id": "Cincin Magnet Kabel Data." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Redam Noise Bersama." },
  { "en": "Apa Itu Differential Mode Choke?", "id": "Induktor Redam Noise Fasa." },
  { "en": "Apa Itu X Capacitor?", "id": "Kapasitor Antar Line." },
  { "en": "Apa Itu Y Capacitor?", "id": "Kapasitor Line Ke Ground." }


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
