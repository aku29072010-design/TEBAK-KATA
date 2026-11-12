<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tebak Kata Nusantara - Game Interaktif</title>
    <!-- Tailwind CSS untuk tata letak dasar dan responsivitas -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Tone.js untuk efek suara dinamis -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>
    <!-- Google Fonts: Fredoka untuk nuansa permainan yang ceria & mudah dibaca -->
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        /* Custom CSS untuk estetika game, animasi, dan efek tombol */
        :root {
            --primary: #6366f1;
            --primary-dark: #4f46e5;
            --secondary: #ec4899;
            --accent: #f59e0b;
            --success: #10b981;
            --danger: #ef4444;
        }

        body {
            font-family: 'Fredoka', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #311042 100%);
            min-height: 100vh;
            color: #f8fafc;
            overflow-x: hidden;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        /* Latar belakang partikel mengambang */
        .bg-shapes {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
            overflow: hidden;
        }

        .shape {
            position: absolute;
            border-radius: 50%;
            filter: blur(60px);
            opacity: 0.35;
            animation: float 15s infinite ease-in-out alternate;
        }

        @keyframes float {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(40px, -40px) scale(1.1); }
        }

        /* Kartu utama kaca (Glassmorphism) */
        .game-card {
            background: rgba(30, 41, 59, 0.75);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 2px solid rgba(255, 255, 255, 0.12);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.6);
        }

        /* Efek tombol 3D ala game */
        .btn-game {
            transition: all 0.12s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            top: 0;
        }

        .btn-game:active:not(:disabled) {
            top: 4px;
            box-shadow: 0 0px 0 transparent !important;
        }

        .btn-primary {
            background: linear-gradient(to bottom, #6366f1, #4f46e5);
            box-shadow: 0 5px 0 #3730a3;
        }
        .btn-primary:hover:not(:disabled) {
            background: linear-gradient(to bottom, #818cf8, #6366f1);
        }

        .btn-accent {
            background: linear-gradient(to bottom, #f59e0b, #d97706);
            box-shadow: 0 5px 0 #92400e;
        }
        .btn-accent:hover:not(:disabled) {
            background: linear-gradient(to bottom, #fbbf24, #f59e0b);
        }

        .btn-success {
            background: linear-gradient(to bottom, #10b981, #059669);
            box-shadow: 0 5px 0 #065f46;
        }

        /* Tombol Keyboard Virtual */
        .key-btn {
            background: linear-gradient(to bottom, #334155, #1e293b);
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 4px 0 #0f172a;
            transition: all 0.1s ease;
        }

        .key-btn:hover:not(:disabled) {
            background: linear-gradient(to bottom, #475569, #334155);
            transform: translateY(-1px);
        }

        .key-btn:active:not(:disabled) {
            transform: translateY(3px);
            box-shadow: 0 1px 0 #0f172a;
        }

        .key-btn.correct {
            background: linear-gradient(to bottom, #10b981, #059669) !important;
            box-shadow: 0 4px 0 #064e3b !important;
            border-color: #34d399;
            color: white;
            cursor: default;
        }

        .key-btn.wrong {
            background: #1e293b !important;
            box-shadow: none !important;
            opacity: 0.35;
            transform: translateY(3px);
            cursor: default;
        }

        /* Kotak Huruf Jawaban */
        .letter-slot {
            background: rgba(15, 23, 42, 0.85);
            border: 2px solid rgba(148, 163, 184, 0.3);
            box-shadow: inset 0 2px 6px rgba(0, 0, 0, 0.5);
            transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
        }

        .letter-slot.revealed {
            background: linear-gradient(135deg, #4f46e5, #7c3aed);
            border-color: #a78bfa;
            transform: scale(1.05);
            box-shadow: 0 4px 12px rgba(124, 58, 237, 0.4);
            animation: popIn 0.35s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        .letter-slot.hinted {
            background: linear-gradient(135deg, #d97706, #f59e0b);
            border-color: #fde68a;
            animation: popIn 0.35s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }

        @keyframes popIn {
            0% { transform: scale(0.5); opacity: 0; }
            70% { transform: scale(1.15); }
            100% { transform: scale(1.05); opacity: 1; }
        }

        /* Animasi Getar saat salah */
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-8px); }
            40%, 80% { transform: translateX(8px); }
        }

        .animate-shake {
            animation: shake 0.4s ease-in-out;
        }

        /* Animasi Detak Hati */
        @keyframes heartbeat {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }
        .heart-active {
            animation: heartbeat 1.5s infinite ease-in-out;
        }

        /* Kustomisasi Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.5);
        }
        ::-webkit-scrollbar-thumb {
            background: #4f46e5;
            border-radius: 10px;
        }
    </style>
</head>
<body class="flex flex-col items-center justify-between p-3 sm:p-6">

    <!-- Dekorasi Latar Belakang -->
    <div class="bg-shapes">
        <div class="shape bg-indigo-600 w-72 h-72 top-10 left-10"></div>
        <div class="shape bg-pink-600 w-80 h-80 bottom-10 right-10" style="animation-delay: -5s;"></div>
        <div class="shape bg-amber-500 w-64 h-64 top-1/2 left-1/2" style="animation-delay: -10s;"></div>
    </div>

    <!-- Header & Kontrol Atas -->
    <header class="w-full max-w-3xl z-10 flex flex-wrap items-center justify-between gap-2 mb-4">
        <div class="flex items-center gap-3">
            <div class="w-11 h-11 rounded-2xl bg-gradient-to-tr from-indigo-500 to-pink-500 flex items-center justify-center shadow-lg border border-white/20">
                <span class="text-2xl">🧩</span>
            </div>
            <div>
                <h1 class="text-xl sm:text-2xl font-bold tracking-wide bg-clip-text text-transparent bg-gradient-to-r from-white via-indigo-200 to-pink-200">
                    TEBAK KATA
                </h1>
                <p class="text-xs text-slate-400">Asah Otak & Kosakata</p>
            </div>
        </div>

        <!-- Panel Statistik: Skor, Streak, & Suara -->
        <div class="flex items-center gap-2 sm:gap-3">
            <div class="bg-slate-800/80 border border-slate-700 px-3 py-1.5 rounded-xl flex items-center gap-1.5 shadow-inner" title="Skor Total">
                <span class="text-amber-400 text-sm sm:text-base">🏆</span>
                <span id="scoreDisplay" class="font-bold text-sm sm:text-base text-amber-300">0</span>
            </div>
            <div class="bg-slate-800/80 border border-slate-700 px-3 py-1.5 rounded-xl flex items-center gap-1.5 shadow-inner" title="Jawaban Beruntun">
                <span class="text-orange-500 text-sm sm:text-base">🔥</span>
                <span id="streakDisplay" class="font-bold text-sm sm:text-base text-orange-300">0</span>
            </div>
            <button id="soundToggleBtn" onclick="toggleSound()" class="bg-slate-800/80 hover:bg-slate-700 border border-slate-700 p-2 rounded-xl transition text-slate-200" title="Hidup/Matikan Suara">
                <span id="soundIcon">🔊</span>
            </button>
        </div>
    </header>

    <!-- Kontainer Utama Permainan -->
    <main class="w-full max-w-3xl z-10 flex-grow flex flex-col justify-center">
        
        <!-- LAYAR 1: PILIH KATEGORI -->
        <section id="categoryScreen" class="game-card rounded-3xl p-6 sm:p-8 text-center transition-all duration-300">
            <div class="inline-block px-4 py-1 rounded-full bg-indigo-500/20 border border-indigo-400/30 text-indigo-300 text-xs sm:text-sm font-semibold mb-3">
                ✨ PILIH KATEGORI PERMAINAN
            </div>
            <h2 class="text-2xl sm:text-3xl font-bold mb-2">Mau Tebak Kata Apa Hari Ini?</h2>
            <p class="text-slate-300 text-sm sm:text-base mb-6 max-w-md mx-auto">
                Pilih satu kategori di bawah ini atau pilih mode campur untuk tantangan acak!
            </p>

            <!-- Grid Kartu Kategori -->
            <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 sm:gap-4 mb-6">
                <button onclick="startGame('binatang')" class="btn-game group bg-slate-800/90 hover:bg-indigo-900/60 border-2 border-slate-700 hover:border-indigo-400 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:scale-110 transition-transform">🦁</span>
                    <span class="font-bold text-base sm:text-lg text-white">Binatang</span>
                    <span class="text-xs text-slate-400">25 Kata</span>
                </button>

                <button onclick="startGame('buah')" class="btn-game group bg-slate-800/90 hover:bg-pink-900/60 border-2 border-slate-700 hover:border-pink-400 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:scale-110 transition-transform">🍉</span>
                    <span class="font-bold text-base sm:text-lg text-white">Buah-buahan</span>
                    <span class="text-xs text-slate-400">25 Kata</span>
                </button>

                <button onclick="startGame('negara')" class="btn-game group bg-slate-800/90 hover:bg-emerald-900/60 border-2 border-slate-700 hover:border-emerald-400 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:scale-110 transition-transform">🌍</span>
                    <span class="font-bold text-base sm:text-lg text-white">Negara</span>
                    <span class="text-xs text-slate-400">25 Kata</span>
                </button>

                <button onclick="startGame('profesi')" class="btn-game group bg-slate-800/90 hover:bg-amber-900/60 border-2 border-slate-700 hover:border-amber-400 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:scale-110 transition-transform">👨‍🚀</span>
                    <span class="font-bold text-base sm:text-lg text-white">Profesi</span>
                    <span class="text-xs text-slate-400">20 Kata</span>
                </button>

                <button onclick="startGame('benda')" class="btn-game group bg-slate-800/90 hover:bg-cyan-900/60 border-2 border-slate-700 hover:border-cyan-400 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:scale-110 transition-transform">🎸</span>
                    <span class="font-bold text-base sm:text-lg text-white">Benda</span>
                    <span class="text-xs text-slate-400">20 Kata</span>
                </button>

                <button onclick="startGame('semua')" class="btn-game group bg-gradient-to-br from-indigo-600/80 to-pink-600/80 hover:from-indigo-500 hover:to-pink-500 border-2 border-indigo-300/50 rounded-2xl p-4 flex flex-col items-center gap-2 shadow-lg">
                    <span class="text-4xl sm:text-5xl group-hover:rotate-12 transition-transform">🎲</span>
                    <span class="font-bold text-base sm:text-lg text-white">Acak Semua</span>
                    <span class="text-xs text-indigo-100">115 Kata</span>
                </button>
            </div>

            <div class="text-xs text-slate-400 bg-slate-900/50 rounded-xl p-3 border border-slate-800">
                💡 <strong>Tips:</strong> Kamu juga bisa menggunakan <strong>Keyboard Fisik (A-Z)</strong> di komputer/laptop untuk menebak lebih cepat!
            </div>
        </section>

        <!-- LAYAR 2: ARENA BERMAIN -->
        <section id="playScreen" class="hidden flex-col gap-4">
            
            <!-- Kartu Soal & Petunjuk -->
            <div id="questionCard" class="game-card rounded-3xl p-5 sm:p-7 relative overflow-hidden">
                <!-- Baris Atas Arena: Badge Kategori & Nyawa -->
                <div class="flex items-center justify-between gap-2 mb-4">
                    <div class="flex items-center gap-2">
                        <button onclick="backToMenu()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 px-3 py-1.5 rounded-xl text-xs sm:text-sm font-semibold border border-slate-600 transition flex items-center gap-1">
                            <span>←</span> Kategori
                        </button>
                        <span id="categoryBadge" class="px-3 py-1.5 rounded-xl bg-indigo-500/20 border border-indigo-400/30 text-indigo-300 text-xs sm:text-sm font-bold uppercase tracking-wider">
                            🦁 BINATANG
                        </span>
                    </div>

                    <!-- Indikator Nyawa (Hati) -->
                    <div id="livesContainer" class="flex items-center gap-1 bg-slate-900/60 px-3 py-1.5 rounded-xl border border-slate-700/70" title="Sisa Kesempatan">
                        <!-- Diisi oleh JS -->
                    </div>
                </div>

                <!-- Kotak Petunjuk / Clues -->
                <div class="bg-slate-900/70 border border-indigo-500/30 rounded-2xl p-4 mb-6 text-center relative">
                    <span class="absolute -top-2.5 left-1/2 -translate-x-1/2 bg-indigo-600 text-white text-[10px] font-bold px-2.5 py-0.5 rounded-full uppercase tracking-widest shadow">
                        Petunjuk Soal
                    </span>
                    <p id="clueText" class="text-base sm:text-lg font-medium text-indigo-100 leading-relaxed mt-1">
                        Memuat petunjuk...
                    </p>
                </div>

                <!-- Area Slot Huruf Kata -->
                <div id="wordSlotsContainer" class="flex flex-wrap items-center justify-center gap-1.5 sm:gap-2.5 my-4 min-h-[60px]">
                    <!-- Kotak huruf akan dirender di sini -->
                </div>

                <!-- Notifikasi Status Inline (Pesan feedback tanpa alert) -->
                <div id="feedbackBanner" class="h-7 flex items-center justify-center mt-2">
                    <span id="feedbackText" class="text-sm font-semibold text-slate-400 transition-all">
                        Pilih huruf di bawah untuk menebak!
                    </span>
                </div>
            </div>

            <!-- Baris Tombol Bantuan di luar area utama -->
            <div class="flex flex-wrap items-center justify-center gap-3">
                <button id="hintBtn" onclick="useHint()" class="btn-game btn-accent text-slate-950 font-bold px-4 py-2.5 rounded-2xl text-sm flex items-center gap-2 disabled:opacity-50 disabled:cursor-not-allowed">
                    <span>💡</span>
                    <span>Buka 1 Huruf</span>
                    <span class="bg-black/20 px-2 py-0.5 rounded-lg text-xs">-20 Poin</span>
                </button>

                <button id="skipBtn" onclick="skipWord()" class="btn-game bg-slate-700 hover:bg-slate-600 text-white font-semibold px-4 py-2.5 rounded-2xl text-sm border border-slate-500 shadow-[0_4px_0_#1e293b] flex items-center gap-2">
                    <span>⏭️</span>
                    <span>Lewati Kata</span>
                </button>
            </div>

            <!-- Keyboard Virtual QWERTY -->
            <div class="game-card rounded-3xl p-3 sm:p-5">
                <div id="keyboardContainer" class="flex flex-col gap-1.5 sm:gap-2 select-none">
                    <!-- Baris tombol huruf QWERTY dirender lewat JS -->
                </div>
            </div>

        </section>
    </main>

    <!-- MODAL HASIL RONDE (MENANG / KALAH) -->
    <div id="resultModal" class="fixed inset-0 z-50 hidden items-center justify-center p-4 bg-slate-950/80 backdrop-blur-sm transition-opacity">
        <div class="game-card w-full max-w-md rounded-3xl p-6 sm:p-8 text-center border-2 border-indigo-400/40 transform transition-all scale-95" id="modalContent">
            <div id="modalEmoji" class="text-6xl mb-3 animate-bounce">🎉</div>
            <h3 id="modalTitle" class="text-2xl sm:text-3xl font-bold mb-1 text-white">Luar Biasa!</h3>
            <p id="modalSubtitle" class="text-slate-300 text-sm mb-5">Kamu berhasil menebak katanya dengan tepat.</p>

            <!-- Tampilan Kata yang Benar & Fakta Singkat -->
            <div class="bg-slate-900/90 border border-slate-700 rounded-2xl p-4 mb-6">
                <span class="text-xs text-slate-400 uppercase tracking-wider block mb-1">Jawabannya Adalah</span>
                <div id="modalWord" class="text-2xl sm:text-3xl font-extrabold tracking-widest text-amber-400 mb-2">
                    KOMODO
                </div>
                <p id="modalFact" class="text-xs sm:text-sm text-indigo-200 bg-indigo-950/60 p-2.5 rounded-xl border border-indigo-500/20">
                    Fakta menarik akan tampil di sini.
                </p>
            </div>

            <!-- Tombol Aksi Selanjutnya -->
            <div class="flex flex-col sm:flex-row gap-3 justify-center">
                <button onclick="nextRound()" class="btn-game btn-success w-full text-white font-bold py-3.5 px-6 rounded-2xl text-base flex items-center justify-center gap-2">
                    <span>Kata Berikutnya</span>
                    <span>➡️</span>
                </button>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="w-full max-w-3xl z-10 text-center mt-4 text-xs text-slate-400">
        Tebak Kata Nusantara • Kumpulkan skor tertinggi!
    </footer>

    <!-- LOGIKA PERMAINAN JAVASCRIPT -->
    <script>
        // ==========================================
        // 1. DATABASE KATA & PETUNJUK (115+ Kata)
        // ==========================================
        const WORD_DATABASE = {
            binatang: [
                { word: "KOMODO", clue: "Kadal raksasa purba endemik dari Nusa Tenggara Timur, Indonesia.", fact: "Komodo memiliki air liur berbisa dan merupakan kadal terbesar di dunia yang masih hidup." },
                { word: "ORANGUTAN", clue: "Kera besar berbulu cokelat kemerahan yang hidup di hutan Kalimantan dan Sumatra.", fact: "Nama 'Orangutan' berasal dari bahasa Melayu yang berarti 'manusia hutan'." },
                { word: "CENDERAWASIH", clue: "Burung cantik asal Papua yang dijuluki sebagai 'Bird of Paradise'.", fact: "Hanya burung Cenderawasih jantan yang memiliki bulu ekor panjang nan indah untuk menarik perhatian betina." },
                { word: "TREnggiling", clue: "Mamalia bersisik keras yang akan menggulung tubuhnya seperti bola saat terancam.", fact: "Trenggiling tidak memiliki gigi dan menggunakan lidahnya yang sangat panjang untuk memakan semut." },
                { word: "BUNGALON", clue: "Reptil yang ahli menyamar dengan mengubah warna kulitnya sesuai lingkungan.", fact: "Mata bunglon bisa bergerak ke dua arah yang berbeda secara bersamaan!" },
                { word: "LUMBA LUMBA", clue: "Mamalia laut cerdas yang berkomunikasi menggunakan gelombang sonar.", fact: "Lumba-lumba tidur dengan hanya mengistirahatkan separuh otaknya saja agar tetap bisa bernapas." },
                { word: "KELELAWAR", clue: "Satu-satunya mamalia yang mampu terbang dan aktif mencari makan di malam hari.", fact: "Kelelawar menggunakan ekolokasi untuk bernavigasi dalam kegelapan total." },
                { word: "JERAPAH", clue: "Hewan darat tertinggi di dunia dengan leher panjang dan corak totol cokelat.", fact: "Meskipun lehernya sangat panjang, jerapah memiliki jumlah tulang leher yang sama dengan manusia, yaitu 7 ruas." },
                { word: "PINGUIN", clue: "Burung berjas hitam-putih yang tidak bisa terbang namun perenang handal di kutub.", fact: "Pinguin memiliki kelenjar khusus di dekat matanya yang menyaring garam dari air laut." },
                { word: "LANDAK", clue: "Hewan pengerat yang seluruh punggungnya ditutupi duri-duri tajam.", fact: "Duri landak sebenarnya adalah rambut yang mengeras dan terbuat dari keratin, sama seperti kuku manusia." },
                { word: "KANGURU", clue: "Hewan berkantung asal Australia yang bergerak dengan cara melompat.", fact: "Kanguru tidak bisa berjalan mundur karena bentuk kaki dan ekornya yang besar." },
                { word: "FLAMINGO", clue: "Burung berkaki jenjang dengan bulu berwarna merah muda yang suka berdiri satu kaki.", fact: "Warna merah muda pada flamingo berasal dari makanan utamanya, yaitu udang dan alga." },
                { word: "GURITA", clue: "Hewan laut bertubuh lunak yang memiliki delapan lengan dan tiga jantung.", fact: "Gurita memiliki darah berwarna biru karena mengandung tembaga, bukan zat besi." },
                { word: "BEKICOT", clue: "Hewan bertubuh lunak yang membawa cangkang spiral di punggungnya dan berjalan lambat.", fact: "Bekicot bisa tidur hingga 3 tahun lamanya jika cuaca terlalu kering." },
                { word: "CAPUNG", clue: "Serangga bersayap transparan ganda yang sering terbang di dekat perairan.", fact: "Capung sudah ada di Bumi sejak 300 juta tahun lalu, jauh sebelum dinosaurus muncul." },
                { word: "CHEETAH", clue: "Kucing besar bertotol hitam yang merupakan pelari tercepat di daratan.", fact: "Cheetah dapat berlari hingga kecepatan 110 km/jam hanya dalam waktu 3 detik." },
                { word: "BADAK", clue: "Mamalia besar berkulit tebal yang memiliki satu atau dua cula di atas hidungnya.", fact: "Cula badak terbuat dari keratin padat, bukan tulang." },
                { word: "KUDANIL", clue: "Mamalia besar semi-akuatik asal Afrika yang sering berendam di sungai.", fact: "Keringat kudanill berwarna kemerahan yang berfungsi sebagai tabir surya alami." },
                { word: "BURUNG HANTU", clue: "Burung pemangsa nokturnal yang kepalanya bisa berputar hingga 270 derajat.", fact: "Bulu sayap burung hantu dirancang khusus agar mereka bisa terbang tanpa mengeluarkan suara." },
                { word: "UBUR UBUR", clue: "Hewan laut transparan berbentuk seperti payung yang memiliki tentakel penyengat.", fact: "Ubur-ubur tidak memiliki otak, jantung, tulang, maupun mata." },
                { word: "ANOA", clue: "Hewan endemik Sulawesi yang bentuknya mirip kerbau namun berukuran kerdil.", fact: "Anoa sering dijuluki sebagai kerbau terkecil di dunia." },
                { word: "BEKANTAN", clue: "Monyet berhidung panjang dan besar yang menjadi maskot fauna Kalimantan Selatan.", fact: "Bekantan adalah perenang ulung dan memiliki selaput di antara jari-jari kakinya." },
                { word: "KALAJENGKING", clue: "Arthropoda bercapit yang memiliki ekor melengkung dengan sengat beracun di ujungnya.", fact: "Tubuh kalajengking akan menyala biru-hijau terang jika disinari lampu ultraviolet (UV)." },
                { word: "MERAK", clue: "Burung indah yang pejantannya bisa mengembangkan bulu ekor seperti kipas raksasa bermotif mata.", fact: "Bulu ekor burung merak dapat mencapai panjang hingga 1,5 meter." },
                { word: "KOALA", clue: "Hewan berkantung pemakan daun eukaliptus yang tidur hingga 20 jam sehari.", fact: "Sidik jari koala sangat mirip dengan sidik jari manusia bahkan di bawah mikroskop." }
            ],
            buah: [
                { word: "DURIAN", clue: "Dijuluki 'Raja Buah', berkulit duri tajam dengan aroma yang sangat khas dan menyengat.", fact: "Durian dilarang dibawa ke dalam transportasi umum dan hotel di banyak negara Asia Tenggara." },
                { word: "MANGGIS", clue: "Dijuluki 'Ratu Buah', berkulit ungu tebal dengan daging buah putih bersih yang manis asam.", fact: "Jumlah kelopak di bagian bawah kulit manggis menunjukkan jumlah ruas daging buah di dalamnya." },
                { word: "RAMBUTAN", clue: "Buah tropis berbentuk bulat dengan kulit berwarna merah yang ditumbuhi rambut-rambut lembut.", fact: "Nama 'Rambutan' diambil langsung dari kata 'rambut' karena bentuk kulitnya." },
                { word: "SEMANGKA", clue: "Buah besar berkulit hijau loreng dengan daging merah segar dan banyak mengandung air.", fact: "Sekitar 92% kandungan di dalam buah semangka adalah air murni." },
                { word: "ALPUKAT", clue: "Buah berkulit hijau tua dengan daging lembut berlemak sehat, sering dibuat jus atau guacamole.", fact: "Secara botani, alpukat sebenarnya diklasifikasikan sebagai buah beri raksasa berbiji tunggal." },
                { word: "BELIMBING", clue: "Buah berwarna kuning kehijauan yang jika dipotong melintang bentuknya menyerupai bintang.", fact: "Dalam bahasa Inggris, buah ini dikenal sebagai 'Starfruit' karena bentuk irisannya." },
                { word: "BUAH NAGA", clue: "Buah dari tanaman kaktus berkulit merah sisik dengan daging putih atau merah berbiji hitam kecil.", fact: "Bunga tanaman buah naga hanya mekar pada malam hari dan layu saat matahari terbit." },
                { word: "NANAS", clue: "Buah tropis berkulit sisik tajam dengan mahkota daun di atasnya, rasanya manis dan segar.", fact: "Nanas mengandung enzim bromelain yang dapat membantu melunakkan daging." },
                { word: "KELAPA", clue: "Buah serbaguna dari pohon nyiur di tepi pantai yang airnya menyegarkan dan dagingnya bersantan.", fact: "Air kelapa muda steril dan pernah digunakan sebagai cairan infus darurat di masa lalu." },
                { word: "SALAK", clue: "Buah asli Indonesia dengan kulit bersisik cokelat menyerupai kulit ular.", fact: "Dalam bahasa Inggris, salak dikenal dengan sebutan 'Snake Fruit'." },
                { word: "SRIKAYA", clue: "Buah hijau berbenjol-benjol dengan daging putih manis bertekstur lembut dan berbiji hitam.", fact: "Srikaya kaya akan vitamin C dan antioksidan yang baik untuk daya tahan tubuh." },
                { word: "STROBERI", clue: "Buah kecil berwarna merah cerah dengan bintik-bintik biji di bagian luar kulitnya.", fact: "Stroberi adalah satu-satunya buah yang bijinya tumbuh di bagian luar, bukan di dalam." },
                { word: "DELIMA", clue: "Buah berkulit merah keras yang di dalamnya penuh dengan ratusan butiran biji merah manis berair.", fact: "Satu buah delima rata-rata mengandung sekitar 600 hingga 800 biji yang bisa dimakan." },
                { word: "SIRSAK", clue: "Buah berkulit hijau berduri lunak dengan daging putih berserat yang rasanya manis asam segar.", fact: "Nama sirsak berasal dari bahasa Belanda 'Zuurzak' yang berarti kantung asam." },
                { word: "MARKISA", clue: "Buah bulat dengan kulit ungu atau kuning yang di dalamnya berisi biji-biji berselaput kuning harum.", fact: "Bunga markisa memiliki bentuk yang sangat rumit dan indah, sering disebut 'Passion Flower'." },
                { word: "KEDONDONG", clue: "Buah hijau renyah berasa asam segar dengan biji berserabut tajam di tengahnya, favorit untuk rujak.", fact: "Serabut kasar pada biji kedondong membuat buah ini unik saat digigit hingga ke tengah." },
                { word: "SAWO", clue: "Buah berbentuk lonjong berkulit cokelat tipis dengan daging buah manis lembut seperti gula merah.", fact: "Getah pohon sawo zaman dahulu digunakan sebagai bahan dasar pembuatan permen karet alami." },
                { word: "JAMBU AIR", clue: "Buah berbentuk lonceng berwarna merah muda atau hijau yang renyah dan sangat berair.", fact: "Jambu air memiliki kandungan air yang sangat tinggi sehingga efektif melepas dahaga." },
                { word: "PEPAYA", clue: "Buah lonjong berdaging jingga kemerahan yang sangat baik untuk melancarkan pencernaan.", fact: "Pepaya mengandung enzim papain yang sangat bermanfaat bagi kesehatan lambung." },
                { word: "KIWI", clue: "Buah mungil berkulit cokelat berbulu halus dengan daging hijau cerah dan biji hitam melingkar.", fact: "Buah kiwi mengandung vitamin C dua kali lebih banyak dibandingkan buah jeruk." },
                { word: "LECI", clue: "Buah kecil berkulit merah kasar yang mudah dikupas dengan daging putih bening beraroma harum.", fact: "Buah leci sudah dibudidayakan di Tiongkok sejak lebih dari 2.000 tahun yang lalu." },
                { word: "MENTIMUN", clue: "Sering dianggap sayur untuk lalapan, buah hijau panjang ini sangat menyegarkan dan rendah kalori.", fact: "Mentimun terdiri dari 95% air dan bisa membantu menurunkan suhu tubuh." },
                { word: "PISANG", clue: "Buah melengkung berkulit kuning saat matang yang kaya akan kalium dan energi.", fact: "Pohon pisang sebenarnya bukanlah pohon berkayu, melainkan tanaman herba raksasa." },
                { word: "ANGGUR", clue: "Buah bulat kecil yang tumbuh bergerombol pada tanaman merambat, tersedia warna ungu dan hijau.", fact: "Buah anggur akan meledak jika dipanaskan di dalam microwave." },
                { word: "MANGGA", clue: "Buah tropis musiman yang harum dan manis, sangat populer jenis Harum Manis dan Gedong Gincu.", fact: "Mangga merupakan buah nasional negara India, Pakistan, dan Filipina." }
            ],
            negara: [
                { word: "INDONESIA", clue: "Negara kepulauan terbesar di dunia dengan semboyan Bhinneka Tunggal Ika.", fact: "Indonesia memiliki lebih dari 17.000 pulau dan merupakan rumah bagi ratusan bahasa daerah." },
                { word: "JEPANG", clue: "Dijuluki 'Negeri Matahari Terbit', terkenal dengan Gunung Fuji, bunga sakura, dan teknologi maju.", fact: "Jepang terdiri dari lebih dari 6.800 pulau dan memiliki angka harapan hidup tertinggi di dunia." },
                { word: "MESIR", clue: "Negara di Afrika Utara yang terkenal dengan Piramida Agung Giza, Sphinx, dan Sungai Nil.", fact: "Sungai Nil di Mesir adalah salah satu sungai terpanjang di dunia." },
                { word: "BRASIL", clue: "Negara terbesar di Amerika Selatan yang terkenal dengan hutan Amazon, karnaval, dan sepak bola.", fact: "Brasil merupakan negara penghasil biji kopi terbesar di dunia selama lebih dari 150 tahun." },
                { word: "ITALIA", clue: "Negara di Eropa yang petanya berbentuk seperti sepatu bot, terkenal dengan pizza, pasta, dan Colosseum.", fact: "Italia memiliki situs Warisan Dunia UNESCO terbanyak dibandingkan negara mana pun di dunia." },
                { word: "AUSTRALIA", clue: "Negara benua di belahan bumi selatan yang terkenal dengan Gedung Opera Sydney dan kanguru.", fact: "Populasi kanguru di Australia jauh lebih banyak dibandingkan jumlah penduduk manusianya." },
                { word: "PRANCIS", clue: "Negara Eropa yang ikonik dengan Menara Eiffel, Museum Louvre, dan mode dunia.", fact: "Prancis adalah negara yang paling banyak dikunjungi wisatawan di seluruh dunia." },
                { word: "KOREA SELATAN", clue: "Negeri Ginseng yang mendunia lewat budaya K-Pop, drama Korea, dan kuliner kimchi.", fact: "Sebagian besar penduduk Korea Selatan memiliki nama marga Kim, Lee, atau Park." },
                { word: "SWISS", clue: "Negara pegunungan Alpen yang terkenal netral, pembuat jam tangan mewah, dan cokelat lezat.", fact: "Swiss memiliki bunker perlindungan nuklir yang cukup untuk menampung seluruh populasinya." },
                { word: "BELANDA", clue: "Dijuluki 'Negeri Kincir Angin' dan bunga tulip, terkenal dengan sistem kanal dan tanggul lautnya.", fact: "Sekitar sepertiga wilayah negara Belanda berada di bawah permukaan air laut." },
                { word: "ARAB SAUDI", clue: "Negara di Timur Tengah tempat berdirinya Kota Suci Mekkah dan Madinah.", fact: "Arab Saudi adalah negara terbesar di dunia yang tidak memiliki satu pun sungai permanen." },
                { word: "KANADA", clue: "Negara di Amerika Utara dengan bendera bergambar daun maple dan memiliki garis pantai terpanjang.", fact: "Kanada memiliki lebih banyak danau dibandingkan gabungan seluruh danau di negara lain di dunia." },
                { word: "INDIA", clue: "Negara di Asia Selatan dengan populasi terbanyak, terkenal dengan Taj Mahal dan industri Bollywood.", fact: "Permainan catur pertama kali ditemukan di India pada abad ke-6." },
                { word: "TURKI", clue: "Negara unik yang wilayahnya terletak di dua benua sekaligus, yaitu Asia dan Eropa.", fact: "Kota Istanbul di Turki adalah satu-satunya kota besar di dunia yang berada di dua benua." },
                { word: "THAILAND", clue: "Dijuluki 'Negeri Gajah Putih', satu-satunya negara di Asia Tenggara yang tidak pernah dijajah Eropa.", fact: "Nama asli ibu kota Bangkok dalam bahasa setempat adalah nama kota terpanjang di dunia." },
                { word: "ISLANDIA", clue: "Negara pulau vulkanik di Eropa Utara dengan pemandangan aurora borealis dan sumber air panas.", fact: "Islandia adalah salah satu negara di dunia yang bebas dari nyamuk." },
                { word: "MEKSIKO", clue: "Negara di Amerika Utara yang terkenal dengan budaya suku Maya, Aztec, serta makanan taco.", fact: "Cokelat pertama kali ditemukan dan dinikmati sebagai minuman oleh peradaban kuno di Meksiko." },
                { word: "SELANDIA BARU", clue: "Negara kepulauan di Pasifik barat daya yang menjadi lokasi syuting film The Lord of the Rings.", fact: "Selandia Baru adalah negara pertama di dunia yang memberikan hak pilih kepada perempuan pada tahun 1893." },
                { word: "MAROKO", clue: "Negara di Afrika Utara dengan kota-kota eksotis seperti Casablanca dan Marrakesh.", fact: "Universitas tertua di dunia yang masih beroperasi (Al-Qarawiyyin) berada di kota Fez, Maroko." },
                { word: "SINGAPURA", clue: "Negara kota modern di Asia Tenggara yang memiliki ikon patung Merlion berkepala singa.", fact: "Singapura adalah salah satu dari hanya tiga negara kota berdaulat yang masih ada di dunia." },
                { word: "YUNANI", clue: "Negeri para dewa mitologi kuno dan tempat lahirnya sistem demokrasi serta Olimpiade.", fact: "Lebih dari 80% wilayah daratan Yunani terdiri dari pegunungan." },
                { word: "SPANYOL", clue: "Negara di Semenanjung Iberia yang terkenal dengan tarian Flamenco dan klub sepak bola kelas dunia.", fact: "Spanyol memiliki restoran tertua di dunia yang masih buka sejak tahun 1725 di Madrid." },
                { word: "ARGENTINA", clue: "Negara asal tarian Tango dan legenda sepak bola dunia di bagian selatan Amerika Latin.", fact: "Nama Argentina berasal dari kata Latin 'argentum' yang berarti perak." },
                { word: "FINLANDIA", clue: "Negara Nordik yang sering dinobatkan sebagai negara paling bahagia di dunia dengan ribuan danau.", fact: "Budaya sauna berasal dari Finlandia; jumlah sauna di sana hampir menyamai jumlah mobil." },
                { word: "MALAYSIA", clue: "Negara tetangga serumpun Indonesia yang memiliki ikon Menara Kembar Petronas di Kuala Lumpur.", fact: "Ruang gua bawah tanah terbesar di dunia (Sarawak Chamber) terletak di Malaysia." }
            ],
            profesi: [
                { word: "ASTRONOT", clue: "Penjelajah luar angkasa yang dilatih untuk mengemudikan pesawat antariksa.", fact: "Di luar angkasa, tinggi badan astronot bisa bertambah hingga 5 cm karena tidak ada gravitasi." },
                { word: "ARKEOLOG", clue: "Ilmuwan yang mempelajari sejarah manusia masa lalu melalui penggalian artefak kuno.", fact: "Arkeolog menggunakan kuas halus dan alat khusus agar tidak merusak benda bersejarah yang rapuh." },
                { word: "ARSITEK", clue: "Ahli yang merancang desain bangunan, gedung, dan tata ruang agar indah sekaligus kokoh.", fact: "Kata arsitek berasal dari bahasa Yunani 'arkhitekton' yang berarti pembangun utama." },
                { word: "DOKTER BEDAH", clue: "Tenaga medis spesialis yang melakukan tindakan operasi untuk mengobati penyakit atau cedera.", fact: "Dokter bedah membutuhkan konsentrasi tinggi dan ketelitian tangan yang luar biasa." },
                { word: "PILOT", clue: "Profesi yang bertugas menerbangkan pesawat terbang dan memastikan keselamatan penumpang di udara.", fact: "Dalam penerbangan komersial, pilot dan kopilot biasanya memakan menu makanan yang berbeda untuk keamanan." },
                { word: "NAHKODA", clue: "Pemimpin tertinggi di atas kapal laut yang bertanggung jawab atas pelayaran dan awak kapal.", fact: "Nahkoda memiliki wewenang hukum tertinggi selama kapal berada di tengah lautan." },
                { word: "KOKI", clue: "Juru masak profesional yang meracik resep dan mengolah hidangan lezat di restoran atau hotel.", fact: "Lipatan pada topi koki tradisional konon melambangkan 100 cara berbeda untuk memasak telur." },
                { word: "JURNALIS", clue: "Pencari, penulis, dan penyampai berita terkini kepada publik melalui media cetak maupun digital.", fact: "Jurnalisme sering disebut sebagai pilar keempat demokrasi." },
                { word: "APOTEKER", clue: "Ahli farmasi yang meracik obat dan memberikan informasi dosis yang tepat kepada pasien.", fact: "Apoteker harus memahami interaksi kimia antar berbagai jenis obat-obatan." },
                { word: "PUSTAKAWAN", clue: "Pengelola koleksi buku dan informasi di perpustakaan agar mudah ditemukan oleh pembaca.", fact: "Pustakawan modern juga ahli dalam mengarsipkan data dan literasi digital." },
                { word: "MASINIS", clue: "Orang yang bertanggung jawab mengoperasikan dan mengendalikan jalannya kereta api.", fact: "Masinis harus hafal setiap rambu, tikungan, dan kemiringan jalur rel yang dilaluinya." },
                { word: "PROGRAMMER", clue: "Pembuat aplikasi, situs web, dan perangkat lunak menggunakan berbagai bahasa pemrograman.", fact: "Programmer pertama di dunia adalah seorang wanita bernama Ada Lovelace pada abad ke-19." },
                { word: "METEOROLOG", clue: "Ilmuwan yang mempelajari atmosfer untuk memprediksi cuaca dan iklim harian.", fact: "Mereka menggunakan satelit cuaca, radar, dan balon udara untuk mengumpulkan data atmosfer." },
                { word: "DALANG", clue: "Seniman tradisional yang memainkan wayang sekaligus mengisi suara seluruh tokoh dalam pertunjukan.", fact: "Seorang dalang bisa memainkan pertunjukan wayang kulit semalam suntuk tanpa naskah tertulis." },
                { word: "FOTOGRAFER", clue: "Seniman yang menangkap momen, cahaya, dan cerita melalui lensa kamera.", fact: "Kata fotografi berasal dari bahasa Yunani yang berarti 'melukis dengan cahaya'." },
                { word: "AKUNTAN", clue: "Ahli keuangan yang mencatat, memeriksa, dan menyusun laporan pembukuan suatu perusahaan.", fact: "Sistem pembukuan berpasangan modern pertama kali dipopulerkan oleh Luca Pacioli pada tahun 1494." },
                { word: "PENGACARA", clue: "Ahli hukum yang memberikan nasihat dan membela klien di dalam maupun di luar pengadilan.", fact: "Profesi ini menuntut kemampuan analisis tajam dan retorika berbicara yang kuat." },
                { word: "VETERINER", clue: "Sebutan lain untuk dokter hewan yang merawat kesehatan hewan peliharaan maupun satwa liar.", fact: "Dokter hewan harus mempelajari anatomi puluhan spesies hewan yang berbeda-beda." },
                { word: "KOREOGRAFER", clue: "Pencipta gerakan tari dan pengatur komposisi penari dalam sebuah pertunjukan seni.", fact: "Koreografer memadukan irama musik dengan ekspresi gerak tubuh manusia." },
                { word: "BARISTA", clue: "Peracik kopi profesional yang ahli membuat espresso dan seni melukis di atas busa susu (latte art).", fact: "Kata barista berasal dari bahasa Italia yang berarti pelayan bar minuman." }
            ],
            benda: [
                { word: "TELESKOP", clue: "Alat optik yang digunakan para astronom untuk mengamati bintang dan planet yang jauh.", fact: "Galileo Galilei adalah orang pertama yang menggunakan teleskop untuk mengamati langit pada tahun 1609." },
                { word: "MIKROSKOP", clue: "Alat laboratorium untuk melihat benda-benda berukuran sangat kecil seperti bakteri dan sel.", fact: "Mikroskop elektron modern mampu memperbesar objek hingga jutaan kali lipat." },
                { word: "KOMPAS", clue: "Alat penunjuk arah mata angin yang jarum magnetnya selalu menunjuk ke arah utara dan selatan.", fact: "Kompas pertama kali ditemukan di Tiongkok kuno menggunakan batu magnet alami." },
                { word: "ANGKLUNG", clue: "Alat musik tradisional asal Jawa Barat yang terbuat dari tabung bambu dan dimainkan dengan digoyangkan.", fact: "Angklung telah diakui oleh UNESCO sebagai Warisan Budaya Takbenda Dunia sejak tahun 2010." },
                { word: "TERMOMETER", clue: "Alat pengukur suhu tubuh manusia maupun suhu udara di suatu ruangan.", fact: "Termometer modern menggunakan sensor digital inframerah yang bisa membaca suhu dalam hitungan detik." },
                { word: "KALEIDOSKOP", clue: "Tabung optik berisi cermin dan manik-manik warna-warni yang membentuk pola simetris indah saat diputar.", fact: "Nama kaleidoskop berasal dari bahasa Yunani yang berarti 'pengamat bentuk yang indah'." },
                { word: "SETRIKA", clue: "Alat rumah tangga yang dipanaskan untuk merapikan dan menghilangkan kerutan pada pakaian.", fact: "Pada zaman kuno, manusia menggunakan wajan logam panas berisi bara api untuk melicinkan pakaian." },
                { word: "PAYUNG", clue: "Alat pelindung lipat yang digunakan untuk melindungi tubuh dari guyuran hujan maupun terik matahari.", fact: "Payung pada awalnya diciptakan di Mesir kuno khusus untuk melindungi bangsawan dari sinar matahari." },
                { word: "KACAMATA", clue: "Alat bantu penglihatan berbingkai lensa yang dipakai di depan mata untuk mengatasi rabun.", fact: "Kacamata pertama kali dibuat di Italia sekitar tahun 1286." },
                { word: "GAMELAN", clue: "Ensembel alat musik tradisional Jawa dan Bali yang terdiri dari gong, kenong, saron, dan kendang.", fact: "Setiap set gamelan tradisional memiliki nada laras pelog atau slendro yang unik." },
                { word: "JAM PASIR", clue: "Alat pengukur waktu kuno berupa dua tabung kaca terhubung yang berisi butiran pasir halus.", fact: "Jam pasir sering digunakan di kapal laut zaman dahulu karena tidak terpengaruh oleh guncangan ombak." },
                { word: "BATERAI", clue: "Benda penyimpan energi kimia yang diubah menjadi arus listrik untuk menyalakan perangkat portabel.", fact: "Baterai modern pertama diciptakan oleh Alessandro Volta pada tahun 1800." },
                { word: "STETOSKOP", clue: "Alat medis yang digunakan dokter untuk mendengarkan detak jantung dan suara pernapasan pasien.", fact: "Stetoskop ditemukan pada tahun 1816 oleh dokter Prancis Rene Laennec menggunakan gulungan kertas." },
                { word: "HELM", clue: "Pelindung kepala keras yang wajib dipakai pengendara sepeda motor demi keselamatan.", fact: "Lapisan busa EPS di dalam helm dirancang untuk menyerap energi benturan saat terjadi kecelakaan." },
                { word: "KAMERA", clue: "Perangkat optik yang digunakan untuk merekam gambar diam maupun video.", fact: "Kamera digital pertama kali diciptakan pada tahun 1975 dan beratnya mencapai hampir 4 kilogram." },
                { word: "PRINTER", clue: "Perangkat keras komputer yang mencetak dokumen dan gambar digital ke atas lembaran kertas.", fact: "Saat ini sudah ada printer 3D yang mampu mencetak benda padat dari bahan plastik hingga logam." },
                { word: "GUNTING", clue: "Alat pemotong kertas atau kain yang terdiri dari dua bilah logam tajam yang bersilangan.", fact: "Gunting sudah digunakan manusia sejak 1.500 tahun sebelum Masehi di Mesir kuno." },
                { word: "KULKAS", clue: "Lemari pendingin elektronik untuk menjaga makanan dan minuman tetap segar lebih lama.", fact: "Suhu ideal di dalam ruang utama kulkas adalah sekitar 1 hingga 4 derajat Celcius." },
                { word: "GITAR", clue: "Alat musik berdawai yang dimainkan dengan cara dipetik menggunakan jari atau plektrum.", fact: "Gitar akustik standar umumnya memiliki 6 senar dengan nada dasar E-A-D-G-B-E." },
                { word: "KERIS", clue: "Senjata tikam tradisional Nusantara berbilah berkelok-kelok yang memiliki nilai seni dan sejarah tinggi.", fact: "Keris Indonesia juga telah terdaftar sebagai Karya Agung Warisan Budaya Lisan dan Takbenda oleh UNESCO." }
            ]
        };

        // Konfigurasi Metadata Kategori
        const CATEGORY_META = {
            binatang: { label: "🦁 Binatang", color: "indigo" },
            buah: { label: "🍉 Buah-buahan", color: "pink" },
            negara: { label: "🌍 Negara", color: "emerald" },
            profesi: { label: "👨‍🚀 Profesi", color: "amber" },
            benda: { label: "🎸 Benda", color: "cyan" },
            semua: { label: "🎲 Acak Semua", color: "purple" }
        };

        // ==========================================
        // 2. STATE & VARIABEL PERMAINAN
        // ==========================================
        let currentCategory = 'semua';
        let currentWordObj = null;
        let targetWord = "";
        let guessedLetters = new Set();
        let hintedLetters = new Set();
        let lives = 6;
        const MAX_LIVES = 6;
        let score = 0;
        let streak = 0;
        let usedWords = new Set();
        let isRoundOver = false;
        let soundEnabled = true;

        // Tata letak Keyboard QWERTY
        const KEYBOARD_ROWS = [
            ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
            ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
            ['Z', 'X', 'C', 'V', 'B', 'N', 'M']
        ];

        // ==========================================
        // 3. SISTEM EFEK SUARA (TONE.JS)
        // ==========================================
        let synth = null;
        let polySynth = null;

        function initAudio() {
            if (!synth && typeof Tone !== 'undefined') {
                synth = new Tone.Synth({
                    oscillator: { type: "triangle" },
                    envelope: { attack: 0.01, decay: 0.1, sustain: 0.05, release: 0.2 }
                }).toDestination();

                polySynth = new Tone.PolySynth(Tone.Synth).toDestination();
                polySynth.volume.value = -6;
            }
        }

        function playSound(type) {
            if (!soundEnabled) return;
            try {
                if (Tone.context.state !== 'running') {
                    Tone.start();
                }
                initAudio();
                if (!synth) return;

                const now = Tone.now();
                switch (type) {
                    case 'click':
                        synth.triggerAttackRelease("C5", "32n", now);
                        break;
                    case 'correct':
                        synth.triggerAttackRelease("E5", "16n", now);
                        synth.triggerAttackRelease("G5", "16n", now + 0.07);
                        break;
                    case 'wrong':
                        synth.triggerAttackRelease("A2", "8n", now);
                        break;
                    case 'hint':
                        synth.triggerAttackRelease("B5", "16n", now);
                        synth.triggerAttackRelease("E6", "16n", now + 0.08);
                        break;
                    case 'win':
                        polySynth.triggerAttackRelease(["C5", "E5", "G5"], "8n", now);
                        polySynth.triggerAttackRelease(["C5", "E5", "G5", "C6"], "4n", now + 0.15);
                        break;
                    case 'lose':
                        synth.triggerAttackRelease("G3", "8n", now);
                        synth.triggerAttackRelease("F#3", "8n", now + 0.15);
                        synth.triggerAttackRelease("F3", "4n", now + 0.3);
                        break;
                }
            } catch (e) {
                console.warn("Audio playback skipped:", e);
            }
        }

        function toggleSound() {
            soundEnabled = !soundEnabled;
            document.getElementById('soundIcon').textContent = soundEnabled ? '🔊' : '🔇';
            if (soundEnabled) playSound('click');
        }

        // ==========================================
        // 4. INISIALISASI & NAVIGASI LAYAR
        // ==========================================
        window.onload = function() {
            renderKeyboard();
            setupPhysicalKeyboard();
        };

        function startGame(category) {
            playSound('click');
            currentCategory = category;
            document.getElementById('categoryScreen').classList.add('hidden');
            document.getElementById('playScreen').classList.remove('hidden');
            document.getElementById('playScreen').classList.add('flex');
            
            loadNewWord();
        }

        function backToMenu() {
            playSound('click');
            document.getElementById('playScreen').classList.add('hidden');
            document.getElementById('playScreen').classList.remove('flex');
            document.getElementById('categoryScreen').classList.remove('hidden');
            closeModal();
        }

        // ==========================================
        // 5. LOGIKA INTI PERMAINAN
        // ==========================================
        function getPoolForCategory(cat) {
            if (cat === 'semua') {
                let all = [];
                for (const key in WORD_DATABASE) {
                    WORD_DATABASE[key].forEach(item => {
                        all.push({ ...item, actualCategory: key });
                    });
                }
                return all;
            } else {
                return WORD_DATABASE[cat].map(item => ({ ...item, actualCategory: cat }));
            }
        }

        function loadNewWord() {
            isRoundOver = false;
            guessedLetters.clear();
            hintedLetters.clear();
            lives = MAX_LIVES;

            const pool = getPoolForCategory(currentCategory);
            // Filter kata yang belum dipakai
            let available = pool.filter(item => !usedWords.has(item.word.toUpperCase()));
            
            // Jika semua kata dalam kategori sudah dimainkan, reset riwayat
            if (available.length === 0) {
                usedWords.clear();
                available = pool;
            }

            // Pilih kata secara acak
            const randomIndex = Math.floor(Math.random() * available.length);
            currentWordObj = available[randomIndex];
            targetWord = currentWordObj.word.toUpperCase();
            usedWords.add(targetWord);

            // Update UI Badge Kategori
            const displayCat = CATEGORY_META[currentWordObj.actualCategory].label;
            document.getElementById('categoryBadge').textContent = displayCat;

            // Update Petunjuk
            document.getElementById('clueText').textContent = currentWordObj.clue;

            // Reset Pesan Feedback
            setFeedback("Pilih huruf di bawah untuk menebak!", "text-slate-400");

            // Render ulang komponen UI
            renderLives();
            renderWordSlots();
            resetKeyboardUI();
            updateHintButton();
        }

        function renderLives() {
            const container = document.getElementById('livesContainer');
            container.innerHTML = '';
            for (let i = 0; i < MAX_LIVES; i++) {
                const heart = document.createElement('span');
                if (i < lives) {
                    heart.textContent = '❤️';
                    heart.className = 'text-sm sm:text-base transition-all duration-300 ' + (lives <= 2 ? 'heart-active' : '');
                } else {
                    heart.textContent = '🖤';
                    heart.className = 'text-sm sm:text-base opacity-40 scale-90 transition-all duration-300';
                }
                container.appendChild(heart);
            }
        }

        function renderWordSlots() {
            const container = document.getElementById('wordSlotsContainer');
            container.innerHTML = '';

            // Pisahkan berdasarkan spasi jika kata lebih dari 1 kata (misal: LUMBA LUMBA)
            const words = targetWord.split(' ');

            words.forEach((wordPart, wIdx) => {
                const wordWrapper = document.createElement('div');
                wordWrapper.className = 'flex flex-wrap justify-center gap-1.5 sm:gap-2 ' + (words.length > 1 ? 'mx-2 my-1' : '');

                for (let i = 0; i < wordPart.length; i++) {
                    const char = wordPart[i];
                    const slot = document.createElement('div');
                    const isRevealed = guessedLetters.has(char);
                    const isHinted = hintedLetters.has(char);

                    // Ukuran responsif menyesuaikan panjang kata
                    const sizeClass = targetWord.length > 10 
                        ? 'w-8 h-10 sm:w-10 sm:h-12 text-lg sm:text-xl' 
                        : 'w-9 h-11 sm:w-12 sm:h-14 text-xl sm:text-2xl';

                    slot.className = `letter-slot ${sizeClass} rounded-xl flex items-center justify-center font-extrabold uppercase select-none`;
                    
                    if (isRevealed) {
                        slot.textContent = char;
                        slot.classList.add(isHinted ? 'hinted' : 'revealed');
                    } else {
                        slot.textContent = '';
                    }

                    wordWrapper.appendChild(slot);
                }
                container.appendChild(wordWrapper);
            });
        }

        function renderKeyboard() {
            const container = document.getElementById('keyboardContainer');
            container.innerHTML = '';

            KEYBOARD_ROWS.forEach(row => {
                const rowDiv = document.createElement('div');
                rowDiv.className = 'flex justify-center gap-1 sm:gap-1.5';

                row.forEach(letter => {
                    const btn = document.createElement('button');
                    btn.id = `key-${letter}`;
                    btn.textContent = letter;
                    btn.className = 'key-btn flex-1 max-w-[44px] h-11 sm:h-12 rounded-xl font-bold text-sm sm:text-base text-slate-100 flex items-center justify-center cursor-pointer';
                    btn.onclick = () => handleGuess(letter);
                    rowDiv.appendChild(btn);
                });

                container.appendChild(rowDiv);
            });
        }

        function resetKeyboardUI() {
            KEYBOARD_ROWS.flat().forEach(letter => {
                const btn = document.getElementById(`key-${letter}`);
                if (btn) {
                    btn.disabled = false;
                    btn.classList.remove('correct', 'wrong');
                }
            });
        }

        function handleGuess(letter, isFromHint = false) {
            if (isRoundOver || guessedLetters.has(letter)) return;

            guessedLetters.add(letter);
            if (isFromHint) {
                hintedLetters.add(letter);
            }

            const keyBtn = document.getElementById(`key-${letter}`);

            // Cek apakah huruf ada di dalam kata target
            if (targetWord.includes(letter)) {
                if (!isFromHint) playSound('correct');
                if (keyBtn) {
                    keyBtn.classList.add('correct');
                    keyBtn.disabled = true;
                }
                renderWordSlots();
                if (!isFromHint) {
                    setFeedback(`Bagus! Huruf "${letter}" ada di dalam kata.`, "text-emerald-400");
                }
                checkWinCondition();
            } else {
                playSound('wrong');
                if (keyBtn) {
                    keyBtn.classList.add('wrong');
                    keyBtn.disabled = true;
                }
                lives--;
                renderLives();

                // Efek getar pada kartu soal
                const card = document.getElementById('questionCard');
                card.classList.remove('animate-shake');
                void card.offsetWidth; // trigger reflow
                card.classList.add('animate-shake');

                setFeedback(`Ups! Tidak ada huruf "${letter}".`, "text-rose-400");
                checkLoseCondition();
            }
            updateHintButton();
        }

        function useHint() {
            if (isRoundOver) return;

            // Cari huruf unik yang belum ditebak
            const uniqueLetters = [...new Set(targetWord.replace(/\s+/g, '').split(''))];
            const unrevealed = uniqueLetters.filter(l => !guessedLetters.has(l));

            if (unrevealed.length === 0) return;

            // Kurangi skor jika punya poin (minimal 0)
            if (score >= 20) {
                score -= 20;
                updateScoreUI();
            }

            playSound('hint');
            const randomLetter = unrevealed[Math.floor(Math.random() * unrevealed.length)];
            setFeedback(`💡 Petunjuk digunakan! Huruf "${randomLetter}" terbuka.`, "text-amber-300");
            handleGuess(randomLetter, true);
        }

        function updateHintButton() {
            const uniqueLetters = [...new Set(targetWord.replace(/\s+/g, '').split(''))];
            const unrevealed = uniqueLetters.filter(l => !guessedLetters.has(l));
            const hintBtn = document.getElementById('hintBtn');
            // Nonaktifkan hint jika tinggal 1 huruf tersisa atau ronde selesai
            hintBtn.disabled = isRoundOver || unrevealed.length <= 1;
        }

        function skipWord() {
            if (isRoundOver) return;
            playSound('wrong');
            streak = 0;
            updateScoreUI();
            isRoundOver = true;
            showResultModal(false, true);
        }

        function checkWinCondition() {
            const lettersOnly = targetWord.replace(/\s+/g, '').split('');
            const allRevealed = lettersOnly.every(char => guessedLetters.has(char));

            if (allRevealed) {
                isRoundOver = true;
                // Hitung bonus skor berdasarkan sisa nyawa & streak
                const basePoints = 50;
                const lifeBonus = lives * 10;
                const streakBonus = streak * 5;
                const totalEarned = basePoints + lifeBonus + streakBonus;

                score += totalEarned;
                streak++;
                updateScoreUI();

                setTimeout(() => {
                    playSound('win');
                    showResultModal(true, false, totalEarned);
                }, 400);
            }
        }

        function checkLoseCondition() {
            if (lives <= 0) {
                isRoundOver = true;
                streak = 0;
                updateScoreUI();

                // Buka semua huruf yang tersisa agar pemain tahu jawabannya
                targetWord.replace(/\s+/g, '').split('').forEach(char => guessedLetters.add(char));
                renderWordSlots();

                setTimeout(() => {
                    playSound('lose');
                    showResultModal(false, false);
                }, 500);
            }
        }

        function updateScoreUI() {
            document.getElementById('scoreDisplay').textContent = score;
            document.getElementById('streakDisplay').textContent = streak;
        }

        function setFeedback(text, colorClass) {
            const el = document.getElementById('feedbackText');
            el.textContent = text;
            el.className = `text-sm font-semibold transition-all ${colorClass}`;
        }

        // ==========================================
        // 6. MODAL HASIL & KONTROL KEYBOARD FISIK
        // ==========================================
        function showResultModal(isWin, isSkipped = false, earnedPoints = 0) {
            const modal = document.getElementById('resultModal');
            const content = document.getElementById('modalContent');
            const emoji = document.getElementById('modalEmoji');
            const title = document.getElementById('modalTitle');
            const subtitle = document.getElementById('modalSubtitle');
            const wordEl = document.getElementById('modalWord');
            const factEl = document.getElementById('modalFact');

            wordEl.textContent = targetWord;
            factEl.textContent = `💡 Fakta: ${currentWordObj.fact}`;

            if (isWin) {
                emoji.textContent = '🎉';
                title.textContent = 'Tebakan Tepat!';
                title.className = 'text-2xl sm:text-3xl font-bold mb-1 text-emerald-400';
                subtitle.textContent = `Hebat! Kamu mendapatkan +${earnedPoints} Poin!`;
            } else if (isSkipped) {
                emoji.textContent = '⏭️';
                title.textContent = 'Kata Dilewati';
                title.className = 'text-2xl sm:text-3xl font-bold mb-1 text-amber-400';
                subtitle.textContent = 'Tidak masalah, ayo coba kata berikutnya!';
            } else {
                emoji.textContent = '💔';
                title.textContent = 'Kesempatan Habis!';
                title.className = 'text-2xl sm:text-3xl font-bold mb-1 text-rose-400';
                subtitle.textContent = 'Jangan menyerah, pelajari jawabannya dan lanjut main!';
            }

            modal.classList.remove('hidden');
            modal.classList.add('flex');
            setTimeout(() => {
                content.classList.remove('scale-95');
                content.classList.add('scale-100');
            }, 10);
        }

        function closeModal() {
            const modal = document.getElementById('resultModal');
            const content = document.getElementById('modalContent');
            content.classList.remove('scale-100');
            content.classList.add('scale-95');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }

        function nextRound() {
            playSound('click');
            closeModal();
            loadNewWord();
        }

        function setupPhysicalKeyboard() {
            document.addEventListener('keydown', (e) => {
                const playScreenVisible = !document.getElementById('playScreen').classList.contains('hidden');
                const modalVisible = !document.getElementById('resultModal').classList.contains('hidden');

                // Tekan Enter saat modal hasil muncul untuk lanjut ke kata berikutnya
                if (modalVisible && e.key === 'Enter') {
                    e.preventDefault();
                    nextRound();
                    return;
                }

                if (!playScreenVisible || isRoundOver) return;

                const key = e.key.toUpperCase();
                if (/^[A-Z]$/.test(key)) {
                    handleGuess(key);
                }
            });
        }
    </script>
</body>
</html>
