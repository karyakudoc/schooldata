<!doctype html>
<html lang="id" class="h-full">
 <head><script src="/_sdk/telemetry_sdk.js"></script>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes, viewport-fit=cover">
  <title>Jejak Karyaku 🎨</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
        * {
            font-family: 'Fredoka', sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-overflow-scrolling: touch;
            box-sizing: border-box;
        }

        html, body {
            height: 100%;
            margin: 0;
        }

        body {
            background: #fff9fb;
            color: #334155;
            overflow-x: hidden;
        }

        .app-wrapper {
            width: 100%;
            min-height: 100%;
            overflow: auto;
        }

        .kids-btn {
            border-radius: 20px;
            font-weight: 700;
            transition: .2s;
        }

        .kids-btn:active {
            transform: scale(.96);
        }

        .btn-primary {
            background: #ff6b9d;
            color: white;
        }

        .btn-secondary {
            background: #60a5fa;
            color: white;
        }

        .card {
            background: white;
            border-radius: 24px;
            box-shadow: 0 6px 24px rgba(0,0,0,.06);
        }

        .gallery-slider {
            display: flex;
            overflow-x: auto;
            gap: 1rem;
            scroll-snap-type: x mandatory;
            padding-bottom: 1rem;
        }

        .gallery-slider::-webkit-scrollbar {
            display: none;
        }

        .gallery-card {
            min-width: 100%;
            scroll-snap-align: start;
        }

        @media(min-width:768px){
            .gallery-card {
                min-width: calc(50% - .5rem);
            }
        }

        .modal {
            background: rgba(0,0,0,.55);
            backdrop-filter: blur(8px);
        }

        .notification-badge {
            position: relative;
        }
        
        .badge-number {
            position: absolute;
            top: -8px;
            right: -8px;
            background: #ef4444;
            color: white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            font-size: 11px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .pin-dots {
            display: flex;
            gap: 8px;
            justify-content: center;
            margin-top: 20px;
        }
        .pin-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #cbd5e1;
            transition: all 0.2s;
        }
        .pin-dot.active {
            background: #ff6b9d;
            width: 24px;
            border-radius: 6px;
        }

        .toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 9999;
            padding: 12px 24px;
            border-radius: 16px;
            font-weight: 700;
            font-size: 14px;
            opacity: 0;
            transition: opacity 0.3s;
            pointer-events: none;
        }
        .toast.show {
            opacity: 1;
        }
        .toast-error {
            background: #fee2e2;
            color: #dc2626;
            border: 1px solid #fca5a5;
        }
        .toast-success {
            background: #dcfce7;
            color: #16a34a;
            border: 1px solid #86efac;
        }
        .toast-info {
            background: #dbeafe;
            color: #2563eb;
            border: 1px solid #93c5fd;
        }

        .inline-confirm {
            display: flex;
            gap: 8px;
            align-items: center;
            animation: fadeIn 0.2s;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
    </style>
  <style>body { box-sizing: border-box; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full">
  <div class="app-wrapper pb-10">
   <div id="toast" class="toast"></div>
   <div id="loading" class="hidden fixed inset-0 z-50 flex items-center justify-center bg-white/90">
    <div class="text-center">
     <div class="animate-spin w-16 h-16 border-4 border-pink-400 border-t-transparent rounded-full mx-auto"></div>
     <p class="mt-4 font-bold text-pink-500">Memuat...</p>
    </div>
   </div>
   <nav class="sticky top-0 z-40 p-3">
    <div class="max-w-6xl mx-auto bg-white rounded-3xl shadow-lg p-4 flex justify-between items-center">
     <h1 id="app-title" class="text-2xl font-black text-pink-500">JejakKaryaku 🎨</h1>
     <div class="flex gap-2 items-center">
      <button onclick="showView('home')" class="px-4 py-2 rounded-xl hover:bg-pink-50 font-bold">Beranda</button> <button id="admin-nav-btn" onclick="openLogin()" class="kids-btn btn-secondary px-5 py-2 relative notification-badge"> Guru 🍎 <span id="notif-floating" class="badge-number hidden"></span> </button>
     </div>
    </div>
   </nav>
   <main class="max-w-6xl mx-auto px-4">
    <section id="view-home" class="view space-y-8">
     <div class="text-center py-8">
      <h2 class="text-5xl font-black text-slate-800">Galeri Teman Hebat 🌈</h2>
      <p class="text-slate-400 mt-2 font-bold">Klik foto, masukkan PIN, lalu lihat karya</p>
     </div>
     <div id="student-grid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-4"></div>
    </section>
    <section id="view-gallery" class="view hidden space-y-5">
     <button onclick="showView('home')" class="kids-btn bg-white px-5 py-3 rounded-2xl shadow font-bold">← Kembali</button>
     <div id="gallery-header" class="card p-6"></div>
     <div class="flex justify-end">
      <button id="download-pdf-btn" onclick="downloadStudentPortfolio()" class="kids-btn bg-green-500 text-white px-5 py-3 rounded-2xl shadow font-bold"> <i data-lucide="file-text" style="width:16px;height:16px;display:inline-block;vertical-align:middle;margin-right:6px;"></i> Download Portofolio PDF </button>
     </div>
     <div id="gallery-grid" class="gallery-slider"></div>
    </section>
    <section id="view-admin" class="view hidden space-y-5">
     <div class="card p-5 flex justify-between items-center">
      <div class="flex items-center gap-3">
       <h2 class="text-3xl font-black">Dashboard Guru 🏫</h2><span id="notif-badge" class="bg-red-500 text-white px-3 py-1 rounded-full text-sm font-bold hidden"></span>
      </div><button onclick="logout()" class="text-red-400 font-bold">Keluar</button>
     </div>
     <div class="grid lg:grid-cols-3 gap-5">
      <div class="space-y-4">
       <button onclick="showModal('modal-student')" class="kids-btn btn-primary w-full py-4 text-lg">+ Tambah Murid</button>
       <div id="admin-student-list" class="space-y-3"></div>
      </div>
      <div class="lg:col-span-2 card p-6">
       <h3 class="text-2xl font-black mb-5">Pesan Orang Tua 💌</h3>
       <div id="admin-comment-log" class="space-y-4 max-h-[600px] overflow-y-auto"></div>
      </div>
     </div>
    </section>
   </main><!-- Modal PIN -->
   <div id="modal-password" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl p-7 w-full max-w-sm">
     <div id="pin-student-info" class="text-center mb-4">
      <img id="pin-student-avatar" class="w-20 h-20 rounded-full mx-auto object-cover mb-2" alt="Avatar siswa">
      <h3 id="pin-student-name" class="text-xl font-black"></h3>
     </div>
     <h3 class="text-2xl font-black text-center mb-4">Masukkan PIN 🔐</h3>
     <div class="space-y-4">
      <div class="flex justify-center gap-3">
       <input type="password" id="pin-input" maxlength="6" class="w-full p-4 text-center text-2xl rounded-2xl bg-slate-100 outline-none tracking-widest" placeholder="••••••">
      </div>
      <div class="pin-dots" id="pin-dots">
       <span class="pin-dot"></span><span class="pin-dot"></span> <span class="pin-dot"></span><span class="pin-dot"></span> <span class="pin-dot"></span><span class="pin-dot"></span>
      </div><button onclick="verifyPassword()" class="kids-btn btn-primary w-full py-4 text-lg">Buka Galeri 🎨</button> <button onclick="hideModal('modal-password')" class="w-full text-slate-400 font-bold">Tutup</button>
     </div>
    </div>
   </div><!-- Modal Login -->
   <div id="modal-login" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl p-7 w-full max-w-sm">
     <h3 class="text-3xl font-black text-center mb-6">Login Guru 🍎</h3>
     <div class="space-y-3">
      <input id="login-user" type="text" placeholder="Username" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <input id="login-pass" type="password" placeholder="Password" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <button onclick="handleLogin()" class="kids-btn btn-secondary w-full py-4 text-lg">Login</button> <button onclick="hideModal('modal-login')" class="w-full text-slate-400 font-bold">Tutup</button>
     </div>
    </div>
   </div><!-- Modal Upload -->
   <div id="modal-upload" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl w-full max-w-2xl max-h-[90%] overflow-y-auto p-6">
     <div class="flex justify-between items-center mb-5">
      <h3 id="upload-modal-title" class="text-2xl font-black">Kelola Karya</h3><button onclick="hideModal('modal-upload')" class="text-2xl text-slate-400">✖</button>
     </div>
     <div class="space-y-4 bg-pink-50 p-5 rounded-3xl">
      <input id="up-judul" type="text" placeholder="Judul karya" class="w-full p-4 rounded-2xl outline-none"> <input id="up-file" type="file" accept="image/*" class="w-full"> <img id="preview-upload" class="hidden w-full rounded-2xl shadow-md" alt="Preview"> <button onclick="processUpload()" class="kids-btn btn-primary w-full py-4">Upload 🚀</button>
     </div>
     <div id="admin-work-list" class="space-y-3 mt-5"></div>
    </div>
   </div><!-- Modal Tambah Murid -->
   <div id="modal-student" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl p-6 w-full max-w-sm">
     <h3 class="text-2xl font-black mb-5">Tambah Murid 👶</h3>
     <div class="space-y-3">
      <input id="st-nama" type="text" placeholder="Nama" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <input id="st-pass" type="text" placeholder="PIN (password untuk akses)" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <input id="st-foto" type="text" placeholder="URL Foto" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <button onclick="saveStudent()" class="kids-btn btn-primary w-full py-4">Simpan</button> <button onclick="hideModal('modal-student')" class="w-full text-slate-400 font-bold">Tutup</button>
     </div>
    </div>
   </div><!-- Modal Edit Murid -->
   <div id="modal-edit-student" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl p-6 w-full max-w-sm">
     <h3 class="text-2xl font-black mb-5">Edit Murid ✏️</h3>
     <div class="space-y-3">
      <input id="edit-st-nama" type="text" placeholder="Nama" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <input id="edit-st-pass" type="text" placeholder="PIN" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <input id="edit-st-foto" type="text" placeholder="URL Foto" class="w-full p-4 rounded-2xl bg-slate-100 outline-none"> <button onclick="saveEditStudent()" class="kids-btn btn-primary w-full py-4">Simpan Perubahan</button> <button onclick="hideModal('modal-edit-student')" class="w-full text-slate-400 font-bold">Tutup</button>
     </div>
    </div>
   </div><!-- Modal Konfirmasi Hapus -->
   <div id="modal-confirm" class="hidden fixed inset-0 modal z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-3xl p-6 w-full max-w-sm text-center">
     <div class="text-5xl mb-4">
      ⚠️
     </div>
     <h3 id="confirm-title" class="text-xl font-black mb-2"></h3>
     <p id="confirm-message" class="text-slate-500 mb-6"></p>
     <div class="flex gap-3">
      <button onclick="hideModal('modal-confirm')" class="kids-btn bg-slate-200 text-slate-700 flex-1 py-3">Batal</button> <button id="confirm-action-btn" class="kids-btn bg-red-500 text-white flex-1 py-3">Hapus</button>
     </div>
    </div>
   </div>
   <script>

const API_URL = ['https://script.google.com/macros/s/AKfycbyLW_X5PHRg-i-2OWa0N9wKSn04bOBMJGrcJig0CYLjPt5f1k7oSsmv5QsBmBfAIsZc/exec'](https://script.google.com/macros/s/AKfycbwrGhzvWcQXMknlpdTo5aClduYCMlczyCMTkKdV2ZsILVwl5WsiPCgUtcz_38fKtN9bqA/exec);

let appData = { students: [] };
let isAdminLoggedIn = false;
let currentStudentId = null;
let pendingStudentId = null;
let editingStudentId = null;

// ==================== TOAST NOTIFICATION ====================
function showToast(message, type = 'info') {
    const toast = document.getElementById('toast');
    toast.className = `toast toast-${type} show`;
    toast.textContent = message;
    setTimeout(() => {
        toast.classList.remove('show');
    }, 3000);
}

// ==================== CONFIRM DIALOG ====================
function showConfirm(title, message, onConfirm) {
    document.getElementById('confirm-title').textContent = title;
    document.getElementById('confirm-message').textContent = message;
    const btn = document.getElementById('confirm-action-btn');
    const newBtn = btn.cloneNode(true);
    btn.parentNode.replaceChild(newBtn, btn);
    newBtn.addEventListener('click', () => {
        hideModal('modal-confirm');
        onConfirm();
    });
    showModal('modal-confirm');
}

// ==================== ELEMENT SDK ====================
const defaultConfig = {
    app_title: 'JejakKaryaku 🎨',
    background_color: '#fff9fb',
    surface_color: '#ffffff',
    text_color: '#334155',
    primary_color: '#ff6b9d',
    secondary_color: '#60a5fa',
    font_family: 'Fredoka',
    font_size: 16
};

function applyConfig(config) {
    const title = config.app_title || defaultConfig.app_title;
    const bg = config.background_color || defaultConfig.background_color;
    const surface = config.surface_color || defaultConfig.surface_color;
    const text = config.text_color || defaultConfig.text_color;
    const primary = config.primary_color || defaultConfig.primary_color;
    const secondary = config.secondary_color || defaultConfig.secondary_color;
    const font = config.font_family || defaultConfig.font_family;
    const size = config.font_size || defaultConfig.font_size;

    document.getElementById('app-title').textContent = title;
    document.body.style.background = bg;
    document.body.style.color = text;
    document.body.style.fontFamily = `${font}, sans-serif`;
    document.body.style.fontSize = `${size}px`;

    document.querySelectorAll('.card').forEach(el => {
        el.style.background = surface;
    });
    document.querySelectorAll('.btn-primary').forEach(el => {
        el.style.background = primary;
    });
    document.querySelectorAll('.btn-secondary').forEach(el => {
        el.style.background = secondary;
    });
    document.querySelectorAll('.text-pink-500').forEach(el => {
        el.style.color = primary;
    });
}

window.elementSdk.init({
    defaultConfig,
    onConfigChange: async (config) => {
        applyConfig(config);
    },
    mapToCapabilities: (config) => ({
        recolorables: [
            { get: () => config.background_color || defaultConfig.background_color, set: (v) => { config.background_color = v; window.elementSdk.setConfig({ background_color: v }); } },
            { get: () => config.surface_color || defaultConfig.surface_color, set: (v) => { config.surface_color = v; window.elementSdk.setConfig({ surface_color: v }); } },
            { get: () => config.text_color || defaultConfig.text_color, set: (v) => { config.text_color = v; window.elementSdk.setConfig({ text_color: v }); } },
            { get: () => config.primary_color || defaultConfig.primary_color, set: (v) => { config.primary_color = v; window.elementSdk.setConfig({ primary_color: v }); } },
            { get: () => config.secondary_color || defaultConfig.secondary_color, set: (v) => { config.secondary_color = v; window.elementSdk.setConfig({ secondary_color: v }); } }
        ],
        borderables: [],
        fontEditable: { get: () => config.font_family || defaultConfig.font_family, set: (v) => { config.font_family = v; window.elementSdk.setConfig({ font_family: v }); } },
        fontSizeable: { get: () => config.font_size || defaultConfig.font_size, set: (v) => { config.font_size = v; window.elementSdk.setConfig({ font_size: v }); } }
    }),
    mapToEditPanelValues: (config) => new Map([
        ["app_title", config.app_title || defaultConfig.app_title]
    ])
});

// ==================== CORE FUNCTIONS ====================
function toggleLoading(v){
    document.getElementById('loading').classList.toggle('hidden', !v);
}

function showModal(id){
    document.getElementById(id).classList.remove('hidden');
}

function hideModal(id){
    document.getElementById(id).classList.add('hidden');
}

function showView(id){
    document.querySelectorAll('.view').forEach(v => v.classList.add('hidden'));
    document.getElementById('view-'+id).classList.remove('hidden');
    window.scrollTo({top:0, behavior:'smooth'});
}

async function fetchData(){
    toggleLoading(true);
    try {
        const res = await fetch(API_URL, {
            method:'POST',
            body: JSON.stringify({ action:'getInitialData' })
        });
        const json = await res.json();
        if(json.success){
            appData.students = json.data || [];
            renderHome();
            if(isAdminLoggedIn) renderAdmin();
        }
    } catch(err){
        showToast('Gagal memuat data', 'error');
    }
    toggleLoading(false);
}

function renderHome(){
    const grid = document.getElementById('student-grid');
    if(!appData.students.length){
        grid.innerHTML = `<div class="col-span-full text-center py-20 text-slate-400 font-bold">Belum ada siswa</div>`;
        return;
    }
    grid.innerHTML = appData.students.map(s=>`
        <div onclick="requestPassword('${s.id}')" class="card p-4 text-center cursor-pointer hover:shadow-xl transition-shadow">
            <img loading="lazy" decoding="async"
                src="${s.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(s.nama)}"
                class="w-full aspect-square rounded-2xl object-cover mb-3"
                alt="${s.nama}"
                onerror="this.style.background='#e5e7eb';this.alt='Foto tidak tersedia';">
            <h3 class="font-black truncate">${s.nama}</h3>
            <p class="text-xs text-slate-400 mt-1 font-bold">${s.karya?.length || 0} karya</p>
        </div>
    `).join('');
}

// ==================== PASSWORD PROTECTION ====================
function requestPassword(studentId){
    const student = appData.students.find(x=>x.id===studentId);
    if(!student) return;
    pendingStudentId = studentId;
    document.getElementById('pin-student-name').innerText = student.nama;
    document.getElementById('pin-student-avatar').src = student.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(student.nama);
    document.getElementById('pin-input').value = '';
    updatePinDots('');
    showModal('modal-password');
    setTimeout(()=> document.getElementById('pin-input').focus(), 100);
}

function updatePinDots(value){
    const dots = document.querySelectorAll('.pin-dot');
    dots.forEach((dot, idx)=>{
        dot.classList.toggle('active', idx < value.length);
    });
}

document.getElementById('pin-input').addEventListener('input', function(e){
    updatePinDots(e.target.value);
});

document.getElementById('pin-input').addEventListener('keypress', function(e){
    if(e.key === 'Enter') verifyPassword();
});

function verifyPassword(){
    const inputPin = document.getElementById('pin-input').value.trim();
    if(!inputPin){
        showToast('Masukkan PIN terlebih dahulu', 'error');
        return;
    }
    const student = appData.students.find(x=>x.id===pendingStudentId);
    if(!student){
        showToast('Siswa tidak ditemukan', 'error');
        hideModal('modal-password');
        return;
    }
    if(inputPin === student.pass){
        hideModal('modal-password');
        renderGallery(pendingStudentId);
    } else {
        showToast('PIN salah! Silakan coba lagi.', 'error');
        document.getElementById('pin-input').value = '';
        updatePinDots('');
        document.getElementById('pin-input').focus();
    }
}

// ==================== RENDER GALLERY ====================
function renderGallery(id){
    currentStudentId = id;
    const s = appData.students.find(x=>x.id===id);
    if(!s) return;

    document.getElementById('gallery-header').innerHTML = `
        <div class="flex items-center gap-5">
            <img loading="lazy" decoding="async"
                src="${s.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(s.nama)}"
                class="w-28 h-28 rounded-3xl object-cover shadow-lg" alt="${s.nama}"
                onerror="this.style.background='#e5e7eb';this.alt='Foto tidak tersedia';">
            <div>
                <h2 class="text-4xl font-black text-pink-500">${s.nama}</h2>
                <p class="text-slate-400 font-bold mt-1">${s.karya.length} karya hebat ✨</p>
            </div>
        </div>
    `;

    const grid = document.getElementById('gallery-grid');
    if(!s.karya.length){
        grid.innerHTML = `<div class="card p-10 text-center text-slate-400 font-bold">Belum ada karya</div>`;
        showView('gallery');
        return;
    }

    grid.innerHTML = s.karya.map(k=>`
        <div class="gallery-card card overflow-hidden">
            <img loading="lazy" decoding="async" src="${k.thumb || k.link}" class="w-full aspect-[4/3] object-cover" alt="${k.judul}"
                onerror="this.style.background='#e5e7eb';this.alt='Gambar tidak tersedia';">
            <div class="p-5">
                <div class="flex justify-between items-start gap-3 mb-3">
                    <h3 class="text-xl font-black text-slate-700">${k.judul}</h3>
                    <span class="text-xs text-slate-400 font-bold whitespace-nowrap">${new Date(k.date).toLocaleDateString('id-ID')}</span>
                </div>
                <div class="space-y-3 max-h-[250px] overflow-y-auto">
                    ${(s.percakapan || []).filter(p=>p.workId===k.id).map(c=>`
                        <div class="bg-slate-50 p-3 rounded-2xl">
                            <p class="text-xs font-black text-blue-500 mb-1">Orang Tua</p>
                            <p class="font-medium text-sm">${c.pesanWali}</p>
                            ${c.balasanGuru ? `
                                <div class="bg-green-100 rounded-2xl p-3 mt-3">
                                    <p class="text-xs font-black text-green-700 mb-1">Guru</p>
                                    <p class="text-sm font-medium text-green-900">${c.balasanGuru}</p>
                                </div>
                            `:''}
                        </div>
                    `).join('')}
                </div>
                <div class="flex gap-2 mt-4">
                    <input id="input-chat-${k.id}" type="text" placeholder="Tulis komentar..." class="flex-1 p-3 rounded-xl border outline-none text-sm">
                    <button onclick="sendComment('${k.id}')" class="kids-btn btn-primary px-5">➤</button>
                </div>
            </div>
        </div>
    `).join('');
    showView('gallery');
}

// ==================== DOWNLOAD PDF ====================
async function downloadStudentPortfolio(){
    if(!currentStudentId) return;
    await exportPortfolio(currentStudentId, false);
}

// ==================== LOGIN/LOGOUT ====================
function openLogin(){
    if(isAdminLoggedIn){
        showView('admin');
        return;
    }
    showModal('modal-login');
}

function handleLogin(){
    const u = document.getElementById('login-user').value;
    const p = document.getElementById('login-pass').value;
    if(u==='admin' && p==='guru123'){
        isAdminLoggedIn = true;
        hideModal('modal-login');
        document.getElementById('admin-nav-btn').innerHTML = 'Dashboard 🏠 <span id="notif-floating" class="badge-number hidden"></span>';
        renderAdmin();
        showView('admin');
    } else {
        showToast('Username atau password salah', 'error');
    }
}

function logout(){
    isAdminLoggedIn = false;
    document.getElementById('admin-nav-btn').innerHTML = 'Guru 🍎 <span id="notif-floating" class="badge-number hidden"></span>';
    showView('home');
}

// ==================== ADMIN ====================
function renderAdmin(){
    const list = document.getElementById('admin-student-list');
    list.innerHTML = appData.students.map(s=>`
        <div class="card p-4">
            <div class="flex justify-between items-center gap-3">
                <div class="flex items-center gap-3">
                    <img loading="lazy" decoding="async"
                        src="${s.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(s.nama)}"
                        class="w-14 h-14 rounded-2xl object-cover" alt="${s.nama}"
                        onerror="this.style.background='#e5e7eb';">
                    <div>
                        <p class="font-black">${s.nama}</p>
                        <p class="text-xs text-slate-400 font-bold">PIN: ${s.pass}</p>
                        <p class="text-xs text-slate-400 font-bold">${s.karya.length} karya</p>
                    </div>
                </div>
                <div class="flex gap-2">
                    <button onclick="editStudent('${s.id}')" class="w-10 h-10 rounded-xl bg-yellow-100 text-yellow-600 flex items-center justify-center">
                        <i data-lucide="pencil" style="width:16px;height:16px;"></i>
                    </button>
                    <button onclick="manageKarya('${s.id}')" class="w-10 h-10 rounded-xl bg-blue-100 text-blue-600 flex items-center justify-center">
                        <i data-lucide="images" style="width:16px;height:16px;"></i>
                    </button>
                    <button onclick="deleteStudent('${s.id}')" class="w-10 h-10 rounded-xl bg-red-100 text-red-500 flex items-center justify-center">
                        <i data-lucide="trash-2" style="width:16px;height:16px;"></i>
                    </button>
                    <button onclick="exportPortfolio('${s.id}', true)" class="w-10 h-10 rounded-xl bg-green-100 text-green-600 flex items-center justify-center">
                        <i data-lucide="file-text" style="width:16px;height:16px;"></i>
                    </button>
                </div>
            </div>
        </div>
    `).join('');
    renderCommentInbox();
    lucide.createIcons();
}

function renderCommentInbox(){
    const wrap = document.getElementById('admin-comment-log');
    if(!wrap) return;

    let allComments = [];
    appData.students.forEach(s=>{
        (s.percakapan || []).forEach(c=>{
            const karya = s.karya.find(k=>k.id===c.workId);
            allComments.push({ ...c, student:s, karya });
        });
    });

    allComments.sort((a,b)=> new Date(b.date) - new Date(a.date));
    const unreplyCount = allComments.filter(x=>!x.balasanGuru).length;

    const notifBadge = document.getElementById('notif-badge');
    if(notifBadge){
        if(unreplyCount > 0){
            notifBadge.classList.remove('hidden');
            notifBadge.innerText = `${unreplyCount} Baru`;
        } else {
            notifBadge.classList.add('hidden');
        }
    }

    const floating = document.getElementById('notif-floating');
    if(floating){
        if(unreplyCount > 0){
            floating.classList.remove('hidden');
            floating.innerText = unreplyCount;
        } else {
            floating.classList.add('hidden');
        }
    }

    if(!allComments.length){
        wrap.innerHTML = `<div class="bg-slate-50 rounded-3xl p-10 text-center text-slate-400 font-bold">Belum ada pesan</div>`;
        return;
    }

    wrap.innerHTML = allComments.map(c=>`
        <div class="bg-slate-50 rounded-3xl p-4">
            <div class="flex items-center gap-3 mb-4">
                <img src="${c.student.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(c.student.nama)}"
                    class="w-14 h-14 rounded-2xl object-cover" alt="${c.student.nama}"
                    onerror="this.style.background='#e5e7eb';">
                <div class="flex-1">
                    <div class="flex items-center gap-2 flex-wrap">
                        <h4 class="font-black text-lg">${c.student.nama}</h4>
                        ${!c.balasanGuru ? `<span class="bg-red-100 text-red-500 text-xs px-2 py-1 rounded-full font-black">Baru</span>` : ''}
                    </div>
                    <p class="text-sm text-slate-400 font-bold">${c.karya?.judul || 'Karya'}</p>
                </div>
            </div>
            ${c.karya && c.karya.thumb ? `<img src="${c.karya.thumb}" class="w-full rounded-2xl mb-4 max-h-[250px] object-cover" alt="${c.karya.judul}" onerror="this.style.display='none';">` : ''}
            <div class="bg-white rounded-2xl p-4">
                <p class="text-xs font-black text-blue-500 mb-1">Pesan Orang Tua</p>
                <p class="font-medium">${c.pesanWali}</p>
            </div>
            ${c.balasanGuru ? `
                <div class="bg-green-100 rounded-2xl p-4 mt-3">
                    <p class="text-xs font-black text-green-700 mb-1">Balasan Guru</p>
                    <p class="font-medium text-green-900">${c.balasanGuru}</p>
                </div>
            ` : `
                <div class="flex gap-2 mt-3">
                    <input id="reply-${c.id}" type="text" placeholder="Balas komentar..." class="flex-1 p-3 rounded-2xl border outline-none">
                    <button onclick="replyComment('${c.id}')" class="kids-btn btn-primary px-5">Balas</button>
                </div>
            `}
        </div>
    `).join('');
}

async function replyComment(commentId){
    const input = document.getElementById(`reply-${commentId}`);
    const text = input.value.trim();
    if(!text) return;
    toggleLoading(true);
    try {
        const res = await fetch(API_URL, {
            method:'POST',
            body: JSON.stringify({ action:'replyComment', commentId, replyText: text })
        });
        const json = await res.json();
        if(json.success){
            await fetchData();
            renderAdmin();
            showToast('Balasan terkirim!', 'success');
        } else {
            showToast('Gagal membalas: ' + (json.error || 'Unknown error'), 'error');
        }
    } catch(err){
        showToast('Gagal membalas', 'error');
    }
    toggleLoading(false);
}

function manageKarya(id){
    currentStudentId = id;
    const s = appData.students.find(x=>x.id===id);
    document.getElementById('upload-modal-title').innerText = `Koleksi ${s.nama}`;
    const list = document.getElementById('admin-work-list');
    list.innerHTML = s.karya.map(k=>`
        <div class="flex justify-between items-center bg-slate-50 p-3 rounded-2xl">
            <div class="flex items-center gap-3">
                <img loading="lazy" decoding="async" src="${k.thumb}" class="w-14 h-14 rounded-xl object-cover" alt="${k.judul}"
                    onerror="this.style.background='#e5e7eb';">
                <div><p class="font-black text-sm">${k.judul}</p></div>
            </div>
            <button onclick="deleteKarya('${k.id}')" class="text-red-400 text-xl">🗑️</button>
        </div>
    `).join('');
    showModal('modal-upload');
}

document.getElementById('up-file').addEventListener('change', function(e){
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = function(ev){
        const img = document.getElementById('preview-upload');
        img.src = ev.target.result;
        img.classList.remove('hidden');
    };
    reader.readAsDataURL(file);
});

async function processUpload(){
    const judul = document.getElementById('up-judul').value.trim();
    const fileInput = document.getElementById('up-file');
    if(!judul || !fileInput.files[0]){
        showToast('Lengkapi judul dan pilih file', 'error');
        return;
    }
    toggleLoading(true);
    const file = fileInput.files[0];
    const reader = new FileReader();
    reader.onload = async(e)=>{
        const img = new Image();
        img.onload = async function(){
            const canvas = document.createElement('canvas');
            let width = img.width;
            let height = img.height;
            const maxWidth = 1200;
            if(width > maxWidth){
                height *= maxWidth / width;
                width = maxWidth;
            }
            canvas.width = width;
            canvas.height = height;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(img,0,0,width,height);
            const compressed = canvas.toDataURL('image/jpeg',0.7);
            const base64 = compressed.split(',')[1];
            try {
                const res = await fetch(API_URL, {
                    method:'POST',
                    body: JSON.stringify({ action:'uploadKarya', studentId: currentStudentId, judul, fileName: file.name, fileData: base64 })
                });
                const json = await res.json();
                if(json.success){
                    document.getElementById('up-judul').value = '';
                    document.getElementById('up-file').value = '';
                    document.getElementById('preview-upload').classList.add('hidden');
                    await fetchData();
                    manageKarya(currentStudentId);
                    showToast('Karya berhasil diupload!', 'success');
                } else {
                    showToast('Upload gagal', 'error');
                }
            } catch(err){
                showToast('Upload gagal', 'error');
            }
            toggleLoading(false);
        };
        img.src = e.target.result;
    };
    reader.readAsDataURL(file);
}

async function deleteKarya(id){
    showConfirm('Hapus Karya?', 'Karya ini akan dihapus permanen.', async () => {
        toggleLoading(true);
        try {
            const res = await fetch(API_URL, {
                method:'POST',
                body: JSON.stringify({ action:'deleteKarya', workId:id })
            });
            const json = await res.json();
            if(json.success){
                await fetchData();
                manageKarya(currentStudentId);
                showToast('Karya dihapus', 'success');
            }
        } catch(err){
            showToast('Gagal hapus karya', 'error');
        }
        toggleLoading(false);
    });
}

async function saveStudent(){
    const nama = document.getElementById('st-nama').value.trim();
    const pass = document.getElementById('st-pass').value.trim();
    const foto = document.getElementById('st-foto').value.trim();
    if(!nama || !pass){
        showToast('Nama dan PIN wajib diisi', 'error');
        return;
    }
    toggleLoading(true);
    try {
        const res = await fetch(API_URL, {
            method:'POST',
            body: JSON.stringify({ action:'addStudent', id:'S'+Date.now(), nama, pass, foto })
        });
        const json = await res.json();
        if(json.success){
            hideModal('modal-student');
            document.getElementById('st-nama').value = '';
            document.getElementById('st-pass').value = '';
            document.getElementById('st-foto').value = '';
            await fetchData();
            renderAdmin();
            showToast('Murid berhasil ditambahkan!', 'success');
        }
    } catch(err){
        showToast('Gagal tambah siswa', 'error');
    }
    toggleLoading(false);
}

function editStudent(id){
    const s = appData.students.find(x=>x.id===id);
    if(!s) return;
    editingStudentId = id;
    document.getElementById('edit-st-nama').value = s.nama;
    document.getElementById('edit-st-pass').value = s.pass;
    document.getElementById('edit-st-foto').value = s.foto || '';
    showModal('modal-edit-student');
}

async function saveEditStudent(){
    const nama = document.getElementById('edit-st-nama').value.trim();
    const pass = document.getElementById('edit-st-pass').value.trim();
    const foto = document.getElementById('edit-st-foto').value.trim();
    if(!nama || !pass){
        showToast('Nama dan PIN wajib diisi', 'error');
        return;
    }
    toggleLoading(true);
    try {
        const res = await fetch(API_URL, {
            method:'POST',
            body: JSON.stringify({ action:'editStudent', id: editingStudentId, nama, pass, foto })
        });
        const json = await res.json();
        if(json.success){
            hideModal('modal-edit-student');
            await fetchData();
            renderAdmin();
            showToast('Data murid diperbarui!', 'success');
        }
    } catch(err){
        showToast('Gagal edit', 'error');
    }
    toggleLoading(false);
}

async function deleteStudent(id){
    const s = appData.students.find(x=>x.id===id);
    showConfirm('Hapus Siswa?', `${s?.nama || 'Siswa'} dan semua datanya akan dihapus permanen.`, async () => {
        toggleLoading(true);
        try {
            const res = await fetch(API_URL, {
                method:'POST',
                body: JSON.stringify({ action:'deleteStudent', id })
            });
            const json = await res.json();
            if(json.success){
                await fetchData();
                renderAdmin();
                showToast('Siswa berhasil dihapus', 'success');
            }
        } catch(err){
            showToast('Gagal hapus siswa', 'error');
        }
        toggleLoading(false);
    });
}

async function sendComment(workId){
    const input = document.getElementById(`input-chat-${workId}`);
    const text = input.value.trim();
    if(!text) return;
    toggleLoading(true);
    try {
        const res = await fetch(API_URL, {
            method:'POST',
            body: JSON.stringify({ action:'addComment', workId, studentId: currentStudentId, comment:text })
        });
        const json = await res.json();
        if(json.success){
            input.value = '';
            await fetchData();
            renderGallery(currentStudentId);
            showToast('Komentar terkirim!', 'success');
        }
    } catch(err){
        showToast('Gagal kirim pesan', 'error');
    }
    toggleLoading(false);
}

// ==================== PDF EXPORT ====================
async function loadImageAsBase64(url){
    return new Promise((resolve,reject)=>{
        const img = new Image();
        img.crossOrigin = 'Anonymous';
        img.onload = function(){
            const canvas = document.createElement('canvas');
            canvas.width = img.width;
            canvas.height = img.height;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(img,0,0);
            resolve(canvas.toDataURL('image/jpeg'));
        };
        img.onerror = reject;
        img.src = url;
    });
}

async function exportPortfolio(id, isAdminExport = false){
    const s = appData.students.find(x=>x.id===id);
    if(!s) return;
    toggleLoading(true);
    try {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({ orientation:'portrait', unit:'mm', format:'a4' });

        doc.setFillColor(255,107,157);
        doc.rect(0,0,210,297,'F');
        doc.setTextColor(255,255,255);
        
        try {
            const studentPhoto = s.foto || 'https://ui-avatars.com/api/?name='+encodeURIComponent(s.nama)+'&background=ffffff&color=ff6b9d&size=200';
            const photoBase64 = await loadImageAsBase64(studentPhoto);
            const imgWidth = 60;
            const imgHeight = 60;
            const x = (210 - imgWidth) / 2;
            const y = 60;
            doc.addImage(photoBase64, 'JPEG', x, y, imgWidth, imgHeight);
            doc.setDrawColor(255,255,255);
            doc.setLineWidth(2);
            doc.circle(x + imgWidth/2, y + imgHeight/2, imgWidth/2 + 2, 'S');
        } catch(err){}
        
        doc.setFontSize(28);
        doc.text('PORTOFOLIO KARYA', 105, 145, { align: 'center' });
        doc.setFontSize(24);
        doc.text(s.nama, 105, 170, { align: 'center' });
        doc.setFontSize(14);
        doc.text(s.karya.length + ' Karya Hebat', 105, 190, { align: 'center' });
        
        doc.addPage();

        for(let i=0;i<s.karya.length;i++){
            const k = s.karya[i];
            doc.setFillColor(255,240,245);
            doc.rect(0,0,210,25,'F');
            doc.setTextColor(40,40,40);
            doc.setFontSize(18);
            doc.text(k.judul, 15, 16);
            
            try {
                const img = await loadImageAsBase64(k.thumb);
                doc.addImage(img, 'JPEG', 15, 35, 180, 100);
            } catch(err){}

            let y = 145;
            doc.setFontSize(11);
            doc.text('Tanggal: ' + new Date(k.date).toLocaleDateString('id-ID'), 15, y);
            y += 15;
            
            const chats = (s.percakapan || []).filter(p=>p.workId===k.id);
            if(chats.length){
                chats.forEach(c=>{
                    if(y > 260){ doc.addPage(); y = 20; }
                    doc.setFillColor(240,248,255);
                    doc.roundedRect(15, y, 180, 25, 4, 4, 'F');
                    doc.setFontSize(10);
                    doc.setTextColor(40,40,40);
                    doc.text('Pesan Orang Tua:', 20, y+8);
                    doc.text(c.pesanWali || '', 20, y+16, { maxWidth: 165 });
                    y += 35;
                    if(c.balasanGuru){
                        if(y > 260){ doc.addPage(); y = 20; }
                        doc.setFillColor(240,255,240);
                        doc.roundedRect(15, y, 180, 25, 4, 4, 'F');
                        doc.text('Balasan Guru:', 20, y+8);
                        doc.text(c.balasanGuru, 20, y+16, { maxWidth: 165 });
                        y += 35;
                    }
                });
            }
            if(i < s.karya.length-1) doc.addPage();
        }
        doc.save('Portofolio-'+s.nama+'.pdf');
        showToast('PDF berhasil didownload!', 'success');
    } catch(err) {
        showToast('Gagal membuat PDF', 'error');
    }
    toggleLoading(false);
}

// ==================== INIT ====================
window.onload = ()=>{
    fetchData();
    lucide.createIcons();
};

</script>
  </div>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'a0c85a37b0b0b4ac',t:'MTc4MTU5NzU3Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
