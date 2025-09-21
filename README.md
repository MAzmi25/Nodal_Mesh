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


    { "en": "Apa Dasar Analisis Nodal?", "id": "Hukum Arus Kirchhoff (KCL)." },
    { "en": "Apa Yang Dicari Dalam Analisis Nodal?", "id": "Tegangan Pada Setiap Simpul (Node)." },
    { "en": "Apa Nama Lain Simpul Referensi?", "id": "Tanah Atau Ground." },
    { "en": "Analisis Nodal Bekerja Pada Rangkaian Apa?", "id": "Rangkaian Planar Dan Non-Planar." },
    { "en": "KCL Menyatakan Apa Tentang Arus Di Simpul?", "id": "Jumlah Arus Masuk Sama Dengan Keluar." },
    { "en": "Variabel Apa Yang Tidak Diketahui Di Nodal?", "id": "Tegangan Simpul Atau Tegangan Node." },
    { "en": "Bagaimana Tegangan Simpul Referensi Ditetapkan?", "id": "Biasanya Nol Volt." },
    { "en": "Langkah Pertama Dalam Analisis Nodal?", "id": "Identifikasi Semua Simpul Dalam Rangkaian." },
    { "en": "Apa Itu Supernode?", "id": "Gabungan Dua Simpul Non-Referensi." },
    { "en": "Kapan Supernode Terbentuk?", "id": "Ketika Sumber Tegangan Antara Dua Simpul." },
    { "en": "Apa Dasar Analisis Mesh?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
    { "en": "Apa Yang Dicari Dalam Analisis Mesh?", "id": "Arus Yang Mengalir Di Setiap Mesh." },
    { "en": "Apa Itu Mesh?", "id": "Loop Yang Tidak Mengandung Loop Lain." },
    { "en": "Analisis Mesh Bekerja Pada Rangkaian Apa?", "id": "Hanya Rangkaian Planar." },
    { "en": "KVL Menyatakan Apa Tentang Tegangan Loop?", "id": "Jumlah Tegangan Dalam Loop Sama Dengan Nol." },
    { "en": "Variabel Apa Yang Tidak Diketahui Di Mesh?", "id": "Arus Mesh Atau Arus Loop." },
    { "en": "Langkah Pertama Dalam Analisis Mesh?", "id": "Identifikasi Semua Mesh Dalam Rangkaian." },
    { "en": "Apa Nama Arus Hipotetis Di Mesh?", "id": "Arus Mesh." },
    { "en": "Apa Itu Supermesh?", "id": "Gabungan Dua Mesh Yang Berdekatan." },
    { "en": "Kapan Supermesh Terbentuk?", "id": "Ketika Sumber Arus Antara Dua Mesh." },
    { "en": "Metode Mana Yang Lebih Umum?", "id": "Analisis Nodal Lebih Umum Digunakan." },
    { "en": "Nodal Menggunakan Persamaan Berbasis Apa?", "id": "Persamaan Arus Di Setiap Simpul." },
    { "en": "Mesh Menggunakan Persamaan Berbasis Apa?", "id": "Persamaan Tegangan Di Setiap Loop." },
    { "en": "Apa Itu Simpul?", "id": "Titik Pertemuan Dua Atau Lebih Elemen." },
    { "en": "Berapa Banyak Simpul Referensi Dalam Satu Rangkaian?", "id": "Hanya Satu Simpul Referensi." },
    { "en": "Arus Mesh Mengalir Searah Apa?", "id": "Biasanya Dianggap Searah Jarum Jam." },
    { "en": "Nodal Cocok Untuk Sumber Apa?", "id": "Sumber Arus." },
    { "en": "Mesh Cocok Untuk Sumber Apa?", "id": "Sumber Tegangan." },
    { "en": "Apa Satuan Tegangan Simpul?", "id": "Volt." },
    { "en": "Apa Satuan Arus Mesh?", "id": "Ampere." },
    { "en": "Bagaimana Cara Menyelesaikan Persamaan Nodal?", "id": "Menggunakan Metode Aljabar Linear." },
    { "en": "Bagaimana Cara Menyelesaikan Persamaan Mesh?", "id": "Menggunakan Aturan Cramer Atau Substitusi." },
    { "en": "Apa Itu Rangkaian Planar?", "id": "Rangkaian Yang Dapat Digambar Datar." },
    { "en": "Persamaan Nodal Didasarkan Pada Elemen Apa?", "id": "Konduktansi Atau Admitansi." },
    { "en": "Persamaan Mesh Didasarkan Pada Elemen Apa?", "id": "Resistansi Atau Impedansi." },
    { "en": "Apa Tujuan Utama Kedua Analisis?", "id": "Menyederhanakan Analisis Rangkaian Kompleks." },
    { "en": "Berapa Jumlah Persamaan Nodal Yang Diperlukan?", "id": "Jumlah Simpul Dikurangi Satu." },
    { "en": "Berapa Jumlah Persamaan Mesh Yang Diperlukan?", "id": "Jumlah Mesh Dalam Rangkaian." },
    { "en": "Apakah Analisis Nodal Selalu Mungkin?", "id": "Ya, Untuk Semua Jenis Rangkaian." },
    { "en": "Apakah Analisis Mesh Selalu Mungkin?", "id": "Tidak, Hanya Untuk Rangkaian Planar." },
    { "en": "Apa Yang Diabaikan Saat Menerapkan KCL?", "id": "Tegangan Pada Simpul Itu Sendiri." },
    { "en": "Apa Yang Diabaikan Saat Menerapkan KVL?", "id": "Arus Yang Mengalir Melalui Loop." },
    { "en": "Hukum Ohm Menghubungkan Apa?", "id": "Tegangan, Arus, Dan Resistansi." },
    { "en": "Simpul Esensial Itu Apa?", "id": "Simpul Yang Menghubungkan Tiga Elemen Atau Lebih." },
    { "en": "Cabang Dalam Rangkaian Itu Apa?", "id": "Jalur Tunggal Yang Menghubungkan Dua Simpul." },
    { "en": "Bagaimana Arus Dinyatakan Dalam Analisis Nodal?", "id": "Sebagai Perbedaan Tegangan Dibagi Resistansi." },
    { "en": "Bagaimana Tegangan Dinyatakan Dalam Analisis Mesh?", "id": "Sebagai Arus Mesh Dikalikan Resistansi." },
    { "en": "Kapan Analisis Nodal Lebih Mudah?", "id": "Ketika Jumlah Simpul Lebih Sedikit." },
    { "en": "Kapan Analisis Mesh Lebih Mudah?", "id": "Ketika Jumlah Mesh Lebih Sedikit." },
    { "en": "Apa Itu Sumber Tegangan Independen?", "id": "Memberikan Tegangan Konstan." },
    { "en": "Apa Itu Sumber Arus Independen?", "id": "Memberikan Arus Konstan." },
    { "en": "Apa Itu Sumber Terkendali?", "id": "Nilainya Tergantung Variabel Rangkaian Lain." },
    { "en": "Nodal Menghasilkan Matriks Apa?", "id": "Matriks Admitansi Simpul." },
    { "en": "Mesh Menghasilkan Matriks Apa?", "id": "Matriks Impedansi Mesh." },
    { "en": "Apa Elemen Diagonal Matriks Admitansi?", "id": "Jumlah Konduktansi Terhubung Ke Simpul." },
    { "en": "Apa Elemen Non-Diagonal Matriks Admitansi?", "id": "Negatif Konduktansi Antara Dua Simpul." },
    { "en": "Apa Elemen Diagonal Matriks Impedansi?", "id": "Jumlah Resistansi Dalam Satu Mesh." },
    { "en": "Apa Elemen Non-Diagonal Matriks Impedansi?", "id": "Negatif Resistansi Bersama Antara Mesh." },
    { "en": "Apakah Arah Arus Penting Di Nodal?", "id": "Ya, Untuk Menentukan Tanda Positif/Negatif." },
    { "en": "Apakah Arah Loop Penting Di Mesh?", "id": "Ya, Untuk Konsistensi Arah Arus." },
    { "en": "Analisis Nodal Berguna Untuk Rangkaian Dengan Banyak?", "id": "Sumber Arus Paralel." },
    { "en": "Analisis Mesh Berguna Untuk Rangkaian Dengan Banyak?", "id": "Sumber Tegangan Seri." },
    { "en": "Siapa Penemu Hukum Arus?", "id": "Gustav Kirchhoff." },
    { "en": "Siapa Penemu Hukum Tegangan?", "id": "Gustav Kirchhoff." },
    { "en": "Apa Langkah Setelah Menulis Persamaan?", "id": "Menyelesaikan Sistem Persamaan Linear Tersebut." },
    { "en": "Bagaimana Cara Memilih Simpul Referensi?", "id": "Simpul Dengan Cabang Terhubung Paling Banyak." },
    { "en": "Apa Konsekuensi Salah Arah Arus Mesh?", "id": "Hasil Arus Akan Bernilai Negatif." },
    { "en": "Apa Fungsi Utama Simpul Referensi?", "id": "Sebagai Titik Potensial Nol." },
    { "en": "Apa Keuntungan Utama Metode Ini?", "id": "Mengurangi Jumlah Persamaan Yang Perlu Diselesaikan." },
    { "en": "Apakah Arus Mesh Adalah Arus Nyata?", "id": "Tidak, Arus Mesh Adalah Konstruk Matematis." },
    { "en": "Apakah Tegangan Simpul Adalah Tegangan Nyata?", "id": "Ya, Relatif Terhadap Simpul Referensi." },
    { "en": "Bagaimana Menangani Sumber Tegangan Di Nodal?", "id": "Menggunakan Konsep Supernode." },
    { "en": "Bagaimana Menangani Sumber Arus Di Mesh?", "id": "Menggunakan Konsep Supermesh." },
    { "en": "Metode Mana Yang Lebih Baik Untuk Komputer?", "id": "Analisis Nodal Lebih Mudah Diprogramkan." },
    { "en": "Apa Itu Loop Independen?", "id": "Setiap Loop Mengandung Setidaknya Satu Cabang Unik." },
    { "en": "Berapa Persamaan Tambahan Untuk Supernode?", "id": "Satu Persamaan Kendala Tegangan." },
    { "en": "Berapa Persamaan Tambahan Untuk Supermesh?", "id": "Satu Persamaan Kendala Arus." },
    { "en": "Elemen Apa Yang Diperlakukan Sama Di Nodal?", "id": "Semua Elemen Pasif." },
    { "en": "Elemen Apa Yang Diperlakukan Sama Di Mesh?", "id": "Semua Elemen Pasif." },
    { "en": "Apa Itu Titik Simpul Tunggal?", "id": "Simpul Yang Tidak Terhubung Ke Mana Pun." },
    { "en": "Apa Peran Resistansi Dalam Persamaan Nodal?", "id": "Sebagai Penyebut Dalam Suku Arus." },
    { "en": "Apa Peran Resistansi Dalam Persamaan Mesh?", "id": "Sebagai Pengali Dalam Suku Tegangan." },
    { "en": "Analisis Rangkaian Adalah Studi Tentang Apa?", "id": "Perilaku Arus Dan Tegangan." },
    { "en": "Apa Definisi Rangkaian Listrik?", "id": "Interkoneksi Dari Berbagai Elemen Listrik." },
    { "en": "Apa Elemen Aktif Dalam Rangkaian?", "id": "Elemen Yang Menghasilkan Energi." },
    { "en": "Contoh Elemen Aktif?", "id": "Baterai Dan Generator." },
    { "en": "Apa Elemen Pasif Dalam Rangkaian?", "id": "Elemen Yang Menyerap Energi." },
    { "en": "Contoh Elemen Pasif?", "id": "Resistor, Kapasitor, Dan Induktor." },
    { "en": "Apa Nama Lengkap KCL?", "id": "Kirchhoff's Current Law." },
    { "en": "Apa Nama Lengkap KVL?", "id": "Kirchhoff's Voltage Law." },
    { "en": "KCL Adalah Bentuk Konservasi Apa?", "id": "Konservasi Muatan Listrik." },
    { "en": "KVL Adalah Bentuk Konservasi Apa?", "id": "Konservasi Energi." },
    { "en": "Bagaimana Tegangan Diukur?", "id": "Antara Dua Titik Dalam Rangkaian." },
    { "en": "Bagaimana Arus Diukur?", "id": "Melalui Suatu Elemen Dalam Rangkaian." },
    { "en": "Apa Syarat Rangkaian Dapat Dianalisis?", "id": "Harus Rangkaian Linear." },
    { "en": "Mengapa Arah Arus Di Asumsikan?", "id": "Untuk Membangun Persamaan Secara Konsisten." },
    { "en": "Apa Hasil Akhir Analisis Nodal?", "id": "Nilai Tegangan Di Setiap Simpul." },
    { "en": "Apa Hasil Akhir Analisis Mesh?", "id": "Nilai Arus Di Setiap Mesh." },
    { "en": "Apakah Kedua Metode Memberikan Hasil Sama?", "id": "Ya, Hasil Akhirnya Selalu Sama." },
    { "en": "Apa Langkah Kedua Analisis Nodal?", "id": "Pilih Satu Simpul Sebagai Referensi." },
    { "en": "Apa Langkah Ketiga Analisis Nodal?", "id": "Terapkan KCL Pada Setiap Simpul Non-Referensi." },
    { "en": "Apa Langkah Kedua Analisis Mesh?", "id": "Berikan Arus Mesh Untuk Setiap Mesh." },
    { "en": "Apa Langkah Ketiga Analisis Mesh?", "id": "Terapkan KVL Pada Setiap Mesh." },
    { "en": "Bagaimana Arus Cabang Dihitung Dari Arus Mesh?", "id": "Selisih Aljabar Dari Arus Mesh Terkait." },
    { "en": "Bagaimana Tegangan Antar Simpul Dihitung?", "id": "Selisih Aljabar Dari Tegangan Simpul Terkait." },
    { "en": "Persamaan Kendala Supernode Menghubungkan Apa?", "id": "Tegangan Dua Simpul Dengan Sumber Tegangan." },
    { "en": "Persamaan Kendala Supermesh Menghubungkan Apa?", "id": "Arus Dua Mesh Dengan Sumber Arus." },
    { "en": "Apakah Supernode Memiliki Tegangan Sendiri?", "id": "Tidak, Supernode Bukan Titik Fisik." },
    { "en": "Hukum Apa Yang Diterapkan Pada Supernode?", "id": "Hukum Arus Kirchhoff (KCL)." },
    { "en": "Hukum Apa Yang Diterapkan Pada Supermesh?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
    { "en": "Apa Satuan Dari Konduktansi?", "id": "Siemens Atau Mho." },
    { "en": "Apa Satuan Dari Resistansi?", "id": "Ohm." },
    { "en": "Bagaimana Konduktansi Terkait Dengan Resistansi?", "id": "Konduktansi Adalah Kebalikan Dari Resistansi." },
    { "en": "Bagaimana Arus Didefinisikan Dalam Nodal?", "id": "(V1 - V2) / R." },
    { "en": "Bagaimana Tegangan Didefinisikan Dalam Mesh?", "id": "I * R." },
    { "en": "Sisi Kanan Persamaan Nodal Berisi Apa?", "id": "Jumlah Sumber Arus Yang Masuk Simpul." },
    { "en": "Sisi Kanan Persamaan Mesh Berisi Apa?", "id": "Jumlah Sumber Tegangan Dalam Mesh." },
    { "en": "Tanda Arus Sumber Di Persamaan Nodal?", "id": "Positif Jika Masuk, Negatif Jika Keluar." },
    { "en": "Tanda Tegangan Sumber Di Persamaan Mesh?", "id": "Positif Jika Searah Loop, Negatif Berlawanan." },
    { "en": "Apa Yang Terjadi Jika Simpul Referensi Diubah?", "id": "Nilai Tegangan Simpul Lainnya Akan Berubah." },
    { "en": "Apakah Arus Cabang Berubah Jika Referensi Diubah?", "id": "Tidak, Arus Cabang Tetap Sama." },
    { "en": "Metode Mana Yang Membutuhkan Lebih Sedikit Persamaan?", "id": "Tergantung Pada Topologi Rangkaian." },
    { "en": "Apa Itu Cabang Esensial?", "id": "Cabang Yang Menghubungkan Dua Simpul Esensial." },
    { "en": "Berapa Jumlah Arus Mesh Dalam Rangkaian?", "id": "Sama Dengan Jumlah Loop Independen." },
    { "en": "Berapa Jumlah Tegangan Simpul Yang Dicari?", "id": "Jumlah Simpul Dikurangi Satu." },
    { "en": "Apa Tujuan Menggunakan Supernode?", "id": "Menghindari Persamaan Untuk Arus Sumber Tegangan." },
    { "en": "Apa Tujuan Menggunakan Supermesh?", "id": "Menghindari Persamaan Untuk Tegangan Sumber Arus." },
    { "en": "Apakah Analisis Nodal Bisa Untuk Sumber Terkendali?", "id": "Ya, Sangat Bisa." },
    { "en": "Apakah Analisis Mesh Bisa Untuk Sumber Terkendali?", "id": "Ya, Sangat Bisa." },
    { "en": "Bagaimana Sumber Arus Independen Diperlakukan Di Nodal?", "id": "Sebagai Nilai Yang Diketahui Di Persamaan." },
    { "en": "Bagaimana Sumber Tegangan Independen Diperlakukan Di Mesh?", "id": "Sebagai Nilai Yang Diketahui Di Persamaan." },
    { "en": "Apa Itu Matriks Simetris?", "id": "Matriks Sama Dengan Transposenya." },
    { "en": "Kapan Matriks Admitansi Simetris?", "id": "Jika Rangkaian Hanya Mengandung Elemen Pasif." },
    { "en": "Kapan Matriks Impedansi Simetris?", "id": "Jika Rangkaian Hanya Mengandung Elemen Pasif." },
    { "en": "Apa Yang Menyebabkan Matriks Menjadi Tidak Simetris?", "id": "Kehadiran Sumber Terkendali." },
    { "en": "Metode Mana Yang Anda Pilih Untuk Sumber Arus?", "id": "Analisis Nodal." },
    { "en": "Metode Mana Yang Anda Pilih Untuk Sumber Tegangan?", "id": "Analisis Mesh." },
    { "en": "Apa Langkah Terakhir Dalam Kedua Analisis?", "id": "Hitung Kuantitas Yang Diinginkan (Arus/Tegangan)." },
    { "en": "Nodal Mereduksi Rangkaian Menjadi Persamaan Apa?", "id": "Sistem Persamaan Linear Simultan." },
    { "en": "Mesh Mereduksi Rangkaian Menjadi Persamaan Apa?", "id": "Sistem Persamaan Linear Simultan." },
    { "en": "Apa Itu Rangkaian Non-Planar?", "id": "Rangkaian Yang Tidak Bisa Digambar Datar." },
    { "en": "Contoh Rangkaian Non-Planar?", "id": "Rangkaian Jembatan Dengan Sambungan Silang." },
    { "en": "Bisakah Mesh Digunakan Untuk Rangkaian Non-Planar?", "id": "Tidak Bisa." },
    { "en": "Bisakah Nodal Digunakan Untuk Rangkaian Non-Planar?", "id": "Ya, Bisa." },
    { "en": "Apa Nama Lain Analisis Nodal?", "id": "Metode Tegangan Simpul." },
    { "en": "Apa Nama Lain Analisis Mesh?", "id": "Metode Arus Loop." },
    { "en": "Apakah Polaritas Tegangan Penting Di Nodal?", "id": "Sangat Penting Untuk Arah Arus." },
    { "en": "Apakah Arah Arus Penting Di Mesh?", "id": "Sangat Penting Untuk Tanda Tegangan." },
    { "en": "Nodal Adalah Aplikasi Langsung Dari?", "id": "Hukum Arus Kirchhoff." },
    { "en": "Mesh Adalah Aplikasi Langsung Dari?", "id": "Hukum Tegangan Kirchhoff." },
    { "en": "Variabel Dalam Nodal Selalu Berbentuk Apa?", "id": "Tegangan." },
    { "en": "Variabel Dalam Mesh Selalu Berbentuk Apa?", "id": "Arus." },
    { "en": "Bagaimana Resistansi Dinyatakan Dalam Persamaan Nodal?", "id": "Sebagai Konduktansi (1/R)." },
    { "en": "Bagaimana Konduktansi Dinyatakan Dalam Persamaan Mesh?", "id": "Sebagai Resistansi (1/G)." },
    { "en": "Apa Prasyarat Untuk Analisis Ini?", "id": "Memahami Hukum Ohm Dan Hukum Kirchhoff." },
    { "en": "Kapan Jumlah Persamaan Nodal Dan Mesh Sama?", "id": "Ketika B = N + L - 1." },
    { "en": "B Singkatan Dari Apa?", "id": "Jumlah Cabang (Branches)." },
    { "en": "N Singkatan Dari Apa?", "id": "Jumlah Simpul (Nodes)." },
    { "en": "L Singkatan Dari Apa?", "id": "Jumlah Loop Independen." },
    { "en": "Metode Mana Yang Lebih Intuitif Bagi Pemula?", "id": "Seringkali Analisis Mesh Dianggap Lebih Intuitif." },
    { "en": "Apa Itu Tegangan Simpul?", "id": "Beda Potensial Antara Simpul Dan Referensi." },
    { "en": "Arus Mesh I1 Hanya Milik Siapa?", "id": "Hanya Milik Mesh 1." },
    { "en": "Arus Cabang Bisa Milik Siapa?", "id": "Satu Atau Dua Mesh Sekaligus." },
    { "en": "Apakah Simpul Referensi Harus Di Bawah?", "id": "Tidak, Bisa Di Mana Saja." },
    { "en": "Pilihan Simpul Referensi Mempengaruhi Apa?", "id": "Kompleksitas Perhitungan." },
    { "en": "Apa Yang Disederhanakan Dengan Memilih Referensi Tepat?", "id": "Jumlah Tegangan Sumber Yang Tidak Diketahui." },
    { "en": "Apakah Mesh Bisa Berbentuk Aneh?", "id": "Ya, Selama Itu Loop Terdalam." },
    { "en": "Apa Itu Jendela Rangkaian?", "id": "Istilah Lain Untuk Mesh." },
    { "en": "Bagaimana Jika Sumber Arus Ada Di Luar Mesh?", "id": "Arus Mesh Terdalam Sama Dengan Sumber Arus." },
    { "en": "Bagaimana Jika Sumber Tegangan Terhubung Ke Referensi?", "id": "Tegangan Simpul Itu Langsung Diketahui." },
    { "en": "Apa Yang Terjadi Pada Persamaan Supernode?", "id": "KCL Diterapkan Ke Seluruh Permukaan Supernode." },
    { "en": "Apa Yang Terjadi Pada Persamaan Supermesh?", "id": "KVL Diterapkan Mengikuti Jalur Supermesh." },
    { "en": "Jalur Supermesh Menghindari Apa?", "id": "Sumber Arus Dan Elemen Paralelnya." },
    { "en": "Analisis Nodal Menggunakan Matriks Apa?", "id": "Matriks Admitansi (Y)." },
    { "en": "Analisis Mesh Menggunakan Matriks Apa?", "id": "Matriks Impedansi (Z)." },
    { "en": "Persamaan Matriks Nodal Adalah?", "id": "YV = I." },
    { "en": "Persamaan Matriks Mesh Adalah?", "id": "ZI = V." },
    { "en": "Vektor I Dalam Persamaan Nodal Berisi Apa?", "id": "Sumber Arus." },
    { "en": "Vektor V Dalam Persamaan Mesh Berisi Apa?", "id": "Sumber Tegangan." },
    { "en": "Vektor V Dalam Persamaan Nodal Berisi Apa?", "id": "Tegangan Simpul Yang Tidak Diketahui." },
    { "en": "Vektor I Dalam Persamaan Mesh Berisi Apa?", "id": "Arus Mesh Yang Tidak Diketahui." },
    { "en": "Inspeksi Visual Digunakan Untuk Membentuk Apa?", "id": "Matriks Admitansi Atau Impedansi." },
    { "en": "Kapan Metode Inspeksi Bisa Digunakan?", "id": "Hanya Untuk Rangkaian Dengan Sumber Independen." },
    { "en": "Nodal Lebih Baik Untuk Berapa Banyak Simpul?", "id": "Lebih Sedikit Simpul Daripada Mesh." },
    { "en": "Mesh Lebih Baik Untuk Berapa Banyak Mesh?", "id": "Lebih Sedikit Mesh Daripada Simpul." },
    { "en": "Apa Itu Analisis Loop Umum?", "id": "Generalisasi Dari Analisis Mesh." },
    { "en": "Apakah Analisis Loop Umum Untuk Rangkaian Non-Planar?", "id": "Ya, Benar." },
    { "en": "Apa Dasar Analisis Loop Umum?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
    { "en": "Bagaimana Cara Memilih Loop Untuk Analisis Umum?", "id": "Setiap Loop Baru Harus Memiliki Elemen Unik." },
    { "en": "Apakah Arus Cabang Selalu Lebih Kecil Dari Arus Mesh?", "id": "Tidak Selalu Benar." },
    { "en": "Kapan Arus Cabang Sama Dengan Arus Mesh?", "id": "Ketika Cabang Itu Hanya Bagian Satu Mesh." },
    { "en": "Tujuan Utama Analisis Rangkaian Adalah?", "id": "Menemukan Semua Arus Dan Tegangan." },
    { "en": "Apa Itu Rangkaian Linear?", "id": "Responnya Proporsional Dengan Inputnya." },
    { "en": "Mengapa Analisis Ini Terbatas Pada Rangkaian Linear?", "id": "Karena Didasarkan Pada Persamaan Linear." },
    { "en": "Apakah Dioda Elemen Linear?", "id": "Tidak, Dioda Adalah Elemen Non-Linear." },
    { "en": "Bisakah Metode Ini Digunakan Untuk Rangkaian AC?", "id": "Ya, Dengan Menggunakan Impedansi Dan Fasor." },
    { "en": "Apa Tipe Sumber Terkendali Pertama?", "id": "Sumber Tegangan Terkendali Tegangan (VCVS)." },
    { "en": "Apa Tipe Sumber Terkendali Kedua?", "id": "Sumber Arus Terkendali Tegangan (VCCS)." },
    { "en": "Apa Tipe Sumber Terkendali Ketiga?", "id": "Sumber Tegangan Terkendali Arus (CCVS)." },
    { "en": "Apa Tipe Sumber Terkendali Keempat?", "id": "Sumber Arus Terkendali Arus (CCCS)." },
    { "en": "Bagaimana Sumber Terkendali Diperlakukan Dalam Analisis?", "id": "Sebagai Variabel Dependen Dalam Persamaan." },
    { "en": "Parameter Penguatan VCVS Tidak Memiliki Satuan Apa?", "id": "Tidak Memiliki Satuan (Volt/Volt)." },
    { "en": "Apa Satuan Parameter Penguatan VCCS?", "id": "Siemens (Ampere/Volt)." },
    { "en": "Apa Satuan Parameter Penguatan CCVS?", "id": "Ohm (Volt/Ampere)." },
    { "en": "Parameter Penguatan CCCS Tidak Memiliki Satuan Apa?", "id": "Tidak Memiliki Satuan (Ampere/Ampere)." },
    { "en": "Di Mana Tegangan Pengontrol VCCS Diukur?", "id": "Di Antara Dua Titik Lain Rangkaian." },
    { "en": "Di Mana Arus Pengontrol CCCS Diukur?", "id": "Melalui Elemen Lain Dalam Rangkaian." },
    { "en": "Apakah Kehadiran Sumber Terkendali Mempengaruhi Pilihan Metode?", "id": "Tidak Secara Langsung." },
    { "en": "Bagaimana Sumber Terkendali Ditulis Dalam Persamaan Nodal?", "id": "Sebagai Fungsi Dari Tegangan Simpul Lain." },
    { "en": "Bagaimana Sumber Terkendali Ditulis Dalam Persamaan Mesh?", "id": "Sebagai Fungsi Dari Arus Mesh Lain." },
    { "en": "Apa Yang Harus Dilakukan Pertama Dengan Sumber Terkendali?", "id": "Tulis Persamaan Kendalanya Terlebih Dahulu." },
    { "en": "Apakah Metode Inspeksi Berlaku Untuk Sumber Terkendali?", "id": "Tidak, Persamaan Harus Diturunkan Secara Manual." },
    { "en": "Apa Yang Terjadi Pada Simetri Matriks?", "id": "Sumber Terkendali Umumnya Merusak Simetri." },
    { "en": "Nodal Atau Mesh Untuk VCVS?", "id": "Keduanya Sama-Sama Layak." },
    { "en": "Nodal Atau Mesh Untuk VCCS?", "id": "Nodal Biasanya Sedikit Lebih Mudah." },
    { "en": "Nodal Atau Mesh Untuk CCVS?", "id": "Mesh Biasanya Sedikit Lebih Mudah." },
    { "en": "Nodal Atau Mesh Untuk CCCS?", "id": "Keduanya Sama-Sama Layak." },
    { "en": "Apa Langkah Tambahan Untuk Sumber Terkendali?", "id": "Substitusikan Persamaan Kendala Ke Persamaan Utama." },
    { "en": "Di Mana Lokasi Fisik Sumber Terkendali?", "id": "Dalam Model Perangkat Elektronik Seperti Transistor." },
    { "en": "Apa Itu Transkonduktansi?", "id": "Parameter Penguatan VCCS." },
    { "en": "Apa Itu Transresistansi?", "id": "Parameter Penguatan CCVS." },
    { "en": "Apa Itu Penguatan Tegangan?", "id": "Parameter Penguatan VCVS." },
    { "en": "Apa Itu Penguatan Arus?", "id": "Parameter Penguatan CCCS." },
    { "en": "Bisakah Supernode Melibatkan Sumber Terkendali?", "id": "Ya, Jika Sumber Tegangan Terkendali." },
    { "en": "Bisakah Supermesh Melibatkan Sumber Terkendali?", "id": "Ya, Jika Sumber Arus Terkendali." },
    { "en": "Pentingkah Membedakan Simpul Pengontrol?", "id": "Sangat Penting Untuk Persamaan Yang Benar." },
    { "en": "Pentingkah Membedakan Cabang Pengontrol?", "id": "Sangat Penting Untuk Persamaan Yang Benar." },
    { "en": "Apa Arti Simbol Berlian?", "id": "Mewakili Sumber Terkendali." },
    { "en": "Apa Arti Simbol Lingkaran?", "id": "Mewakili Sumber Independen." },
    { "en": "Jika Hanya Ada Sumber Tegangan, Metode Apa?", "id": "Mesh." },
    { "en": "Jika Hanya Ada Sumber Arus, Metode Apa?", "id": "Nodal." },
    { "en": "Jika Rangkaian Punya Banyak Simpul Paralel?", "id": "Nodal Lebih Mudah." },
    { "en": "Jika Rangkaian Punya Banyak Loop Seri?", "id": "Mesh Lebih Mudah." },
    { "en": "Apa Kelemahan Utama Analisis Mesh?", "id": "Keterbatasannya Pada Rangkaian Planar." },
    { "en": "Apa Kelemahan Utama Analisis Nodal?", "id": "Bisa Menjadi Rumit Dengan Sumber Tegangan." },
    { "en": "Apakah Perlu Menghitung Semua Arus Mesh?", "id": "Tidak, Hanya Yang Diperlukan Untuk Jawaban." },
    { "en": "Apakah Perlu Menghitung Semua Tegangan Simpul?", "id": "Tidak, Hanya Yang Diperlukan Untuk Jawaban." },
    { "en": "Apa Alat Matematis Yang Sering Digunakan?", "id": "Aturan Cramer Untuk Menyelesaikan Sistem Persamaan." },
    { "en": "Apa Itu Determinan Matriks?", "id": "Nilai Skalar Yang Dihitung Dari Matriks." },
    { "en": "Apa Kondisi Agar Solusi Unik Ada?", "id": "Determinan Matriks Koefisien Tidak Nol." },
    { "en": "Apa Yang Terjadi Jika Determinan Nol?", "id": "Rangkaian Mungkin Tidak Stabil Atau Redundan." },
    { "en": "Metode Mana Yang Lebih Mudah Diotomatisasi Komputer?", "id": "Nodal Karena Pembentukan Matriksnya Teratur." },
    { "en": "Bisakah Satu Rangkaian Dianalisis Dengan Kedua Metode?", "id": "Ya, Dan Hasilnya Harus Sama." },
    { "en": "Apa Gunanya Memeriksa Jawaban?", "id": "Memastikan Konservasi Daya Terpenuhi." },
    { "en": "Apa Itu Konservasi Daya?", "id": "Total Daya Yang Disuplai Sama Dengan Diserap." },
    { "en": "Bagaimana Menghitung Daya Pada Resistor?", "id": "I Kuadrat R Atau V Kuadrat Per R." },
    { "en": "Bagaimana Menghitung Daya Pada Sumber?", "id": "Tegangan Dikalikan Arus." },
    { "en": "Kapan Sumber Menyerap Daya?", "id": "Ketika Arus Mengalir Ke Terminal Positif." },
    { "en": "Kapan Sumber Menyuplai Daya?", "id": "Ketika Arus Keluar Dari Terminal Positif." },
    { "en": "Apakah Tegangan Simpul Bisa Negatif?", "id": "Ya, Relatif Terhadap Referensi." },
    { "en": "Apakah Arus Mesh Bisa Negatif?", "id": "Ya, Artinya Arah Sebenarnya Berlawanan." },
    { "en": "Apa Yang Direpresentasikan Simbol Tanah (Ground)?", "id": "Potensial Referensi Nol Volt." },
    { "en": "Apa Beda Loop Dan Mesh?", "id": "Mesh Adalah Tipe Loop Paling Sederhana." },
    { "en": "Semua Mesh Adalah Loop?", "id": "Ya, Benar." },
    { "en": "Semua Loop Adalah Mesh?", "id": "Tidak, Belum Tentu." },
    { "en": "Rangkaian Jembatan Wheatstone Planar Atau Tidak?", "id": "Ya, Itu Adalah Rangkaian Planar." },
    { "en": "Apa Itu Graph Rangkaian?", "id": "Representasi Rangkaian Dengan Garis Dan Titik." },
    { "en": "Titik Dalam Graph Mewakili Apa?", "id": "Simpul Rangkaian." },
    { "en": "Garis Dalam Graph Mewakili Apa?", "id": "Cabang Rangkaian." },
    { "en": "Topologi Rangkaian Mempelajari Apa?", "id": "Struktur Geometris Rangkaian." },
    { "en": "Analisis Mana Yang Lebih Terkait Dengan Topologi?", "id": "Keduanya Sangat Terkait Dengan Topologi." },
    { "en": "Bagaimana Menangani Resistor Paralel?", "id": "Gabungkan Menjadi Satu Resistor Ekuivalen." },
    { "en": "Bagaimana Menangani Resistor Seri?", "id": "Gabungkan Menjadi Satu Resistor Ekuivalen." },
    { "en": "Penyederhanaan Rangkaian Dilakukan Kapan?", "id": "Sebelum Menerapkan Analisis Nodal Atau Mesh." },
    { "en": "Apa Itu Transformasi Sumber?", "id": "Mengubah Sumber Tegangan Ke Sumber Arus." },
    { "en": "Apa Tujuan Transformasi Sumber?", "id": "Menyederhanakan Rangkaian Sebelum Analisis." },
    { "en": "Bisakah Analisis Ini Digunakan Untuk Kapasitor?", "id": "Ya, Dalam Domain Frekuensi (AC)." },
    { "en": "Bisakah Analisis Ini Digunakan Untuk Induktor?", "id": "Ya, Dalam Domain Frekuensi (AC)." },
    { "en": "Apa Itu Impedansi?", "id": "Ukuran Oposisi Terhadap Arus Bolak-Balik." },
    { "en": "Apa Itu Admitansi?", "id": "Ukuran Kemudahan Arus Bolak-Balik Mengalir." },
    { "en": "Impedansi Adalah Generalisasi Dari Apa?", "id": "Resistansi." },
    { "en": "Admitansi Adalah Generalisasi Dari Apa?", "id": "Konduktansi." },
    { "en": "Apa Yang Menggantikan R Dalam Analisis AC?", "id": "Impedansi Z." },
    { "en": "Apa Yang Menggantikan G Dalam Analisis AC?", "id": "Admitansi Y." },
    { "en": "Variabel Dalam Analisis AC Disebut Apa?", "id": "Fasor." },
    { "en": "Apakah KCL Berlaku Untuk Fasor?", "id": "Ya, KCL Berlaku Untuk Fasor Arus." },
    { "en": "Apakah KVL Berlaku Untuk Fasor?", "id": "Ya, KVL Berlaku Untuk Fasor Tegangan." },
    { "en": "Apa Hasil Analisis Rangkaian AC?", "id": "Besaran Dan Fasa Dari Tegangan/Arus." },
    { "en": "Perhitungan AC Menggunakan Bilangan Apa?", "id": "Bilangan Kompleks." },
    { "en": "Apa Definisi Titik Referensi Bersama?", "id": "Simpul Yang Digunakan Sebagai Referensi Umum." },
    { "en": "Apa Itu Loop Tertutup?", "id": "Jalur Yang Dimulai Dan Diakhiri Di Simpul Sama." },
    { "en": "Kenapa Penting Mengidentifikasi Semua Simpul?", "id": "Agar Tidak Ada Persamaan Yang Terlewat." },
    { "en": "Kenapa Penting Mengidentifikasi Semua Mesh?", "id": "Agar Tidak Ada Persamaan Yang Terlewat." },
    { "en": "Apa Dampak Kesalahan Tanda Aljabar?", "id": "Jawaban Akhir Akan Salah." },
    { "en": "Bagaimana Memeriksa Kembali Persamaan?", "id": "Pastikan Setiap Elemen Diperhitungkan Dengan Benar." },
    { "en": "Analisis Nodal Fokus Pada Konservasi Apa?", "id": "Konservasi Muatan." },
    { "en": "Analisis Mesh Fokus Pada Konservasi Apa?", "id": "Konservasi Energi." },
    { "en": "Apa Itu Jaringan Listrik?", "id": "Istilah Lain Untuk Rangkaian Listrik." },
    { "en": "Apa Beda Analisis Dan Desain Rangkaian?", "id": "Analisis Menemukan Output, Desain Menemukan Rangkaian." },
    { "en": "Apakah Metode Ini Untuk Analisis Atau Desain?", "id": "Utamanya Untuk Analisis." },
    { "en": "Apa Itu Matriks Admitansi Simpul?", "id": "Matriks Koefisien Dalam Analisis Nodal." },
    { "en": "Apa Itu Matriks Impedansi Mesh?", "id": "Matriks Koefisien Dalam Analisis Mesh." },
    { "en": "Elemen Diagonal Y11 Terdiri Dari Apa?", "id": "Jumlah Semua Konduktansi Terhubung Ke Simpul 1." },
    { "en": "Elemen Non-Diagonal Y12 Terdiri Dari Apa?", "id": "Negatif Konduktansi Antara Simpul 1 Dan 2." },
    { "en": "Elemen Diagonal Z11 Terdiri Dari Apa?", "id": "Jumlah Semua Resistansi Dalam Mesh 1." },
    { "en": "Elemen Non-Diagonal Z12 Terdiri Dari Apa?", "id": "Negatif Resistansi Bersama Antara Mesh 1 Dan 2." },
    { "en": "Tanda Resistansi Bersama Tergantung Apa?", "id": "Arah Arus Mesh Yang Berdekatan." },
    { "en": "Jika Arus Mesh Searah Di Resistor Bersama?", "id": "Tanda Negatif Dalam Elemen Matriks." },
    { "en": "Jika Arus Mesh Berlawanan Di Resistor Bersama?", "id": "Tanda Negatif Dalam Elemen Matriks (Umumnya)." },
    { "en": "Kapan Tanda Elemen Z Non-Diagonal Positif?", "id": "Jika Kedua Arus Mengalir Berlawanan Arah." },
    { "en": "Ukuran Matriks Nodal Ditentukan Oleh Apa?", "id": "Jumlah Simpul Non-Referensi." },
    { "en": "Ukuran Matriks Mesh Ditentukan Oleh Apa?", "id": "Jumlah Mesh." },
    { "en": "Matriks Y Selalu Berbentuk Apa?", "id": "Matriks Persegi." },
    { "en": "Matriks Z Selalu Berbentuk Apa?", "id": "Matriks Persegi." },
    { "en": "Apa Invers Dari Matriks Impedansi?", "id": "Matriks Admitansi (Untuk Jaringan Dua Port)." },
    { "en": "Vektor Solusi Dalam Nodal Adalah?", "id": "Vektor Tegangan Simpul." },
    { "en": "Vektor Solusi Dalam Mesh Adalah?", "id": "Vektor Arus Mesh." },
    { "en": "Vektor Sumber Dalam Nodal Adalah?", "id": "Vektor Sumber Arus Ekuivalen." },
    { "en": "Vektor Sumber Dalam Mesh Adalah?", "id": "Vektor Sumber Tegangan Ekuivalen." },
    { "en": "Bagaimana Metode Inspeksi Mempercepat Analisis?", "id": "Memungkinkan Penulisan Matriks Secara Langsung." },
    { "en": "Apa Syarat Utama Metode Inspeksi?", "id": "Tidak Ada Sumber Terkendali." },
    { "en": "Bagaimana Sumber Tegangan Diubah Untuk Inspeksi Nodal?", "id": "Diubah Menjadi Sumber Arus Ekuivalen." },
    { "en": "Bagaimana Sumber Arus Diubah Untuk Inspeksi Mesh?", "id": "Tidak Bisa Diubah Secara Langsung." },
    { "en": "Apa Itu Simpul Terapung (Floating)?", "id": "Simpul Yang Tidak Terhubung Ke Referensi." },
    { "en": "Apa Yang Dibutuhkan Oleh Sumber Tegangan Terapung?", "id": "Pendekatan Supernode." },
    { "en": "Apakah Analisis Mesh Memiliki Konsep Terapung?", "id": "Tidak Ada Konsep Serupa." },
    { "en": "Bagaimana Menemukan Arus Melalui Sumber Tegangan?", "id": "Setelah Menemukan Semua Tegangan Simpul." },
    { "en": "Bagaimana Menemukan Tegangan Pada Sumber Arus?", "id": "Setelah Menemukan Semua Arus Mesh." },
    { "en": "Kesalahan Umum Saat Menulis Persamaan KCL?", "id": "Salah Menentukan Arah Arus (Masuk/Keluar)." },
    { "en": "Kesalahan Umum Saat Menulis Persamaan KVL?", "id": "Salah Menentukan Tanda Jatuh Tegangan." },
    { "en": "Apa Tanda Jatuh Tegangan Pada Resistor?", "id": "Positif Di Sisi Arus Masuk." },
    { "en": "Apa Itu Tegangan Naik (Voltage Rise)?", "id": "Peningkatan Potensial, Seperti Melewati Sumber Tegangan." },
    { "en": "Apa Itu Jatuh Tegangan (Voltage Drop)?", "id": "Penurunan Potensial, Seperti Melewati Resistor." },
    { "en": "KVL Adalah Penjumlahan Aljabar Dari Apa?", "id": "Tegangan Naik Dan Jatuh Tegangan." },
    { "en": "Dalam KVL, Jumlah Tegangan Sama Dengan?", "id": "Nol." },
    { "en": "Dalam KCL, Jumlah Arus Sama Dengan?", "id": "Nol." },
    { "en": "Apa Hubungan Nodal Dengan Thevenin?", "id": "Dapat Digunakan Untuk Menemukan Tegangan Thevenin." },
    { "en": "Apa Hubungan Mesh Dengan Norton?", "id": "Dapat Digunakan Untuk Menemukan Arus Norton." },
    { "en": "Tegangan Thevenin Adalah Tegangan Apa?", "id": "Tegangan Rangkaian Terbuka (Open-Circuit)." },
    { "en": "Arus Norton Adalah Arus Apa?", "id": "Arus Hubung Singkat (Short-Circuit)." },
    { "en": "Bisakah Mesh Digunakan Untuk Menemukan Tegangan Thevenin?", "id": "Ya, Dengan Menghitung Arus Terlebih Dahulu." },
    { "en": "Bisakah Nodal Digunakan Untuk Menemukan Arus Norton?", "id": "Ya, Dengan Menghitung Tegangan Terlebih Dahulu." },
    { "en": "Analisis Mana Yang Lebih Fleksibel?", "id": "Analisis Nodal Karena Berlaku Umum." },
    { "en": "Apa Itu Prinsip Superposisi?", "id": "Efek Total Adalah Jumlah Efek Individual." },
    { "en": "Apakah Nodal/Mesh Diperlukan Untuk Superposisi?", "id": "Ya, Untuk Menganalisis Setiap Sumber Individual." },
    { "en": "Saat Superposisi, Sumber Tegangan Lain Menjadi Apa?", "id": "Hubung Singkat (Short Circuit)." },
    { "en": "Saat Superposisi, Sumber Arus Lain Menjadi Apa?", "id": "Rangkaian Terbuka (Open Circuit)." },
    { "en": "Apakah Superposisi Berlaku Untuk Sumber Terkendali?", "id": "Tidak, Sumber Terkendali Harus Selalu Aktif." },
    { "en": "Apakah Superposisi Berlaku Untuk Daya?", "id": "Tidak, Karena Daya Bukan Kuantitas Linear." },
    { "en": "Apa Keuntungan Menggunakan Nodal/Mesh?", "id": "Pendekatan Sistematis Dan Terorganisir." },
    { "en": "Apa Kerugian Menggunakan Nodal/Mesh?", "id": "Membutuhkan Penyelesaian Sistem Persamaan Besar." },
    { "en": "Untuk Rangkaian Sederhana, Metode Apa Yang Cepat?", "id": "Pembagian Tegangan Atau Arus." },
    { "en": "Aturan Pembagi Tegangan Untuk Komponen Apa?", "id": "Resistor Dalam Konfigurasi Seri." },
    { "en": "Aturan Pembagi Arus Untuk Komponen Apa?", "id": "Resistor Dalam Konfigurasi Paralel." },
    { "en": "Apakah Analisis Nodal Sama Dengan Analisis Simpul?", "id": "Ya, Keduanya Istilah Yang Sama." },
    { "en": "Apakah Analisis Mesh Sama Dengan Analisis Loop?", "id": "Mesh Adalah Kasus Khusus Analisis Loop." },
    { "en": "Apa Itu 'Ground Chassis'?", "id": "Referensi Yang Terhubung Ke Rangka Perangkat." },
    { "en": "Apa Itu 'Earth Ground'?", "id": "Referensi Yang Terhubung Fisik Ke Bumi." },
    { "en": "Tegangan Simpul Diukur Relatif Terhadap Apa?", "id": "Simpul Referensi Atau Ground." },
    { "en": "Berapa Banyak Cara Menulis KVL Untuk Loop?", "id": "Dua, Tergantung Titik Awal Dan Arah." },
    { "en": "Apa Arti Tanda Panah Pada Sumber Arus?", "id": "Menunjukkan Arah Aliran Arus Konvensional." },
    { "en": "Apa Arti Tanda +/- Pada Sumber Tegangan?", "id": "Menunjukkan Polaritas Tegangan." },
    { "en": "Arus Konvensional Mengalir Dari Mana?", "id": "Potensial Tinggi Ke Rendah." },
    { "en": "Aliran Elektron Mengalir Dari Mana?", "id": "Potensial Rendah Ke Tinggi." },
    { "en": "Analisis Rangkaian Menggunakan Arus Apa?", "id": "Arus Konvensional." },
    { "en": "Apa Arti 'Floating Voltage Source'?", "id": "Sumber Tegangan Tanpa Koneksi Ke Referensi." },
    { "en": "Bagaimana Menangani 'Floating Voltage Source'?", "id": "Harus Menggunakan Pendekatan Supernode." },
    { "en": "Apakah Kawat Ideal Memiliki Resistansi?", "id": "Tidak, Resistansinya Dianggap Nol." },
    { "en": "Apa Artinya Tegangan Di Sepanjang Kawat Ideal?", "id": "Tegangan Sama Di Semua Titik Kawat." },
    { "en": "Sebuah Simpul Sebenarnya Merepresentasikan Apa?", "id": "Seluruh Kawat Penghubung." },
    { "en": "Jika Tiga Mesh Bertemu Di Satu Resistor?", "id": "Itu Tidak Mungkin Dalam Rangkaian Planar." },
    { "en": "Berapa Maksimal Mesh Berbagi Satu Resistor?", "id": "Dua Mesh." },
    { "en": "Jika N < M, Metode Apa Yang Lebih Baik?", "id": "Analisis Nodal (N: Simpul, M: Mesh)." },
    { "en": "Jika M < N, Metode Apa Yang Lebih Baik?", "id": "Analisis Mesh (M: Mesh, N: Simpul)." },
    { "en": "Bagaimana Menemukan Daya Yang Diserap Resistor?", "id": "Gunakan V*I, I^2*R, Atau V^2/R." },
    { "en": "Tanda Daya Yang Diserap Selalu Apa?", "id": "Selalu Positif." },
    { "en": "Apa Itu Konvensi Tanda Pasif?", "id": "Arus Masuk Terminal Positif Elemen Pasif." },
    { "en": "Apakah Analisis Ini Bisa Untuk Rangkaian DC?", "id": "Ya, Ini Adalah Aplikasi Utamanya." },
    { "en": "Apa Karakteristik Rangkaian DC?", "id": "Tegangan Dan Arus Konstan Seiring Waktu." },
    { "en": "Perangkat Lunak Apa Yang Menggunakan Analisis Ini?", "id": "SPICE (Simulation Program with Integrated Circuit Emphasis)." },
    { "en": "Apa Algoritma Dasar Di Balik SPICE?", "id": "Analisis Nodal Yang Dimodifikasi." },
    { "en": "Mengapa SPICE Memilih Nodal?", "id": "Karena Sangat Umum Dan Mudah Diimplementasikan." },
    { "en": "Apa Itu Persamaan Karakteristik Elemen?", "id": "Hubungan V-I Untuk Setiap Komponen." },
    { "en": "Contoh Persamaan Karakteristik?", "id": "V = IR Untuk Resistor." },
    { "en": "Nodal Menggabungkan Hukum Ohm Dan?", "id": "KCL." },
    { "en": "Mesh Menggabungkan Hukum Ohm Dan?", "id": "KVL." },
    { "en": "Apa Itu Rangkaian Ekuivalen?", "id": "Rangkaian Dengan Perilaku Terminal Sama." },
    { "en": "Apakah Nodal/Mesh Memberi Rangkaian Ekuivalen?", "id": "Tidak, Mereka Menganalisis Rangkaian Asli." },
    { "en": "Berapa Arus Yang Mengalir Dalam Rangkaian Terbuka?", "id": "Nol Ampere." },
    { "en": "Berapa Tegangan Pada Hubung Singkat Ideal?", "id": "Nol Volt." },
    { "en": "Apa Itu 'Knot' Dalam Teori Rangkaian?", "id": "Istilah Lama Untuk Simpul (Node)." },
    { "en": "Apa Itu 'Twigs' Dalam Teori Graph?", "id": "Cabang Dari Pohon Rentang." },
    { "en": "Apa Itu 'Links' Dalam Teori Graph?", "id": "Cabang Yang Bukan Bagian Pohon Rentang." },
    { "en": "Jumlah Persamaan KVL Independen Sama Dengan?", "id": "Jumlah Links." },
    { "en": "Jumlah Persamaan KCL Independen Sama Dengan?", "id": "Jumlah Simpul Dikurangi Satu." },
    { "en": "Apa Definisi Formal Dari Simpul?", "id": "Titik Terminal Dari Dua Atau Lebih Cabang." },
    { "en": "Apa Definisi Formal Dari Cabang?", "id": "Mewakili Satu Elemen Seperti Resistor." },
    { "en": "Apa Definisi Formal Dari Loop?", "id": "Setiap Jalur Tertutup Dalam Sebuah Rangkaian." },
    { "en": "Berapa Persamaan Simpul Untuk N Simpul?", "id": "N Dikurangi Satu Persamaan." },
    { "en": "Berapa Persamaan Mesh Untuk B Cabang Dan N Simpul?", "id": "B Dikurangi N Ditambah Satu Persamaan." },
    { "en": "Rumus B-N+1 Berlaku Untuk Rangkaian Apa?", "id": "Rangkaian Planar." },
    { "en": "Apa Langkah Pertama Sebelum Analisis?", "id": "Gambarkan Dan Beri Label Rangkaian Dengan Jelas." },
    { "en": "Label Apa Yang Diperlukan Untuk Analisis Nodal?", "id": "Label Tegangan Untuk Setiap Simpul." },
    { "en": "Label Apa Yang Diperlukan Untuk Analisis Mesh?", "id": "Label Arus Untuk Setiap Mesh." },
    { "en": "Bagaimana Cara Mengatasi Sumber Arus Dalam Mesh?", "id": "Gunakan Pendekatan Supermesh." },
    { "en": "Bagaimana Cara Mengatasi Sumber Tegangan Dalam Nodal?", "id": "Gunakan Pendekatan Supernode." },
    { "en": "Apa Yang Diwakili Oleh V_ab?", "id": "Tegangan Di Titik A Relatif Terhadap B." },
    { "en": "V_ab Sama Dengan Apa?", "id": "V_a Dikurangi V_b." },
    { "en": "Jika V_ab Positif, Titik Mana Lebih Tinggi?", "id": "Titik A Memiliki Potensial Lebih Tinggi." },
    { "en": "Asumsi Arah Arus Di Nodal Pengaruhnya Apa?", "id": "Tanda Dalam Persamaan KCL." },
    { "en": "Asumsi Arah Loop Di Mesh Pengaruhnya Apa?", "id": "Tanda Dalam Persamaan KVL." },
    { "en": "Jika Semua Arus Diasumsikan Keluar Simpul?", "id": "Jumlah Semua Arus Sama Dengan Nol." },
    { "en": "Apa Itu 'Modified Nodal Analysis'?", "id": "Variasi Yang Menangani Sumber Tegangan Secara Langsung." },
    { "en": "Modified Nodal Analysis Menambahkan Variabel Apa?", "id": "Arus Yang Mengalir Melalui Sumber Tegangan." },
    { "en": "Apa Keuntungan Modified Nodal Analysis?", "id": "Lebih Umum Dan Kuat Untuk Simulasi." },
    { "en": "Apakah Mesh Bisa Digunakan Dalam 3D?", "id": "Tidak, Mesh Adalah Konsep 2D." },
    { "en": "Apakah Nodal Bisa Digunakan Dalam 3D?", "id": "Ya, Simpul Dan KCL Adalah Konsep Umum." },
    { "en": "Apa Hubungan Antara Analisis Rangkaian Dan Aljabar Linear?", "id": "Analisis Rangkaian Menghasilkan Sistem Persamaan Linear." },
    { "en": "Pentingkah Memeriksa Ulang Jumlah Persamaan?", "id": "Ya, Untuk Memastikan Sistem Dapat Diselesaikan." },
    { "en": "Jumlah Persamaan Harus Sama Dengan Apa?", "id": "Jumlah Variabel Yang Tidak Diketahui." },
    { "en": "Bagaimana Mengonversi Sumber Tegangan Ke Arus?", "id": "I_s = V_s / R_s." },
    { "en": "Resistor Dalam Transformasi Sumber Ditempatkan Di Mana?", "id": "Paralel Dengan Sumber Arus Baru." },
    { "en": "Bagaimana Mengonversi Sumber Arus Ke Tegangan?", "id": "V_s = I_s * R_s." },
    { "en": "Resistor Dalam Transformasi Sumber Ditempatkan Di Mana?", "id": "Seri Dengan Sumber Tegangan Baru." },
    { "en": "Bisakah Sumber Ideal Ditransformasi?", "id": "Tidak, Harus Ada Resistor Internal." },
    { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Sumber Tanpa Resistansi Internal." },
    { "en": "Apa Itu Sumber Arus Ideal?", "id": "Sumber Dengan Resistansi Internal Tak Terhingga." },
    { "en": "Apakah Transformasi Sumber Mengubah Rangkaian Internal?", "id": "Ya, Tetapi Perilaku Terminalnya Sama." },
    { "en": "Apakah Analisis Nodal/Mesh Perlu Transformasi Sumber?", "id": "Tidak Perlu, Tapi Bisa Menyederhanakan." },
    { "en": "Metode Mana Yang Lebih Terpengaruh Oleh Topologi?", "id": "Mesh, Karena Terbatas Pada Rangkaian Planar." },
    { "en": "Apa Itu 'Cut Set'?", "id": "Kumpulan Cabang Yang Memisahkan Graph." },
    { "en": "Analisis Cut Set Adalah Generalisasi Dari Apa?", "id": "Analisis Nodal." },
    { "en": "Analisis Tie Set Adalah Generalisasi Dari Apa?", "id": "Analisis Mesh." },
    { "en": "Hukum Apa Yang Digunakan Analisis Cut Set?", "id": "Hukum Arus Kirchhoff." },
    { "en": "Hukum Apa Yang Digunakan Analisis Tie Set?", "id": "Hukum Tegangan Kirchhoff." },
    { "en": "Apakah Jawaban Akhir Bergantung Pada Metode?", "id": "Tidak, Jawaban Fisik Harus Sama." },
    { "en": "Apa Yang Harus Dilakukan Jika Hasil Berbeda?", "id": "Periksa Kembali Semua Langkah Perhitungan." },
    { "en": "Mana Yang Lebih Fundamental, KCL Atau KVL?", "id": "Keduanya Sama-Sama Fundamental." },
    { "en": "KCL Berasal Dari Persamaan Maxwell Apa?", "id": "Persamaan Kontinuitas Muatan." },
    { "en": "KVL Berasal Dari Persamaan Maxwell Apa?", "id": "Hukum Induksi Faraday." },
    { "en": "Apakah Analisis Ini Berlaku Pada Frekuensi Sangat Tinggi?", "id": "Tidak, Efek Garis Transmisi Menjadi Penting." },
    { "en": "Pada Frekuensi Rendah, Rangkaian Disebut Apa?", "id": "Rangkaian Elemen Ter lumped (Lumped-Element Circuit)." },
    { "en": "Analisis Nodal/Mesh Untuk Rangkaian Apa?", "id": "Rangkaian Elemen Ter lumped." },
    { "en": "Apa Asumsi Utama Rangkaian Elemen Ter lumped?", "id": "Efek Terjadi Seketika Di Seluruh Rangkaian." },
    { "en": "Pentingkah Memahami Dasar Teori Sebelum Menggunakan Software?", "id": "Sangat Penting Untuk Menginterpretasi Hasil." },
    { "en": "Jika Sebuah Resistor Paralel Dengan Sumber Tegangan?", "id": "Tegangan Pada Resistor Tersebut Sudah Diketahui." },
    { "en": "Jika Sebuah Resistor Seri Dengan Sumber Arus?", "id": "Arus Melalui Resistor Tersebut Sudah Diketahui." },
    { "en": "Dapatkah Analisis Ini Menemukan Respon Transien?", "id": "Ya, Dengan Menyelesaikan Persamaan Diferensial." },
    { "en": "Apa Yang Terjadi Pada Kapasitor Di DC Steady State?", "id": "Menjadi Rangkaian Terbuka." },
    { "en": "Apa Yang Terjadi Pada Induktor Di DC Steady State?", "id": "Menjadi Hubung Singkat." },
    { "en": "Analisis DC Steady State Dilakukan Kapan?", "id": "Setelah Semua Transien Mereda." },
    { "en": "Apa Itu 'Degree' Dari Sebuah Simpul?", "id": "Jumlah Cabang Yang Terhubung Ke Simpul." },
    { "en": "Bagaimana Menangani Sumber Tegangan Antara Simpul Dan Ground?", "id": "Itu Menetapkan Tegangan Simpul Secara Langsung." },
    { "en": "Bagaimana Menangani Sumber Arus Paralel?", "id": "Jumlahkan Secara Aljabar Menjadi Satu Sumber." },
    { "en": "Bagaimana Menangani Sumber Tegangan Seri?", "id": "Jumlahkan Secara Aljabar Menjadi Satu Sumber." },
    { "en": "Apakah Mungkin Memiliki Tegangan Tanpa Arus?", "id": "Ya, Pada Rangkaian Terbuka." },
    { "en": "Apakah Mungkin Memiliki Arus Tanpa Tegangan?", "id": "Ya, Pada Hubung Singkat Ideal." },
    { "en": "Tujuan Mengasumsikan Arah Arus Adalah Untuk?", "id": "Menetapkan Referensi Untuk Jatuh Tegangan." },
    { "en": "Apa Itu Dualitas Dalam Rangkaian?", "id": "Hubungan Antara Pasangan Konsep (Nodal/Mesh)." },
    { "en": "Apa Dual Dari Tegangan?", "id": "Arus." },
    { "en": "Apa Dual Dari Simpul?", "id": "Loop." },
    { "en": "Apa Dual Dari Resistansi?", "id": "Konduktansi." },
    { "en": "Apa Dual Dari KCL?", "id": "KVL." },
    { "en": "Apa Dual Dari Rangkaian Seri?", "id": "Rangkaian Paralel." },
    { "en": "Apa Dual Dari Hubung Singkat?", "id": "Rangkaian Terbuka." },
    { "en": "Apa Dual Dari Kapasitansi?", "id": "Induktansi." },
    { "en": "Memahami Dualitas Membantu Dalam Hal Apa?", "id": "Menerapkan Konsep Serupa Pada Situasi Berbeda." },
    { "en": "Apakah Analisis Nodal Dan Mesh Dual Satu Sama Lain?", "id": "Ya, Mereka Adalah Metode Dual." },
    { "en": "Apa Langkah Paling Rawan Kesalahan?", "id": "Menyiapkan Sistem Persamaan Awal Dengan Benar." },
    { "en": "Bagaimana Cara Berlatih Analisis Rangkaian?", "id": "Dengan Mengerjakan Banyak Masalah Latihan." },
    { "en": "Apakah Ada Batas Jumlah Simpul Atau Mesh?", "id": "Secara Teoretis Tidak, Praktis Dibatasi Komputasi." },
    { "en": "Apa Itu 'Ground Loop'?", "id": "Kondisi Dengan Beberapa Jalur Ke Ground." },
    { "en": "Apakah 'Ground Loop' Mempengaruhi Analisis Ideal?", "id": "Tidak, Tapi Penting Dalam Rangkaian Nyata." },
    { "en": "Dalam Nodal, Setiap Persamaan Adalah Jumlah Dari Apa?", "id": "Arus." },
    { "en": "Dalam Mesh, Setiap Persamaan Adalah Jumlah Dari Apa?", "id": "Tegangan." },
    { "en": "Bagaimana Tegangan Diukur Dalam Praktik?", "id": "Menggunakan Voltmeter Secara Paralel." },
    { "en": "Bagaimana Arus Diukur Dalam Praktik?", "id": "Menggunakan Ammeter Secara Seri." },
    { "en": "Apakah Voltmeter Ideal Mempengaruhi Rangkaian?", "id": "Tidak, Resistansi Internalnya Tak Terhingga." },
    { "en": "Apakah Ammeter Ideal Mempengaruhi Rangkaian?", "id": "Tidak, Resistansi Internalnya Nol." },
    { "en": "Apa Itu 'Loading Effect'?", "id": "Pengaruh Alat Ukur Terhadap Rangkaian." },
    { "en": "Analisis Nodal Dan Mesh Mengasumsikan Alat Ukur Apa?", "id": "Alat Ukur Ideal." },
    { "en": "Apa Perbedaan Antara Rangkaian Dan Jaringan?", "id": "Seringkali Digunakan Secara Bergantian." },
    { "en": "Apa Itu 'Port' Dalam Rangkaian?", "id": "Sepasang Terminal Untuk Input Atau Output." },
    { "en": "Analisis Nodal/Mesh Fokus Pada Perilaku Apa?", "id": "Perilaku Internal Rangkaian." },
    { "en": "Apa Yang Dibutuhkan Untuk Menganalisis Transistor?", "id": "Model Rangkaian Ekuivalen Transistor." },
    { "en": "Model Transistor Seringkali Mengandung Apa?", "id": "Sumber Arus Terkendali." },
    { "en": "Metode Apa Yang Lebih Sering Untuk Rangkaian Transistor?", "id": "Analisis Nodal." },
    { "en": "Apa Yang Terjadi Jika Sumber Arus Paralel?", "id": "Dapat Dijumlahkan Secara Aljabar." },
    { "en": "Apa Yang Terjadi Jika Sumber Tegangan Seri?", "id": "Dapat Dijumlahkan Secara Aljabar." },
    { "en": "Bisakah Sumber Tegangan Paralel?", "id": "Hanya Jika Tegangannya Sama Persis." },
    { "en": "Apa Yang Terjadi Jika Sumber Tegangan Berbeda Paralel?", "id": "Secara Teoretis Menciptakan Arus Tak Terhingga." },
    { "en": "Bisakah Sumber Arus Seri?", "id": "Hanya Jika Arusnya Sama Persis." },
    { "en": "Apa Yang Terjadi Jika Sumber Arus Berbeda Seri?", "id": "Melanggar KCL, Situasi Tidak Mungkin." },
    { "en": "Apa Itu Titik Tunggal Dalam Rangkaian?", "id": "Simpul Yang Tidak Terhubung Ke Mana Pun." },
    { "en": "Apakah Analisis Rangkaian Selalu Memiliki Solusi?", "id": "Hampir Selalu, Kecuali Rangkaian Tidak Konsisten." },
    { "en": "Apa Itu Persamaan Redundan?", "id": "Persamaan Yang Tidak Memberikan Informasi Baru." },
    { "en": "Bagaimana Menghindari Persamaan Redundan Di Nodal?", "id": "Jangan Menulis Persamaan KCL Untuk Simpul Referensi." },
    { "en": "Bagaimana Menghindari Persamaan Redundan Di Mesh?", "id": "Pastikan Setiap Loop Adalah Mesh." },
    { "en": "Kapan Analisis Rangkaian Menjadi Sangat Rumit?", "id": "Dengan Banyak Sumber Terkendali." },
    { "en": "Apa Langkah Verifikasi Sederhana?", "id": "Periksa Arah Arus Dan Polaritas Tegangan." },
    { "en": "Apa Itu 'Inspeksi Cepat'?", "id": "Memeriksa Kasus Ekstrem (Resistor Nol/Tak Hingga)." },
    { "en": "Apa Itu 'Pohon' Dalam Teori Graph?", "id": "Subgraph Yang Menghubungkan Semua Simpul Tanpa Loop." },
    { "en": "Apa Hubungan 'Pohon' Dengan Analisis Nodal?", "id": "Tegangan Cabang Pohon Menentukan Semua Tegangan." },
    { "en": "Apa Itu 'Co-tree'?", "id": "Kumpulan Cabang Yang Tidak Ada Di Pohon." },
    { "en": "Apa Hubungan 'Co-tree' Dengan Analisis Mesh?", "id": "Setiap Cabang Co-tree Menciptakan Loop Fundamental." },
    { "en": "Apa Itu Loop Fundamental?", "id": "Loop Yang Terdiri Dari Satu Link Co-tree." },
    { "en": "Analisis Mesh Pada Dasarnya Adalah Analisis Apa?", "id": "Analisis Berbasis Loop Fundamental." },
    { "en": "Mana Yang Lebih Mudah Dipilih, Simpul Atau Mesh?", "id": "Simpul Lebih Mudah Diidentifikasi Secara Visual." },
    { "en": "Apa Jebakan Dalam Mengidentifikasi Mesh?", "id": "Menganggap Loop Luar Sebagai Mesh." },
    { "en": "Apa Itu Loop Luar?", "id": "Loop Yang Mengelilingi Seluruh Rangkaian." },
    { "en": "Apakah Loop Luar Adalah Sebuah Mesh?", "id": "Tidak, Karena Mengandung Loop Lain Di Dalamnya." },
    { "en": "Apakah Perlu Menulis Persamaan Untuk Arus Diketahui?", "id": "Tidak, Nilainya Langsung Digunakan." },
    { "en": "Apakah Perlu Menulis Persamaan Untuk Tegangan Diketahui?", "id": "Tidak, Nilainya Langsung Digunakan." },
    { "en": "Simpul Dengan Sumber Tegangan Ke Ground Artinya Apa?", "id": "Tegangan Simpul Tersebut Sudah Diketahui." },
    { "en": "Mesh Dengan Sumber Arus Di Tepi Artinya Apa?", "id": "Arus Mesh Tersebut Sudah Diketahui." },
    { "en": "Apa Intuisi Di Balik KCL?", "id": "Muatan Tidak Dapat Terakumulasi Di Sebuah Titik." },
    { "en": "Apa Intuisi Di Balik KVL?", "id": "Energi Potensial Listrik Bersifat Konservatif." },
    { "en": "Bagaimana Memodelkan Saklar Terbuka?", "id": "Sebagai Resistansi Tak Terhingga." },
    { "en": "Bagaimana Memodelkan Saklar Tertutup?", "id": "Sebagai Resistansi Nol (Kawat)." },
    { "en": "Apa Yang Terjadi Saat Saklar Berubah Posisi?", "id": "Menciptakan Rangkaian Baru Yang Perlu Dianalisis." },
    { "en": "Apa Arti Tanda Negatif Pada Jawaban Tegangan?", "id": "Potensial Sebenarnya Lebih Rendah Dari Referensi." },
    { "en": "Apa Arti Tanda Negatif Pada Jawaban Arus?", "id": "Arah Aliran Sebenarnya Berlawanan Dari Asumsi." },
    { "en": "Bagaimana Menghitung Arus Di Resistor Bersama?", "id": "Selisih Aljabar Dari Dua Arus Mesh." },
    { "en": "Arah Arus Resistor Bersama Ditentukan Oleh Apa?", "id": "Arus Mesh Yang Lebih Besar." },
    { "en": "Bagaimana Memeriksa Jawaban Dengan KVL?", "id": "Jumlahkan Tegangan Di Sekitar Loop Apapun." },
    { "en": "Bagaimana Memeriksa Jawaban Dengan KCL?", "id": "Jumlahkan Arus Di Simpul Apapun." },
    { "en": "Apa Yang Terjadi Jika Persamaan Terlalu Sedikit?", "id": "Sistem Memiliki Solusi Tak Terhingga." },
    { "en": "Apa Yang Terjadi Jika Persamaan Terlalu Banyak?", "id": "Sistem Mungkin Tidak Memiliki Solusi." },
    { "en": "Apa Peran Utama Analisis Rangkaian Dalam Teknik Elektro?", "id": "Fondasi Untuk Semua Bidang Lainnya." },
    { "en": "Mengapa Penting Untuk Rapi Dalam Bekerja?", "id": "Mengurangi Kesalahan Aljabar Dan Konseptual." },
    { "en": "Apakah Mungkin Ada Simpul Dengan Satu Cabang?", "id": "Tidak, Itu Hanya Bagian Dari Cabang." },
    { "en": "Apa Langkah Awal Untuk Rangkaian Kompleks?", "id": "Gambar Ulang Agar Lebih Jelas Dan Rapi." },
    { "en": "Bagaimana Menggambar Ulang Rangkaian?", "id": "Jaga Konektivitas Antar Simpul Tetap Sama." },
    { "en": "Apa Itu 'Ground Semu' (Virtual Ground)?", "id": "Simpul Yang Stabil Di Nol Volt Tapi Tidak Terhubung Ground." },
    { "en": "Di Mana 'Virtual Ground' Ditemukan?", "id": "Dalam Rangkaian Op-Amp Ideal." },
    { "en": "Apakah Analisis Nodal Berguna Untuk Op-Amp?", "id": "Sangat Berguna, Terutama Di Simpul Input." },
    { "en": "Berapa Arus Yang Masuk Ke Input Op-Amp Ideal?", "id": "Nol Ampere." },
    { "en": "Aturan Emas Op-Amp Apa Yang Terkait Nodal?", "id": "Tegangan Di Kedua Input Sama." },
    { "en": "Apa Itu Jaringan Resistor Tangga (Ladder Network)?", "id": "Rangkaian Dengan Struktur Berulang Seri-Paralel." },
    { "en": "Metode Apa Yang Efisien Untuk Jaringan Tangga?", "id": "Analisis Dari Ujung Ke Awal." },
    { "en": "Bisakah Nodal/Mesh Digunakan Untuk Jaringan Tangga?", "id": "Ya, Tapi Mungkin Bukan Cara Tercepat." },
    { "en": "Apa Itu Simetri Dalam Rangkaian?", "id": "Ketika Bagian Rangkaian Adalah Cerminan." },
    { "en": "Bagaimana Simetri Bisa Menyederhanakan Analisis?", "id": "Tegangan Atau Arus Di Titik Simetris Sama." },
    { "en": "Apa Itu Transformasi Delta-Wye (Pi-Tee)?", "id": "Metode Untuk Mengubah Konfigurasi Tiga Resistor." },
    { "en": "Kapan Transformasi Delta-Wye Berguna?", "id": "Ketika Ada Jembatan Yang Tidak Seimbang." },
    { "en": "Apakah Nodal/Mesh Memerlukan Transformasi Delta-Wye?", "id": "Tidak, Mereka Bisa Menganalisis Jembatan Langsung." },
    { "en": "Transformasi Delta-Wye Adalah Alternatif Untuk Apa?", "id": "Analisis Nodal Atau Mesh." },
    { "en": "Apa Pilihan Variabel Dasar Di Nodal?", "id": "Tegangan Simpul." },
    { "en": "Apa Pilihan Variabel Dasar Di Mesh?", "id": "Arus Mesh." },
    { "en": "Apa Arti 'Menganalisis Sepenuhnya' Sebuah Rangkaian?", "id": "Menemukan Semua Tegangan Dan Arus Cabang." },
    { "en": "Langkah Mana Yang Paling Konseptual?", "id": "Menyiapkan Persamaan Dengan Benar." },
    { "en": "Langkah Mana Yang Paling Matematis?", "id": "Menyelesaikan Sistem Persamaan." },
    { "en": "Apa Ciri Rangkaian Pasif?", "id": "Tidak Mengandung Sumber Energi." },
    { "en": "Apakah Rangkaian Pasif Bisa Dianalisis?", "id": "Ya, Jika Ada Input Eksternal." },
    { "en": "Apa Perbedaan Simpul Dan Simpul Esensial?", "id": "Simpul Esensial Menghubungkan Tiga Atau Lebih Elemen." },
    { "en": "Persamaan Nodal Hanya Perlu Ditulis Untuk Simpul Apa?", "id": "Simpul Esensial." },
    { "en": "Mengapa Simpul Non-Esensial Bisa Diabaikan?", "id": "Hanya Menghubungkan Dua Elemen Secara Seri." },
    { "en": "Apa Itu 'Self-Conductance'?", "id": "Istilah Lain Untuk Elemen Diagonal Matriks Y." },
    { "en": "Apa Itu 'Mutual-Conductance'?", "id": "Istilah Lain Untuk Elemen Non-Diagonal Matriks Y." },
    { "en": "Apa Itu 'Self-Resistance'?", "id": "Istilah Lain Untuk Elemen Diagonal Matriks Z." },
    { "en": "Apa Itu 'Mutual-Resistance'?", "id": "Istilah Lain Untuk Elemen Non-Diagonal Matriks Z." },
    { "en": "Tanda Elemen Matriks Y Non-Diagonal Selalu Apa?", "id": "Selalu Negatif (Untuk Rangkaian Pasif)." },
    { "en": "Tanda Elemen Matriks Y Diagonal Selalu Apa?", "id": "Selalu Positif." },
    { "en": "Tanda Elemen Matriks Z Diagonal Selalu Apa?", "id": "Selalu Positif." },
    { "en": "Apakah Perlu Menghafal Rumus Matriks?", "id": "Tidak, Lebih Baik Memahami Proses Penurunannya." },
    { "en": "Di Mana Tegangan Simpul Sebenarnya Ada?", "id": "Antara Simpul Tersebut Dan Simpul Referensi." },
    { "en": "Di Mana Arus Mesh Sebenarnya Mengalir?", "id": "Itu Adalah Konstruk Matematis, Bukan Arus Fisik." },
    { "en": "Arus Cabang Adalah Arus Apa?", "id": "Arus Fisik Aktual Yang Mengalir." },
    { "en": "Tegangan Elemen Adalah Tegangan Apa?", "id": "Beda Potensial Aktual Di Antara Terminalnya." },
    { "en": "Apa Hubungan Tegangan Elemen Dan Tegangan Simpul?", "id": "Tegangan Elemen Adalah Selisih Tegangan Simpul." },
    { "en": "Apakah Hasil Analisis Bergantung Pada Posisi Komponen?", "id": "Tidak, Hanya Pada Bagaimana Mereka Terhubung." },
    { "en": "Apa Yang Disebut 'Skema Rangkaian'?", "id": "Diagram Grafis Dari Rangkaian Listrik." },
    { "en": "Bagaimana Jika Rangkaian Memiliki Banyak Bagian Terputus?", "id": "Analisis Setiap Bagian Secara Terpisah." },
    { "en": "Apa Pentingnya Latihan Teratur?", "id": "Membangun Kecepatan Dan Akurasi." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Alat Untuk Membuat Prototipe Rangkaian Sementara." },
    { "en": "Bisakah Hasil Analisis Diverifikasi Di Breadboard?", "id": "Ya, Menggunakan Multimeter." },
    { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Serbaguna." },
    { "en": "Fungsi Dasar Multimeter?", "id": "Mengukur Tegangan, Arus, Dan Resistansi." },
    { "en": "Bagaimana Jika Hasil Praktik Dan Teori Berbeda?", "id": "Periksa Toleransi Komponen Dan Kesalahan Pengukuran." },
    { "en": "Apa Itu Kondisi Awal Dalam Rangkaian?", "id": "Tegangan Dan Arus Saat T=0." },
    { "en": "Apakah Analisis Ini Menghitung Kondisi Awal?", "id": "Tidak, Ini Untuk Analisis Keadaan Tunak." },
    { "en": "Apa Itu Keadaan Tunak (Steady State)?", "id": "Kondisi Setelah Semua Efek Transien Hilang." },
    { "en": "Keadaan Tunak DC Terjadi Di Rangkaian Apa?", "id": "Rangkaian Dengan Sumber DC." },
    { "en": "Keadaan Tunak Sinusoidal Terjadi Di Rangkaian Apa?", "id": "Rangkaian Dengan Sumber AC Sinusoidal." },
    { "en": "Analisis Nodal/Mesh Untuk Keadaan Tunak Apa?", "id": "Keduanya, DC Dan AC." },
    { "en": "Apa Perbedaan Utama Analisis DC Dan AC?", "id": "Penggunaan Bilangan Kompleks Dalam Analisis AC." },
    { "en": "Apa Itu Fasor Tegangan?", "id": "Representasi Kompleks Dari Tegangan Sinusoidal." },
    { "en": "Apa Itu Fasor Arus?", "id": "Representasi Kompleks Dari Arus Sinusoidal." },
    { "en": "Apakah KCL/KVL Berlaku Untuk Fasor?", "id": "Ya, Dalam Bentuk Penjumlahan Vektor Kompleks." },
    { "en": "Apa Yang Menggantikan Resistansi R Di Rangkaian AC?", "id": "Impedansi Z." },
    { "en": "Apa Impedansi Resistor?", "id": "R (Hanya Bagian Riil)." },
    { "en": "Apa Impedansi Induktor Ideal?", "id": "jωL (Bagian Imajiner Positif)." },
    { "en": "Apa Impedansi Kapasitor Ideal?", "id": "-j/ωC (Bagian Imajiner Negatif)." },
    { "en": "Apa Itu 'j' Dalam Bilangan Kompleks?", "id": "Akar Kuadrat Dari Negatif Satu." },
    { "en": "Apa Itu ω (Omega)?", "id": "Frekuensi Sudut Sumber AC (2πf)." },
    { "en": "Analisis Mesh Dalam AC Menggunakan Matriks Apa?", "id": "Matriks Impedansi Kompleks." },
    { "en": "Analisis Nodal Dalam AC Menggunakan Matriks Apa?", "id": "Matriks Admitansi Kompleks." },
    { "en": "Apa Admitansi Resistor?", "id": "G Atau 1/R." },
    { "en": "Apa Admitansi Induktor?", "id": "-j/ωL." },
    { "en": "Apa Admitansi Kapasitor?", "id": "jωC." },
    { "en": "Solusi Dari Analisis AC Memberikan Informasi Apa?", "id": "Amplitudo Dan Fasa Dari Setiap Variabel." },
    { "en": "Apa Itu Diagram Fasor?", "id": "Representasi Grafis Dari Fasor Dalam Bidang Kompleks." },
    { "en": "Bagaimana Mendapatkan Respon Waktu Dari Fasor?", "id": "Ubah Kembali Ke Bentuk Sinusoidal." },
    { "en": "Apa Urutan Langkah Analisis AC?", "id": "Domain Waktu -> Domain Frekuensi -> Analisis -> Domain Waktu." },
    { "en": "Apa Itu Domain Frekuensi?", "id": "Representasi Rangkaian Menggunakan Fasor Dan Impedansi." },
    { "en": "Apa Itu Domain Waktu?", "id": "Representasi Rangkaian Menggunakan Fungsi Waktu." },
    { "en": "Mengapa Domain Frekuensi Digunakan?", "id": "Mengubah Persamaan Diferensial Menjadi Persamaan Aljabar." },
    { "en": "Apakah Pilihan Simpul Referensi Mempengaruhi Fasa?", "id": "Ya, Karena Fasa Adalah Relatif." },
    { "en": "Apa Itu Sirkuit Resonansi?", "id": "Rangkaian RLC Di Mana Reaktansi Saling Menghilangkan." },
    { "en": "Pada Resonansi, Impedansi Total Menjadi Apa?", "id": "Murni Resistif (Minimum Atau Maksimum)." },
    { "en": "Bisakah Nodal/Mesh Menganalisis Resonansi?", "id": "Ya, Mereka Dapat Menemukan Kondisi Resonansi." },
    { "en": "Apa Itu Filter Pasif?", "id": "Rangkaian RLC Yang Melewatkan Frekuensi Tertentu." },
    { "en": "Bagaimana Menganalisis Filter?", "id": "Gunakan Analisis AC Pada Berbagai Frekuensi." },
    { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Fasor Output Terhadap Fasor Input." },
    { "en": "Nodal/Mesh Dapat Digunakan Untuk Menemukan Apa?", "id": "Fungsi Transfer Rangkaian." },
    { "en": "Apa Itu 'Common-Drain Amplifier'?", "id": "Konfigurasi Penguat Transistor." },
    { "en": "Metode Apa Yang Digunakan Untuk Menganalisisnya?", "id": "Analisis Nodal Pada Model Rangkaian Ekuivalen." },
    { "en": "Apa Itu Jaringan Dua Gerbang (Two-Port Network)?", "id": "Rangkaian Dengan Satu Pasang Terminal Input Dan Output." },
    { "en": "Bagaimana Jaringan Dua Gerbang Dikarakterisasi?", "id": "Menggunakan Parameter Z, Y, H, Atau T." },
    { "en": "Bagaimana Parameter Y (Admitansi) Diukur?", "id": "Dengan Menghubung Singkat Salah Satu Gerbang." },
    { "en": "Bagaimana Parameter Z (Impedansi) Diukur?", "id": "Dengan Membuka Rangkaian Salah Satu Gerbang." },
    { "en": "Apakah Ada Hubungan Antara Parameter Ini?", "id": "Ya, Mereka Dapat Dikonversi Satu Sama Lain." },
    { "en": "Apa Itu Rangkaian Resiprokal?", "id": "Sifat Input Dan Outputnya Dapat Ditukar." },
    { "en": "Kondisi Resiprokal Untuk Parameter Z?", "id": "Z12 Sama Dengan Z21." },
    { "en": "Kondisi Resiprokal Untuk Parameter Y?", "id": "Y12 Sama Dengan Y21." },
    { "en": "Apa Itu Rangkaian Simetris?", "id": "Karakteristiknya Sama Dilihat Dari Kedua Gerbang." },
    { "en": "Kondisi Simetris Untuk Parameter Z?", "id": "Z11 Sama Dengan Z22." },
    { "en": "Kondisi Simetris Untuk Parameter Y?", "id": "Y11 Sama Dengan Y22." },
    { "en": "Apa Itu Sumber Tegangan AC?", "id": "Sumber Yang Menghasilkan Tegangan Sinusoidal." },
    { "en": "Apa Nilai Rata-rata Dari Gelombang Sinus?", "id": "Nol." },
    { "en": "Apa Nilai RMS Dari Gelombang Sinus?", "id": "Amplitudo Puncak Dibagi Akar Dua." },
    { "en": "Tegangan Jala-jala PLN Diukur Dalam Apa?", "id": "Nilai RMS (Root Mean Square)." },
    { "en": "Daya Dalam Rangkaian AC Ada Berapa Jenis?", "id": "Tiga: Daya Nyata, Reaktif, Dan Kompleks." },
    { "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Yang Benar-benar Digunakan (Watt)." },
    { "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Yang Disimpan Dan Dilepaskan (VAR)." },
    { "en": "Apa Itu Daya Kompleks (Complex Power)?", "id": "Kombinasi Vektor Dari Daya Nyata Dan Reaktif (VA)." },
    { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Antara Daya Nyata Dan Daya Semu." },
    { "en": "Faktor Daya Ideal Bernilai Berapa?", "id": "Satu (Beban Murni Resistif)." },
    { "en": "Bagaimana Analisis Rangkaian Terhubung Ke Daya?", "id": "Tegangan Dan Arus Adalah Dasar Perhitungan Daya." },
    { "en": "Apa Itu Beban Induktif?", "id": "Beban Yang Menyerap Daya Reaktif (Motor)." },
    { "en": "Apa Itu Beban Kapasitif?", "id": "Beban Yang Menyuplai Daya Reaktif (Kapasitor Bank)." },
    { "en": "Apa Itu Koreksi Faktor Daya?", "id": "Menambahkan Kapasitor Untuk Mengimbangi Beban Induktif." },
    { "en": "Apa Itu Sistem Tiga Fasa?", "id": "Sistem Tenaga Listrik Dengan Tiga Sumber AC." },
    { "en": "Berapa Perbedaan Fasa Antar Sumber Tiga Fasa?", "id": "Seratus Dua Puluh Derajat." },
    { "en": "Bagaimana Menganalisis Sistem Tiga Fasa Seimbang?", "id": "Dengan Menganalisis Satu Fasa Saja (Analisis Per Fasa)." },
    { "en": "Apakah Analisis Per Fasa Menggunakan Nodal/Mesh?", "id": "Ya, Pada Rangkaian Ekuivalen Satu Fasa." },
    { "en": "Apa Itu Konfigurasi Hubungan Bintang (Wye)?", "id": "Terminal Negatif Sumber Dihubungkan Bersama." },
    { "en": "Apa Itu Konfigurasi Hubungan Delta (Delta)?", "id": "Sumber Dihubungkan Dalam Konfigurasi Loop." },
    { "en": "Apa Itu Tegangan Fasa?", "id": "Tegangan Antara Satu Jalur Dan Netral." },
    { "en": "Apa Itu Tegangan Saluran (Line Voltage)?", "id": "Tegangan Antara Dua Jalur." },
    { "en": "Dalam Hubungan Bintang, Apa Hubungan Tegangan?", "id": "Tegangan Saluran Akar Tiga Kali Tegangan Fasa." },
    { "en": "Dalam Hubungan Delta, Apa Hubungan Tegangan?", "id": "Tegangan Saluran Sama Dengan Tegangan Fasa." },
    { "en": "Apa Itu Arus Fasa?", "id": "Arus Yang Mengalir Melalui Satu Beban Fasa." },
    { "en": "Apa Itu Arus Saluran (Line Current)?", "id": "Arus Yang Mengalir Di Jalur Transmisi." },
    { "en": "Dalam Hubungan Bintang, Apa Hubungan Arus?", "id": "Arus Saluran Sama Dengan Arus Fasa." },
    { "en": "Dalam Hubungan Delta, Apa Hubungan Arus?", "id": "Arus Saluran Akar Tiga Kali Arus Fasa." },
    { "en": "Apakah Analisis Nodal Bisa Untuk Sistem Tidak Seimbang?", "id": "Ya, Tapi Harus Menganalisis Ketiga Fasa." },
    { "en": "Apakah Analisis Mesh Bisa Untuk Sistem Tidak Seimbang?", "id": "Ya, Tapi Lebih Rumit Daripada Nodal." },
    { "en": "Mengapa Sistem Tiga Fasa Digunakan?", "id": "Lebih Efisien Untuk Transmisi Daya." },
    { "en": "Apa Itu Transformator Ideal?", "id": "Perangkat Yang Mengubah Tegangan Tanpa Kehilangan Daya." },
    { "en": "Bagaimana Memasukkan Transformator Dalam Analisis Mesh?", "id": "Menggunakan Persamaan Kendala Tegangan Dan Arus." },
    { "en": "Apa Itu Impedansi Tercermin (Reflected Impedance)?", "id": "Impedansi Beban Dilihat Dari Sisi Primer." },
    { "en": "Apa Peran Analisis Rangkaian Dalam Desain Chip?", "id": "Menganalisis Waktu Tunda Dan Konsumsi Daya." },
    { "en": "Apakah Kawat Pada Chip Ideal?", "id": "Tidak, Memiliki Resistansi Dan Kapasitansi Parasitik." },
    { "en": "Model Apa Yang Digunakan Untuk Kawat Pada Chip?", "id": "Model Rangkaian RC Terdistribusi." },
    { "en": "Analisis Apa Yang Digunakan Untuk Model RC?", "id": "Analisis Nodal." },
    { "en": "Apa Itu 'Ground Bounce'?", "id": "Fluktuasi Tegangan Pada Jalur Ground." },
    { "en": "Apa Itu 'Crosstalk'?", "id": "Interferensi Antara Jalur Sinyal Yang Berdekatan." },
    { "en": "Bagaimana Menganalisis 'Crosstalk'?", "id": "Dengan Memodelkan Kapasitansi Dan Induktansi Mutual." },
    { "en": "Apakah Analisis Rangkaian Penting Untuk AI?", "id": "Ya, Untuk Merancang Perangkat Keras AI (Neuromorphic)." },
    { "en": "Apa Itu Memristor?", "id": "Elemen Rangkaian Keempat (Setelah R, L, C)." },
    { "en": "Ringkasan: Nodal Berbasis Hukum Apa?", "id": "Hukum Arus Kirchhoff (KCL)." },
    { "en": "Ringkasan: Mesh Berbasis Hukum Apa?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
    { "en": "Ringkasan: Variabel Tidak Diketahui Di Nodal?", "id": "Tegangan Simpul." },
    { "en": "Ringkasan: Variabel Tidak Diketahui Di Mesh?", "id": "Arus Mesh." },
    { "en": "Ringkasan: Nodal Cocok Untuk Sumber Apa?", "id": "Sumber Arus." },
    { "en": "Ringkasan: Mesh Cocok Untuk Sumber Apa?", "id": "Sumber Tegangan." },
    { "en": "Ringkasan: Kapan Supernode Dibutuhkan?", "id": "Sumber Tegangan Antara Simpul Non-Referensi." },
    { "en": "Ringkasan: Kapan Supermesh Dibutuhkan?", "id": "Sumber Arus Di Antara Dua Mesh." },
    { "en": "Ringkasan: Mesh Hanya Untuk Rangkaian Apa?", "id": "Rangkaian Planar." },
    { "en": "Ringkasan: Nodal Untuk Rangkaian Apa?", "id": "Semua Jenis Rangkaian." },
    { "en": "Apa Kata Kunci Untuk Nodal?", "id": "Simpul, Referensi, KCL, Tegangan." },
    { "en": "Apa Kata Kunci Untuk Mesh?", "id": "Loop, Planar, KVL, Arus." },
    { "en": "Elemen Apa Yang Diperlakukan Sama Di Kedua Metode?", "id": "Resistor (Atau Impedansi)." },
    { "en": "Apa Hasil Akhir Dari Kedua Metode?", "id": "Nilai Numerik Untuk Tegangan Atau Arus." },
    { "en": "Apa Yang Harus Dilakukan Setelah Mendapat Hasil?", "id": "Hitung Kuantitas Lain Yang Diminta." },
    { "en": "Apa Konsep Paling Fundamental?", "id": "Hukum Ohm (V=IR)." },
    { "en": "Apa Dua Hukum Paling Fundamental?", "id": "Hukum Kirchhoff (KCL Dan KVL)." },
    { "en": "Nodal Dan Mesh Adalah Aplikasi Sistematis Dari?", "id": "Hukum-Hukum Fundamental Tersebut." },
    { "en": "Apa Definisi Arus Listrik?", "id": "Laju Aliran Muatan Listrik." },
    { "en": "Apa Definisi Tegangan Listrik?", "id": "Energi Potensial Per Satuan Muatan." },
    { "en": "Apa Definisi Resistansi?", "id": "Hambatan Terhadap Aliran Arus." },
    { "en": "Apa Definisi Rangkaian Seri?", "id": "Elemen Terhubung Ujung Ke Ujung, Satu Jalur." },
    { "en": "Apa Definisi Rangkaian Paralel?", "id": "Elemen Terhubung Di Antara Dua Simpul Yang Sama." },
    { "en": "Di Rangkaian Seri, Besaran Apa Yang Sama?", "id": "Arus Sama Untuk Semua Elemen." },
    { "en": "Di Rangkaian Paralel, Besaran Apa Yang Sama?", "id": "Tegangan Sama Untuk Semua Elemen." },
    { "en": "Apakah Mungkin Menganalisis Tanpa Ground?", "id": "Tidak, Referensi Potensial Selalu Diperlukan." },
    { "en": "Apa Nama Lain Beda Potensial?", "id": "Tegangan." },
    { "en": "Apa Nama Lain Gaya Gerak Listrik (GGL)?", "id": "Tegangan Sumber." },
    { "en": "Apakah GGL Dan Jatuh Tegangan Sama?", "id": "Tidak, GGL Adalah Sumber, Jatuh Tegangan Adalah Beban." },
    { "en": "Apa Itu 'Beban' Dalam Rangkaian?", "id": "Elemen Yang Mengonsumsi Energi." },
    { "en": "Apa Tujuan Utama Seorang Insinyur Rangkaian?", "id": "Mendesain Dan Menganalisis Rangkaian Fungsional." },
    { "en": "Langkah Mana Yang Paling Membutuhkan Intuisi?", "id": "Memilih Metode Analisis Yang Paling Efisien." },
    { "en": "Kapan Harus Memilih Nodal?", "id": "Ketika Jumlah Simpul Esensial Sedikit." },
    { "en": "Kapan Harus Memilih Mesh?", "id": "Ketika Rangkaian Planar Dan Jumlah Mesh Sedikit." },
    { "en": "Apa Itu 'Kawat Jumper'?", "id": "Penghubung Sementara Dengan Resistansi Rendah." },
    { "en": "Bagaimana Memodelkan Baterai Nyata?", "id": "Sumber Tegangan Ideal Seri Dengan Resistor Internal." },
    { "en": "Bagaimana Memodelkan Sumber Arus Nyata?", "id": "Sumber Arus Ideal Paralel Dengan Resistor Internal." },
    { "en": "Apa Yang Menyebabkan Kehilangan Daya Di Sumber Nyata?", "id": "Resistansi Internal." },
    { "en": "Apa Itu Efisiensi Sumber?", "id": "Rasio Daya Ke Beban Terhadap Total Daya." },
    { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Daya Maksimum Terjadi Saat Resistansi Beban Sama." },
    { "en": "Sama Dengan Apa Resistansi Bebannya?", "id": "Resistansi Thevenin Dari Sumber." },
    { "en": "Apakah Analisis Nodal/Mesh Membantu Menemukan R_Thevenin?", "id": "Ya, Untuk Menemukan V_oc Dan I_sc." },
    { "en": "Rumus R_Thevenin Adalah?", "id": "V_oc Dibagi I_sc." },
    { "en": "Cara Lain Menemukan R_Thevenin?", "id": "Matikan Sumber Independen Dan Hitung Resistansi Ekuivalen." },
    { "en": "Bagaimana Mematikan Sumber Tegangan Independen?", "id": "Ganti Dengan Hubung Singkat." },
    { "en": "Bagaimana Mematikan Sumber Arus Independen?", "id": "Ganti Dengan Rangkaian Terbuka." },
    { "en": "Bagaimana Dengan Sumber Terkendali Saat Mencari R_Thevenin?", "id": "Sumber Terkendali Tidak Boleh Dimatikan." },
    { "en": "Apa Yang Dilakukan Jika Ada Sumber Terkendali?", "id": "Gunakan Sumber Uji (Tegangan Atau Arus)." },
    { "en": "Apa Itu Sumber Uji?", "id": "Sumber Eksternal Dengan Nilai Yang Diketahui." },
    { "en": "R_Thevenin Dihitung Sebagai Apa Dengan Sumber Uji?", "id": "Tegangan Uji Dibagi Arus Uji." },
    { "en": "Apakah Resistansi Thevenin Bisa Negatif?", "id": "Ya, Jika Rangkaian Mengandung Sumber Terkendali." },
    { "en": "Resistansi Negatif Berarti Rangkaian Bertindak Sebagai?", "id": "Sumber Energi." },
    { "en": "Apa Itu Resistansi Norton?", "id": "Sama Dengan Resistansi Thevenin." },
    { "en": "Apa Rangkaian Ekuivalen Thevenin?", "id": "Sumber Tegangan Seri Dengan Resistor Thevenin." },
    { "en": "Apa Rangkaian Ekuivalen Norton?", "id": "Sumber Arus Paralel Dengan Resistor Norton." },
    { "en": "Apa Tujuan Utama Teorema Ekuivalensi?", "id": "Menyederhanakan Bagian Rangkaian Untuk Analisis Beban." },
    { "en": "Apakah Analisis Rangkaian Adalah Seni Atau Sains?", "id": "Keduanya, Membutuhkan Logika Dan Kreativitas." },
    { "en": "Bagaimana Jika Rangkaian Tidak Punya Solusi Unik?", "id": "Biasanya Menandakan Ada Kesalahan Dalam Model." },
    { "en": "Apa Ciri Rangkaian Yang Baik?", "id": "Stabil, Efisien, Dan Memenuhi Spesifikasi." },
    { "en": "Bagaimana Analisis Rangkaian Berkembang Seiring Waktu?", "id": "Dari Perhitungan Manual Ke Simulasi Komputer Canggih." },
    { "en": "Apa Peran Matematika Dalam Analisis Rangkaian?", "id": "Sebagai Bahasa Untuk Mendeskripsikan Dan Menyelesaikan Masalah." },
    { "en": "Aljabar Linear Penting Untuk?", "id": "Menyelesaikan Sistem Persamaan." },
    { "en": "Kalkulus Penting Untuk?", "id": "Menganalisis Rangkaian Dengan Kapasitor Dan Induktor." },
    { "en": "Bilangan Kompleks Penting Untuk?", "id": "Analisis Rangkaian AC Keadaan Tunak." },
    { "en": "Apa Yang Harus Dilakukan Jika Terjebak Pada Masalah?", "id": "Periksa Kembali Asumsi Dasar Dan Persamaan." },
    { "en": "Apa Itu 'Sanity Check'?", "id": "Memeriksa Apakah Jawaban Masuk Akal Secara Fisik." },
    { "en": "Contoh 'Sanity Check'?", "id": "Daya Yang Diserap Tidak Bisa Negatif." },
    { "en": "Apa Urutan Belajar Teori Rangkaian?", "id": "DC, Kemudian AC, Lalu Transien." },
    { "en": "Apakah Nodal/Mesh Berlaku Untuk Ketiganya?", "id": "Ya, Dengan Modifikasi Sesuai." },
    { "en": "Apa Yang Paling Penting Diingat Tentang Nodal?", "id": "KCL Di Setiap Simpul." },
    { "en": "Apa Yang Paling Penting Diingat Tentang Mesh?", "id": "KVL Di Setiap Loop." },
    { "en": "Apa Simbol Untuk Sumber Tegangan Independen?", "id": "Lingkaran Dengan Tanda Plus Dan Minus." },
    { "en": "Apa Simbol Untuk Sumber Arus Independen?", "id": "Lingkaran Dengan Tanda Panah Di Dalamnya." },
    { "en": "Apa Simbol Untuk Sumber Tegangan Terkendali?", "id": "Berlian Dengan Tanda Plus Dan Minus." },
    { "en": "Apa Simbol Untuk Sumber Arus Terkendali?", "id": "Berlian Dengan Tanda Panah Di Dalamnya." },
    { "en": "Apa Itu 'Node Datum'?", "id": "Istilah Lain Untuk Simpul Referensi." },
    { "en": "Berapa Jumlah Minimum Simpul Dalam Rangkaian?", "id": "Dua Simpul." },
    { "en": "Berapa Jumlah Minimum Loop Dalam Rangkaian?", "id": "Satu Loop." },
    { "en": "Apa Itu 'Short Circuit'?", "id": "Jalur Dengan Resistansi Hampir Nol." },
    { "en": "Apa Itu 'Open Circuit'?", "id": "Jalur Dengan Resistansi Hampir Tak Terhingga." },
    { "en": "Di Mana Arus Cenderung Mengalir?", "id": "Melalui Jalur Dengan Resistansi Paling Rendah." },
    { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Dengan Tiga Terminal." },
    { "en": "Bagaimana Potensiometer Digunakan Sebagai Pembagi Tegangan?", "id": "Dengan Menggunakan Ketiga Terminalnya." },
    { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Dengan Dua Terminal." },
    { "en": "Apa Fondasi Dari Semua Teknik Elektro?", "id": "Teori Rangkaian Listrik." },
    { "en": "Apa Saran Belajar Terbaik?", "id": "Latihan, Latihan, Dan Latihan." },
    { "en": "Apakah Kesalahan Bagian Dari Belajar?", "id": "Ya, Kesalahan Membantu Memahami Konsep." },
    { "en": "Mengapa Analisis Rangkaian Penting?", "id": "Karena Rangkaian Ada Di Semua Perangkat Elektronik." },
    { "en": "Apa Langkah Terakhir Yang Baik?", "id": "Tanyakan Pada Diri Sendiri 'Apakah Jawaban Ini Masuk Akal?'." },
    { "en": "Apa Itu Rangkaian Resiprokal?", "id": "Rasio Tegangan Dan Arus Tetap Sama." },
    { "en": "Apa Ciri Rangkaian Linear?", "id": "Mematuhi Prinsip Superposisi." },
    { "en": "Apakah Semua Rangkaian Praktis Linear?", "id": "Tidak, Banyak Yang Non-Linear (Contoh: Dioda)." },
    { "en": "Bagaimana Menganalisis Rangkaian Non-Linear?", "id": "Menggunakan Metode Grafis Atau Aproksimasi Linear." },
    { "en": "Apa Itu Garis Beban (Load Line)?", "id": "Grafik Yang Digunakan Untuk Menganalisis Rangkaian Non-Linear." },
    { "en": "Apa Itu Titik Operasi (Q-Point)?", "id": "Titik Kerja DC Dari Perangkat Non-Linear." },
    { "en": "Apakah Nodal/Mesh Langsung Berlaku Untuk Dioda?", "id": "Tidak, Karena Dioda Non-Linear." },
    { "en": "Pondasi Analisis Rangkaian Terdiri Dari Apa?", "id": "Hukum Ohm Dan Hukum Kirchhoff." }



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
