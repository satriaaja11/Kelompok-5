<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pengusir Malas - Ayo Fokus!</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#165DFF',
                        secondary: '#36CFC9',
                        accent: '#F53F3F',
                        light: '#F2F3F5',
                        dark: '#1D2129',
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .shadow-soft {
                box-shadow: 0 8px 32px rgba(22, 93, 255, 0.15);
            }
            .animate-bounce-slow {
                animation: bounce 2s infinite;
            }
            @keyframes bounce {
                0%, 100% { transform: translateY(0); }
                50% { transform: translateY(-10px); }
            }
            .press-active {
                transform: scale(0.92) !important;
                filter: brightness(0.9) !important;
            }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-cyan-50 min-h-screen font-sans text-dark">
    <div class="container mx-auto px-4 py-8 max-w-3xl">
        <!-- Header -->
        <header class="text-center mb-10">
            <h1 class="text-[clamp(2rem,5vw,3rem)] font-bold text-primary mb-3">
                <i class="fa fa-hand-paper-o mr-3"></i>Pengusir Malas
            </h1>
            <p class="text-gray-600 text-lg">Tekan dan tahan gambar pulpen di bawah ini ✊. Jangan dilepas jika ingin jadi produktif!</p>
        </header>

        <!-- Main Card -->
        <div class="bg-white rounded-[20px] p-8 shadow-soft relative overflow-hidden">
            <div class="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-primary to-secondary"></div>

            <!-- Area Interaksi -->
            <div class="flex flex-col items-center justify-center py-10">
                <p id="instruksi" class="text-gray-700 font-medium mb-6 text-center">Tekan dan tahan pulpen ini sekarang!</p>
                
                <!-- Elemen Gambar Pulpen -->
                <div id="areaTekan" class="cursor-pointer select-none transition-all duration-200 ease-out mb-8 animate-bounce-slow">
                    <img src="https://cdn-icons-png.flaticon.com/512/2826/2826631.png" 
                         alt="Ikon Pulpen" 
                         class="w-40 h-40 object-contain drop-shadow-lg">
                </div>

                <!-- Hasil Waktu -->
                <div class="text-center mb-6">
                    <p class="text-gray-500 mb-2">Kamu bertahan selama:</p>
                    <p id="waktuHasil" class="text-[clamp(1.8rem,4vw,2.5rem)] font-bold text-primary">0.00 <span class="text-lg">detik</span></p>
                </div>

                <!-- Pesan Peringatan -->
                <div id="pesanPeringatan" class="hidden bg-accent/10 text-accent px-6 py-3 rounded-xl font-medium flex items-center gap-2">
                    <i class="fa fa-exclamation-circle"></i>
                    <span>Ayo Fokus! Jangan malas lagi ya!</span>
                </div>
            </div>

            <!-- Riwayat Singkat -->
            <div class="mt-8 pt-6 border-t border-gray-200">
                <h3 class="font-semibold text-gray-700 mb-2">Rekor Terbaru:</h3>
                <p id="rekorTerbaru" class="text-gray-600 italic">Belum ada catatan. Mulai sekarang!</p>
            </div>
        </div>

        <!-- Footer -->
        <footer class="text-center mt-8 text-gray-500 text-sm">
            <p>✦ Semangat terus, kesuksesan menantimu! ✦</p>
        </footer>
    </div>

    <!-- Elemen Audio -->
    <audio id="suarafokus" preload="auto">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" type="audio/ogg">
        <source src="https://actions.google.com/sounds/v1/alarms/beep_short.mp3" type="audio/mpeg">
        Browser kamu tidak mendukung pemutar suara.
    

    <script>
        // Deklarasi Variabel
        const areaTekan = document.getElementById('areaTekan');
        const instruksi = document.getElementById('instruksi');
        const waktuHasil = document.getElementById('waktuHasil');
        const pesanPeringatan = document.getElementById('pesanPeringatan');
        const rekorTerbaru = document.getElementById('rekorTerbaru');
        const suarafokus = document.getElementById('suarafokus');

        let waktuMulai;
        let intervalWaktu;
        let sedangMenekan = false;
        let rekor = 0;

        // Fungsi saat tombol ditekan
        function mulaiTahan(e) {
            e.preventDefault();
            sedangMenekan = true;
            areaTekan.classList.add('press-active');
            pesanPeringatan.classList.add('hidden');
            instruksi.textContent = "Bagus! Terus tahan... Jangan dilepas!";
            instruksi.className = "text-green-600 font-medium mb-6 text-center";

            // Catat waktu mulai
            waktuMulai = Date.now();
            
            // Hitung waktu berjalan
            clearInterval(intervalWaktu);
            intervalWaktu = setInterval(() => {
                const sekarang = Date.now();
                const durasi = (sekarang - waktuMulai) / 1000;
                waktuHasil.innerHTML = `${durasi.toFixed(2)} <span class="text-lg">detik</span>`;
            }, 10);
        }

        // Fungsi saat tombol dilepas
        function lepasTahan() {
            if (!sedangMenekan) return;
            
            sedangMenekan = false;
            areaTekan.classList.remove('press-active');
            clearInterval(intervalWaktu);

            // Hitung total durasi
            const waktuSelesai = Date.now();
            const durasiAkhir = (waktuSelesai - waktuMulai) / 1000;
            
            // Tampilkan pesan peringatan dan suara
            pesanPeringatan.classList.remove('hidden');
            suarafokus.currentTime = 0;
            suarafokus.play().catch(err => console.log("Autoplay dibatasi browser: ", err));

            // Ubah instruksi
            instruksi.textContent = "Yah dilepas nih... Ayo coba lagi!";
            instruksi.className = "text-accent font-medium mb-6 text-center";

            // Simpan rekor jika lebih lama
            if (durasiAkhir > rekor) {
                rekor = durasiAkhir;
                rekorTerbaru.textContent = `${rekor.toFixed(2)} detik (Rekor baru!)`;
            } else {
                rekorTerbaru.textContent = `${durasiAkhir.toFixed(2)} detik | Rekor tertinggi: ${rekor.toFixed(2)} detik`;
            }
        }

        // Event Listener untuk Desktop
        areaTekan.addEventListener('mousedown', mulaiTahan);
        document.addEventListener('mouseup', lepasTahan);

        // Event Listener untuk HP/Layar Sentuh
        areaTekan.addEventListener('touchstart', mulaiTahan);
        document.addEventListener('touchend', lepasTahan);
        document.addEventListener('touchcancel', lepasTahan);
    </script>
</body>
</html>
