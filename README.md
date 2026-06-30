<!DOCTYPE html>
<html lang="id" class="light">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Eco Sport Jersey - Admin Panel</title>
<script src="https://cdn.tailwindcss.com"></script>
<script>
tailwind.config={darkMode:'class',theme:{extend:{fontFamily:{sans:['Plus Jakarta Sans','sans-serif']}}}}
</script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.31/jspdf.plugin.autotable.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
<style>
*{scrollbar-width:thin;scrollbar-color:#cbd5e1 transparent}
.dark *{scrollbar-color:#475569 transparent}
::-webkit-scrollbar{width:8px;height:8px}
::-webkit-scrollbar-thumb{background:#cbd5e1;border-radius:4px}
::-webkit-scrollbar-track{background:transparent}
.dark ::-webkit-scrollbar-thumb{background:#475569}

body {
  font-family: 'Plus Jakarta Sans', sans-serif;
  transition: background .3s, color .3s;
}
.light body {
  background-color: #f8fafc;
  background-image: linear-gradient(to bottom right, #ffffff, #eff6ff 200px);
  background-attachment: fixed;
}
.dark body {
  background-color: #020617;
  background-image: linear-gradient(to bottom right, #0f172a, #020617 200px);
  background-attachment: fixed;
}

.font-800 { font-weight: 800 !important; }

.sidebar {
  width: 260px;
  transition: transform .3s, box-shadow .3s;
  box-shadow: 4px 0 24px rgba(79, 70, 229, 0.05);
}
@media(max-width:1023px){.sidebar{transform:translateX(-100%);position:fixed;z-index:50;height:100vh}.sidebar.open{transform:translateX(0)}}
.sidebar-overlay{display:none;position:fixed;inset:0;background:rgba(15,23,42,.4);z-index:40;backdrop-filter:blur(6px)}.sidebar-overlay.active{display:block}

.nav-item{transition:all .25s ease;border-left:3px solid transparent; position:relative;}
.light .nav-item:hover{background:#eef2ff;border-left-color:#c7d2fe;color:#3730a3}
.dark .nav-item:hover{background:rgba(99,102,241,.1);border-left-color:#6366f1;color:#a5b4fc}
.light .nav-item.active{background:#eef2ff;border-left-color:#4f46e5;color:#4338ca;font-weight:600}
.dark .nav-item.active{background:rgba(99,102,241,.2);border-left-color:#818cf8;color:#c7d2fe;font-weight:600}

.stat-card{transition:all .3s cubic-bezier(0.4, 0, 0.2, 1); box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02), 0 2px 4px -1px rgba(0,0,0,0.02);}
.stat-card:hover{transform:translateY(-4px)}
.light .stat-card:hover{box-shadow:0 20px 25px -5px rgba(79,70,229,.08), 0 10px 10px -5px rgba(0,0,0,.04)}
.dark .stat-card:hover{box-shadow:0 20px 25px -5px rgba(0,0,0,.5), 0 10px 10px -5px rgba(0,0,0,.2)}

.modal-enter{animation:modalIn .3s cubic-bezier(0.4, 0, 0.2, 1)}
@keyframes modalIn{from{opacity:0;transform:scale(.96) translateY(20px)}to{opacity:1;transform:scale(1) translateY(0)}}
.toast-enter{animation:toastIn .4s cubic-bezier(0.4, 0, 0.2, 1)}
@keyframes toastIn{from{opacity:0;transform:translateX(120%)}to{opacity:1;transform:translateX(0)}}
.toast-exit{animation:toastOut .35s cubic-bezier(0.4, 0, 0.2, 1) forwards}
@keyframes toastOut{to{opacity:0;transform:translateX(120%)}}

.table-row{transition:background .2s}
.light .table-row:hover{background:#f8fafc}
.dark .table-row:hover{background:rgba(255,255,255,.03)}

.badge{padding:3px 12px;border-radius:9999px;font-size:11px;font-weight:600;text-transform:uppercase;letter-spacing:.5px; display:inline-flex; align-items:center;}

.fade-in{animation:fadeIn .4s cubic-bezier(0.4, 0, 0.2, 1)}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:translateY(0)}}

input,select,textarea{transition:border-color .2s,box-shadow .2s; -webkit-appearance:none; -moz-appearance:none; appearance:none;}
select {
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20width%3D%22292.4%22%20height%3D%22292.4%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath%20fill%3D%22%2394a3b8%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E");
  background-repeat: no-repeat;
  background-position: right 1rem center;
  background-size: 0.65em auto;
}
input:focus,select:focus,textarea:focus{outline:none}
.light input:focus,.light select:focus,.light textarea:focus{border-color:#4f46e5!important;box-shadow:0 0 0 4px rgba(79,70,229,.1)!important}
.dark input:focus,.dark select:focus,.dark textarea:focus{border-color:#818cf8!important;box-shadow:0 0 0 4px rgba(129,140,248,.15)!important}

.btn-primary{transition:all .2s; background: linear-gradient(135deg, #4f46e5, #7c3aed); color:#fff; box-shadow: 0 4px 14px rgba(79,70,229,0.25);}
.btn-primary:hover{transform:translateY(-2px); box-shadow: 0 8px 20px rgba(79,70,229,0.35); filter: brightness(1.1);}

.btn-export{transition:all .2s}.btn-export:hover{transform:translateY(-2px); box-shadow: 0 4px 10px rgba(0,0,0,0.06);}

.empty-state{display:flex;flex-direction:column;align-items:center;justify-content:center;padding:60px 20px}
.empty-state i{font-size:52px;margin-bottom:16px; opacity:0.8}
.light .empty-state i{color:#e2e8f0}.dark .empty-state i{color:#334155}
.empty-state p{font-size:14px}
.light .empty-state p{color:#94a3b8}.dark .empty-state p{color:#64748b}
.empty-state .title{font-size:16px;font-weight:700;margin-bottom:4px}
.light .empty-state .title{color:#64748b}.dark .empty-state .title{color:#94a3b8}

.theme-toggle{position:relative;width:48px;height:26px;border-radius:13px;cursor:pointer;transition:background .3s}
.light .theme-toggle{background:#e2e8f0}.dark .theme-toggle{background:#475569}
.theme-toggle .knob{position:absolute;top:3px;width:20px;height:20px;border-radius:50%;transition:all .3s cubic-bezier(0.4, 0, 0.2, 1);display:flex;align-items:center;justify-content:center;font-size:11px}
.light .theme-toggle .knob{left:3px;background:#fff;color:#f59e0b;box-shadow:0 1px 3px rgba(0,0,0,.1)}
.dark .theme-toggle .knob{left:25px;background:#1e293b;color:#94a3b8;box-shadow:0 1px 3px rgba(0,0,0,.3)}

.light .bg-slate-50 { background-color: #ffffff !important; }
.light .border-slate-100 { border-color: #e2e8f0 !important; }
.light .border-slate-200 { border-color: #e2e8f0 !important; }

/* ===== PRINT NOTA STYLES ===== */
@media print {
  body * { visibility: hidden !important; }
  #notaPrintArea, #notaPrintArea * { visibility: visible !important; }
  #notaPrintArea {
    position: fixed !important;
    left: 0 !important;
    top: 0 !important;
    width: 100% !important;
    background: #fff !important;
    color: #1e293b !important;
    z-index: 99999 !important;
    padding: 0 !important;
    margin: 0 !important;
  }
  @page { size: A4; margin: 12mm; }
}

.nota-preview-wrapper {
  background: #f1f5f9;
  border-radius: 16px;
  padding: 32px;
  position: relative;
}
.dark .nota-preview-wrapper { background: rgba(30,41,59,0.5); }

.nota-paper {
  background: #ffffff;
  max-width: 800px;
  margin: 0 auto;
  border-radius: 12px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.08), 0 1px 4px rgba(0,0,0,0.04);
  overflow: hidden;
  position: relative;
}
.dark .nota-paper { box-shadow: 0 4px 24px rgba(0,0,0,0.4); }

.nota-paper::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 6px;
  background: linear-gradient(90deg, #4f46e5, #7c3aed, #a855f7);
}

.nota-header {
  padding: 32px 40px 24px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: 2px dashed #e2e8f0;
}
.nota-body { padding: 24px 40px; }
.nota-footer {
  padding: 20px 40px 32px;
  border-top: 2px dashed #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.nota-table { width: 100%; border-collapse: collapse; }
.nota-table th {
  background: #f8fafc;
  padding: 10px 12px;
  text-align: left;
  font-size: 11px;
  font-weight: 700;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  border-bottom: 2px solid #e2e8f0;
}
.nota-table td {
  padding: 12px;
  font-size: 13px;
  color: #334155;
  border-bottom: 1px solid #f1f5f9;
}
.nota-table tr:last-child td { border-bottom: none; }

.nota-stamp {
  width: 120px; height: 120px;
  border: 3px solid #4f46e5;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #4f46e5;
  transform: rotate(-15deg);
  opacity: 0.7;
  font-weight: 800;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 1px;
  line-height: 1.3;
  text-align: center;
}
.nota-stamp.paid { border-color: #10b981; color: #10b981; }
.nota-stamp.cancelled { border-color: #ef4444; color: #ef4444; }

.nota-qr-placeholder {
  width: 80px; height: 80px;
  border: 2px dashed #cbd5e1;
  border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  color: #94a3b8; font-size: 10px; text-align: center;
}

.nota-watermark {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%) rotate(-30deg);
  font-size: 100px;
  font-weight: 900;
  color: rgba(79,70,229,0.03);
  pointer-events: none;
  white-space: nowrap;
  letter-spacing: 10px;
}

/* Nota selector card */
.nota-order-card {
  border: 2px solid #e2e8f0;
  border-radius: 16px;
  padding: 16px 20px;
  cursor: pointer;
  transition: all 0.25s;
  background: #fff;
}
.dark .nota-order-card { border-color: #334155; background: #1e293b; }
.nota-order-card:hover { border-color: #a5b4fc; box-shadow: 0 4px 12px rgba(79,70,229,0.1); transform: translateY(-2px); }
.nota-order-card.selected { border-color: #4f46e5; background: #eef2ff; box-shadow: 0 4px 16px rgba(79,70,229,0.15); }
.dark .nota-order-card.selected { background: rgba(79,70,229,0.15); border-color: #818cf8; }

/* Settings panel for nota */
.nota-setting-group {
  padding: 16px;
  border-radius: 12px;
  background: rgba(79,70,229,0.03);
  border: 1px solid rgba(79,70,229,0.08);
}
.dark .nota-setting-group { background: rgba(79,70,229,0.06); border-color: rgba(129,140,248,0.1); }
</style>
</head>
<body class="light:bg-slate-50 dark:bg-slate-950 light:text-slate-800 dark:text-slate-200">

<!-- LOGIN SCREEN -->
<div id="loginScreen" class="fixed inset-0 z-[100] flex items-center justify-center p-4 bg-gradient-to-br from-indigo-600 via-purple-600 to-pink-500 hidden">
  <div class="absolute inset-0 bg-black/10 backdrop-blur-sm"></div>
  <div class="relative bg-white dark:bg-slate-800 rounded-3xl shadow-2xl p-8 w-full max-w-md border border-white/50 modal-enter">
    <div class="text-center mb-8">
      <div class="w-16 h-16 rounded-2xl bg-gradient-to-tr from-indigo-500 to-purple-500 mx-auto flex items-center justify-center text-white text-2xl font-black shadow-lg mb-4">E</div>
      <h1 class="text-2xl font-extrabold text-slate-800 dark:text-white">Eco Sport Jersey</h1>
      <p class="text-slate-400 dark:text-slate-400 text-sm mt-1">Silakan masuk ke akun admin Anda</p>
    </div>
    <div id="loginError" class="hidden mb-4 bg-red-100 text-red-600 text-sm text-center py-2 rounded-lg border border-red-200">Username atau password salah!</div>
    <div class="space-y-4">
      <div>
        <label class="block text-sm font-semibold text-slate-600 dark:text-slate-300 mb-1">Username</label>
        <div class="relative">
          <i class="fas fa-user absolute left-4 top-1/2 -translate-y-1/2 text-slate-300"></i>
          <input id="loginUser" type="text" class="w-full pl-11 pr-4 py-3 rounded-xl bg-slate-50 dark:bg-slate-700 border border-slate-200 dark:border-slate-600 text-slate-800 dark:text-white focus:border-indigo-500" placeholder="admin" onkeypress="if(event.key==='Enter')document.getElementById('loginPass').focus()">
        </div>
      </div>
      <div>
        <label class="block text-sm font-semibold text-slate-600 dark:text-slate-300 mb-1">Password</label>
        <div class="relative">
          <i class="fas fa-lock absolute left-4 top-1/2 -translate-y-1/2 text-slate-300"></i>
          <input id="loginPass" type="password" class="w-full pl-11 pr-4 py-3 rounded-xl bg-slate-50 dark:bg-slate-700 border border-slate-200 dark:border-slate-600 text-slate-800 dark:text-white focus:border-indigo-500" placeholder="admin123" onkeypress="if(event.key==='Enter')login()">
        </div>
      </div>
      <button onclick="login()" class="w-full py-3 rounded-xl bg-gradient-to-r from-indigo-500 to-purple-500 text-white font-bold shadow-lg shadow-indigo-500/30 hover:shadow-indigo-500/50 hover:-translate-y-0.5 transition-all">Masuk</button>
      <p class="text-center text-xs text-slate-400">Gunakan <b>admin</b> / <b>admin123</b> untuk demo</p>
    </div>
  </div>
</div>

<!-- APP CONTAINER -->
<div id="appContainer" class="hidden">
<div id="sidebarOverlay" class="sidebar-overlay" onclick="toggleSidebar()"></div>
<div class="flex h-screen overflow-hidden">
  <aside id="sidebar" class="sidebar light:bg-white/80 dark:bg-slate-900/80 backdrop-blur-xl light:text-slate-500 dark:text-slate-400 flex flex-col flex-shrink-0 light:border-r dark:border-r dark:border-slate-800 border-slate-200">
    <div class="p-5 light:border-b dark:border-b dark:border-slate-800 border-slate-100">
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-500 to-purple-500 flex items-center justify-center text-white font-black text-lg shadow-md">E</div>
        <div><h1 class="light:text-slate-800 dark:text-slate-100 font-bold text-base leading-tight">Eco Sport</h1><p class="light:text-slate-400 dark:text-slate-500 text-xs">Jersey Konveksi</p></div>
      </div>
    </div>
    <nav class="flex-1 py-4 px-3 space-y-1 overflow-y-auto">
      <a onclick="go('dashboard')" data-page="dashboard" class="nav-item active flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-chart-pie w-5 text-center"></i> Dashboard</a>
      <a onclick="go('pesanan')" data-page="pesanan" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-clipboard-list w-5 text-center"></i> Pesanan</a>
      <a onclick="go('pelanggan')" data-page="pelanggan" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-users w-5 text-center"></i> Pelanggan</a>
      <a onclick="go('produk')" data-page="produk" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-shirt w-5 text-center"></i> Produk</a>
      <a onclick="go('perekapan')" data-page="perekapan" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-calculator w-5 text-center"></i> Perekapan</a>
      <a onclick="go('nota')" data-page="nota" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-receipt w-5 text-center"></i> Cetak Nota</a>
      <a onclick="go('laporan')" data-page="laporan" class="nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm"><i class="fas fa-file-lines w-5 text-center"></i> Laporan</a>
    </nav>
    <div class="p-4 light:border-t dark:border-t dark:border-slate-800 border-slate-100">
      <div class="flex items-center gap-3">
        <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-indigo-400 to-purple-400 flex items-center justify-center text-white font-bold text-sm">A</div>
        <div><p class="light:text-slate-700 dark:text-slate-200 text-sm font-medium">Admin</p><p class="light:text-slate-400 dark:text-slate-500 text-xs">Manager</p></div>
      </div>
    </div>
  </aside>
  <div class="flex-1 flex flex-col overflow-hidden">
    <header class="light:bg-white/70 dark:bg-slate-900/70 backdrop-blur-xl light:border-b dark:border-b dark:border-slate-800 border-slate-200 px-4 lg:px-6 py-3 flex items-center justify-between flex-shrink-0 sticky top-0 z-30">
      <div class="flex items-center gap-3">
        <button onclick="toggleSidebar()" class="lg:hidden w-9 h-9 rounded-lg light:bg-slate-100 dark:bg-slate-800 light:text-slate-500 dark:text-slate-400 hover:opacity-70 flex items-center justify-center"><i class="fas fa-bars"></i></button>
        <h2 id="pageTitle" class="text-lg font-bold light:text-slate-800 dark:text-slate-100">Dashboard</h2>
      </div>
      <div class="flex items-center gap-3">
        <span class="text-xs light:text-slate-400 dark:text-slate-500 hidden sm:block" id="currentDate"></span>
        <div class="theme-toggle" onclick="toggleTheme()" title="Ganti Tema"><div class="knob"><i class="fas fa-sun" id="themeIcon"></i></div></div>
        <button onclick="resetData()" class="w-9 h-9 rounded-lg light:bg-red-50 dark:bg-red-950/30 light:text-red-400 dark:text-red-500/50 hover:opacity-70 flex items-center justify-center" title="Reset Data"><i class="fas fa-rotate-left text-sm"></i></button>
        <button onclick="logout()" class="px-4 py-2 rounded-xl bg-red-500 text-white font-semibold text-sm hover:bg-red-600 flex items-center gap-2 shadow-sm shadow-red-500/30"><i class="fas fa-right-from-bracket"></i> <span class="hidden sm:block">Keluar</span></button>
      </div>
    </header>
    <main id="content" class="flex-1 overflow-y-auto p-4 lg:p-6"></main>
  </div>
</div>

<div id="modalOverlay" class="fixed inset-0 bg-black/40 z-50 flex items-center justify-center p-4 hidden backdrop-blur-sm" onclick="if(event.target===this)closeModal()">
  <div id="modalBox" class="light:bg-white dark:bg-slate-900 rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] flex flex-col modal-enter light:border dark:border dark:border-slate-800">
    <div class="flex items-center justify-between px-6 py-4 light:border-b dark:border-b dark:border-slate-800 border-slate-100">
      <h3 id="modalTitle" class="font-bold text-lg light:text-slate-800 dark:text-slate-100"></h3>
      <button onclick="closeModal()" class="w-8 h-8 rounded-lg light:hover:bg-slate-100 dark:hover:bg-slate-800 light:text-slate-400 dark:text-slate-500 flex items-center justify-center"><i class="fas fa-xmark"></i></button>
    </div>
    <div id="modalBody" class="flex-1 overflow-y-auto px-6 py-4"></div>
    <div id="modalFooter" class="px-6 py-4 light:border-t dark:border-t dark:border-slate-800 border-slate-100 flex justify-end gap-2"></div>
  </div>
</div>

<div id="toastContainer" class="fixed top-4 right-4 z-[60] space-y-2"></div>

<div id="confirmOverlay" class="fixed inset-0 bg-black/40 z-[55] flex items-center justify-center p-4 hidden backdrop-blur-sm">
  <div class="light:bg-white dark:bg-slate-900 rounded-2xl shadow-2xl w-full max-w-sm p-6 modal-enter text-center light:border dark:border dark:border-slate-800">
    <div class="w-14 h-14 rounded-full light:bg-red-50 dark:bg-red-950/30 flex items-center justify-center mx-auto mb-4"><i class="fas fa-triangle-exclamation light:text-red-400 dark:text-red-500 text-xl"></i></div>
    <p id="confirmMsg" class="light:text-slate-600 dark:text-slate-300 mb-6 text-sm"></p>
    <div class="flex gap-3 justify-center">
      <button onclick="closeConfirm()" class="px-5 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 light:text-slate-600 dark:text-slate-300 font-semibold text-sm hover:opacity-80">Batal</button>
      <button id="confirmBtn" class="px-5 py-2 rounded-xl bg-red-500 text-white font-semibold text-sm hover:bg-red-600">Hapus</button>
    </div>
  </div>
</div>
</div>

<!-- NOTA PRINT AREA (tersembunyi, hanya muncul saat print) -->
<div id="notaPrintArea" style="display:none;"></div>

<script>
/* ===== STATE ===== */
var APP = { data: null, page: 'dashboard', charts: {}, theme: 'light', isLoggedIn: false, selectedNotaId: null, notaSettings: { namaToko: 'Eco Sport Jersey', alamatToko: 'Jl. Olahraga No. 88, Bandung', telpToko: '0812-3456-7890', emailToko: 'info@ecosportjersey.com', showStamp: true, showQR: true, notaType: 'invoice' } };

function gid() { return 'id_' + Date.now() + '_' + Math.random().toString(36).substr(2, 6); }
function fRp(n) { return 'Rp ' + (n || 0).toLocaleString('id-ID'); }
function fDt(d) { if (!d) return '-'; return new Date(d).toLocaleDateString('id-ID', { day: '2-digit', month: 'short', year: 'numeric' }); }
function fDtFull(d) { if (!d) return '-'; return new Date(d).toLocaleDateString('id-ID', { day: '2-digit', month: 'long', year: 'numeric' }); }
function fToday() { return new Date().toISOString().split('T')[0]; }
function fNow() { var d=new Date(); return String(d.getHours()).padStart(2,'0')+':'+String(d.getMinutes()).padStart(2,'0'); }

/* ===== DATA ===== */
function emptyData() { return { pelanggan: [], produk: [], pesanan: [], perekapan: [] }; }
function loadData() {
  try { var s = localStorage.getItem('ecoSportData'); if (s) return JSON.parse(s); } catch(e) {}
  var d = emptyData(); saveData(d); return d;
}
function saveData(d) { localStorage.setItem('ecoSportData', JSON.stringify(d)); }
function loadNotaSettings() {
  try { var s = localStorage.getItem('ecoNotaSettings'); if (s) { var p=JSON.parse(s); for(var k in p) APP.notaSettings[k]=p[k]; } } catch(e) {}
}
function saveNotaSettings() { localStorage.setItem('ecoNotaSettings', JSON.stringify(APP.notaSettings)); }

/* ===== AUTH ===== */
function login() {
  var u = document.getElementById('loginUser').value;
  var p = document.getElementById('loginPass').value;
  if (u === 'admin' && p === 'admin123') {
    localStorage.setItem('ecoSportLogin', 'true');
    APP.isLoggedIn = true;
    document.getElementById('loginScreen').classList.add('hidden');
    document.getElementById('appContainer').classList.remove('hidden');
    go('dashboard');
    toast('Selamat datang kembali, Admin!', 'success');
  } else {
    document.getElementById('loginError').classList.remove('hidden');
    document.getElementById('loginPass').value = '';
    setTimeout(function(){ document.getElementById('loginError').classList.add('hidden'); }, 3000);
  }
}
function logout() {
  localStorage.removeItem('ecoSportLogin');
  APP.isLoggedIn = false;
  document.getElementById('appContainer').classList.add('hidden');
  document.getElementById('loginScreen').classList.remove('hidden');
}

/* ===== THEME ===== */
function toggleTheme() {
  APP.theme = APP.theme === 'light' ? 'dark' : 'light';
  document.documentElement.className = APP.theme;
  localStorage.setItem('ecoTheme', APP.theme);
  document.getElementById('themeIcon').className = APP.theme === 'dark' ? 'fas fa-moon' : 'fas fa-sun';
  if (APP.page === 'dashboard') { destroyCharts(); initCharts(); }
}
function loadTheme() {
  var t = localStorage.getItem('ecoTheme');
  if (t === 'dark' || t === 'light') APP.theme = t;
  document.documentElement.className = APP.theme;
  document.getElementById('themeIcon').className = APP.theme === 'dark' ? 'fas fa-moon' : 'fas fa-sun';
}

/* ===== NAVIGATION ===== */
var TITLES = { dashboard:'Dashboard', pesanan:'Manajemen Pesanan', pelanggan:'Data Pelanggan', produk:'Katalog Produk', perekapan:'Perekapan Keuangan', nota:'Cetak Nota', laporan:'Laporan & Export' };

function go(page) {
  APP.page = page;
  document.getElementById('pageTitle').textContent = TITLES[page] || page;
  var items = document.querySelectorAll('.nav-item');
  for (var i=0; i<items.length; i++) items[i].classList.toggle('active', items[i].getAttribute('data-page')===page);
  destroyCharts();
  render();
  if (window.innerWidth < 1024) toggleSidebar(false);
}
function toggleSidebar(force) {
  var sb = document.getElementById('sidebar');
  var ov = document.getElementById('sidebarOverlay');
  var open = force !== undefined ? force : !sb.classList.contains('open');
  sb.classList.toggle('open', open);
  ov.classList.toggle('active', open);
}
function destroyCharts() { for(var k in APP.charts){ if(APP.charts[k].destroy) APP.charts[k].destroy(); } APP.charts={}; }

/* ===== UI HELPERS ===== */
function C(icon, title, desc) {
  return '<div class="empty-state"><i class="fas fa-'+icon+'"></i><p class="title">'+title+'</p><p>'+desc+'</p></div>';
}
function sBadge(s) {
  var m = { 'Baru':'bg-blue-100 text-blue-600 dark:bg-blue-900/30 dark:text-blue-400', 'Proses':'bg-amber-100 text-amber-600 dark:bg-amber-900/30 dark:text-amber-400', 'Siap Kirim':'bg-purple-100 text-purple-600 dark:bg-purple-900/30 dark:text-purple-400', 'Selesai':'bg-emerald-100 text-emerald-600 dark:bg-emerald-900/30 dark:text-emerald-400', 'Batal':'bg-red-100 text-red-600 dark:bg-red-900/30 dark:text-red-400' };
  return '<span class="badge '+(m[s]||m['Baru'])+'">'+s+'</span>';
}
var CBG = 'light:bg-white dark:bg-slate-900 light:border-slate-100 dark:border-slate-800';
var IBG = 'bg-indigo-50 dark:bg-slate-800';
var ICLR = 'text-indigo-500 dark:text-indigo-400';
var TCLR = 'light:text-slate-800 dark:text-slate-100';
var TCLR2 = 'light:text-slate-700 dark:text-slate-300';
var SCLR = 'light:text-slate-400 dark:text-slate-500';
var INP = 'border light:border-slate-200 dark:border-slate-700 rounded-xl px-4 py-2.5 text-sm light:bg-white dark:bg-slate-800 light:text-slate-800 dark:text-slate-100';

function mkField(label, name, type, val, extra) {
  var ro = (extra||'').indexOf('readonly')>=0;
  return '<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">'+label+'</label><input id="f-'+name+'" type="'+type+'" value="'+(val||'')+'" class="w-full '+INP+' '+(ro?'light:!bg-slate-50 dark:!bg-slate-800/50 light:!text-slate-400 dark:!text-slate-500 cursor-not-allowed':'')+'" '+(extra||'')+'></div>';
}
function mkSelect(label, name, opts, extra) {
  return '<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">'+label+'</label><select id="f-'+name+'" class="w-full '+INP+'" '+(extra||'')+'>'+opts+'</select></div>';
}
function mkArea(label, name, val) {
  return '<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">'+label+'</label><textarea id="f-'+name+'" rows="2" class="w-full '+INP+' resize-none">'+(val||'')+'</textarea></div>';
}
function FV(names) { var o={}; for(var i=0;i<names.length;i++){ var el=document.getElementById('f-'+names[i]); o[names[i]]=el?el.value:''; } return o; }

function showModal(title, body, footer) {
  document.getElementById('modalTitle').textContent=title;
  document.getElementById('modalBody').innerHTML=body;
  document.getElementById('modalFooter').innerHTML=footer;
  document.getElementById('modalOverlay').classList.remove('hidden');
  document.body.style.overflow='hidden';
}
function closeModal() { document.getElementById('modalOverlay').classList.add('hidden'); document.body.style.overflow=''; }

function toast(msg, type) {
  type=type||'success';
  var colors={success:'#4f46e5',error:'#dc2626',warning:'#d97706',info:'#0284c7'};
  var icons={success:'fa-check-circle',error:'fa-circle-xmark',warning:'fa-triangle-exclamation',info:'fa-circle-info'};
  var el=document.createElement('div');
  el.className='toast-enter flex items-center gap-3 px-5 py-3 rounded-xl text-white shadow-lg text-sm font-medium min-w-[280px]';
  el.style.background=colors[type]||colors.success;
  el.innerHTML='<i class="fas '+(icons[type]||icons.success)+'"></i><span class="flex-1">'+msg+'</span><button onclick="this.parentElement.remove()" class="hover:opacity-70"><i class="fas fa-xmark"></i></button>';
  document.getElementById('toastContainer').appendChild(el);
  setTimeout(function(){ el.classList.add('toast-exit'); setTimeout(function(){el.remove();},300); },3000);
}

var _cfmCb=null;
function showConfirm(msg, cb) { _cfmCb=cb; document.getElementById('confirmMsg').textContent=msg; document.getElementById('confirmOverlay').classList.remove('hidden'); }
function closeConfirm() { document.getElementById('confirmOverlay').classList.add('hidden'); _cfmCb=null; }
document.getElementById('confirmBtn').addEventListener('click', function(){ var cb=_cfmCb; closeConfirm(); if(cb) cb(); });

function resetData() {
  showConfirm('Yakin ingin mereset semua data?', function(){ APP.data=emptyData(); saveData(APP.data); go(APP.page); toast('Data berhasil direset'); });
}

/* ===== RENDER ===== */
function render() {
  var c = document.getElementById('content');
  var fn = { dashboard:rDash, pesanan:rPesanan, pelanggan:rPelanggan, produk:rProduk, perekapan:rRekap, nota:rNota, laporan:rLaporan };
  c.innerHTML = (fn[APP.page]||rDash)();
  c.scrollTop = 0;
  if (APP.page === 'dashboard') initCharts();
}

/* ----- DASHBOARD ----- */
function rDash() {
  var d=APP.data, tP=d.pesanan.length, rev=0, aktif=0, tC=d.pelanggan.length;
  for(var i=0;i<d.pesanan.length;i++){ if(d.pesanan[i].status!=='Batal') rev+=d.pesanan[i].total; if(d.pesanan[i].status==='Baru'||d.pesanan[i].status==='Proses'||d.pesanan[i].status==='Siap Kirim') aktif++; }
  var has=tP>0;
  var recent=d.pesanan.slice().sort(function(a,b){return b.tglOrder.localeCompare(a.tglOrder);}).slice(0,5);
  return '<div class="fade-in space-y-6"><div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">'+
    statCard('fa-clipboard-list','Total',tP,'Total Pesanan','bg-blue-100 text-blue-500')+
    statCard('fa-coins','Revenue',fRp(rev),'Total Pendapatan','bg-emerald-100 text-emerald-500')+
    statCard('fa-clock','Aktif',aktif,'Pesanan Aktif','bg-amber-100 text-amber-500')+
    statCard('fa-users','Client',tC,'Pelanggan','bg-purple-100 text-purple-500')+
    '</div><div class="grid grid-cols-1 lg:grid-cols-3 gap-4">'+
    '<div class="lg:col-span-2 '+CBG+' rounded-2xl p-5 border shadow-sm"><h3 class="font-bold '+TCLR2+' mb-4">Pendapatan Bulanan</h3>'+(has?'<canvas id="chartRevenue" height="110"></canvas>':C('chart-bar','Belum Ada Data','Tambahkan pesanan untuk melihat grafik'))+'</div>'+
    '<div class="'+CBG+' rounded-2xl p-5 border shadow-sm"><h3 class="font-bold '+TCLR2+' mb-4">Status Pesanan</h3>'+(has?'<canvas id="chartStatus" height="160"></canvas>':C('chart-pie','Belum Ada Data','Status pesanan akan tampil di sini'))+'</div></div>'+
    '<div class="'+CBG+' rounded-2xl border shadow-sm overflow-hidden"><div class="px-5 py-4 light:border-b dark:border-b dark:border-slate-800 border-slate-100 flex items-center justify-between"><h3 class="font-bold '+TCLR2+'">Pesanan Terbaru</h3>'+(has?'<button onclick="go(\'pesanan\')" class="text-sm text-indigo-500 font-semibold hover:opacity-70">Lihat Semua <i class="fas fa-arrow-right ml-1 text-xs"></i></button>':'')+'</div>'+
    (has?'<div class="overflow-x-auto"><table class="w-full text-sm"><thead><tr class="text-left '+SCLR+' light:border-b dark:border-b dark:border-slate-800 border-slate-50 text-xs"><th class="px-5 py-3 font-semibold">NOMOR PESANAN</th><th class="px-5 py-3 font-semibold">Pelanggan</th><th class="px-5 py-3 font-semibold">Produk</th><th class="px-5 py-3 font-semibold">Total</th><th class="px-5 py-3 font-semibold">Status</th><th class="px-5 py-3 font-semibold">Tgl</th></tr></thead><tbody>'+
    recent.map(function(o){ var pl=d.pelanggan.find(function(p){return p.id===o.pelangganId;}); var pr=d.produk.find(function(p){return p.id===o.produkId;}); return '<tr class="table-row light:border-b dark:border-b dark:border-slate-800 border-slate-50"><td class="px-5 py-3 font-mono font-semibold '+TCLR2+'">'+o.noPO+'</td><td class="px-5 py-3 '+SCLR+'">'+(pl?pl.nama:'-')+'</td><td class="px-5 py-3 '+SCLR+'">'+(pr?pr.nama:'-')+'</td><td class="px-5 py-3 font-semibold '+TCLR+'">'+fRp(o.total)+'</td><td class="px-5 py-3">'+sBadge(o.status)+'</td><td class="px-5 py-3 '+SCLR+'">'+fDt(o.tglOrder)+'</td></tr>'; }).join('')+
    '</tbody></table></div>':C('inbox','Belum Ada Pesanan','Mulai tambah pesanan pertama Anda'))+'</div></div>';
}
function statCard(icon,label,val,sub,colorClass) {
  return '<div class="stat-card '+CBG+' rounded-2xl p-5 border shadow-sm"><div class="flex items-center justify-between mb-3"><div class="w-11 h-11 rounded-xl flex items-center justify-center '+colorClass+'"><i class="fas '+icon+'"></i></div><span class="text-xs font-semibold '+SCLR+' '+IBG+' px-2 py-1 rounded-lg">'+label+'</span></div><p class="text-2xl font-800 '+TCLR+'">'+val+'</p><p class="text-sm '+SCLR+' mt-1">'+sub+'</p></div>';
}

function initCharts() {
  var d=APP.data; if(d.pesanan.length===0) return;
  var dk=APP.theme==='dark';
  var gc=dk?'rgba(255,255,255,.06)':'#f1f5f9';
  var tc=dk?'#64748b':'#94a3b8';
  var months=['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Agu','Sep','Okt','Nov','Des'];
  var rd=[]; for(var i=0;i<12;i++) rd.push(0);
  for(var j=0;j<d.pesanan.length;j++){ if(d.pesanan[j].status!=='Batal') rd[new Date(d.pesanan[j].tglOrder).getMonth()]+=d.pesanan[j].total; }
  var e1=document.getElementById('chartRevenue');
  if(e1) APP.charts.rev=new Chart(e1,{type:'bar',data:{labels:months,datasets:[{label:'Pendapatan',data:rd,backgroundColor:'#4f46e5',hoverBackgroundColor:'#4338ca',borderRadius:6,borderSkipped:false}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,ticks:{callback:function(v){return fRp(v);},color:tc},grid:{color:gc}},x:{grid:{display:false},ticks:{color:tc}}}}});
  var ss=['Baru','Proses','Siap Kirim','Selesai','Batal'];
  var sc=ss.map(function(s){return d.pesanan.filter(function(o){return o.status===s;}).length;});
  var sbg=['#3b82f6','#f59e0b','#a855f7','#10b981','#ef4444'];
  var e2=document.getElementById('chartStatus');
  if(e2) APP.charts.st=new Chart(e2,{type:'doughnut',data:{labels:ss,datasets:[{data:sc,backgroundColor:sbg,borderColor:dk?'#0f172a':'#ffffff',borderWidth:3}]},options:{responsive:true,cutout:'65%',plugins:{legend:{position:'bottom',labels:{padding:12,usePointStyle:true,pointStyleWidth:8,font:{size:11},color:dk?'#94a3b8':'#64748b'}}}}});
}

/* ----- PESANAN ----- */
function rPesanan() {
  var d=APP.data, has=d.pesanan.length>0;
  return '<div class="fade-in space-y-4"><div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3"><div class="flex items-center gap-2 flex-wrap">'+
    '<input id="searchPesanan" type="text" placeholder="Cari no PO / pelanggan..." class="'+INP+' w-64" oninput="filterPesanan()">'+
    '<select id="filterStatus" class="'+INP+'" onchange="filterPesanan()"><option value="">Semua Status</option><option value="Baru">Baru</option><option value="Proses">Proses</option><option value="Siap Kirim">Siap Kirim</option><option value="Selesai">Selesai</option><option value="Batal">Batal</option></select></div>'+
    '<div class="flex gap-2">'+(has?'<button onclick="expPesPDF()" class="btn-export flex items-center gap-2 px-4 py-2 '+IBG+' '+SCLR+' rounded-xl text-sm font-semibold hover:opacity-80 light:border-slate-200 dark:border-slate-700 border"><i class="fas fa-file-pdf"></i> PDF</button><button onclick="expPesExcel()" class="btn-export flex items-center gap-2 px-4 py-2 '+IBG+' '+SCLR+' rounded-xl text-sm font-semibold hover:opacity-80 light:border-slate-200 dark:border-slate-700 border"><i class="fas fa-file-excel"></i> Excel</button>':'')+
    '<button onclick="formPesanan()" class="btn-primary px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2"><i class="fas fa-plus"></i> Tambah</button></div></div>'+
    '<div class="'+CBG+' rounded-2xl border shadow-sm overflow-hidden"><div class="overflow-x-auto" id="pesTable">'+(has?buildPesTable(d.pesanan):C('clipboard-list','Belum Ada Pesanan','Klik tombol Tambah untuk membuat pesanan baru'))+'</div></div></div>';
}
function buildPesTable(list) {
  var d=APP.data, th='text-left '+SCLR+' light:border-b dark:border-b dark:border-slate-800 border-slate-50 text-xs';
  var bv='w-8 h-8 rounded-lg light:hover:bg-indigo-50 dark:hover:bg-slate-800 light:text-indigo-400 dark:text-slate-500 hover:opacity-70 inline-flex items-center justify-center';
  var bd='w-8 h-8 rounded-lg light:hover:bg-red-50 dark:hover:bg-red-950/30 light:text-slate-300 dark:text-slate-600 hover:!text-red-400 inline-flex items-center justify-center';
  var bn='w-8 h-8 rounded-lg light:hover:bg-emerald-50 dark:hover:bg-emerald-950/30 light:text-emerald-400 dark:text-slate-500 hover:opacity-70 inline-flex items-center justify-center';
  return '<table class="w-full text-sm"><thead><tr class="'+th+'"><th class="px-4 py-3 font-semibold">NOMOR PESANAN</th><th class="px-4 py-3 font-semibold">Pelanggan</th><th class="px-4 py-3 font-semibold">Produk</th><th class="px-4 py-3 font-semibold">Qty</th><th class="px-4 py-3 font-semibold">Total</th><th class="px-4 py-3 font-semibold">Status</th><th class="px-4 py-3 font-semibold">Deadline</th><th class="px-4 py-3 font-semibold text-center">Aksi</th></tr></thead><tbody>'+
  list.map(function(o){
    var pl=d.pelanggan.find(function(p){return p.id===o.pelangganId;});
    var pr=d.produk.find(function(p){return p.id===o.produkId;});
    return '<tr class="table-row light:border-b dark:border-b dark:border-slate-800 border-slate-50"><td class="px-4 py-3 font-mono font-semibold '+TCLR2+' text-xs">'+o.noPO+'</td><td class="px-4 py-3"><p class="font-medium '+TCLR2+'">'+(pl?pl.nama:'-')+'</p><p class="text-xs '+SCLR+'">'+(pl?pl.tim:'')+'</p></td><td class="px-4 py-3 '+SCLR+' text-xs">'+(pr?pr.nama:'-')+'</td><td class="px-4 py-3 font-semibold">'+o.jumlah+'</td><td class="px-4 py-3 font-semibold '+TCLR+'">'+fRp(o.total)+'</td><td class="px-4 py-3">'+sBadge(o.status)+'</td><td class="px-4 py-3 '+SCLR+' text-xs">'+fDt(o.deadline)+'</td><td class="px-4 py-3 text-center whitespace-nowrap"><button onclick="goNota(\''+o.id+'\')" class="'+bn+'" title="Cetak Nota"><i class="fas fa-receipt text-xs"></i></button><button onclick="detailPesanan(\''+o.id+'\')" class="'+bv+'" title="Detail"><i class="fas fa-eye text-xs"></i></button><button onclick="formPesanan(\''+o.id+'\')" class="'+bv+'" title="Edit"><i class="fas fa-pen text-xs"></i></button><button onclick="hapusPesanan(\''+o.id+'\')" class="'+bd+'" title="Hapus"><i class="fas fa-trash text-xs"></i></button></td></tr>';
  }).join('')+'</tbody></table>';
}
function filterPesanan() {
  var q=(document.getElementById('searchPesanan').value||'').toLowerCase();
  var s=document.getElementById('filterStatus').value;
  var list=APP.data.pesanan;
  if(q) list=list.filter(function(o){ var pl=APP.data.pelanggan.find(function(p){return p.id===o.pelangganId;}); return o.noPO.toLowerCase().indexOf(q)>=0||(pl&&pl.nama.toLowerCase().indexOf(q)>=0); });
  if(s) list=list.filter(function(o){return o.status===s;});
  document.getElementById('pesTable').innerHTML=list.length?buildPesTable(list):C('search','Tidak Ditemukan','Coba ubah kata kunci');
}

/* Navigasi cepat ke nota dari tabel pesanan */
function goNota(pesananId) {
  APP.selectedNotaId = pesananId;
  go('nota');
}

function formPesanan(id) {
  var d=APP.data, o=id?d.pesanan.find(function(x){return x.id===id;}):null;
  if(!d.pelanggan.length) return toast('Tambahkan pelanggan terlebih dahulu','warning');
  if(!d.produk.length) return toast('Tambahkan produk terlebih dahulu','warning');
  var nextNo='PO-'+new Date().getFullYear()+'-'+String(d.pesanan.length+1).padStart(3,'0');
  var pOpt=d.pelanggan.map(function(p){return '<option value="'+p.id+'"'+(o&&o.pelangganId===p.id?' selected':'')+'>'+p.nama+' - '+p.tim+'</option>';}).join('');
  var prOpt=d.produk.map(function(p){return '<option value="'+p.id+'"'+(o&&o.produkId===p.id?' selected':'')+'>'+p.nama+' ('+fRp(p.harga)+')</option>';}).join('');
  var stOpt=['Baru','Proses','Siap Kirim','Selesai','Batal'].map(function(s){return '<option'+(o&&o.status===s?' selected':'')+'>'+s+'</option>';}).join('');
  var body='<div class="grid grid-cols-1 md:grid-cols-2 gap-4">'+
    mkField('NOMOR PESANAN','noPO','text',o?o.noPO:nextNo,'readonly')+
    mkSelect('Pelanggan','pelangganId',pOpt)+
    mkSelect('Produk','produkId',prOpt,'onchange="onProdChange()"')+
    mkField('Jumlah','jumlah','number',o?o.jumlah:'','oninput="calcTotal()" min="1"')+
    
    mkField('Harga Satuan','hargaSatuan','number',o?o.hargaSatuan:'','oninput="calcTotal()" min="0"')+
    mkField('Total','total','number',o?o.total:'','readonly')+
    mkField('DP','dp','number',o?o.dp:'','oninput="calcTotal()" min="0"')+
    mkField('Sisa','sisa','number',o?o.sisa:'','readonly')+
    '<div class="md:col-span-2 grid grid-cols-2 gap-4">'+mkSelect('Status','status',stOpt)+mkField('Tgl Order','tglOrder','date',o?o.tglOrder:fToday())+'</div>'+
    mkField('Pengambilan','deadline','date',o?o.deadline:'')+'<div></div>'+
    '<div class="md:col-span-2">'+mkArea('Catatan','catatan',o?o.catatan:'')+'</div></div>';
  showModal(o?'Edit Pesanan':'Tambah Pesanan Baru', body,
    '<button onclick="closeModal()" class="px-4 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 '+SCLR+' font-semibold text-sm hover:opacity-80">Batal</button>'+
    '<button onclick="simpanPesanan(\''+(id||'')+'\')" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Simpan</button>');
}
function onProdChange(){ var pr=APP.data.produk.find(function(p){return p.id===document.getElementById('f-produkId').value;}); if(pr){document.getElementById('f-hargaSatuan').value=pr.harga; calcTotal();} }
function calcTotal() {
  var j=parseInt(document.getElementById('f-jumlah').value)||0;
  var h=parseInt(document.getElementById('f-hargaSatuan').value)||0;
  document.getElementById('f-total').value=j*h;
  var dp=parseInt(document.getElementById('f-dp').value)||0;
  document.getElementById('f-sisa').value=(j*h)-dp;
}
function simpanPesanan(id) {
  var f=FV(['noPO','pelangganId','produkId','jumlah','ukuran','warna','hargaSatuan','total','dp','sisa','status','tglOrder','deadline','catatan']);
  f.jumlah=parseInt(f.jumlah)||0; f.hargaSatuan=parseInt(f.hargaSatuan)||0; f.total=parseInt(f.total)||0; f.dp=parseInt(f.dp)||0; f.sisa=parseInt(f.sisa)||0;
  if(!f.pelangganId||!f.produkId||f.jumlah<=0) return toast('Lengkapi semua field wajib','error');
  if(id){ for(var i=0;i<APP.data.pesanan.length;i++){ if(APP.data.pesanan[i].id===id){ for(var k in f) APP.data.pesanan[i][k]=f[k]; break; } } toast('Pesanan diperbarui'); }
  else { f.id=gid(); APP.data.pesanan.push(f); toast('Pesanan baru ditambahkan'); }
  saveData(APP.data); closeModal(); go('pesanan');
}
function hapusPesanan(id) {
  showConfirm('Yakin ingin menghapus pesanan ini?',function(){ APP.data.pesanan=APP.data.pesanan.filter(function(o){return o.id!==id;}); saveData(APP.data); go('pesanan'); toast('Pesanan dihapus'); });
}
function detailPesanan(id) {
  var o=APP.data.pesanan.find(function(x){return x.id===id;}); if(!o) return;
  var pl=APP.data.pelanggan.find(function(p){return p.id===o.pelangganId;});
  var pr=APP.data.produk.find(function(p){return p.id===o.produkId;});
  var box='bg-indigo-50 dark:bg-slate-800 rounded-xl p-3';
  var lbl=SCLR+' text-xs mb-1';
  showModal('Detail Pesanan',
    '<div class="space-y-4"><div class="grid grid-cols-2 gap-4 text-sm">'+
    '<div><p class="'+lbl+'">NOMOR PESANAN</p><p class="font-bold '+TCLR+' font-mono">'+o.noPO+'</p></div>'+
    '<div><p class="'+lbl+'">Status</p>'+sBadge(o.status)+'</div>'+
    '<div><p class="'+lbl+'">Pelanggan</p><p class="font-semibold '+TCLR2+'">'+(pl?pl.nama:'-')+'</p></div>'+
    '<div><p class="'+lbl+'">Produk</p><p class="font-semibold '+TCLR2+'">'+(pr?pr.nama:'-')+'</p></div>'+
    '<div><p class="'+lbl+'">Jumlah</p><p class="font-semibold">'+o.jumlah+' pcs</p></div>'+
    '<div><p class="'+lbl+'">Ukuran</p><p class="font-semibold">'+o.ukuran+'</p></div>'+
    '<div><p class="'+lbl+'">Warna</p><p class="font-semibold">'+o.warna+'</p></div>'+
    '<div><p class="'+lbl+'">Deadline</p><p class="font-semibold">'+fDt(o.deadline)+'</p></div></div>'+
    '<hr class="light:border-slate-100 dark:border-slate-800">'+
    '<div class="grid grid-cols-3 gap-4 text-sm text-center">'+
    '<div class="'+box+'"><p class="text-xs '+SCLR+'">Total</p><p class="font-bold '+TCLR+'">'+fRp(o.total)+'</p></div>'+
    '<div class="'+box+'"><p class="text-xs '+SCLR+'">DP</p><p class="font-bold '+TCLR+'">'+fRp(o.dp)+'</p></div>'+
    '<div class="'+box+'"><p class="text-xs '+SCLR+'">Sisa</p><p class="font-bold '+TCLR+'">'+fRp(o.sisa)+'</p></div></div>'+
    (o.catatan?'<div class="text-sm"><p class="'+lbl+'">Catatan</p><p class="'+SCLR+'">'+o.catatan+'</p></div>':'')+'</div>',
    '<button onclick="closeModal();goNota(\''+o.id+'\')" class="px-4 py-2 rounded-xl light:bg-emerald-50 dark:bg-emerald-950/30 text-emerald-600 dark:text-emerald-400 font-semibold text-sm hover:opacity-80 flex items-center gap-2"><i class="fas fa-receipt"></i> Cetak Nota</button>'+
    '<button onclick="closeModal()" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Tutup</button>');
}

/* ----- PELANGGAN ----- */
function rPelanggan() {
  var d=APP.data, has=d.pelanggan.length>0;
  return '<div class="fade-in space-y-4"><div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3">'+
    '<input id="searchPel" type="text" placeholder="Cari nama / tim..." class="'+INP+' w-64" oninput="filterPel()">'+
    '<button onclick="formPelanggan()" class="btn-primary px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2"><i class="fas fa-plus"></i> Tambah Pelanggan</button></div>'+
    '<div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4" id="pelGrid">'+(has?buildPelCards(d.pelanggan):C('users','Belum Ada Pelanggan','Klik tombol Tambah untuk menambah pelanggan'))+'</div></div>';
}
function buildPelCards(list) {
  var d=APP.data, bh='w-8 h-8 rounded-lg light:hover:bg-indigo-50 dark:hover:bg-slate-800 light:text-indigo-400 dark:text-slate-600 hover:!text-red-400 flex items-center justify-center';
  return list.map(function(p){
    var oc=0, ts=0;
    for(var i=0;i<d.pesanan.length;i++){ if(d.pesanan[i].pelangganId===p.id){ oc++; if(d.pesanan[i].status!=='Batal') ts+=d.pesanan[i].total; } }
    return '<div class="'+CBG+' rounded-2xl border shadow-sm p-5 hover:shadow-md transition-shadow"><div class="flex items-start justify-between mb-3"><div class="flex items-center gap-3"><div class="w-11 h-11 rounded-full bg-gradient-to-tr from-indigo-400 to-purple-400 flex items-center justify-center text-white font-bold">'+p.nama.charAt(0)+'</div><div><p class="font-bold '+TCLR+'">'+p.nama+'</p><p class="text-xs '+SCLR+'">'+p.tim+'</p></div></div><div class="flex gap-1"><button onclick="formPelanggan(\''+p.id+'\')" class="'+bh+'"><i class="fas fa-pen text-xs"></i></button><button onclick="hapusPelanggan(\''+p.id+'\')" class="'+bh+'"><i class="fas fa-trash text-xs"></i></button></div></div><div class="space-y-1 text-xs '+SCLR+' mb-3"><p><i class="fas fa-phone w-4 light:text-indigo-300 dark:text-slate-600"></i> '+p.telepon+'</p><p><i class="fas fa-envelope w-4 light:text-indigo-300 dark:text-slate-600"></i> '+(p.email||'-')+'</p><p><i class="fas fa-location-dot w-4 light:text-indigo-300 dark:text-slate-600"></i> '+p.alamat+'</p></div><div class="flex gap-3 pt-3 light:border-t dark:border-t dark:border-slate-800 border-slate-50"><div class="text-center flex-1"><p class="text-lg font-bold '+TCLR+'">'+oc+'</p><p class="text-xs '+SCLR+'">Pesanan</p></div><div class="text-center flex-1"><p class="text-sm font-bold '+TCLR2+'">'+fRp(ts)+'</p><p class="text-xs '+SCLR+'">Total</p></div></div></div>';
  }).join('');
}
function filterPel() {
  var q=(document.getElementById('searchPel').value||'').toLowerCase();
  var list=APP.data.pelanggan;
  if(q) list=list.filter(function(p){return p.nama.toLowerCase().indexOf(q)>=0||p.tim.toLowerCase().indexOf(q)>=0;});
  document.getElementById('pelGrid').innerHTML=list.length?buildPelCards(list):C('search','Tidak Ditemukan','Coba ubah kata kunci');
}
function formPelanggan(id) {
  var o=id?APP.data.pelanggan.find(function(x){return x.id===id;}):null;
  var body='<div class="space-y-4">'+mkField('Nama','nama','text',o?o.nama:'')+mkField('Tim / Instansi','tim','text',o?o.tim:'')+mkField('Telepon','telepon','text',o?o.telepon:'')+mkField('Email','email','email',o?o.email:'')+mkField('Alamat','alamat','text',o?o.alamat:'')+'</div>';
  showModal(o?'Edit Pelanggan':'Tambah Pelanggan', body,
    '<button onclick="closeModal()" class="px-4 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 '+SCLR+' font-semibold text-sm hover:opacity-80">Batal</button>'+
    '<button onclick="simpanPelanggan(\''+(id||'')+'\')" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Simpan</button>');
}
function simpanPelanggan(id) {
  var f=FV(['nama','tim','telepon','email','alamat']);
  if(!f.nama) return toast('Nama wajib diisi','error');
  if(id){ for(var i=0;i<APP.data.pelanggan.length;i++){ if(APP.data.pelanggan[i].id===id){ for(var k in f) APP.data.pelanggan[i][k]=f[k]; break; } } toast('Pelanggan diperbarui'); }
  else { f.id=gid(); APP.data.pelanggan.push(f); toast('Pelanggan ditambahkan'); }
  saveData(APP.data); closeModal(); go('pelanggan');
}
function hapusPelanggan(id) {
  showConfirm('Yakin ingin menghapus pelanggan ini?',function(){ APP.data.pelanggan=APP.data.pelanggan.filter(function(x){return x.id!==id;}); saveData(APP.data); go('pelanggan'); toast('Pelanggan dihapus'); });
}

/* ----- PRODUK ----- */
function rProduk() {
  var d=APP.data, has=d.produk.length>0;
  var kats=[]; d.produk.forEach(function(p){ if(kats.indexOf(p.kategori)<0) kats.push(p.kategori); });
  return '<div class="fade-in space-y-4"><div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3"><div class="flex items-center gap-2 flex-wrap">'+
    '<input id="searchPrd" type="text" placeholder="Cari produk..." class="'+INP+' w-56" oninput="filterPrd()">'+
    (has?'<select id="filterKat" class="'+INP+'" onchange="filterPrd()"><option value="">Semua Kategori</option>'+kats.map(function(k){return '<option>'+k+'</option>';}).join('')+'</select>':'')+
    '</div><button onclick="formProduk()" class="btn-primary px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2"><i class="fas fa-plus"></i> Tambah Produk</button></div>'+
    '<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4" id="prdGrid">'+(has?buildPrdCards(d.produk):C('shirt','Belum Ada Produk','Klik tombol Tambah untuk menambah katalog produk'))+'</div></div>';
}
function buildPrdCards(list) {
  var icons={'Futsal':'fa-futbol','Sepak Bola':'fa-futbol','Basket':'fa-basketball','Badminton':'fa-table-tennis-paddle-ball','Running':'fa-person-running','Cycling':'fa-bicycle','Training':'fa-dumbbell'};
  return list.map(function(p){
    var icon=icons[p.kategori]||'fa-shirt';
    var oc=0; for(var i=0;i<APP.data.pesanan.length;i++){ if(APP.data.pesanan[i].produkId===p.id) oc++; }
    var bh='w-7 h-7 rounded-lg light:bg-white dark:bg-slate-800 shadow-sm light:border-slate-100 dark:border-slate-700 border flex items-center justify-center';
    return '<div class="'+CBG+' rounded-2xl border shadow-sm overflow-hidden hover:shadow-md transition-shadow group"><div class="h-32 bg-gradient-to-tr from-indigo-50 to-purple-50 dark:from-slate-800 dark:to-slate-700 flex items-center justify-center relative"><i class="fas '+icon+' text-4xl text-indigo-400 dark:text-slate-600 group-hover:opacity-80 transition-opacity"></i><span class="absolute top-3 left-3 badge bg-white dark:bg-slate-800 light:border-slate-200 dark:border-slate-700 border '+SCLR+'">'+p.kategori+'</span><div class="absolute top-3 right-3 flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity"><button onclick="formProduk(\''+p.id+'\')" class="'+bh+' light:text-indigo-400 dark:text-slate-500 hover:opacity-70"><i class="fas fa-pen text-xs"></i></button><button onclick="hapusProduk(\''+p.id+'\')" class="'+bh+' light:text-slate-300 dark:text-slate-600 hover:!text-red-400"><i class="fas fa-trash text-xs"></i></button></div></div><div class="p-4"><h4 class="font-bold '+TCLR+' text-sm mb-1">'+p.nama+'</h4><p class="text-xs '+SCLR+' mb-3 truncate">'+p.deskripsi+'</p><div class="flex items-end justify-between"><p class="text-lg font-800 text-indigo-500 dark:text-indigo-400">'+fRp(p.harga)+'</p><p class="text-xs '+SCLR+'">'+oc+' pesanan</p></div></div></div>';
  }).join('');
}
function filterPrd() {
  var q=(document.getElementById('searchPrd').value||'').toLowerCase();
  var k=document.getElementById('filterKat')?document.getElementById('filterKat').value:'';
  var list=APP.data.produk;
  if(q) list=list.filter(function(p){return p.nama.toLowerCase().indexOf(q)>=0;});
  if(k) list=list.filter(function(p){return p.kategori===k;});
  document.getElementById('prdGrid').innerHTML=list.length?buildPrdCards(list):C('search','Tidak Ditemukan','Coba ubah kata kunci');
}
function formProduk(id) {
  var o=id?APP.data.produk.find(function(x){return x.id===id;}):null;
  var kOpt=['Jersey Printing','Kolor Printing','Jaket Printing','Banner/Spanduk','Sticker','Sablon Manual','Celana Training','Lainnya'].map(function(k){return '<option'+(o&&o.kategori===k?' selected':'')+'>'+k+'</option>';}).join('');
  var body='<div class="space-y-4">'+mkField('Nama Produk','nama','text',o?o.nama:'')+mkSelect('Kategori','kategori',kOpt)+mkField('Harga','harga','number',o?o.harga:'','min="0"')+mkArea('Deskripsi','deskripsi',o?o.deskripsi:'')+'</div>';
  showModal(o?'Edit Produk':'Tambah Produk', body,
    '<button onclick="closeModal()" class="px-4 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 '+SCLR+' font-semibold text-sm hover:opacity-80">Batal</button>'+
    '<button onclick="simpanProduk(\''+(id||'')+'\')" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Simpan</button>');
}
function simpanProduk(id) {
  var f=FV(['nama','kategori','harga','deskripsi']);
  f.harga=parseInt(f.harga)||0;
  if(!f.nama||f.harga<=0) return toast('Lengkapi semua field wajib','error');
  if(id){ for(var i=0;i<APP.data.produk.length;i++){ if(APP.data.produk[i].id===id){ for(var k in f) APP.data.produk[i][k]=f[k]; break; } } toast('Produk diperbarui'); }
  else { f.id=gid(); APP.data.produk.push(f); toast('Produk ditambahkan'); }
  saveData(APP.data); closeModal(); go('produk');
}
function hapusProduk(id) {
  showConfirm('Yakin ingin menghapus produk ini?',function(){ APP.data.produk=APP.data.produk.filter(function(x){return x.id!==id;}); saveData(APP.data); go('produk'); toast('Produk dihapus'); });
}

/* ----- PEREKAPAN ----- */
function rRekap() {
  var d=APP.data;
  var totalPendapatan=0, totalDP=0, totalSisa=0, pesananAktif=0, pesananSelesai=0, pesananBatal=0;
  for(var i=0;i<d.pesanan.length;i++){
    var p=d.pesanan[i];
    if(p.status!=='Batal'){ totalPendapatan+=p.total; totalDP+=p.dp; totalSisa+=p.sisa; }
    if(p.status==='Selesai') pesananSelesai++;
    if(p.status==='Batal') pesananBatal++;
    if(p.status==='Baru'||p.status==='Proses'||p.status==='Siap Kirim') pesananAktif++;
  }
  var persenDP=totalPendapatan>0?Math.round((totalDP/totalPendapatan)*100):0;
  var persenSisa=totalPendapatan>0?Math.round((totalSisa/totalPendapatan)*100):0;
  return '<div class="fade-in space-y-6"><div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">'+
    statCard('fa-sack-dollar','Pendapatan',fRp(totalPendapatan),'Total (non-Batal)','bg-emerald-100 text-emerald-500')+
    statCard('fa-hand-holding-dollar','DP Masuk',fRp(totalDP),persenDP+'% dari total','bg-blue-100 text-blue-500')+
    statCard('fa-money-bill-transfer','Sisa Piutang',fRp(totalSisa),persenSisa+'% dari total','bg-amber-100 text-amber-500')+
    statCard('fa-circle-check','Selesai',pesananSelesai,'Pesanan selesai','bg-green-100 text-green-500')+
    '</div>'+
    '<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">'+
    '<div class="'+CBG+' rounded-2xl p-5 border shadow-sm"><h3 class="font-bold '+TCLR2+' mb-4">Ringkasan Status</h3><div class="space-y-3">'+
    rekapBar('Baru',d.pesanan.filter(function(p){return p.status==='Baru';}).length,d.pesanan.length,'#3b82f6')+
    rekapBar('Proses',d.pesanan.filter(function(p){return p.status==='Proses';}).length,d.pesanan.length,'#f59e0b')+
    rekapBar('Siap Kirim',d.pesanan.filter(function(p){return p.status==='Siap Kirim';}).length,d.pesanan.length,'#a855f7')+
    rekapBar('Selesai',pesananSelesai,d.pesanan.length,'#10b981')+
    rekapBar('Batal',pesananBatal,d.pesanan.length,'#ef4444')+
    '</div></div>'+
    '<div class="'+CBG+' rounded-2xl p-5 border shadow-sm"><h3 class="font-bold '+TCLR2+' mb-4">Piutang Pelanggan</h3>'+
    (d.pesanan.filter(function(p){return p.sisa>0&&p.status!=='Batal';}).length?
    '<div class="space-y-3">'+d.pesanan.filter(function(p){return p.sisa>0&&p.status!=='Batal';}).sort(function(a,b){return b.sisa-a.sisa;}).slice(0,8).map(function(p){
      var pl=d.pelanggan.find(function(x){return x.id===p.pelangganId;});
      return '<div class="flex items-center justify-between text-sm"><div><p class="font-medium '+TCLR2+'">'+(pl?pl.nama:'-')+'</p><p class="text-xs '+SCLR+'">'+p.noPO+'</p></div><p class="font-bold text-amber-500">'+fRp(p.sisa)+'</p></div>';
    }).join('')+'</div>':'<div class="text-center py-8"><i class="fas fa-check-circle text-3xl text-emerald-400 mb-2"></i><p class="text-sm '+SCLR+'">Semua piutang lunas</p></div>')+
    '</div></div></div>';
}
function rekapBar(label,count,total,color) {
  var pct=total>0?Math.round((count/total)*100):0;
  return '<div><div class="flex justify-between text-sm mb-1"><span class="font-medium '+TCLR2+'">'+label+'</span><span class="'+SCLR+'">'+count+' ('+pct+'%)</span></div><div class="h-2.5 bg-slate-100 dark:bg-slate-800 rounded-full overflow-hidden"><div class="h-full rounded-full transition-all" style="width:'+pct+'%;background:'+color+'"></div></div></div>';
}

/* ===== CETAK NOTA ===== */
function rNota() {
  var d = APP.data;
  var hasPesanan = d.pesanan.length > 0;

  /* Jika ada selectedNotaId dari goNota, langsung tampilkan preview */
  var selectedId = APP.selectedNotaId;
  APP.selectedNotaId = null;

  /* Opsi pesanan untuk dropdown */
  var pesOpts = '<option value="">-- Pilih Pesanan --</option>';
  for (var i = 0; i < d.pesanan.length; i++) {
    var o = d.pesanan[i];
    var pl = d.pelanggan.find(function(p){ return p.id === o.pelangganId; });
    pesOpts += '<option value="' + o.id + '"' + (selectedId === o.id ? ' selected' : '') + '>' + o.noPO + ' — ' + (pl ? pl.nama : '-') + ' (' + fRp(o.total) + ')</option>';
  }

  var ns = APP.notaSettings;

  return '<div class="fade-in space-y-6">' +

    /* Header dengan ikon */
    '<div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">' +
      '<div class="flex items-center gap-3">' +
        '<div class="w-12 h-12 rounded-2xl bg-gradient-to-tr from-emerald-400 to-teal-500 flex items-center justify-center text-white text-xl shadow-lg shadow-emerald-500/20"><i class="fas fa-receipt"></i></div>' +
        '<div><h3 class="text-xl font-bold ' + TCLR + '">Cetak Nota</h3><p class="text-sm ' + SCLR + '">Buat dan cetak nota pesanan pelanggan</p></div>' +
      '</div>' +
      '<div class="flex items-center gap-2">' +
        '<button onclick="openNotaSettings()" class="flex items-center gap-2 px-4 py-2 ' + IBG + ' ' + SCLR + ' rounded-xl text-sm font-semibold hover:opacity-80 light:border-slate-200 dark:border-slate-700 border"><i class="fas fa-gear"></i> Pengaturan</button>' +
        '<button onclick="printNota()" id="btnPrintNota" class="flex items-center gap-2 px-5 py-2 rounded-xl text-sm font-semibold text-white bg-gradient-to-r from-emerald-500 to-teal-500 shadow-lg shadow-emerald-500/20 hover:shadow-emerald-500/40 hover:-translate-y-0.5 transition-all' + (!selectedId ? ' opacity-50 pointer-events-none' : '') + '"><i class="fas fa-print"></i> Cetak Nota</button>' +
      '</div>' +
    '</div>' +

    /* Panel Pilih Pesanan + Pengaturan Cepat */
    '<div class="grid grid-cols-1 lg:grid-cols-3 gap-4">' +

      /* Kolom kiri: Pilih pesanan */
      '<div class="lg:col-span-1 space-y-4">' +
        '<div class="' + CBG + ' rounded-2xl border shadow-sm p-5">' +
          '<h4 class="font-bold ' + TCLR2 + ' text-sm mb-3"><i class="fas fa-clipboard-list mr-2 text-indigo-400"></i>Pilih Pesanan</h4>' +
          (hasPesanan ?
            '<select id="notaSelectPesanan" class="w-full ' + INP + ' mb-3" onchange="previewNota()">' + pesOpts + '</select>' +
            '<div id="notaQuickInfo" class="text-xs ' + SCLR + ' space-y-1">' +
              (selectedId ? getNotaQuickInfo(selectedId) : '<p class="text-center py-4 opacity-60">Pilih pesanan untuk melihat info</p>') +
            '</div>'
            : C('clipboard-list', 'Belum Ada Pesanan', 'Tambah pesanan terlebih dahulu')) +
        '</div>' +

        /* Tipe Nota */
        '<div class="' + CBG + ' rounded-2xl border shadow-sm p-5">' +
          '<h4 class="font-bold ' + TCLR2 + ' text-sm mb-3"><i class="fas fa-file-invoice mr-2 text-amber-400"></i>Tipe Nota</h4>' +
          '<div class="space-y-2">' +
            '<label class="flex items-center gap-3 px-4 py-2.5 rounded-xl cursor-pointer transition-all hover:bg-indigo-50 dark:hover:bg-slate-800 border-2 ' + (ns.notaType === 'invoice' ? 'border-indigo-400 bg-indigo-50 dark:bg-indigo-950/30' : 'border-transparent') + '">' +
              '<input type="radio" name="notaType" value="invoice" ' + (ns.notaType === 'invoice' ? 'checked' : '') + ' onchange="changeNotaType(this.value)" class="accent-indigo-500"><div><p class="font-semibold text-sm ' + TCLR2 + '">Invoice</p><p class="text-xs ' + SCLR + '">Nota lengkap dengan detail harga</p></div></label>' +
            '<label class="flex items-center gap-3 px-4 py-2.5 rounded-xl cursor-pointer transition-all hover:bg-indigo-50 dark:hover:bg-slate-800 border-2 ' + (ns.notaType === 'surat-jalan' ? 'border-indigo-400 bg-indigo-50 dark:bg-indigo-950/30' : 'border-transparent') + '">' +
              '<input type="radio" name="notaType" value="surat-jalan" ' + (ns.notaType === 'surat-jalan' ? 'checked' : '') + ' onchange="changeNotaType(this.value)" class="accent-indigo-500"><div><p class="font-semibold text-sm ' + TCLR2 + '">Surat Jalan</p><p class="text-xs ' + SCLR + '">Tanpa harga, untuk pengiriman</p></div></label>' +
            '<label class="flex items-center gap-3 px-4 py-2.5 rounded-xl cursor-pointer transition-all hover:bg-indigo-50 dark:hover:bg-slate-800 border-2 ' + (ns.notaType === 'kwitansi' ? 'border-indigo-400 bg-indigo-50 dark:bg-indigo-950/30' : 'border-transparent') + '">' +
              '<input type="radio" name="notaType" value="kwitansi" ' + (ns.notaType === 'kwitansi' ? 'checked' : '') + ' onchange="changeNotaType(this.value)" class="accent-indigo-500"><div><p class="font-semibold text-sm ' + TCLR2 + '">Kwitansi</p><p class="text-xs ' + SCLR + '">Bukti pembayaran</p></div></label>' +
          '</div>' +
        '</div>' +

        /* Opsi tambahan */
        '<div class="' + CBG + ' rounded-2xl border shadow-sm p-5">' +
          '<h4 class="font-bold ' + TCLR2 + ' text-sm mb-3"><i class="fas fa-sliders mr-2 text-purple-400"></i>Opsi</h4>' +
          '<div class="space-y-3">' +
            '<label class="flex items-center gap-3 cursor-pointer"><input type="checkbox" id="notaOptStamp" ' + (ns.showStamp ? 'checked' : '') + ' onchange="toggleNotaOption(\'showStamp\',this.checked)" class="accent-indigo-500 w-4 h-4"><span class="text-sm ' + TCLR2 + '">Tampilkan stampil status</span></label>' +
            '<label class="flex items-center gap-3 cursor-pointer"><input type="checkbox" id="notaOptQR" ' + (ns.showQR ? 'checked' : '') + ' onchange="toggleNotaOption(\'showQR\',this.checked)" class="accent-indigo-500 w-4 h-4"><span class="text-sm ' + TCLR2 + '">Tampilkan QR placeholder</span></label>' +
          '</div>' +
        '</div>' +
      '</div>' +

      /* Kolom kanan: Preview Nota */
      '<div class="lg:col-span-2">' +
        '<div class="' + CBG + ' rounded-2xl border shadow-sm overflow-hidden">' +
          '<div class="px-5 py-3 light:border-b dark:border-b dark:border-slate-800 border-slate-100 flex items-center justify-between">' +
            '<h4 class="font-bold ' + TCLR2 + ' text-sm"><i class="fas fa-eye mr-2 text-emerald-400"></i>Preview Nota</h4>' +
            '<span class="text-xs ' + SCLR + '">A4 — 210 x 297 mm</span>' +
          '</div>' +
          '<div class="p-4">' +
            '<div class="nota-preview-wrapper" id="notaPreviewWrapper">' +
              (selectedId ? buildNotaHTML(selectedId) : buildNotaEmpty()) +
            '</div>' +
          '</div>' +
        '</div>' +
      '</div>' +

    '</div></div>';
}

function getNotaQuickInfo(pesananId) {
  var o = APP.data.pesanan.find(function(x){ return x.id === pesananId; });
  if (!o) return '';
  var pl = APP.data.pelanggan.find(function(p){ return p.id === o.pelangganId; });
  var pr = APP.data.produk.find(function(p){ return p.id === o.produkId; });
  return '<div class="space-y-1.5">' +
    '<div class="flex justify-between"><span>No. PO:</span><span class="font-semibold ' + TCLR2 + '">' + o.noPO + '</span></div>' +
    '<div class="flex justify-between"><span>Pelanggan:</span><span class="font-semibold ' + TCLR2 + '">' + (pl ? pl.nama : '-') + '</span></div>' +
    '<div class="flex justify-between"><span>Produk:</span><span class="font-semibold ' + TCLR2 + '">' + (pr ? pr.nama : '-') + '</span></div>' +
    '<div class="flex justify-between"><span>Jumlah:</span><span class="font-semibold ' + TCLR2 + '">' + o.jumlah + ' pcs</span></div>' +
    '<div class="flex justify-between"><span>Total:</span><span class="font-bold text-indigo-500">' + fRp(o.total) + '</span></div>' +
    '<div class="flex justify-between"><span>Status:</span>' + sBadge(o.status) + '</div>' +
  '</div>';
}

function buildNotaEmpty() {
  return '<div class="nota-paper"><div class="empty-state" style="min-height:400px;"><i class="fas fa-receipt" style="font-size:48px;color:#cbd5e1;"></i><p class="title">Pilih Pesanan</p><p>Pilih pesanan dari daftar untuk melihat preview nota</p></div></div>';
}

function changeNotaType(val) {
  APP.notaSettings.notaType = val;
  saveNotaSettings();
  previewNota();
  /* Update visual radio */
  var labels = document.querySelectorAll('input[name="notaType"]');
  labels.forEach(function(inp) {
    var lbl = inp.closest('label');
    if (inp.value === val) {
      lbl.classList.add('border-indigo-400', 'bg-indigo-50');
      lbl.classList.remove('border-transparent');
    } else {
      lbl.classList.remove('border-indigo-400', 'bg-indigo-50');
      lbl.classList.add('border-transparent');
    }
  });
}

function toggleNotaOption(key, val) {
  APP.notaSettings[key] = val;
  saveNotaSettings();
  previewNota();
}

function previewNota() {
  var sel = document.getElementById('notaSelectPesanan');
  var id = sel ? sel.value : '';
  var wrapper = document.getElementById('notaPreviewWrapper');
  var btnPrint = document.getElementById('btnPrintNota');

  /* Update quick info */
  var qi = document.getElementById('notaQuickInfo');
  if (qi) {
    qi.innerHTML = id ? getNotaQuickInfo(id) : '<p class="text-center py-4 opacity-60">Pilih pesanan untuk melihat info</p>';
  }

  if (!id) {
    if (wrapper) wrapper.innerHTML = buildNotaEmpty();
    if (btnPrint) { btnPrint.classList.add('opacity-50', 'pointer-events-none'); }
    return;
  }

  if (btnPrint) { btnPrint.classList.remove('opacity-50', 'pointer-events-none'); }
  if (wrapper) wrapper.innerHTML = buildNotaHTML(id);
}

function buildNotaHTML(pesananId) {
  var d = APP.data;
  var o = d.pesanan.find(function(x){ return x.id === pesananId; });
  if (!o) return buildNotaEmpty();
  var pl = d.pelanggan.find(function(p){ return p.id === o.pelangganId; });
  var pr = d.produk.find(function(p){ return p.id === o.produkId; });
  var ns = APP.notaSettings;
  var type = ns.notaType;

  /* Judul berdasarkan tipe */
  var title = 'INVOICE';
  var subtitle = 'Nota Penjualan';
  if (type === 'surat-jalan') { title = 'SURAT JALAN'; subtitle = 'Delivery Order'; }
  if (type === 'kwitansi') { title = 'KWITANSI'; subtitle = 'Bukti Pembayaran'; }

  /* Stampil berdasarkan status */
  var stampHTML = '';
  if (ns.showStamp) {
    if (o.status === 'Selesai') {
      stampHTML = '<div class="nota-stamp paid">LUNAS</div>';
    } else if (o.status === 'Batal') {
      stampHTML = '<div class="nota-stamp cancelled">BATAL</div>';
    } else {
      stampHTML = '<div class="nota-stamp">' + o.status.toUpperCase() + '</div>';
    }
  }

  /* QR placeholder */
  var qrHTML = ns.showQR ? '<div class="nota-qr-placeholder"><i class="fas fa-qrcode text-2xl mb-1" style="color:#cbd5e1;"></i><span>QR Code</span></div>' : '';

  /* Watermark */
  var watermark = '';
  if (o.status === 'Batal') watermark = '<div class="nota-watermark">BATAL</div>';
  else if (o.status === 'Selesai') watermark = '<div class="nota-watermark">LUNAS</div>';

  /* Tabel item untuk invoice */
  var itemTableHTML = '';
  if (type === 'invoice') {
    itemTableHTML = '<table class="nota-table"><thead><tr><th style="width:30px;">No</th><th>Deskripsi</th><th style="width:60px;">Qty</th><th style="width:50px;">Ukuran</th><th style="width:60px;">Warna</th><th style="width:120px;" class="text-right">Harga Satuan</th><th style="width:130px;" class="text-right">Subtotal</th></tr></thead><tbody>' +
      '<tr><td>1</td><td><strong>' + (pr ? pr.nama : '-') + '</strong><br><span style="color:#94a3b8;font-size:11px;">' + (pr ? pr.kategori : '') + '</span></td><td>' + o.jumlah + '</td><td>' + (o.ukuran || '-') + '</td><td>' + (o.warna || '-') + '</td><td class="text-right">' + fRp(o.hargaSatuan) + '</td><td class="text-right"><strong>' + fRp(o.jumlah * o.hargaSatuan) + '</strong></td></tr>' +
      '</tbody></table>';
  } else if (type === 'surat-jalan') {
    itemTableHTML = '<table class="nota-table"><thead><tr><th style="width:30px;">No</th><th>Deskripsi</th><th style="width:60px;">Qty</th><th style="width:60px;">Ukuran</th><th style="width:60px;">Warna</th><th style="width:120px;">Keterangan</th></tr></thead><tbody>' +
      '<tr><td>1</td><td><strong>' + (pr ? pr.nama : '-') + '</strong></td><td>' + o.jumlah + '</td><td>' + (o.ukuran || '-') + '</td><td>' + (o.warna || '-') + '</td><td>-</td></tr>' +
      '</tbody></table>';
  } else if (type === 'kwitansi') {
    itemTableHTML = '<table class="nota-table"><thead><tr><th style="width:30px;">No</th><th>Deskripsi</th><th style="width:130px;" class="text-right">Jumlah</th></tr></thead><tbody>' +
      '<tr><td>1</td><td>Pembayaran DP untuk ' + (pr ? pr.nama : '-') + ' (' + o.noPO + ')</td><td class="text-right">' + fRp(o.dp) + '</td></tr>' +
      (o.sisa <= 0 ? '<tr><td>2</td><td>Pelunasan sisa pembayaran</td><td class="text-right">' + fRp(o.total - o.dp) + '</td></tr>' : '') +
      '</tbody></table>';
  }

  /* Total section untuk invoice */
  var totalSection = '';
  if (type === 'invoice') {
    totalSection = '<div style="display:flex;justify-content:flex-end;margin-top:16px;"><div style="width:280px;">' +
      '<div style="display:flex;justify-content:space-between;padding:6px 0;font-size:13px;color:#64748b;"><span>Subtotal</span><span>' + fRp(o.jumlah * o.hargaSatuan) + '</span></div>' +
      '<div style="display:flex;justify-content:space-between;padding:6px 0;font-size:13px;color:#64748b;"><span>DP Dibayar</span><span style="color:#10b981;">- ' + fRp(o.dp) + '</span></div>' +
      '<div style="height:1px;background:#e2e8f0;margin:8px 0;"></div>' +
      '<div style="display:flex;justify-content:space-between;padding:8px 0;font-size:16px;font-weight:800;color:#1e293b;"><span>Sisa Pembayaran</span><span style="color:' + (o.sisa > 0 ? '#ef4444' : '#10b981') + ';">' + fRp(o.sisa) + '</span></div>' +
      (o.sisa <= 0 ? '<div style="text-align:right;font-size:11px;color:#10b981;font-weight:700;margin-top:4px;">PEMBAYARAN LUNAS</div>' : '') +
      '</div></div>';
  } else if (type === 'kwitansi') {
    totalSection = '<div style="display:flex;justify-content:flex-end;margin-top:16px;"><div style="width:280px;">' +
      '<div style="height:1px;background:#e2e8f0;margin:8px 0;"></div>' +
      '<div style="display:flex;justify-content:space-between;padding:8px 0;font-size:16px;font-weight:800;color:#1e293b;"><span>Total Dibayar</span><span>' + fRp(o.dp) + '</span></div>' +
      (o.sisa > 0 ? '<div style="display:flex;justify-content:space-between;padding:4px 0;font-size:13px;color:#64748b;"><span>Sisa</span><span style="color:#ef4444;">' + fRp(o.sisa) + '</span></div>' : '<div style="text-align:right;font-size:11px;color:#10b981;font-weight:700;margin-top:4px;">LUNAS</div>') +
      '</div></div>';
  }

  /* Tanda tangan untuk kwitansi */
  var signatureSection = '';
  if (type === 'kwitansi') {
    signatureSection = '<div style="display:flex;justify-content:space-between;margin-top:32px;">' +
      '<div style="text-align:center;"><p style="font-size:12px;color:#64748b;margin-bottom:60px;">Penerima</p><p style="font-size:13px;font-weight:600;color:#1e293b;border-top:1px solid #e2e8f0;padding-top:8px;min-width:160px;">___________________</p></div>' +
      '<div style="text-align:center;"><p style="font-size:12px;color:#64748b;margin-bottom:60px;">Hormat Kami</p><p style="font-size:13px;font-weight:600;color:#1e293b;border-top:1px solid #e2e8f0;padding-top:8px;min-width:160px;">' + ns.namaToko + '</p></div>' +
      '</div>';
  }

  /* Footer nota */
  var footerSection = '';
  if (type !== 'kwitansi') {
    footerSection = '<div class="nota-footer">' +
      '<div style="font-size:11px;color:#94a3b8;max-width:400px;">' +
        '<p style="font-weight:600;color:#64748b;margin-bottom:4px;">Syarat & Ketentuan:</p>' +
        '<p>1. Pembayaran sisa dilakukan sebelum pengiriman</p>' +
        '<p>2. Komplain maksimal 1x24 jam setelah penerimaan</p>' +
        '<p>3. Barang yang sudah dicetak tidak dapat dikembalikan</p>' +
      '</div>' +
      (qrHTML ? '<div style="text-align:center;">' + qrHTML + '<p style="font-size:10px;color:#94a3b8;margin-top:4px;">' + o.noPO + '</p></div>' : '') +
    '</div>';
  } else {
    footerSection = signatureSection;
  }

  return '<div class="nota-paper" id="notaContent">' +
    watermark +
    '<div class="nota-header">' +
      '<div>' +
        '<div style="display:flex;align-items:center;gap:12px;margin-bottom:8px;">' +
          '<div style="width:44px;height:44px;border-radius:10px;background:linear-gradient(135deg,#4f46e5,#7c3aed);display:flex;align-items:center;justify-content:center;color:#fff;font-weight:900;font-size:18px;">E</div>' +
          '<div><p style="font-size:18px;font-weight:800;color:#1e293b;line-height:1.2;">' + ns.namaToko + '</p><p style="font-size:11px;color:#94a3b8;">Konveksi Jersey Olahraga Premium</p></div>' +
        '</div>' +
        '<div style="font-size:11px;color:#64748b;margin-left:56px;line-height:1.6;">' +
          '<p><i class="fas fa-location-dot" style="width:14px;color:#a5b4fc;"></i> ' + ns.alamatToko + '</p>' +
          '<p><i class="fas fa-phone" style="width:14px;color:#a5b4fc;"></i> ' + ns.telpToko + '</p>' +
          '<p><i class="fas fa-envelope" style="width:14px;color:#a5b4fc;"></i> ' + ns.emailToko + '</p>' +
        '</div>' +
      '</div>' +
      '<div style="text-align:right;position:relative;">' +
        '<p style="font-size:22px;font-weight:900;color:#4f46e5;letter-spacing:1px;">' + title + '</p>' +
        '<p style="font-size:11px;color:#94a3b8;margin-bottom:12px;">' + subtitle + '</p>' +
        '<div style="font-size:12px;color:#64748b;line-height:1.8;">' +
          '<p><span style="color:#94a3b8;">No:</span> <strong style="color:#1e293b;font-family:monospace;">' + o.noPO + '</strong></p>' +
          '<p><span style="color:#94a3b8;">Tanggal:</span> ' + fDtFull(o.tglOrder) + '</p>' +
          '<p><span style="color:#94a3b8;">Deadline:</span> ' + fDtFull(o.deadline) + '</p>' +
        '</div>' +
        stampHTML +
      '</div>' +
    '</div>' +

    '<div class="nota-body">' +
      /* Info pelanggan */
      '<div style="display:flex;gap:24px;margin-bottom:20px;">' +
        '<div style="flex:1;background:#f8fafc;border-radius:10px;padding:14px 16px;">' +
          '<p style="font-size:10px;font-weight:700;color:#94a3b8;text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;">Ditagihkan Kepada</p>' +
          '<p style="font-size:14px;font-weight:700;color:#1e293b;">' + (pl ? pl.nama : '-') + '</p>' +
          '<p style="font-size:12px;color:#64748b;line-height:1.6;margin-top:4px;">' +
            (pl ? '<i class="fas fa-building" style="width:14px;color:#a5b4fc;"></i> ' + pl.tim + '<br>' : '') +
            (pl ? '<i class="fas fa-phone" style="width:14px;color:#a5b4fc;"></i> ' + pl.telepon : '') +
            (pl && pl.alamat ? '<br><i class="fas fa-location-dot" style="width:14px;color:#a5b4fc;"></i> ' + pl.alamat : '') +
          '</p>' +
        '</div>' +
        (type === 'surat-jalan' ?
          '<div style="flex:1;background:#f8fafc;border-radius:10px;padding:14px 16px;">' +
            '<p style="font-size:10px;font-weight:700;color:#94a3b8;text-transform:uppercase;letter-spacing:1px;margin-bottom:6px;">Alamat Pengiriman</p>' +
            '<p style="font-size:13px;color:#1e293b;font-weight:600;">' + (pl ? pl.alamat : '-') + '</p>' +
            '<p style="font-size:11px;color:#64748b;margin-top:4px;">Kirim sebelum: ' + fDtFull(o.deadline) + '</p>' +
          '</div>' : '') +
      '</div>' +

      /* Tabel item */
      itemTableHTML +

      /* Total */
      totalSection +

      /* Catatan */
      (o.catatan ? '<div style="margin-top:16px;background:#fefce8;border-radius:8px;padding:10px 14px;border-left:3px solid #f59e0b;"><p style="font-size:10px;font-weight:700;color:#d97706;text-transform:uppercase;letter-spacing:0.5px;margin-bottom:2px;">Catatan</p><p style="font-size:12px;color:#92400e;">' + o.catatan + '</p></div>' : '') +

    '</div>' +

    footerSection +

    '<div style="padding:12px 40px;text-align:center;border-top:1px solid #f1f5f9;">' +
      '<p style="font-size:10px;color:#cbd5e1;">Dicetak pada ' + fDtFull(fToday()) + ' ' + fNow() + ' — ' + ns.namaToko + '</p>' +
    '</div>' +

  '</div>';
}

/* Cetak nota ke printer */
function printNota() {
  var sel = document.getElementById('notaSelectPesanan');
  var id = sel ? sel.value : '';
  if (!id) return toast('Pilih pesanan terlebih dahulu', 'warning');

  var printArea = document.getElementById('notaPrintArea');
  printArea.innerHTML = buildNotaHTML(id);
  printArea.style.display = 'block';

  setTimeout(function() {
    window.print();
    setTimeout(function() {
      printArea.style.display = 'none';
    }, 500);
  }, 200);
}

/* Pengaturan Nota */
function openNotaSettings() {
  var ns = APP.notaSettings;
  var body = '<div class="space-y-4">' +
    '<p class="text-sm ' + SCLR + '">Atur informasi toko yang akan tampil di nota</p>' +
    mkField('Nama Toko', 'nsNama', 'text', ns.namaToko) +
    mkField('Alamat', 'nsAlamat', 'text', ns.alamatToko) +
    mkField('Telepon', 'nsTelp', 'text', ns.telpToko) +
    mkField('Email', 'nsEmail', 'text', ns.emailToko) +
  '</div>';
  showModal('Pengaturan Nota', body,
    '<button onclick="closeModal()" class="px-4 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 ' + SCLR + ' font-semibold text-sm hover:opacity-80">Batal</button>' +
    '<button onclick="saveNotaSettingsForm()" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Simpan</button>');
}
function saveNotaSettingsForm() {
  var f = FV(['nsNama', 'nsAlamat', 'nsTelp', 'nsEmail']);
  APP.notaSettings.namaToko = f.nsNama;
  APP.notaSettings.alamatToko = f.nsAlamat;
  APP.notaSettings.telpToko = f.nsTelp;
  APP.notaSettings.emailToko = f.nsEmail;
  saveNotaSettings();
  closeModal();
  previewNota();
  toast('Pengaturan nota disimpan');
}

/* ----- LAPORAN ----- */
function rLaporan() {
  var d = APP.data;
  var totalRev = 0, totalDP = 0, totalSisa = 0;
  for (var i = 0; i < d.pesanan.length; i++) {
    if (d.pesanan[i].status !== 'Batal') { totalRev += d.pesanan[i].total; totalDP += d.pesanan[i].dp; totalSisa += d.pesanan[i].sisa; }
  }
  return '<div class="fade-in space-y-4"><div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3">' +
    '<h3 class="text-lg font-bold ' + TCLR + '">Laporan Penjualan</h3>' +
    '<div class="flex gap-2">' +
    '<button onclick="expLapPDF()" class="btn-export flex items-center gap-2 px-4 py-2 ' + IBG + ' ' + SCLR + ' rounded-xl text-sm font-semibold hover:opacity-80 light:border-slate-200 dark:border-slate-700 border"><i class="fas fa-file-pdf"></i> Export PDF</button>' +
    '<button onclick="expLapExcel()" class="btn-export flex items-center gap-2 px-4 py-2 ' + IBG + ' ' + SCLR + ' rounded-xl text-sm font-semibold hover:opacity-80 light:border-slate-200 dark:border-slate-700 border"><i class="fas fa-file-excel"></i> Export Excel</button>' +
    '</div></div>' +
    '<div class="grid grid-cols-1 sm:grid-cols-3 gap-4">' +
    statCard('fa-coins', 'Revenue', fRp(totalRev), 'Total Pendapatan', 'bg-emerald-100 text-emerald-500') +
    statCard('fa-hand-holding-dollar', 'DP', fRp(totalDP), 'DP Masuk', 'bg-blue-100 text-blue-500') +
    statCard('fa-money-bill-transfer', 'Piutang', fRp(totalSisa), 'Sisa Belum Dibayar', 'bg-amber-100 text-amber-500') +
    '</div>' +
    '<div class="' + CBG + ' rounded-2xl border shadow-sm overflow-hidden"><div class="overflow-x-auto">' +
    (d.pesanan.length ? buildLapTable(d.pesanan) : C('file-lines', 'Belum Ada Data', 'Tambahkan pesanan untuk melihat laporan')) +
    '</div></div></div>';
}
function buildLapTable(list) {
  var d = APP.data, th = 'text-left ' + SCLR + ' light:border-b dark:border-b dark:border-slate-800 border-slate-50 text-xs';
  return '<table class="w-full text-sm"><thead><tr class="' + th + '"><th class="px-4 py-3 font-semibold">No. PO</th><th class="px-4 py-3 font-semibold">Tanggal</th><th class="px-4 py-3 font-semibold">Pelanggan</th><th class="px-4 py-3 font-semibold">Produk</th><th class="px-4 py-3 font-semibold">Qty</th><th class="px-4 py-3 font-semibold">Total</th><th class="px-4 py-3 font-semibold">DP</th><th class="px-4 py-3 font-semibold">Sisa</th><th class="px-4 py-3 font-semibold">Status</th></tr></thead><tbody>' +
    list.map(function(o) {
      var pl = d.pelanggan.find(function(p) { return p.id === o.pelangganId; });
      var pr = d.produk.find(function(p) { return p.id === o.produkId; });
      return '<tr class="table-row light:border-b dark:border-b dark:border-slate-800 border-slate-50"><td class="px-4 py-3 font-mono font-semibold text-xs ' + TCLR2 + '">' + o.noPO + '</td><td class="px-4 py-3 ' + SCLR + '">' + fDt(o.tglOrder) + '</td><td class="px-4 py-3 ' + TCLR2 + '">' + (pl ? pl.nama : '-') + '</td><td class="px-4 py-3 ' + SCLR + '">' + (pr ? pr.nama : '-') + '</td><td class="px-4 py-3">' + o.jumlah + '</td><td class="px-4 py-3 font-semibold ' + TCLR + '">' + fRp(o.total) + '</td><td class="px-4 py-3 text-emerald-500 font-semibold">' + fRp(o.dp) + '</td><td class="px-4 py-3 font-semibold ' + (o.sisa > 0 ? 'text-amber-500' : 'text-emerald-500') + '">' + fRp(o.sisa) + '</td><td class="px-4 py-3">' + sBadge(o.status) + '</td></tr>';
    }).join('') +
    '</tbody></table>';
}

/* ===== EXPORT FUNCTIONS ===== */
function expPesPDF() {
  var d = APP.data;
  var jsPDF = window.jspdf.jsPDF;
  var doc = new jsPDF();
  doc.setFontSize(16); doc.text('Eco Sport Jersey - Daftar Pesanan', 14, 20);
  doc.setFontSize(10); doc.setTextColor(100); doc.text('Dicetak: ' + fDtFull(fToday()), 14, 28);
  var rows = d.pesanan.map(function(o) {
    var pl = d.pelanggan.find(function(p) { return p.id === o.pelangganId; });
    var pr = d.produk.find(function(p) { return p.id === o.produkId; });
    return [o.noPO, fDt(o.tglOrder), pl ? pl.nama : '-', pr ? pr.nama : '', o.jumlah, fRp(o.total), o.status];
  });
  doc.autoTable({ head: [['No PO', 'Tanggal', 'Pelanggan', 'Produk', 'Qty', 'Total', 'Status']], body: rows, startY: 34, styles: { fontSize: 9 }, headStyles: { fillColor: [79, 70, 229] } });
  doc.save('pesanan-ecosport.pdf');
  toast('PDF berhasil diunduh');
}
function expPesExcel() {
  var d = APP.data;
  var rows = d.pesanan.map(function(o) {
    var pl = d.pelanggan.find(function(p) { return p.id === o.pelangganId; });
    var pr = d.produk.find(function(p) { return p.id === o.produkId; });
    return { 'No PO': o.noPO, 'Tanggal': o.tglOrder, 'Pelanggan': pl ? pl.nama : '', 'Produk': pr ? pr.nama : '', 'Jumlah': o.jumlah, 'Total': o.total, 'DP': o.dp, 'Sisa': o.sisa, 'Status': o.status };
  });
  var ws = XLSX.utils.json_to_sheet(rows);
  var wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, 'Pesanan');
  XLSX.writeFile(wb, 'pesanan-ecosport.xlsx');
  toast('Excel berhasil diunduh');
}
function expLapPDF() {
  var d = APP.data;
  var jsPDF = window.jspdf.jsPDF;
  var doc = new jsPDF();
  doc.setFontSize(16); doc.text('Eco Sport Jersey - Laporan Penjualan', 14, 20);
  doc.setFontSize(10); doc.setTextColor(100); doc.text('Dicetak: ' + fDtFull(fToday()), 14, 28);
  var rows = d.pesanan.map(function(o) {
    var pl = d.pelanggan.find(function(p) { return p.id === o.pelangganId; });
    return [o.noPO, fDt(o.tglOrder), pl ? pl.nama : '-', o.jumlah, fRp(o.total), fRp(o.dp), fRp(o.sisa), o.status];
  });
  doc.autoTable({ head: [['No PO', 'Tanggal', 'Pelanggan', 'Qty', 'Total', 'DP', 'Sisa', 'Status']], body: rows, startY: 34, styles: { fontSize: 9 }, headStyles: { fillColor: [79, 70, 229] } });
  doc.save('laporan-ecosport.pdf');
  toast('Laporan PDF berhasil diunduh');
}
function expLapExcel() {
  var d = APP.data;
  var rows = d.pesanan.map(function(o) {
    var pl = d.pelanggan.find(function(p) { return p.id === o.pelangganId; });
    return { 'No PO': o.noPO, 'Tanggal': o.tglOrder, 'Pelanggan': pl ? pl.nama : '', 'Jumlah': o.jumlah, 'Total': o.total, 'DP': o.dp, 'Sisa': o.sisa, 'Status': o.status };
  });
  var ws = XLSX.utils.json_to_sheet(rows);
  var wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, 'Laporan');
  XLSX.writeFile(wb, 'laporan-ecosport.xlsx');
  toast('Laporan Excel berhasil diunduh');
}

/* ===== INIT ===== */
function initApp() {
  loadTheme();
  loadNotaSettings();
  APP.data = loadData();

  /* Tanggal di header */
  document.getElementById('currentDate').textContent = new Date().toLocaleDateString('id-ID', { weekday: 'long', day: 'numeric', month: 'long', year: 'numeric' });

  /* Cek login */
  if (localStorage.getItem('ecoSportLogin') === 'true') {
    APP.isLoggedIn = true;
    document.getElementById('appContainer').classList.remove('hidden');
    go('dashboard');
  } else {
    document.getElementById('loginScreen').classList.remove('hidden');
  }

  /* Demo data jika kosong */
  if (APP.data.pelanggan.length === 0 && APP.data.produk.length === 0) {
    seedDemoData();
  }
}

function seedDemoData() {
  var d = APP.data;

  d.pelanggan = [
    { id: gid(), nama: 'Budi Santoso', tim: 'FC Garuda', telepon: '081234567890', email: 'budi@garuda.id', alamat: 'Jl. Merdeka No. 10, Jakarta' },
    { id: gid(), nama: 'Rina Wati', tim: 'Srikandi VC', telepon: '082345678901', email: 'rina@srikandi.id', alamat: 'Jl. Sudirman No. 55, Bandung' },
    { id: gid(), nama: 'Ahmad Fauzi', tim: 'Muda FC', telepon: '083456789012', email: 'fauzi@ Mudafc.id', alamat: 'Jl. Asia Afrika No. 88, Surabaya' },
  ];

  d.produk = [
    { id: gid(), nama: 'Jersey Futsal Pro', kategori: 'Futsal', harga: 150000, deskripsi: 'Jersey futsal premium dry-fit bahan ringan' },
    { id: gid(), nama: 'Jersey Bola Elite', kategori: 'Sepak Bola', harga: 185000, deskripsi: 'Jersey sepak bola elite dengan teknologi cool-dry' },
    { id: gid(), nama: 'Jersey Basket Max', kategori: 'Basket', harga: 175000, deskripsi: 'Jersey basket breathable mesh panel' },
  ];

  var p1 = d.pelanggan[0], p2 = d.pelanggan[1], p3 = d.pelanggan[2];
  var pr1 = d.produk[0], pr2 = d.produk[1], pr3 = d.produk[2];

  d.pesanan = [
    { id: gid(), noPO: 'PO-2025-001', pelangganId: p1.id, produkId: pr1.id, jumlah: 15, ukuran: 'S:3, M:5, L:4, XL:3', warna: 'Merah', hargaSatuan: 150000, total: 2250000, dp: 1000000, sisa: 1250000, status: 'Proses', tglOrder: '2025-06-01', deadline: '2025-07-01', catatan: 'Logo di dada depan, nama belakang' },
    { id: gid(), noPO: 'PO-2025-002', pelangganId: p2.id, produkId: pr2.id, jumlah: 12, ukuran: 'S:2, M:4, L:4, XL:2', warna: 'Biru Navy', hargaSatuan: 185000, total: 2220000, dp: 2220000, sisa: 0, status: 'Selesai', tglOrder: '2025-05-15', deadline: '2025-06-15', catatan: '' },
    { id: gid(), noPO: 'PO-2025-003', pelangganId: p3.id, produkId: pr3.id, jumlah: 10, ukuran: 'M:5, L:5', warna: 'Hijau', hargaSatuan: 175000, total: 1750000, dp: 500000, sisa: 1250000, status: 'Baru', tglOrder: '2025-06-10', deadline: '2025-07-20', catatan: 'Desain custom tim' },
  ];

  saveData(d);
}

initApp();
</script>
<!-- ===== SUPER ADMIN + ROLE-BASED ACCESS CONTROL ===== -->
<!-- Sisipkan blok ini sebelum </body>, jangan ubah kode di atasnya -->
<script>
(function(){

  /* ==========================================================
     KONFIGURASI AKSES — Ubah array di bawah sesuai kebutuhan.
     
     Halaman yang tersedia:
       'dashboard'  → Dashboard
       'pesanan'    → Manajemen Pesanan
       'pelanggan'  → Data Pelanggan
       'produk'     → Katalog Produk
       'perekapan'  → Perekapan Keuangan
       'nota'       → Cetak Nota
       'laporan'    → Laporan & Export
       'kelolaAdmin'→ Kelola Akun Admin (khusus superadmin)
     ========================================================== */
  var PERMISSIONS = {
    'superadmin': [
      'dashboard','pesanan','pelanggan','produk',
      'perekapan','nota','laporan','kelolaAdmin'
    ],
    'admin': [
      'dashboard','pesanan','nota','pelanggan','produk'
    ]
  };
  /* ========================================================== */

  TITLES['kelolaAdmin'] = 'Kelola Admin';
  APP.userRole = null;

  var _login  = window.login;
  var _go     = window.go;
  var _render = window.render;
  var _logout = window.logout;
  var _goNota = window.goNota;

  /* ===== DATA AKUN ADMIN ===== */
  function loadAdmins(){
    try{ var s=localStorage.getItem('ecoAdminUsers'); if(s) return JSON.parse(s); }catch(e){}
    var def=[
      {id:'sa_root',username:'superadmin',password:'super123',nama:'Super Admin',role:'superadmin',active:true,createdAt:new Date().toISOString()},
      {id:'sa_def', username:'admin',     password:'admin123', nama:'Admin',       role:'admin',     active:true,createdAt:new Date().toISOString()}
    ];
    localStorage.setItem('ecoAdminUsers',JSON.stringify(def));
    return def;
  }
  function saveAdmins(l){ localStorage.setItem('ecoAdminUsers',JSON.stringify(l)); }

  /* ===== CEK AKSES ===== */
  function canAccess(page){
    var p=PERMISSIONS[APP.userRole]||[];
    return p.indexOf(page)>=0;
  }
  function firstAllowed(){
    var p=PERMISSIONS[APP.userRole]||['dashboard'];
    return p[0];
  }

  /* ===== OVERRIDE LOGIN ===== */
  window.login = function(){
    var u=document.getElementById('loginUser').value.trim();
    var p=document.getElementById('loginPass').value;
    var admins=loadAdmins(), found=null;
    for(var i=0;i<admins.length;i++){
      if(admins[i].username===u && admins[i].password===p && admins[i].active){ found=admins[i]; break; }
    }
    if(found){
      APP.userRole=found.role;
      APP.isLoggedIn=true;
      localStorage.setItem('ecoSportLogin','true');
      localStorage.setItem('ecoUserRole',found.role);
      localStorage.setItem('ecoUserName',found.nama);
      document.getElementById('loginScreen').classList.add('hidden');
      document.getElementById('appContainer').classList.remove('hidden');
      applyRoleUI();
      _go('dashboard');
      var cr=found.role==='superadmin'?' <i class="fas fa-crown text-amber-400"></i>':'';
      toast('Selamat datang, '+found.nama+'!'+cr,'success');
      return;
    }
    _login();
  };

  /* ===== OVERRIDE LOGOUT ===== */
  window.logout = function(){
    _logout();
    APP.userRole=null;
    localStorage.removeItem('ecoUserRole');
    localStorage.removeItem('ecoUserName');
  };

  /* ===== OVERRIDE GO — Pembatasan akses halaman ===== */
  window.go = function(page){
    if(!canAccess(page)){
      toast('Anda tidak memiliki akses ke halaman tersebut','error');
      _go(firstAllowed());
      return;
    }
    _go(page);
  };

  /* ===== OVERRIDE GO NOTA — Blokir dari tombol di tabel ===== */
  window.goNota = function(id){
    if(!canAccess('nota')){
      toast('Fitur cetak nota hanya untuk Super Admin','warning');
      return;
    }
    _goNota(id);
  };

  /* ===== OVERRIDE RENDER ===== */
  window.render = function(){
    if(APP.page==='kelolaAdmin' && APP.userRole==='superadmin'){
      var c=document.getElementById('content');
      c.innerHTML=rKelolaAdmin();
      c.scrollTop=0;
      return;
    }
    _render();
    postProcessRestrictions();
  };

  /* ===== Sembunyikan tombol aksi yang tidak diizinkan ===== */
  function postProcessRestrictions(){
    // Sembunyikan tombol "Cetak Nota" di tabel pesanan
    if(!canAccess('nota')){
      var nb=document.querySelectorAll('button[title="Cetak Nota"]');
      for(var i=0;i<nb.length;i++) nb[i].style.display='none';
    }
    // Sembunyikan tombol export PDF/Excel di pesanan
    if(!canAccess('laporan')){
      var eb=document.querySelectorAll('.btn-export');
      for(var j=0;j<eb.length;j++) eb[j].style.display='none';
    }
  }

  /* ===== APPLY UI BERDASARKAN ROLE ===== */
  function applyRoleUI(){
    var perms=PERMISSIONS[APP.userRole]||[];

    // Tampilkan/sembunyikan nav item sesuai permission
    var navItems=document.querySelectorAll('#sidebar nav .nav-item');
    for(var i=0;i<navItems.length;i++){
      var pg=navItems[i].getAttribute('data-page');
      if(pg && perms.indexOf(pg)<0){
        navItems[i].style.display='none';
      } else {
        navItems[i].style.display='';
      }
    }

    // Inject nav Kelola Admin hanya untuk superadmin
    if(APP.userRole==='superadmin') injectSaNav();
    else stripSuperUI();

    // Sembunyikan tombol Reset Data untuk non-superadmin
    var rb=document.querySelectorAll('button[title="Reset Data"]');
    for(var j=0;j<rb.length;j++) rb[j].style.display=APP.userRole==='superadmin'?'':'none';

    // Update info user di sidebar bawah
    refreshSidebarUser();
  }

  function injectSaNav(){
    if(document.getElementById('navSaAdmin')) return;
    var nav=document.querySelector('#sidebar nav');
    if(!nav) return;
    var sep=document.createElement('div');
    sep.id='saSep';
    sep.style.cssText='margin:8px 12px;border-top:1px solid '+(APP.theme==='dark'?'#1e293b':'#e2e8f0');
    nav.appendChild(sep);
    var a=document.createElement('a');
    a.id='navSaAdmin';
    a.setAttribute('data-page','kelolaAdmin');
    a.className='nav-item flex items-center gap-3 px-4 py-2.5 rounded-lg cursor-pointer text-sm';
    a.innerHTML='<i class="fas fa-user-shield w-5 text-center"></i> Kelola Admin';
    a.onclick=function(){ window.go('kelolaAdmin'); };
    nav.appendChild(a);
  }

  function stripSuperUI(){
    var n=document.getElementById('navSaAdmin'); if(n) n.remove();
    var s=document.getElementById('saSep');      if(s) s.remove();
  }

  function refreshSidebarUser(){
    var box=document.querySelector('#sidebar > div:last-child');
    if(!box) return;
    var isSA=APP.userRole==='superadmin';
    var name=localStorage.getItem('ecoUserName')||(isSA?'Super Admin':'Admin');
    var init=name.charAt(0).toUpperCase();
    var grad=isSA?'from-amber-400 to-orange-500':'from-indigo-400 to-purple-400';
    var roleTxt=isSA?'Super Administrator':'Admin';
    var roleClr=isSA?'color:#f59e0b;font-weight:700':'color:'+(APP.theme==='dark'?'#64748b':'#94a3b8');
    box.innerHTML='<div class="flex items-center gap-3">'
      +'<div class="w-9 h-9 rounded-full bg-gradient-to-tr '+grad+' flex items-center justify-center text-white font-bold text-sm">'
        +(isSA?'<i class="fas fa-crown text-xs"></i>':init)
      +'</div>'
      +'<div><p class="light:text-slate-700 dark:text-slate-200 text-sm font-medium">'+name+'</p>'
      +'<p class="text-xs" style="'+roleClr+'">'+(isSA?'<i class="fas fa-crown" style="font-size:8px;margin-right:3px"></i>':'')+roleTxt+'</p></div></div>';
  }

  /* ===== PATCH HINT LOGIN ===== */
  function patchLoginHint(){
    var el=document.querySelector('#loginScreen p.text-center');
    if(el && !el.dataset.saPatched){
      el.dataset.saPatched='1';
      el.innerHTML='Demo: <b>admin</b>/<b>admin123</b> &nbsp;<span style="opacity:.35">|</span>&nbsp; <b>superadmin</b>/<b>super123</b>';
    }
  }

  /* ================================================
     HALAMAN KELOLA ADMIN
     ================================================ */
  function rKelolaAdmin(){
    var admins=loadAdmins();
    var cards=admins.map(mkAdminCard).join('');
    var permInfo=buildPermSummary();
    return '<div class="fade-in space-y-5">'
      +'<div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-3">'
        +'<div>'
          +'<h3 class="font-bold '+TCLR2+' text-base">Daftar Pengguna</h3>'
          +'<p class="text-sm '+SCLR+' mt-0.5">Kelola akun yang dapat mengakses panel ini.</p>'
        +'</div>'
        +'<button onclick="saFormAdmin()" class="btn-primary px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2"><i class="fas fa-user-plus"></i> Tambah Admin</button>'
      +'</div>'
      /* Info ringkasan akses */
      +'<div class="'+CBG+' rounded-2xl border shadow-sm p-5">'
        +'<h4 class="font-bold '+TCLR2+' text-sm mb-3"><i class="fas fa-shield-halved '+ICLR+' mr-2"></i>Ringkasan Akses per Role</h4>'
        +permInfo
      +'</div>'
      +'<div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4" id="saAdminGrid">'
        +(cards||C('users','Tidak Ada Data','Belum ada akun admin terdaftar'))
      +'</div></div>';
  }

  function buildPermSummary(){
    var roles=['superadmin','admin'];
    var labels={superadmin:'Super Admin',admin:'Admin'};
    var colors={superadmin:'#f59e0b',admin:'#6366f1'};
    var allPages=['dashboard','pesanan','pelanggan','produk','perekapan','nota','laporan','kelolaAdmin'];
    var html='<div class="overflow-x-auto"><table class="w-full text-sm"><thead><tr class="text-left '+SCLR+' text-xs"><th class="pb-2 pr-4 font-semibold">Halaman</th>';
    for(var r=0;r<roles.length;r++) html+='<th class="pb-2 pr-4 font-semibold"><span style="color:'+colors[roles[r]]+'">'+labels[roles[r]]+'</span></th>';
    html+='</tr></thead><tbody>';
    for(var i=0;i<allPages.length;i++){
      var pg=allPages[i];
      html+='<tr class="light:border-t dark:border-t dark:border-slate-800 border-slate-50">';
      html+='<td class="py-2 pr-4 font-medium '+TCLR2+'">'+(TITLES[pg]||pg)+'</td>';
      for(var r2=0;r2<roles.length;r2++){
        var has=PERMISSIONS[roles[r2]].indexOf(pg)>=0;
        html+='<td class="py-2 pr-4">'+(has?'<i class="fas fa-check-circle text-emerald-500"></i> <span class="text-xs text-emerald-600 dark:text-emerald-400">Ya</span>':'<i class="fas fa-times-circle text-red-300 dark:text-red-800"></i> <span class="text-xs '+SCLR+'">Tidak</span>')+'</td>';
      }
      html+='</tr>';
    }
    html+='</tbody></table></div>';
    return html;
  }

  function mkAdminCard(a){
    var isSA=a.role==='superadmin';
    var grad=isSA?'from-amber-400 to-orange-500':'from-indigo-400 to-purple-400';
    var sClr=a.active?'background:#d1fae5;color:#059669':'background:#fee2e2;color:#dc2626';
    var sTxt=a.active?'Aktif':'Nonaktif';
    var rClr=isSA?'background:#fef3c7;color:#b45309':(APP.theme==='dark'?'background:#1e293b;color:#94a3b8':'background:#f1f5f9;color:#64748b');
    var rTxt=isSA?'Super Admin':'Admin';
    var bgC=APP.theme==='dark'?'background:#0f172a;border-color:#1e293b':'background:#ffffff;border-color:#e2e8f0';
    var txC=APP.theme==='dark'?'#e2e8f0':'#1e293b';
    var txC2=APP.theme==='dark'?'#94a3b8':'#64748b';

    var acts='';
    if(!isSA){
      acts='<button onclick="saFormAdmin(\''+a.id+'\')" class="w-8 h-8 rounded-lg '+(APP.theme==='dark'?'hover:!bg-slate-800':'hover:!bg-indigo-50')+' '+(APP.theme==='dark'?'text-slate-500':'text-indigo-400')+' hover:opacity-70 flex items-center justify-center" title="Edit"><i class="fas fa-pen text-xs"></i></button>'
        +'<button onclick="saHapusAdmin(\''+a.id+'\')" class="w-8 h-8 rounded-lg '+(APP.theme==='dark'?'hover:!bg-red-950/30':'hover:!bg-red-50')+' text-slate-300 dark:text-slate-600 hover:!text-red-400 flex items-center justify-center" title="Hapus"><i class="fas fa-trash text-xs"></i></button>'
        +'<button onclick="saToggleAdmin(\''+a.id+'\')" class="w-8 h-8 rounded-lg '+(APP.theme==='dark'?'hover:!bg-amber-950/30':'hover:!bg-amber-50')+' text-slate-300 dark:text-slate-600 hover:!text-amber-400 flex items-center justify-center" title="'+(a.active?'Nonaktifkan':'Aktifkan')+'"><i class="fas fa-'+(a.active?'ban':'check')+' text-xs"></i></button>';
    }

    return '<div style="'+bgC+'" class="rounded-2xl border shadow-sm p-5 hover:shadow-md transition-shadow relative overflow-hidden">'
      +(isSA?'<div class="absolute top-0 right-0 w-24 h-24" style="background:radial-gradient(circle at top right,rgba(245,158,11,.08),transparent 70%)"></div>':'')
      +'<div class="flex items-start justify-between mb-4">'
        +'<div class="flex items-center gap-3">'
          +'<div class="w-12 h-12 rounded-xl bg-gradient-to-tr '+grad+' flex items-center justify-center text-white font-bold text-lg shadow-md">'
            +(isSA?'<i class="fas fa-crown text-sm"></i>':a.nama.charAt(0).toUpperCase())
          +'</div>'
          +'<div><p class="font-bold" style="color:'+txC+'">'+a.nama+'</p><p class="text-xs font-mono" style="color:'+txC2+'">@'+a.username+'</p></div>'
        +'</div>'
        +'<div class="flex gap-1">'+acts+'</div>'
      +'</div>'
      +'<div class="flex items-center gap-2 flex-wrap">'
        +'<span class="badge" style="'+rClr+'">'+(isSA?'<i class="fas fa-crown" style="font-size:8px;margin-right:3px"></i>':'')+rTxt+'</span>'
        +'<span class="badge" style="'+sClr+'">'+sTxt+'</span>'
      +'</div>'
      +(a.createdAt?'<p class="text-xs mt-3" style="color:'+txC2+'"><i class="fas fa-calendar-plus" style="margin-right:4px"></i>Dibuat: '+fDt(a.createdAt)+'</p>':'')
    +'</div>';
  }

  /* ===== CRUD ADMIN ===== */
  window.saFormAdmin = function(id){
    var admins=loadAdmins();
    var o=id?admins.find(function(x){return x.id===id;}):null;
    var body='<div class="space-y-4">'
      +mkField('Nama Lengkap','sa_nama','text',o?o.nama:'')
      +'<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">Username</label>'
        +'<input id="sa_username" type="text" value="'+(o?o.username:'')+'" class="w-full '+INP+'" placeholder="huruf kecil, tanpa spasi" oninput="this.value=this.value.toLowerCase().replace(/[^a-z0-9_]/g,\'\')"></div>'
      +'<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">Password'+(o?' <span class="font-normal '+SCLR+'">(kosongkan jika tidak diubah)</span>':'')+'</label>'
        +'<div class="relative"><input id="sa_pass" type="password" class="w-full '+INP+' pr-10" placeholder="'+(o?'••••••••':'Minimal 4 karakter')+'">'
        +'<button type="button" onclick="var el=document.getElementById(\'sa_pass\');el.type=el.type===\'password\'?\'text\':\'password\'" class="absolute right-3 top-1/2 -translate-y-1/2 '+SCLR+' hover:opacity-70"><i class="fas fa-eye text-xs"></i></button></div></div>'
      +'<div><label class="block text-sm font-semibold light:text-slate-700 dark:text-slate-300 mb-1">Konfirmasi Password</label>'
        +'<input id="sa_pass2" type="password" class="w-full '+INP+'" placeholder="Ulangi password"></div>'
    +'</div>';
    showModal(o?'Edit Admin':'Tambah Admin Baru',body,
      '<button onclick="closeModal()" class="px-4 py-2 rounded-xl light:bg-slate-100 dark:bg-slate-800 '+SCLR+' font-semibold text-sm hover:opacity-80">Batal</button>'
      +'<button onclick="saSimpanAdmin(\''+(id||'')+'\')" class="btn-primary px-5 py-2 rounded-xl text-sm font-semibold">Simpan</button>');
  };

  window.saSimpanAdmin = function(id){
    var nama=document.getElementById('sa_nama').value.trim();
    var username=document.getElementById('sa_username').value.trim();
    var pw=document.getElementById('sa_pass').value;
    var pw2=document.getElementById('sa_pass2').value;
    if(!nama) return toast('Nama wajib diisi','error');
    if(!username) return toast('Username wajib diisi','error');
    if(username.length<3) return toast('Username minimal 3 karakter','error');
    var admins=loadAdmins();
    for(var i=0;i<admins.length;i++){
      if(admins[i].username===username && admins[i].id!==id) return toast('Username sudah digunakan','error');
    }
    if(id){
      for(var j=0;j<admins.length;j++){
        if(admins[j].id===id){
          admins[j].nama=nama; admins[j].username=username;
          if(pw){
            if(pw!==pw2) return toast('Konfirmasi password tidak cocok','error');
            if(pw.length<4) return toast('Password minimal 4 karakter','error');
            admins[j].password=pw;
          }
          break;
        }
      }
      toast('Data admin diperbarui');
    } else {
      if(!pw) return toast('Password wajib diisi untuk akun baru','error');
      if(pw!==pw2) return toast('Konfirmasi password tidak cocok','error');
      if(pw.length<4) return toast('Password minimal 4 karakter','error');
      admins.push({id:gid(),username:username,password:pw,nama:nama,role:'admin',active:true,createdAt:new Date().toISOString()});
      toast('Admin baru berhasil ditambahkan');
    }
    saveAdmins(admins); closeModal(); window.go('kelolaAdmin');
  };

  window.saHapusAdmin = function(id){
    showConfirm('Yakin ingin menghapus akun admin ini secara permanen?',function(){
      saveAdmins(loadAdmins().filter(function(a){return a.id!==id;}));
      window.go('kelolaAdmin'); toast('Akun admin dihapus');
    });
  };

  window.saToggleAdmin = function(id){
    var admins=loadAdmins();
    for(var i=0;i<admins.length;i++){
      if(admins[i].id===id){
        admins[i].active=!admins[i].active;
        toast('Akun '+(admins[i].active?'diaktifkan':'dinonaktifkan'),admins[i].active?'success':'warning');
        break;
      }
    }
    saveAdmins(admins); window.go('kelolaAdmin');
  };

  /* ===== INIT ===== */
  function init(){
    loadAdmins();
    patchLoginHint();
    if(localStorage.getItem('ecoSportLogin')){
      APP.userRole=localStorage.getItem('ecoUserRole')||'admin';
      if(!canAccess(APP.page)) APP.page=firstAllowed();
      setTimeout(function(){ applyRoleUI(); },50);
    }
  }

  if(document.readyState==='loading') document.addEventListener('DOMContentLoaded',init);
  else init();

})();
</script></body></body>
</html>
