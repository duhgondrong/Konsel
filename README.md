<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TKP (Terima Konseling Problematik)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .sidebar-icon { transition: all 0.2s ease-in-out; }
        .sidebar-icon:hover { background-color: #3b82f6; color: white; }
        [x-cloak] { display: none !important; }
        /* Style untuk konten chat yang memiliki pre-wrap */
        .whitespace-pre-wrap { white-space: pre-wrap; }
    </style>
</head>
<body class="bg-gray-200 antialiased">

    <!-- Container Utama -->
    <div id="app" class="h-screen flex flex-col">

        <!-- Layar Login -->
        <div id="login-screen" class="flex items-center justify-center h-full bg-cover bg-center" style="background-image: url('photo-1698689171085-31ea060a8a8e.jpg');">
            <div class="w-full max-w-md p-8 space-y-8 bg-white/90 backdrop-blur-sm rounded-2xl shadow-lg text-center">
                <div>
                    <i class="fas fa-hands-helping fa-3x text-blue-500"></i>
                    <h2 class="mt-4 text-3xl font-bold text-gray-900">
                        Selamat Datang di TKP
                    </h2>
                    <p class="mt-2 text-gray-600">
                        Pilih peran Anda untuk masuk ke sistem.
                    </p>
                </div>
                <div class="space-y-4">
                    <button onclick="login('siswa')" class="w-full flex items-center justify-center gap-3 py-3 px-4 text-lg font-semibold rounded-lg text-white bg-blue-600 hover:bg-blue-700 transition-colors">
                        <i class="fas fa-user-graduate"></i> Masuk sebagai Siswa
                    </button>
                    <button onclick="openModal('guru-password-modal')" class="w-full flex items-center justify-center gap-3 py-3 px-4 text-lg font-semibold rounded-lg text-white bg-green-600 hover:bg-green-700 transition-colors">
                        <i class="fas fa-chalkboard-teacher"></i> Masuk sebagai Guru BK
                    </button>
                    <button onclick="login('orangtua')" class="w-full flex items-center justify-center gap-3 py-3 px-4 text-lg font-semibold rounded-lg text-white bg-purple-600 hover:bg-purple-700 transition-colors">
                        <i class="fas fa-user-shield"></i> Masuk sebagai Orang Tua
                    </button>
                    <button onclick="openModal('admin-password-modal')" class="w-full flex items-center justify-center gap-3 py-3 px-4 text-lg font-semibold rounded-lg text-white bg-gray-800 hover:bg-gray-900 transition-colors">
                        <i class="fas fa-user-cog"></i> Masuk sebagai Admin
                    </button>
                </div>
                 <p id="login-error" class="text-sm text-red-600 h-6 mt-2"></p> <!-- Elemen untuk mesej ralat -->
                 <p id="auth-status" class="text-xs text-gray-500 pt-4">Mengautentikasi...</p>
                 <p class="text-xs text-gray-400">Ini adalah prototipe. Data akan direset secara berkala.</p>
            </div>
        </div>

        <!-- Tampilan Aplikasi Utama (Setelah Login) -->
        <div id="main-app" class="hidden flex h-full">
            <!-- Sidebar -->
            <aside class="w-64 bg-white shadow-md flex flex-col">
                <div class="p-6 text-center border-b">
                    <h1 class="text-2xl font-bold text-blue-600"><i class="fas fa-hands-helping"></i> TKP</h1>
                    <p id="user-role-display" class="text-sm text-gray-500 mt-1 capitalize"></p>
                </div>
                <nav id="sidebar-nav" class="flex-1 p-4 space-y-2">
                    <!-- Navigasi dinamis akan ditambahkan di sini oleh JavaScript -->
                </nav>
                <div class="p-4 border-t">
                     <p class="text-xs text-gray-600">User ID:</p>
                     <p id="user-id-display" class="text-xs text-gray-500 break-words"></p>
                    <button onclick="logout()" class="w-full mt-4 flex items-center justify-center gap-2 py-2 px-4 font-semibold rounded-lg text-red-600 bg-red-100 hover:bg-red-200 transition-colors">
                        <i class="fas fa-sign-out-alt"></i> Logout
                    </button>
                </div>
            </aside>

            <!-- Konten Utama -->
            <main class="flex-1 p-6 md:p-8 overflow-y-auto bg-cover bg-center bg-fixed" style="background-image: url('premium_photo-1700182582584-7411ec09675e.jpg');">
                
                <!-- Konten dinamis akan ditampilkan di sini -->
                <div id="content-area">
                    <!-- Dashboard -->
                    <div id="dashboard-view" class="page-view"></div>
                    
                    <!-- Profil -->
                    <div id="profil-view" class="page-view hidden">
                        <h2 class="text-2xl font-bold mb-4 text-white drop-shadow-lg">Profil Saya</h2>
                        <div class="bg-white/90 backdrop-blur-sm p-6 rounded-lg shadow">
                            <p>Fitur profil akan menampilkan detail pengguna sesuai dengan perannya.</p>
                        </div>
                    </div>
                    
                    <!-- Jadwal Konseling -->
                    <div id="jadwal-view" class="page-view hidden"></div>
                    
                    <!-- Materi BK -->
                    <div id="materi-view" class="page-view hidden"></div>
                    
                    <!-- Laporan & Keluhan -->
                    <div id="laporan-view" class="page-view hidden"></div>
                    
                    <!-- Kegiatan Sekolah -->
                    <div id="kegiatan-view" class="page-view hidden"></div>

                    <!-- AI Chat View -->
                    <div id="ai-chat-view" class="page-view hidden h-full flex flex-col"></div>

                    <!-- Admin: Kelola User View -->
                    <div id="kelola-user-view" class="page-view hidden"></div>
                </div>
            </main>
        </div>
    </div>
    
    <!-- Modal Booking Janji Temu -->
    <div id="booking-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-lg">
            <h3 class="text-xl font-bold mb-4">Buat Janji Konseling</h3>
            <div class="space-y-4">
                <div>
                    <label for="counselor-select" class="block text-sm font-medium text-gray-700">Pilih Guru BK</label>
                    <select id="counselor-select" class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500"></select>
                </div>
                <div>
                    <label for="booking-date" class="block text-sm font-medium text-gray-700">Tanggal</label>
                    <input type="date" id="booking-date" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm">
                </div>
                <div>
                    <label for="booking-topic" class="block text-sm font-medium text-gray-700">Topik Konseling</label>
                    <textarea id="booking-topic" rows="3" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Contoh: Kesulitan belajar, masalah pertemanan, rencana karir, dll."></textarea>
                </div>
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('booking-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="submitBooking()" class="py-2 px-4 bg-blue-600 text-white rounded-md hover:bg-blue-700">Ajukan Janji</button>
            </div>
        </div>
    </div>
    
    <!-- Modal Materi BK -->
    <div id="material-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-xl">
            <h3 id="material-modal-title" class="text-xl font-bold mb-4">Buat Materi Baru</h3>
            <div class="space-y-4">
                <div>
                    <label for="material-title" class="block text-sm font-medium text-gray-700">Judul Materi</label>
                    <input type="text" id="material-title" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Contoh: Tips Manajemen Stres">
                </div>
                <div>
                    <label for="material-content" class="block text-sm font-medium text-gray-700">Isi Materi</label>
                    <textarea id="material-content" rows="8" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Tulis konten edukatif di sini..."></textarea>
                </div>
                <input type="hidden" id="material-doc-id">
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('material-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="submitMaterial()" class="py-2 px-4 bg-green-600 text-white rounded-md hover:bg-green-700">Simpan Materi</button>
            </div>
        </div>
    </div>

    <!-- Modal Laporan/Keluhan -->
    <div id="report-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-lg">
            <h3 class="text-xl font-bold mb-4">Ajukan Laporan/Keluhan Baru</h3>
            <div class="space-y-4">
                <div>
                    <label for="report-type" class="block text-sm font-medium text-gray-700">Jenis Laporan</label>
                    <select id="report-type" class="mt-1 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm">
                        <option value="Kehilangan Barang">Kehilangan Barang</option>
                        <option value="Pembullyan">Pembullyan</option>
                        <option value="Keluhan Individu">Keluhan Individu</option>
                        <option value="Keluhan Kelompok">Keluhan Kelompok</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>
                <div>
                    <label for="report-subject" class="block text-sm font-medium text-gray-700">Subjek (Singkat)</label>
                    <input type="text" id="report-subject" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Contoh: Dompet hilang di kantin">
                </div>
                <div>
                    <label for="report-details" class="block text-sm font-medium text-gray-700">Detail Laporan</label>
                    <textarea id="report-details" rows="5" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Jelaskan secara rinci kejadian atau keluhan Anda."></textarea>
                </div>
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('report-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="submitReport()" class="py-2 px-4 bg-red-600 text-white rounded-md hover:bg-red-700">Kirim Laporan</button>
            </div>
        </div>
    </div>

    <!-- Modal Kegiatan Sekolah -->
    <div id="activity-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-xl">
            <h3 id="activity-modal-title" class="text-xl font-bold mb-4">Buat Kegiatan Baru</h3>
            <div class="space-y-4">
                <div>
                    <label for="activity-title" class="block text-sm font-medium text-gray-700">Nama Kegiatan</label>
                    <input type="text" id="activity-title" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Contoh: Lomba Kebersihan Kelas">
                </div>
                <div>
                    <label for="activity-date" class="block text-sm font-medium text-gray-700">Tanggal Kegiatan</label>
                    <input type="date" id="activity-date" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm">
                </div>
                <div>
                    <label for="activity-content" class="block text-sm font-medium text-gray-700">Deskripsi/Detail</label>
                    <textarea id="activity-content" rows="5" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm" placeholder="Jelaskan detail, tujuan, dan sasaran kegiatan."></textarea>
                </div>
                <input type="hidden" id="activity-doc-id">
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('activity-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="submitActivity()" class="py-2 px-4 bg-blue-600 text-white rounded-md hover:bg-blue-700">Simpan Kegiatan</button>
            </div>
        </div>
    </div>

    <!-- Modal Password Guru BK (BARU) -->
    <div id="guru-password-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-sm">
            <h3 class="text-xl font-bold mb-4">Login Guru BK</h3>
            <p class="text-sm text-gray-600 mb-4">Silakan masukkan kata sandi untuk melanjutkan.</p>
            <div>
                <label for="guru-password-input" class="block text-sm font-medium text-gray-700">Kata Sandi</label>
                <input type="password" id="guru-password-input" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                <p id="guru-password-error" class="text-red-600 text-sm mt-2 h-5"></p> <!-- Area pesan error -->
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('guru-password-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="handleGuruLogin()" class="py-2 px-4 bg-green-600 text-white rounded-md hover:bg-green-700">Masuk</button>
            </div>
        </div>
    </div>

    <!-- Modal Password Admin (BARU) -->
    <div id="admin-password-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-sm">
            <h3 class="text-xl font-bold mb-4">Login Admin</h3>
            <p class="text-sm text-gray-600 mb-4">Silakan masukkan kata sandi admin untuk melanjutkan.</p>
            <div>
                <label for="admin-password-input" class="block text-sm font-medium text-gray-700">Kata Sandi</label>
                <input type="password" id="admin-password-input" class="mt-1 block w-full py-2 px-3 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
                <p id="admin-password-error" class="text-red-600 text-sm mt-2 h-5"></p> <!-- Area pesan error -->
            </div>
            <div class="mt-6 flex justify-end gap-3">
                <button onclick="closeModal('admin-password-modal')" class="py-2 px-4 bg-gray-200 text-gray-800 rounded-md hover:bg-gray-300">Batal</button>
                <button onclick="handleAdminLogin()" class="py-2 px-4 bg-gray-800 text-white rounded-md hover:bg-gray-900">Masuk</button>
            </div>
        </div>
    </div>


    <script type="module">
        // Import Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signOut, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, doc, addDoc, onSnapshot, query, where, updateDoc, setDoc, getDocs, deleteDoc, Timestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // --- KONFIGURASI FIREBASE ---
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : { apiKey: "DEMO", authDomain: "DEMO", projectId: "DEMO" };
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        // --- GLOBAL STATE ---
        let currentUser = null;
        let userRole = null;
        let userId = null;
        let userName = null; // New global state for user name
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        let allMaterials = []; 
        let allReports = [];
        let allActivities = [];
        let allUsers = []; // State untuk menyimpan semua data pengguna (untuk Admin)

        const loginScreen = document.getElementById('login-screen');
        const mainApp = document.getElementById('main-app');
        const sidebarNav = document.getElementById('sidebar-nav');
        const authStatus = document.getElementById('auth-status');
        let quoteInterval; // Untuk mengontrol siklus kutipan

        // Data Kutipan Filosofis Tak Terbatas
        const philosophicalQuotes = [
            "Pendidikan adalah senjata paling mematikan di dunia, karena dengan pendidikan, Anda dapat mengubah dunia. - Nelson Mandela",
            "Mimpi-mimpi terbesar dimulai dari pikiran yang tenang. - Dalai Lama",
            "Masa depan adalah milik mereka yang percaya pada keindahan mimpi-mimpi mereka. - Eleanor Roosevelt",
            "Jadilah perubahan yang ingin kamu lihat di dunia. - Mahatma Gandhi",
            "Hidup adalah 10% apa yang terjadi pada kita dan 90% bagaimana kita bereaksi terhadapnya. - Charles R. Swindoll",
            "Kegagalan adalah kesempatan untuk memulai lagi dengan lebih cerdas. - Henry Ford",
            "Kebaikan adalah bahasa yang dapat didengar oleh orang tuli dan dilihat oleh orang buta. - Mark Twain",
            "Bukan seberapa banyak yang kita miliki, tapi seberapa banyak yang kita nikmati, itulah kebahagiaan. - Charles Spurgeon",
            "Satu-satunya cara untuk melakukan pekerjaan hebat adalah mencintai apa yang Anda lakukan. - Steve Jobs",
            "Waktu yang Anda nikmati sia-sia bukanlah waktu yang sia-sia. - Marthe Troly-Curtin",
            "Kehidupan yang tak teruji tidak layak dijalani. - Socrates",
            "Jika Anda ingin mengangkat diri Anda, angkat orang lain. - Booker T. Washington",
            "Ketakutan adalah penjara. Harapan adalah kunci. - Stephen King",
            "Kesabaran bukanlah kemampuan untuk menunggu, melainkan kemampuan untuk mempertahankan sikap baik saat menunggu. - Joyce Meyer",
            "Perubahan adalah hukum kehidupan. Dan mereka yang hanya melihat masa lalu atau masa kini pasti akan kehilangan masa depan. - John F. Kennedy"
        ];
        
        // --- FUNGSI TAMPILAN (RENDERING) ---
        const navLinks = {
            siswa: [
                { name: 'Dashboard', icon: 'fa-tachometer-alt', view: 'dashboard-view' },
                { name: 'Profil', icon: 'fa-user-circle', view: 'profil-view' },
                { name: 'Jadwal Konseling', icon: 'fa-calendar-alt', view: 'jadwal-view' },
                { name: 'Materi BK', icon: 'fa-book', view: 'materi-view' },
                { name: 'Kegiatan Sekolah', icon: 'fa-running', view: 'kegiatan-view' }, 
                { name: 'Laporan & Keluhan', icon: 'fa-flag', view: 'laporan-view' }, 
                { name: 'Ngobrol Bareng AI', icon: 'fa-robot', view: 'ai-chat-view' },
            ],
            guru: [
                { name: 'Dashboard', icon: 'fa-tachometer-alt', view: 'dashboard-view' },
                { name: 'Profil', icon: 'fa-user-tie', view: 'profil-view' },
                { name: 'Jadwal Konseling', icon: 'fa-calendar-check', view: 'jadwal-view' },
                { name: 'Materi BK', icon: 'fa-book', view: 'materi-view' },
                { name: 'Kegiatan Sekolah', icon: 'fa-running', view: 'kegiatan-view' },
                { name: 'Laporan & Keluhan', icon: 'fa-flag', view: 'laporan-view' }, 
                { name: 'Ngobrol Bareng AI', icon: 'fa-robot', view: 'ai-chat-view' },
            ],
            orangtua: [
                { name: 'Dashboard', icon: 'fa-tachometer-alt', view: 'dashboard-view' },
                { name: 'Profil Anak', icon: 'fa-child', view: 'profil-view' },
                { name: 'Materi BK', icon: 'fa-book', view: 'materi-view' },
                { name: 'Kegiatan Sekolah', icon: 'fa-running', view: 'kegiatan-view' },
                { name: 'Laporan & Keluhan', icon: 'fa-flag', view: 'laporan-view' }, 
                { name: 'Ngobrol Bareng AI', icon: 'fa-robot', view: 'ai-chat-view' },
            ],
            admin: [
                { name: 'Dashboard', icon: 'fa-tachometer-alt', view: 'dashboard-view' },
                { name: 'Kelola User', icon: 'fa-users-cog', view: 'kelola-user-view' },
            ]
        };

        function renderSidebar(role) {
            sidebarNav.innerHTML = '';
            navLinks[role].forEach(link => {
                const button = document.createElement('button');
                button.className = 'w-full flex items-center gap-3 py-2 px-4 rounded-lg text-gray-700 hover:bg-blue-500 hover:text-white transition-colors';
                button.innerHTML = `<i class="fas ${link.icon} w-6 text-center"></i><span>${link.name}</span>`;
                button.onclick = () => switchView(link.view);
                sidebarNav.appendChild(button);
            });
        }
        
        function switchView(viewId) {
            document.querySelectorAll('.page-view').forEach(view => {
                view.classList.add('hidden');
            });
            document.getElementById(viewId).classList.remove('hidden');
            
            // Clear previous quote cycle
            if (quoteInterval) clearInterval(quoteInterval);

            // Panggil listener yang sesuai saat berganti view
            if (viewId === 'dashboard-view') {
                renderDashboard(userRole);
                startQuoteCycle();
            } else if (viewId === 'materi-view') {
                renderMateriView();
                listenToMaterials();
            } else if (viewId === 'jadwal-view') {
                renderJadwal(userRole);
            } else if (viewId === 'laporan-view') {
                renderLaporanView(userRole);
                listenToReports(userRole);
            } else if (viewId === 'kegiatan-view') {
                renderKegiatanView(userRole);
                listenToActivities();
            } else if (viewId === 'ai-chat-view') {
                renderAIChatView();
            } else if (viewId === 'kelola-user-view') {
                renderKelolaUserView();
                listenToAllUsers();
            }
        }
        
        function startQuoteCycle() {
            const quoteElement = document.getElementById('philosophical-quote');
            if (!quoteElement) return;

            const displayQuote = () => {
                const randomIndex = Math.floor(Math.random() * philosophicalQuotes.length);
                const quote = philosophicalQuotes[randomIndex];
                quoteElement.innerHTML = `<i class="fas fa-quote-left mr-3 text-blue-400"></i><span class="italic">${quote}</span><i class="fas fa-quote-right ml-3 text-blue-400"></i>`;
            };

            displayQuote(); // Initial quote
            quoteInterval = setInterval(displayQuote, 10000); // Change every 10 seconds
        }


        async function renderDashboard(role) {
            const view = document.getElementById('dashboard-view');
            
            let content = `<h2 class="text-3xl font-bold mb-6 text-white drop-shadow-lg">Dashboard ${role.charAt(0).toUpperCase() + role.slice(1)}</h2>`;
            
            // Kotak Kutipan Filosofis
            content += `
                <div class="bg-white/90 backdrop-blur-sm p-6 mb-6 rounded-xl shadow-lg border-l-4 border-blue-500 transition-shadow duration-300">
                    <div id="philosophical-quote" class="text-lg font-medium text-gray-700 text-center animate-pulse">
                        Memuat kata-kata bijak...
                    </div>
                </div>
            `;
            
            if (role === 'siswa') {
                content += `
                    <div class="grid md:grid-cols-2 gap-6">
                        <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                            <h3 class="font-bold text-lg text-blue-600">Jadwal Konseling Mendatang</h3>
                            <p class="text-gray-600 mt-2">Anda belum memiliki jadwal. Buat janji di halaman Jadwal Konseling.</p>
                        </div>
                        <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                            <h3 class="font-bold text-lg text-green-600">Progres Saya</h3>
                            <p class="text-gray-600 mt-2">Ringkasan perkembangan dan catatan dari Guru BK akan tampil di sini.</p>
                        </div>
                    </div>
                `;
            } else if (role === 'guru') {
                content += `
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                        <h3 class="font-bold text-lg mb-4 text-red-600"><i class="fas fa-exclamation-triangle mr-2"></i>Permintaan Janji Temu Tertunda</h3>
                        <div id="pending-requests" class="space-y-3">
                           <p class="text-gray-500">Memuat permintaan...</p>
                        </div>
                    </div>
                `;
            } else if (role === 'admin') {
                content += `
                    <div class="grid md:grid-cols-3 gap-6">
                         <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow text-center">
                            <h3 class="font-bold text-lg text-blue-600">Total Pengguna</h3>
                            <p id="total-users-stat" class="text-4xl font-bold mt-2">${allUsers.length}</p>
                        </div>
                        <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow text-center">
                            <h3 class="font-bold text-lg text-red-600">Total Laporan</h3>
                            <p id="total-reports-stat" class="text-4xl font-bold mt-2">${allReports.length}</p>
                        </div>
                         <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow text-center">
                            <h3 class="font-bold text-lg text-green-600">Total Materi</h3>
                            <p id="total-materials-stat" class="text-4xl font-bold mt-2">${allMaterials.length}</p>
                        </div>
                    </div>
                `;
            } else { // orangtua
                 content += `
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                        <h3 class="font-bold text-lg text-purple-600">Notifikasi Terbaru</h3>
                        <p class="text-gray-600 mt-2">Belum ada notifikasi baru mengenai perkembangan anak Anda.</p>
                    </div>
                `;
            }
            view.innerHTML = content;
            if(role === 'guru') listenToPendingRequests();
        }

        function renderMateriView() {
            // ... (Kode renderMateriView sama seperti sebelumnya)
            const view = document.getElementById('materi-view');
            const createButton = userRole === 'guru' ? `
                <button onclick="openMaterialModal()" class="py-2 px-4 bg-green-600 text-white font-semibold rounded-lg shadow-md hover:bg-green-700 transition-colors">
                    <i class="fas fa-plus mr-2"></i>Buat Materi Baru
                </button>
            ` : '';
            
            view.innerHTML = `
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold text-white drop-shadow-lg">Materi Bimbingan Konseling</h2>
                    ${createButton}
                </div>
                
                <div class="mb-6">
                    <input type="text" id="material-search" onkeyup="filterMaterials(this.value)" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500" placeholder="Cari materi berdasarkan judul atau isi...">
                </div>

                <div id="material-list" class="space-y-4">
                    <p class="text-gray-500">Memuat daftar materi...</p>
                </div>
            `;
        }
        
        // --- START: FUNGSI BARU LAPORAN & KELUHAN ---
        function renderLaporanView(role) {
            const view = document.getElementById('laporan-view');
            let content = `<h2 class="text-3xl font-bold mb-6 text-white drop-shadow-lg">Laporan & Keluhan Sekolah</h2>`;
            
            if (role !== 'guru') { // Siswa & Orang Tua
                content += `
                    <div class="flex justify-end items-center mb-6">
                        <button onclick="openModal('report-modal')" class="py-2 px-4 bg-red-600 text-white font-semibold rounded-lg shadow-md hover:bg-red-700 transition-colors">
                            <i class="fas fa-exclamation-circle mr-2"></i>Ajukan Laporan Baru
                        </button>
                    </div>
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                        <h3 class="font-bold text-lg mb-4 text-red-600">Laporan yang Saya Ajukan</h3>
                        <div id="user-reports-list" class="space-y-4">
                            <p class="text-gray-500">Memuat laporan...</p>
                        </div>
                    </div>
                `;
            } else { // Guru BK
                 content += `
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow">
                        <h3 class="font-bold text-lg mb-4 text-red-600">Semua Laporan & Keluhan Masuk</h3>
                        <div id="all-reports-list" class="space-y-4">
                            <p class="text-gray-500">Memuat laporan...</p>
                        </div>
                    </div>
                `;
            }
            view.innerHTML = content;
        }

        function listenToReports(role) {
            const reportCollection = collection(db, `/artifacts/${appId}/public/data/reports`);
            let q;
            let containerId;

            if (role === 'guru') {
                q = query(reportCollection); // Guru lihat semua
                containerId = 'all-reports-list';
            } else {
                q = query(reportCollection, where("reporterId", "==", userId)); // Siswa/Ortu lihat laporan sendiri
                containerId = 'user-reports-list';
            }
            
            const container = document.getElementById(containerId);
            if (!container) return;
            
            onSnapshot(q, (querySnapshot) => {
                allReports = [];
                container.innerHTML = '';
                
                if(querySnapshot.empty) {
                    container.innerHTML = `<p class="text-gray-500">${role === 'guru' ? 'Tidak ada laporan yang masuk.' : 'Anda belum pernah mengajukan laporan.'}</p>`;
                    return;
                }
                
                querySnapshot.forEach((doc) => {
                    const report = doc.data();
                    allReports.push({ id: doc.id, ...report });
                    
                    const isGuru = role === 'guru';
                    const statusBadge = getReportStatusBadge(report.status);
                    const reporterInfo = isGuru ? `<p class="text-xs text-gray-500 mt-1">Pelapor: ${report.reporterName}</p>` : '';
                    
                    container.innerHTML += `
                        <div class="border rounded-lg p-4 flex justify-between items-start bg-red-50/90 backdrop-blur-sm">
                            <div>
                                <h4 class="font-semibold text-gray-900">${report.type} - ${report.subject}</h4>
                                ${reporterInfo}
                                <p class="text-sm text-gray-600">Tanggal: ${new Date(report.createdAt?.toDate ? report.createdAt.toDate() : report.createdAt).toLocaleDateString('id-ID')}</p>
                            </div>
                            <div class="text-right">
                                ${statusBadge}
                                ${isGuru && report.status === 'pending' ? `<button onclick="updateReportStatus('${doc.id}', 'in-progress')" class="mt-2 text-xs font-semibold text-blue-700 bg-blue-100 rounded-md px-2 py-1 hover:bg-blue-200">Proses</button>` : ''}
                                ${isGuru && report.status === 'in-progress' ? `<button onclick="updateReportStatus('${doc.id}', 'resolved')" class="mt-2 text-xs font-semibold text-green-700 bg-green-100 rounded-md px-2 py-1 hover:bg-green-200">Selesaikan</button>` : ''}
                                <button onclick="showReportDetail('${doc.id}')" class="mt-2 text-xs font-semibold text-gray-700 bg-gray-100 rounded-md px-2 py-1 hover:bg-gray-200">Detail</button>
                            </div>
                        </div>
                    `;
                });
            }, (error) => {
                console.error("Error in report listener:", error);
                container.innerHTML = '<p class="text-red-500">Gagal memuat laporan. Periksa koneksi.</p>';
            });
        }
        
        window.submitReport = async () => {
            const type = document.getElementById('report-type').value;
            const subject = document.getElementById('report-subject').value.trim();
            const details = document.getElementById('report-details').value.trim();

            if(!type || !subject || !details) {
                console.error("Harap isi semua kolom laporan."); 
                return;
            }

            try {
                await addDoc(collection(db, `/artifacts/${appId}/public/data/reports`), {
                    type: type,
                    subject: subject,
                    details: details,
                    reporterId: userId,
                    reporterRole: userRole,
                    reporterName: userName, // Use global userName
                    status: 'pending',
                    createdAt: new Date()
                });
                closeModal('report-modal');
                console.log("Laporan berhasil diajukan."); 
            } catch (e) {
                console.error("Error adding report: ", e);
                console.log("Gagal mengajukan laporan.");
            }
        }
        
        window.updateReportStatus = async (docId, status) => {
             const docRef = doc(db, `/artifacts/${appId}/public/data/reports`, docId);
            try {
                await updateDoc(docRef, { status: status });
                console.log(`Status laporan diubah menjadi ${status}.`);
            } catch (e) {
                console.error("Error updating report status:", e);
            }
        }
        
        window.showReportDetail = (reportId) => {
            const report = allReports.find(r => r.id === reportId);
            if (report) {
                 alert(`DETAIL LAPORAN:\n\nJenis: ${report.type}\nSubjek: ${report.subject}\nPelapor: ${report.reporterName}\nStatus: ${report.status.toUpperCase()}\nTanggal: ${new Date(report.createdAt?.toDate ? report.createdAt.toDate() : report.createdAt).toLocaleString('id-ID')}\n\nDetail:\n${report.details}`);
            }
        }
        
        function getReportStatusBadge(status) {
             switch(status) {
                case 'pending': return '<span class="bg-yellow-100 text-yellow-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Tertunda</span>';
                case 'in-progress': return '<span class="bg-blue-100 text-blue-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Diproses</span>';
                case 'resolved': return '<span class="bg-green-100 text-green-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Selesai</span>';
                default: return '';
            }
        }
        // --- END: FUNGSI BARU LAPORAN & KELUHAN ---

        // --- START: FUNGSI BARU KELOLA USER (ADMIN) ---
        function renderKelolaUserView() {
            const view = document.getElementById('kelola-user-view');
            view.innerHTML = `
                <h2 class="text-3xl font-bold mb-6 text-white drop-shadow-lg">Kelola Pengguna Sistem</h2>
                <div class="bg-white/90 backdrop-blur-sm p-6 rounded-xl shadow overflow-x-auto">
                    <table class="w-full text-sm text-left text-gray-700">
                        <thead class="text-xs text-gray-800 uppercase bg-gray-200">
                            <tr>
                                <th scope="col" class="px-6 py-3">Nama Pengguna</th>
                                <th scope="col" class="px-6 py-3">Peran (Role)</th>
                                <th scope="col" class="px-6 py-3">User ID</th>
                                <th scope="col" class="px-6 py-3 text-center">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="user-list-table">
                            <tr><td colspan="4" class="text-center p-4">Memuat data pengguna...</td></tr>
                        </tbody>
                    </table>
                </div>
            `;
        }

        function listenToAllUsers() {
            if (userRole !== 'admin') return;
            const q = query(collection(db, `/artifacts/${appId}/public/data/users`));
            const container = document.getElementById('user-list-table');

            onSnapshot(q, (querySnapshot) => {
                allUsers = [];
                container.innerHTML = '';
                
                if (querySnapshot.empty) {
                    container.innerHTML = '<tr><td colspan="4" class="text-center p-4">Tidak ada pengguna terdaftar.</td></tr>';
                    return;
                }

                querySnapshot.forEach((doc) => {
                    const user = doc.data();
                    allUsers.push({ id: doc.id, ...user });
                });
                
                // Update stat di dashboard jika view-nya adalah dashboard
                if (document.getElementById('dashboard-view').classList.contains('page-view') && !document.getElementById('dashboard-view').classList.contains('hidden')) {
                    document.getElementById('total-users-stat').textContent = allUsers.length;
                }


                allUsers.forEach(user => {
                    const isCurrentUser = user.uid === userId;
                    const roleOptions = ['siswa', 'guru', 'orangtua', 'admin']
                        .map(role => `<option value="${role}" ${user.role === role ? 'selected' : ''}>${role}</option>`)
                        .join('');
                    
                    const row = document.createElement('tr');
                    row.className = 'bg-white border-b';
                    row.innerHTML = `
                        <td class="px-6 py-4 font-medium text-gray-900">${user.name}</td>
                        <td class="px-6 py-4">
                            <select onchange="updateUserRole('${user.uid}', this.value)" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5" ${isCurrentUser ? 'disabled' : ''}>
                                ${roleOptions}
                            </select>
                        </td>
                        <td class="px-6 py-4 text-gray-500 font-mono text-xs">${user.uid}</td>
                        <td class="px-6 py-4 text-center">
                            <button onclick="deleteUser('${user.uid}', '${user.name}')" class="font-medium text-red-600 hover:underline" ${isCurrentUser ? 'disabled' : ''} title="${isCurrentUser ? 'Tidak dapat menghapus diri sendiri' : 'Hapus pengguna'}">
                                <i class="fas fa-trash"></i>
                            </button>
                        </td>
                    `;
                    container.appendChild(row);
                });

            }, (error) => {
                console.error("Error listening to users:", error);
                container.innerHTML = '<tr><td colspan="4" class="text-center p-4 text-red-500">Gagal memuat data pengguna.</td></tr>';
            });
        }
        
        window.updateUserRole = async (targetUserId, newRole) => {
            if (userRole !== 'admin') return;
            const docRef = doc(db, `/artifacts/${appId}/public/data/users`, targetUserId);
            try {
                await updateDoc(docRef, { role: newRole });
                console.log(`Peran untuk user ${targetUserId} berhasil diubah menjadi ${newRole}.`);
            } catch (e) {
                console.error("Gagal mengubah peran pengguna:", e);
            }
        };

        window.deleteUser = async (targetUserId, targetUserName) => {
            if (userRole !== 'admin') return;
            if (confirm(`Apakah Anda yakin ingin menghapus pengguna "${targetUserName}" (${targetUserId})? Tindakan ini tidak dapat diurungkan.`)) {
                try {
                    await deleteDoc(doc(db, `/artifacts/${appId}/public/data/users`, targetUserId));
                    console.log(`Pengguna "${targetUserName}" berhasil dihapus.`);
                } catch (e) {
                    console.error("Gagal menghapus pengguna:", e);
                }
            }
        };
        // --- END: FUNGSI BARU KELOLA USER (ADMIN) ---

        // --- START: FUNGSI BARU KEGIATAN SEKOLAH ---
        function renderKegiatanView(role) {
            const view = document.getElementById('kegiatan-view');
            const createButton = role === 'guru' ? `
                <button onclick="openActivityModal()" class="py-2 px-4 bg-orange-600 text-white font-semibold rounded-lg shadow-md hover:bg-orange-700 transition-colors">
                    <i class="fas fa-plus mr-2"></i>Buat Kegiatan Baru
                </button>
            ` : '';
            
            view.innerHTML = `
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold text-white drop-shadow-lg">Agenda Kegiatan Sekolah</h2>
                    ${createButton}
                </div>
                
                <div id="activity-list" class="space-y-4">
                    <p class="text-gray-500">Memuat daftar kegiatan...</p>
                </div>
            `;
        }

        function listenToActivities() {
            const q = query(collection(db, `/artifacts/${appId}/public/data/activities`));
            const container = document.getElementById('activity-list');
            if (!container) return;

            onSnapshot(q, (querySnapshot) => {
                allActivities = [];
                container.innerHTML = '';
                
                if(querySnapshot.empty) {
                    container.innerHTML = '<p class="text-gray-500 bg-white/90 backdrop-blur-sm p-4 rounded-lg">Belum ada kegiatan yang terjadwal.</p>';
                    return;
                }
                
                querySnapshot.forEach((doc) => {
                    const activity = doc.data();
                    allActivities.push({ id: doc.id, ...activity });
                    
                    const isGuru = userRole === 'guru';
                    const dateDisplay = activity.date instanceof Timestamp ? activity.date.toDate().toLocaleDateString('id-ID') : activity.date;

                    const actionButtons = isGuru ? `
                        <div class="space-x-2 mt-3 text-right">
                            <button onclick="openActivityModal('${doc.id}')" class="text-sm text-blue-600 hover:text-blue-800"><i class="fas fa-edit"></i> Edit</button>
                            <button onclick="deleteActivity('${doc.id}', '${activity.title}')" class="text-sm text-red-600 hover:text-red-800"><i class="fas fa-trash"></i> Hapus</button>
                        </div>
                    ` : '';

                    container.innerHTML += `
                        <div class="bg-white/90 backdrop-blur-sm p-5 rounded-lg shadow border-l-4 border-orange-500">
                            <h4 class="text-xl font-bold text-gray-900">${activity.title}</h4>
                            <p class="text-sm text-orange-600 mb-3"><i class="fas fa-calendar-alt mr-1"></i> Tanggal: ${dateDisplay}</p>
                            <div class="text-gray-700 whitespace-pre-wrap">${activity.content.substring(0, 200)}...</div>
                            ${actionButtons}
                        </div>
                    `;
                });
            }, (error) => {
                console.error("Error in activity listener:", error);
                container.innerHTML = '<p class="text-red-500">Gagal memuat kegiatan.</p>';
            });
        }
        
        window.openActivityModal = (activityId = null) => {
            if (userRole !== 'guru') return; 

            const modal = document.getElementById('activity-modal');
            const titleInput = document.getElementById('activity-title');
            const dateInput = document.getElementById('activity-date');
            const contentInput = document.getElementById('activity-content');
            const docIdInput = document.getElementById('activity-doc-id');
            const modalTitle = document.getElementById('activity-modal-title');
            
            if (activityId) {
                const activity = allActivities.find(a => a.id === activityId);
                if (activity) {
                    modalTitle.textContent = 'Edit Kegiatan Sekolah';
                    titleInput.value = activity.title;
                    dateInput.value = activity.date instanceof Timestamp ? activity.date.toDate().toISOString().split('T')[0] : activity.date;
                    contentInput.value = activity.content;
                    docIdInput.value = activityId;
                }
            } else {
                modalTitle.textContent = 'Buat Kegiatan Baru';
                titleInput.value = '';
                dateInput.value = '';
                contentInput.value = '';
                docIdInput.value = '';
            }

            modal.classList.remove('hidden');
        }

        window.submitActivity = async () => {
            if (userRole !== 'guru') return;

            const title = document.getElementById('activity-title').value.trim();
            const date = document.getElementById('activity-date').value.trim();
            const content = document.getElementById('activity-content').value.trim();
            const activityId = document.getElementById('activity-doc-id').value;

            if (!title || !date || !content) {
                console.error("Semua kolom kegiatan harus diisi.");
                return;
            }
            
            // Konversi string tanggal ke Timestamp
            const activityDateTimestamp = Timestamp.fromDate(new Date(date));

            const activityData = {
                title: title,
                date: activityDateTimestamp,
                content: content,
                authorId: userId,
                author: userName, // Use global userName
            };

            try {
                if (activityId) {
                    // Update
                    const docRef = doc(db, `/artifacts/${appId}/public/data/activities`, activityId);
                    await updateDoc(docRef, activityData);
                    console.log("Kegiatan berhasil diperbarui.");
                } else {
                    // Create
                    await addDoc(collection(db, `/artifacts/${appId}/public/data/activities`), {
                        ...activityData,
                        createdAt: new Date(),
                    });
                    console.log("Kegiatan baru berhasil dibuat.");
                }
                closeModal('activity-modal');
            } catch (e) {
                console.error("Gagal menyimpan kegiatan:", e);
            }
        }
        
        window.deleteActivity = async (activityId, title) => {
            if (userRole !== 'guru') return;

            // Menggunakan confirm temporary, harus diganti modal custom
            if (confirm(`Apakah Anda yakin ingin menghapus kegiatan "${title}"?`)) {
                try {
                    await deleteDoc(doc(db, `/artifacts/${appId}/public/data/activities`, activityId));
                    console.log(`Kegiatan "${title}" berhasil dihapus.`);
                } catch (e) {
                    console.error("Gagal menghapus kegiatan:", e);
                }
            }
        }
        // --- END: FUNGSI BARU KEGIATAN SEKOLAH ---

        // --- FUNGSI LAMA (Dipertahankan dan diperbarui sedikit) ---
        function getStatusBadge(status) {
            switch(status) {
                case 'pending': return '<span class="bg-yellow-100 text-yellow-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Tertunda</span>';
                case 'approved': return '<span class="bg-green-100 text-green-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Disetujui</span>';
                case 'rejected': return '<span class="bg-red-100 text-red-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Ditolak</span>';
                case 'completed': return '<span class="bg-blue-100 text-blue-800 text-xs font-medium px-2.5 py-0.5 rounded-full">Selesai</span>';
                default: return '';
            }
        }

        async function setupUser(role) {
            userRole = role;

            // FIX 1: Pastikan auth.currentUser tersedia dan perbarui userId dari sana
            if (!auth.currentUser || !auth.currentUser.uid) {
                console.error("CRITICAL: User ID not available during setup. Authentication failed or not complete.");
                return; 
            }
            
            userId = auth.currentUser.uid; // Pastikan global userId benar

            const userRef = doc(db, `/artifacts/${appId}/public/data/users`, userId);
            
            // Check if user exists, if not, create one, and fetch user data
            const usersCollection = collection(db, `/artifacts/${appId}/public/data/users`);
            const q = query(usersCollection, where("uid", "==", userId));
            const querySnapshot = await getDocs(q);
            
            let userData = null;

            if (querySnapshot.empty) {
                let name = `User ${userId.substring(0, 5)}`;
            if (role === 'guru') name = `Guru BK ${userId.substring(0, 5)}`;
            if (role === 'orangtua') name = `Orang Tua ${userId.substring(0, 5)}`;
            if (role === 'admin') name = `Admin ${userId.substring(0, 5)}`;
            userData = { uid: userId, role: role, name: name, docId: userId };
            await setDoc(userRef, userData);
        } else {
             userData = querySnapshot.docs[0].data();
                 // Jika peran di Firestore berbeda, perbarui (misal: dari siswa ke ortu)
                 if (userData.role !== role) {
                    await updateDoc(userRef, { role: role });
                 }
            }
            
            userName = userData.name; // Set global userName
            
            document.getElementById('user-role-display').textContent = `${role.charAt(0).toUpperCase() + role.slice(1)}: ${userName}`; // Tampilkan nama
            document.getElementById('user-id-display').textContent = userId;
            renderSidebar(role);
            
            // Initialize all views and switch to dashboard
            renderDashboard(role);
            renderJadwal(role);
            renderMateriView();
            renderAIChatView();
            renderLaporanView(role);
            renderKegiatanView(role);
            if (role === 'admin') renderKelolaUserView();

            switchView('dashboard-view');
            
            loginScreen.classList.add('hidden');
            mainApp.classList.remove('hidden');
        }

        function renderAIChatView() {
             const view = document.getElementById('ai-chat-view');
             view.innerHTML = `
                <h2 class="text-3xl font-bold mb-4 text-white drop-shadow-lg">Ngobrol Bareng AI Konselor</h2>
                <div class="bg-white/90 backdrop-blur-sm rounded-lg shadow flex-1 flex flex-col">
                    <div id="chat-messages" class="flex-1 p-4 space-y-4 overflow-y-auto">
                        <div class="flex items-start gap-3">
                            <div class="bg-blue-500 text-white p-2 rounded-full h-10 w-10 flex items-center justify-center flex-shrink-0">
                                <i class="fas fa-robot"></i>
                            </div>
                            <div class="bg-gray-200 p-3 rounded-lg max-w-lg">
                                <p class="text-sm">Halo! Saya AI Konselor. Ada yang bisa saya bantu? Anda bisa bertanya tentang apa saja, mulai dari stres, motivasi, hingga perencanaan karir.</p>
                            </div>
                        </div>
                    </div>
                    <div class="p-4 border-t">
                        <form id="ai-chat-form" class="flex items-center gap-3">
                            <input type="text" id="ai-chat-input" class="flex-1 p-3 border rounded-lg focus:ring-2 focus:ring-blue-500" placeholder="Ketik pesan Anda di sini..." autocomplete="off">
                            <button type="submit" class="py-3 px-5 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition-colors disabled:bg-gray-400">
                                <i class="fas fa-paper-plane"></i>
                            </button>
                        </form>
                    </div>
                </div>
            `;

            document.getElementById('ai-chat-form').addEventListener('submit', (e) => {
                e.preventDefault();
                handleAIChatSubmit();
            });
        }
        
        async function handleAIChatSubmit() {
            // ... (Kode handleAIChatSubmit sama seperti sebelumnya)
            const input = document.getElementById('ai-chat-input');
            const form = document.getElementById('ai-chat-form');
            const submitButton = form.querySelector('button[type="submit"]');
            const messagesContainer = document.getElementById('chat-messages');
            const userQuery = input.value.trim();

            if (!userQuery) return;

            // Display user message
            messagesContainer.innerHTML += `
                <div class="flex items-start gap-3 justify-end">
                    <div class="bg-blue-600 text-white p-3 rounded-lg max-w-lg">
                        <p class="text-sm">${userQuery}</p>
                    </div>
                     <div class="bg-gray-300 text-gray-800 p-2 rounded-full h-10 w-10 flex items-center justify-center flex-shrink-0">
                        <i class="fas fa-user"></i>
                    </div>
                </div>
            `;
            
            input.value = '';
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
            submitButton.disabled = true;

            // Display typing indicator
            const typingIndicatorId = 'typing-' + Date.now();
            messagesContainer.innerHTML += `
                 <div id="${typingIndicatorId}" class="flex items-start gap-3">
                    <div class="bg-blue-500 text-white p-2 rounded-full h-10 w-10 flex items-center justify-center flex-shrink-0">
                        <i class="fas fa-robot"></i>
                    </div>
                    <div class="bg-gray-200 p-3 rounded-lg max-w-lg">
                        <p class="text-sm italic">AI sedang mengetik...</p>
                    </div>
                </div>
            `;
            messagesContainer.scrollTop = messagesContainer.scrollHeight;

            // Gemini API Call (Using Exponential Backoff for Robustness)
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
            const systemPrompt = "Anda adalah AI Konselor yang bijaksana, empatik, dan suportif. Berikan nasihat yang membangun, positif, dan praktis dalam bahasa Indonesia. Jangan menghakimi, dan selalu berikan perspektif yang menenangkan.";

            const MAX_RETRIES = 3;
            for (let i = 0; i < MAX_RETRIES; i++) {
                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({
                            contents: [{ parts: [{ text: userQuery }] }],
                            systemInstruction: { parts: [{ text: systemPrompt }] },
                        })
                    });

                    if (!response.ok) {
                        if (response.status === 429 && i < MAX_RETRIES - 1) {
                            const delay = Math.pow(2, i) * 1000;
                            await new Promise(resolve => setTimeout(resolve, delay));
                            continue; // Retry
                        }
                        throw new Error(`API error: ${response.statusText}`);
                    }

                    const result = await response.json();
                    const aiResponse = result.candidates?.[0]?.content?.parts?.[0]?.text || "Maaf, saya tidak dapat memproses permintaan Anda saat ini.";
                    
                    // Display AI response
                    const typingIndicator = document.getElementById(typingIndicatorId);
                    typingIndicator.innerHTML = `
                        <div class="flex items-start gap-3">
                            <div class="bg-blue-500 text-white p-2 rounded-full h-10 w-10 flex items-center justify-center flex-shrink-0">
                                <i class="fas fa-robot"></i>
                            </div>
                            <div class="bg-gray-200 p-3 rounded-lg max-w-lg whitespace-pre-wrap">
                                <p class="text-sm">${aiResponse.replace(/\n/g, '<br>')}</p>
                            </div>
                        </div>
                    `;
                    break; // Exit loop on success

                } catch (error) {
                    console.error("AI Chat Error:", error);
                    if (i === MAX_RETRIES - 1) {
                         const typingIndicator = document.getElementById(typingIndicatorId);
                         typingIndicator.innerHTML = `
                            <div class="flex items-start gap-3">
                                <div class="bg-red-500 text-white p-2 rounded-full h-10 w-10 flex items-center justify-center flex-shrink-0">
                                    <i class="fas fa-exclamation-triangle"></i>
                                </div>
                                <div class="bg-red-100 p-3 rounded-lg max-w-lg">
                                    <p class="text-sm text-red-700">Terjadi kesalahan setelah beberapa kali mencoba. Coba lagi nanti.</p>
                                </div>
                            </div>
                        `;
                    }
                }
            }
            
            submitButton.disabled = false;
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }


        async function renderJadwal(role) {
            const view = document.getElementById('jadwal-view');
            // ... (Kode renderJadwal sama seperti sebelumnya)
            if (role === 'siswa') {
                view.innerHTML = `
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-white drop-shadow-lg">Jadwal Konseling</h2>
                        <button onclick="openModal('booking-modal')" class="py-2 px-4 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 transition-colors">
                            <i class="fas fa-plus mr-2"></i>Buat Janji Baru
                        </button>
                    </div>
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-lg shadow">
                        <h3 class="font-bold text-lg mb-4">Riwayat Janji Temu Saya</h3>
                        <div id="student-appointments" class="space-y-4">
                            <p class="text-gray-500">Memuat riwayat...</p>
                        </div>
                    </div>
                `;
                listenToStudentAppointments();
                populateCounselors();
            } else { // guru
                view.innerHTML = `
                    <h2 class="text-2xl font-bold text-white drop-shadow-lg mb-6">Kelola Jadwal Konseling</h2>
                    <div class="bg-white/90 backdrop-blur-sm p-6 rounded-lg shadow">
                         <h3 class="font-bold text-lg mb-4">Semua Janji Temu</h3>
                         <div id="counselor-appointments" class="space-y-4">
                             <p class="text-gray-500">Memuat jadwal...</p>
                         </div>
                    </div>
                `;
                listenToCounselorAppointments();
            }
        }

        async function populateCounselors() {
            // ... (Kode populateCounselors sama seperti sebelumnya)
            const select = document.getElementById('counselor-select');
            if (!select) return; // Guard for modal not open
            select.innerHTML = '<option>Memuat guru...</option>';
            try {
                const q = query(collection(db, `/artifacts/${appId}/public/data/users`), where("role", "==", "guru"));
                const querySnapshot = await getDocs(q);
                select.innerHTML = '';
                if(querySnapshot.empty) {
                     select.innerHTML += `<option value="">Tidak ada Guru BK</option>`;
                } else {
                     querySnapshot.forEach((doc) => {
                        select.innerHTML += `<option value="${doc.data().docId}">${doc.data().name}</option>`;
                    });
                }
            } catch (error) {
                console.error("Error loading counselors:", error);
                select.innerHTML = '<option>Gagal memuat guru</option>';
            }
        }
        
        async function submitBooking() {
            // ... (Kode submitBooking sama seperti sebelumnya)
            const counselorId = document.getElementById('counselor-select').value;
            const date = document.getElementById('booking-date').value;
            const topic = document.getElementById('booking-topic').value;

            if(!counselorId || !date || !topic) {
                console.error("Harap isi semua kolom."); 
                return;
            }

            try {
                await addDoc(collection(db, `/artifacts/${appId}/public/data/appointments`), {
                    studentId: userId,
                    studentName: userName, // Use global userName
                    counselorId: counselorId,
                    date: date,
                    topic: topic,
                    status: 'pending',
                    createdAt: new Date()
                });
                closeModal('booking-modal');
                console.log("Permintaan janji temu berhasil diajukan."); 
            } catch (e) {
                console.error("Error adding document: ", e);
                console.log("Gagal mengajukan janji temu.");
            }
        }
        
        function listenToStudentAppointments() {
             // ... (Kode listenToStudentAppointments sama seperti sebelumnya)
            const container = document.getElementById('student-appointments');
            const q = query(collection(db, `/artifacts/${appId}/public/data/appointments`), where("studentId", "==", userId));
            if (!container) return;
            onSnapshot(q, (querySnapshot) => {
                if(querySnapshot.empty) {
                    container.innerHTML = '<p class="text-gray-500">Anda belum memiliki riwayat janji temu.</p>';
                    return;
                }
                container.innerHTML = '';
                querySnapshot.forEach((doc) => {
                    const appt = doc.data();
                    container.innerHTML += `
                        <div class="border rounded-lg p-4 flex justify-between items-center bg-white/70">
                            <div>
                                <p class="font-semibold">${appt.topic}</p>
                                <p class="text-sm text-gray-600">Tanggal: ${appt.date}</p>
                            </div>
                            ${getStatusBadge(appt.status)}
                        </div>
                    `;
                });
            }, (error) => {
                if (error.code === 'permission-denied') {
                    console.warn("Firestore listener permission denied (Student Appointments).");
                    container.innerHTML = '<p class="text-red-500">Gagal memuat riwayat. Periksa izin akses.</p>';
                } else {
                    console.error("Uncaught Error in snapshot listener (Student Appointments):", error);
                }
            });
        }

        function listenToPendingRequests() {
            const container = document.getElementById('pending-requests');
            // FIX: Kueri hanya dengan counselorId untuk mengelakkan keperluan indeks komposit.
            // Penapisan sebelah klien akan digunakan untuk status.
            const q = query(collection(db, `/artifacts/${appId}/public/data/appointments`), where("counselorId", "==", userId));
            if (!container) return;
            
            onSnapshot(q, (querySnapshot) => {
                const pendingAppointments = [];
                querySnapshot.forEach((doc) => {
                    const appt = doc.data();
                    if (appt.status === 'pending') {
                        // Tambah id doc pada objek untuk digunakan dalam klik butang
                        pendingAppointments.push({ id: doc.id, ...appt });
                    }
                });

                if (pendingAppointments.length === 0) {
                    container.innerHTML = '<p class="text-gray-500">Tidak ada permintaan tertunda saat ini.</p>';
                    return;
                }

                container.innerHTML = '';
                pendingAppointments.forEach((appt) => {
                    container.innerHTML += `
                        <div class="border rounded-lg p-3 bg-yellow-50/90 backdrop-blur-sm">
                            <p><span class="font-semibold">${appt.studentName}</span> meminta sesi konseling.</p>
                            <p class="text-sm text-gray-600">Topik: ${appt.topic}</p>
                            <p class="text-sm text-gray-600">Tanggal: ${appt.date}</p>
                            <div class="mt-2 space-x-2">
                                <button onclick="updateAppointmentStatus('${appt.id}', 'approved')" class="px-2 py-1 text-xs font-semibold text-green-700 bg-green-100 rounded-md hover:bg-green-200">Setujui</button>
                                <button onclick="updateAppointmentStatus('${appt.id}', 'rejected')" class="px-2 py-1 text-xs font-semibold text-red-700 bg-red-100 rounded-md hover:bg-red-200">Tolak</button>
                            </div>
                        </div>
                    `;
                });
            }, (error) => {
                if (error.code === 'permission-denied') {
                    console.warn("Firestore listener permission denied (Guru BK appointments).");
                    container.innerHTML = '<p class="text-red-500">Gagal memuat permintaan. Periksa izin akses.</p>';
                } else {
                    console.error("Uncaught Error in snapshot listener (Pending Requests):", error);
                }
            });
        }

        function listenToCounselorAppointments() {
             // ... (Kode listenToCounselorAppointments sama seperti sebelumnya)
            const container = document.getElementById('counselor-appointments');
            const q = query(collection(db, `/artifacts/${appId}/public/data/appointments`), where("counselorId", "==", userId));
            if (!container) return;
            onSnapshot(q, (querySnapshot) => {
                 if(querySnapshot.empty) {
                    container.innerHTML = '<p class="text-gray-500">Anda belum memiliki jadwal janji temu.</p>';
                    return;
                }
                container.innerHTML = '';
                querySnapshot.forEach((doc) => {
                    const appt = doc.data();
                    container.innerHTML += `
                         <div class="border rounded-lg p-4 flex justify-between items-center bg-white/70">
                            <div>
                                <p class="font-semibold">${appt.topic} - (${appt.studentName})</p>
                                <p class="text-sm text-gray-600">Tanggal: ${appt.date}</p>
                            </div>
                            ${getStatusBadge(appt.status)}
                        </div>
                    `;
                });
            }, (error) => {
                if (error.code === 'permission-denied') {
                    console.warn("Firestore listener permission denied (Counselor all appointments).");
                    container.innerHTML = '<p class="text-red-500">Gagal memuat jadwal. Periksa izin akses.</p>';
                } else {
                    console.error("Uncaught Error in snapshot listener (Counselor Appointments):", error);
                }
            });
        }
        
        async function updateAppointmentStatus(docId, status) {
             // ... (Kode updateAppointmentStatus sama seperti sebelumnya)
            const docRef = doc(db, `/artifacts/${appId}/public/data/appointments`, docId);
            try {
                await updateDoc(docRef, { status: status });
                console.log(`Janji temu telah ${status === 'approved' ? 'disetujui' : 'ditolak'}.`);
            } catch (e) {
                console.error("Error updating document:", e);
                console.log("Gagal memperbarui status janji temu.");
            }
        }
        
        // Listener Materi BK (Dipertahankan)
        function listenToMaterials() {
            const q = query(collection(db, `/artifacts/${appId}/public/data/materials`));
            onSnapshot(q, (querySnapshot) => {
                allMaterials = []; 
                querySnapshot.forEach((doc) => {
                    allMaterials.push({ id: doc.id, ...doc.data() });
                });
                filterMaterials(document.getElementById('material-search')?.value || '');
            }, (error) => {
                if (error.code === 'permission-denied') {
                    console.warn("Firestore listener permission denied (Materials).");
                } else {
                    console.error("Uncaught Error in snapshot listener (Materials):", error);
                }
                document.getElementById('material-list').innerHTML = '<p class="text-red-500">Gagal memuat materi. Periksa izin akses.</p>';
            });
        }
        
        window.filterMaterials = (searchTerm) => {
             // ... (Kode filterMaterials sama seperti sebelumnya)
            const container = document.getElementById('material-list');
            const lowerCaseSearch = searchTerm.toLowerCase();
            
            const filteredMaterials = allMaterials.filter(material => 
                material.title.toLowerCase().includes(lowerCaseSearch) || 
                material.content.toLowerCase().includes(lowerCaseSearch) ||
                material.author.toLowerCase().includes(lowerCaseSearch)
            );

            if (filteredMaterials.length === 0) {
                container.innerHTML = '<p class="text-gray-500 bg-white/90 backdrop-blur-sm p-4 rounded-lg">Tidak ada materi yang ditemukan.</p>';
                return;
            }
            
            container.innerHTML = '';
            filteredMaterials.forEach(material => {
                const isGuru = userRole === 'guru';
                const actionButtons = isGuru ? `
                    <div class="space-x-2 mt-3 text-right">
                        <button onclick="openMaterialModal('${material.id}')" class="text-sm text-blue-600 hover:text-blue-800"><i class="fas fa-edit"></i> Edit</button>
                        <button onclick="deleteMaterial('${material.id}', '${material.title}')" class="text-sm text-red-600 hover:text-red-800"><i class="fas fa-trash"></i> Hapus</button>
                    </div>
                ` : '';

                container.innerHTML += `
                    <div class="bg-white/90 backdrop-blur-sm p-5 rounded-lg shadow border-l-4 border-blue-500">
                        <h4 class="text-xl font-bold text-gray-900">${material.title}</h4>
                        <p class="text-sm text-gray-500 mb-3">Oleh: ${material.author} | Dibuat: ${new Date(material.createdAt?.toDate ? material.createdAt.toDate() : material.createdAt).toLocaleDateString('id-ID')}</p>
                        <div class="text-gray-700 whitespace-pre-wrap">${material.content.substring(0, 200)}... <span class="text-blue-500 cursor-pointer" onclick="showMaterialDetail('${material.id}')">[Baca Selengkapnya]</span></div>
                        ${actionButtons}
                    </div>
                `;
            });
        }

        window.openMaterialModal = (materialId = null) => {
             // ... (Kode openMaterialModal sama seperti sebelumnya)
            const modal = document.getElementById('material-modal');
            const titleInput = document.getElementById('material-title');
            const contentInput = document.getElementById('material-content');
            const docIdInput = document.getElementById('material-doc-id');
            const modalTitle = document.getElementById('material-modal-title');
            
            if (materialId) {
                const material = allMaterials.find(m => m.id === materialId);
                if (material) {
                    modalTitle.textContent = 'Edit Materi BK';
                    titleInput.value = material.title;
                    contentInput.value = material.content;
                    docIdInput.value = materialId;
                }
            } else {
                modalTitle.textContent = 'Buat Materi Baru';
                titleInput.value = '';
                contentInput.value = '';
                docIdInput.value = '';
            }

            modal.classList.remove('hidden');
        }
        
        window.submitMaterial = async () => {
             // ... (Kode submitMaterial sama seperti sebelumnya)
            const title = document.getElementById('material-title').value.trim();
            const content = document.getElementById('material-content').value.trim();
            const materialId = document.getElementById('material-doc-id').value;

            if (!title || !content) {
                console.error("Judul dan isi materi harus diisi.");
                return;
            }

            const materialData = {
                title: title,
                content: content,
                authorId: userId,
                author: userName, // Use global userName
            };

            try {
                if (materialId) {
                    const docRef = doc(db, `/artifacts/${appId}/public/data/materials`, materialId);
                    await updateDoc(docRef, materialData);
                    console.log("Materi berhasil diperbarui.");
                } else {
                    await addDoc(collection(db, `/artifacts/${appId}/public/data/materials`), {
                        ...materialData,
                        createdAt: new Date(),
                    });
                    console.log("Materi baru berhasil dibuat.");
                }
                closeModal('material-modal');
            } catch (e) {
                console.error("Gagal menyimpan materi:", e);
            }
        }
        
        window.deleteMaterial = async (materialId, title) => {
             // ... (Kode deleteMaterial sama seperti sebelumnya)
            if (confirm(`Apakah Anda yakin ingin menghapus materi "${title}"?`)) {
                try {
                    await deleteDoc(doc(db, `/artifacts/${appId}/public/data/materials`, materialId));
                    console.log(`Materi "${title}" berhasil dihapus.`);
                } catch (e) {
                    console.error("Gagal menghapus materi:", e);
                }
            }
        }
        
        window.showMaterialDetail = (materialId) => {
             // ... (Kode showMaterialDetail sama seperti sebelumnya)
            const material = allMaterials.find(m => m.id === materialId);
            if (material) {
                alert(`DETAIL MATERI:\n\nJudul: ${material.title}\nPenulis: ${material.author}\n\nIsi:\n${material.content}`);
            }
        }

        // --- AUTH & WINDOW FUNCTIONS ---
        window.login = (role) => {
            const errorElement = document.getElementById('login-error');
            if (errorElement) errorElement.textContent = '';
            setupUser(role);
        };

        window.handleGuruLogin = () => {
            const passwordInput = document.getElementById('guru-password-input');
            const password = passwordInput.value;
            const errorElement = document.getElementById('guru-password-error');
            
            if (password === "bismillah") {
                errorElement.textContent = '';
                closeModal('guru-password-modal');
                login('guru');
                passwordInput.value = ''; // Kosongkan field kata sandi
            } else {
                errorElement.textContent = "Kata sandi yang Anda masukkan salah.";
                passwordInput.value = ''; // Kosongkan field kata sandi untuk keselamatan
            }
        };

        window.handleAdminLogin = () => {
            const passwordInput = document.getElementById('admin-password-input');
            const password = passwordInput.value;
            const errorElement = document.getElementById('admin-password-error');
            
            if (password === "admin123") { // Kata sandi untuk admin
                errorElement.textContent = '';
                closeModal('admin-password-modal');
                login('admin');
                passwordInput.value = '';
            } else {
                errorElement.textContent = "Kata sandi yang Anda masukkan salah.";
                passwordInput.value = '';
            }
        };

        window.logout = async () => {
            if (quoteInterval) clearInterval(quoteInterval);
            await signOut(auth);
            currentUser = null;
            userRole = null;
            userId = null;
            userName = null;
            loginScreen.classList.remove('hidden');
            mainApp.classList.add('hidden');
        };

        window.switchView = switchView;
        window.openModal = (id) => document.getElementById(id).classList.remove('hidden');
        window.closeModal = (id) => {
            document.getElementById(id).classList.add('hidden');
            // Tambahan: Kosongkan field & ralat modal kata sandi guru apabila ditutup
            if (id === 'guru-password-modal') {
                document.getElementById('guru-password-input').value = '';
                document.getElementById('guru-password-error').textContent = '';
            }
            if (id === 'admin-password-modal') {
                document.getElementById('admin-password-input').value = '';
                document.getElementById('admin-password-error').textContent = '';
            }
        };
        window.submitBooking = submitBooking;
        window.updateAppointmentStatus = updateAppointmentStatus;
        window.submitReport = submitReport; 
        window.updateReportStatus = updateReportStatus;
        window.showReportDetail = showReportDetail;
        window.openActivityModal = openActivityModal;
        window.submitActivity = submitActivity;
        window.deleteActivity = deleteActivity;
        window.handleGuruLogin = handleGuruLogin; // Tambahkan fungsi baru ke window
        window.handleAdminLogin = handleAdminLogin;
        window.updateUserRole = updateUserRole;
        window.deleteUser = deleteUser;


        // Inisialisasi Aplikasi
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                currentUser = user;
                userId = user.uid;
                authStatus.textContent = "Autentikasi berhasil. Pilih peran Anda.";
                // Jangan auto-login. Biarkan pengguna memilih peran.
            } else {
                 try {
                    authStatus.textContent = "Mencoba masuk...";
                    if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                        await signInWithCustomToken(auth, __initial_auth_token);
                    } else {
                        await signInAnonymously(auth);
                    }
                } catch (error) {
                    console.error("Authentication error:", error);
                    authStatus.textContent = "Gagal melakukan autentikasi.";
                }
            }
        });
    </script>
</body>
</html>

