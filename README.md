<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoLoop Pro - Real-time Waste Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { background-color: #0b0f19; font-family: 'Segoe UI', sans-serif; color: #f3f4f6; }
        .glass { background: rgba(17, 24, 39, 0.75); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.07); }
        .nav-active { color: #10b981; background: rgba(16, 185, 129, 0.12); border-radius: 16px; }
        .page-tab { display: none; }
        .page-tab.active { display: block; animation: fadeIn 0.4s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="pb-24">

    <!-- USER REGISTER PORTAL -->
    <div id="auth-gate" class="fixed inset-0 bg-slate-950 z-[100] flex items-center justify-center p-4">
        <div class="glass w-full max-w-md p-6 rounded-3xl border border-gray-800 space-y-4">
            <div class="text-center space-y-2">
                <img src="https://i.ibb.co/VWVgL0gq/image-7.png" class="w-16 h-16 mx-auto rounded-2xl shadow-lg border border-emerald-500/30" alt="EcoLoop Logo">
                <h2 class="text-lg font-black text-white">EcoLoop Pro ජාලය</h2>
                <p class="text-xs text-gray-400">ගිණුමක් සාදා ඇප් එක සක්‍රීය කරගන්න.</p>
            </div>
            <div class="space-y-3 text-xs">
                <input type="text" id="auth-name" placeholder="ඔබේ නම" class="w-full p-3.5 bg-gray-900 border border-gray-800 rounded-xl text-white outline-none focus:border-emerald-500">
                <input type="tel" id="auth-phone" placeholder="දුරකථන අංකය" class="w-full p-3.5 bg-gray-900 border border-gray-800 rounded-xl text-white font-mono outline-none focus:border-emerald-500">
                <select id="auth-role" class="w-full p-3.5 bg-gray-900 border border-gray-800 rounded-xl text-white outline-none font-bold">
                    <option value="Household User">කුණු විකුණන කෙනෙක්</option>
                    <option value="Collector/Vendor">මිලදී ගන්නා මුදලාලි</option>
                </select>
                <button onclick="saveUserSession()" class="w-full bg-gradient-to-r from-emerald-500 to-cyan-500 text-slate-950 font-black py-4 rounded-xl uppercase tracking-wider">ඇප් එකට ඇතුල් වන්න 🚀</button>
            </div>
        </div>
    </div>

    <!-- MAIN SYSTEM HEADER -->
    <header class="glass sticky top-0 z-40 px-4 py-3 flex justify-between items-center border-b border-gray-800/80">
        <div class="flex items-center gap-2">
            <img src="https://i.ibb.co/VWVgL0gq/image-7.png" class="w-7 h-7 rounded-lg object-cover border border-emerald-500/20" alt="Logo">
            <h1 class="text-xs font-black tracking-widest text-white">ECOLOOP PRO</h1>
        </div>
        <div class="flex items-center gap-2">
            <div id="profile-badge" class="bg-gray-950 px-3 py-1 rounded-full border border-gray-800 text-[10px] text-gray-400 font-bold">Loading...</div>
            <button onclick="logout()" class="text-red-400 text-xs p-1" title="Log Out"><i class="fa-solid fa-right-from-bracket"></i></button>
        </div>
    </header>

    <!-- CONTENT SCREEN GRID -->
    <main class="p-4 max-w-md mx-auto w-full space-y-4">

        <!-- TAB 1: ADD WASTE -->
        <section id="page-deposit" class="page-tab active space-y-4">
            <div class="glass p-5 rounded-2xl border border-gray-800 space-y-4">
                <h3 class="text-xs font-black text-gray-400 uppercase tracking-wider"><i class="fa-solid fa-square-plus text-emerald-400"></i> 1. සැබෑ කුණු තොගයක් එකතු කිරීම</h3>
                
                <div class="grid grid-cols-2 gap-3 text-xs">
                    <div class="space-y-1">
                        <label class="text-gray-400">කුණු වර්ගය</label>
                        <select id="w-type" class="w-full p-3 bg-gray-950 border border-gray-800 rounded-xl text-white font-bold focus:outline-none">
                            <option value="PET Plastics">PET ප්ලාස්ටික්</option>
                            <option value="Scrap Iron">යකඩ බඩු</option>
                            <option value="Glass Bottles">වීදුරු බෝතල්</option>
                            <option value="Cardboard">කාඩ්බෝඩ් / පත්තර</option>
                        </select>
                    </div>
                    <div class="space-y-1">
                        <label class="text-gray-400">බර (KG වලින්)</label>
                        <input type="number" id="w-weight" value="10" class="w-full p-3 bg-gray-950 border border-gray-800 rounded-xl text-white font-bold focus:outline-none">
                    </div>
                </div>

                <div class="space-y-1 text-xs">
                    <label class="text-gray-400">ලොකේෂන් / ලිපිනය</label>
                    <input type="text" id="w-location" placeholder="ගම හෝ නගරය සඳහන් කරන්න" class="w-full p-3 bg-gray-950 border border-gray-800 rounded-xl text-white focus:outline-none">
                </div>

                <button onclick="addWasteItem()" class="w-full bg-gradient-to-r from-emerald-500 to-teal-500 text-slate-950 font-black py-4 rounded-xl text-xs uppercase tracking-wider shadow-lg shadow-emerald-500/10">වෙළඳපොලට නිකුත් කරන්න 🔗</button>
            </div>
        </section>

        <!-- TAB 2: LIVE MARKETPLACE -->
        <section id="page-market" class="page-tab space-y-4">
            <div class="flex justify-between items-center px-1">
                <h3 class="text-xs font-black text-gray-400 uppercase tracking-wider"><i class="fa-solid fa-shop text-cyan-400"></i> 2. සජීවී වෙළඳපොල</h3>
                <button onclick="clearAllData()" class="text-[10px] text-red-400 font-bold underline bg-transparent border-none">සියල්ල මකන්න</button>
            </div>

            <!-- NEW WORK: CATEGORY FILTER BUTTONS -->
            <div class="flex gap-1.5 overflow-x-auto pb-1 text-[10px] scrollbar-none">
                <button onclick="filterMarket('All')" class="bg-emerald-500 text-slate-950 font-bold px-3 py-1.5 rounded-full whitespace-nowrap">සියල්ල</button>
                <button onclick="filterMarket('PET Plastics')" class="bg-gray-900 border border-gray-800 text-gray-300 px-3 py-1.5 rounded-full whitespace-nowrap">Plastics</button>
                <button onclick="filterMarket('Scrap Iron')" class="bg-gray-900 border border-gray-800 text-gray-300 px-3 py-1.5 rounded-full whitespace-nowrap">Iron</button>
                <button onclick="filterMarket('Glass Bottles')" class="bg-gray-900 border border-gray-800 text-gray-300 px-3 py-1.5 rounded-full whitespace-nowrap">Glass</button>
            </div>

            <div id="market-list" class="space-y-3">
                <!-- Data will instantly be loaded here from storage -->
            </div>
        </section>

    </main>

    <!-- NAVIGATION BAR -->
    <nav class="glass border-t border-gray-800/80 fixed bottom-0 left-0 right-0 h-16 flex max-w-md mx-auto z-40 px-2 py-1.5 gap-2">
        <button onclick="switchPage('page-deposit')" id="btn-page-deposit" class="nav-tab flex-1 flex flex-col justify-center items-center text-[10px] font-bold gap-1 text-gray-400 animate-pulse">
            <i class="fa-solid fa-circle-plus text-base"></i> කුණු දාන්න
        </button>
        <button onclick="switchPage('page-market')" id="btn-page-market" class="nav-tab flex-1 flex flex-col justify-center items-center text-[10px] font-bold gap-1 text-gray-400">
            <i class="fa-solid fa-table-cells-large text-base"></i> මාකට් එක
        </button>
    </nav>

    <script>
        let currentUser = null;
        let currentFilter = 'All';

        window.onload = function() {
            let savedUser = localStorage.getItem('ecoloop_user');
            if(savedUser) {
                currentUser = JSON.parse(savedUser);
                document.getElementById('auth-gate').classList.add('hidden');
                document.getElementById('profile-badge').innerText = currentUser.name;
            }
            renderMarketItems();
        }

        function saveUserSession() {
            const name = document.getElementById('auth-name').value.trim();
            const phone = document.getElementById('auth-phone').value.trim();
            const role = document.getElementById('auth-role').value;

            if(!name || !phone) { alert("කරුණාකර විස්තර ඇතුලත් කරන්න!"); return; }

            currentUser = { name, phone, role };
            localStorage.setItem('ecoloop_user', JSON.stringify(currentUser));
            document.getElementById('auth-gate').classList.add('hidden');
            document.getElementById('profile-badge').innerText = name;
            renderMarketItems();
        }

        function logout() {
            if(confirm("ගිණුමෙන් ඉවත් වෙන්නද බන්?")) {
                localStorage.removeItem('ecoloop_user');
                location.reload();
            }
        }

        function switchPage(id) {
            document.querySelectorAll('.page-tab').forEach(el => el.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.querySelectorAll('.nav-tab').forEach(el => el.classList.remove('nav-active'));
            document.getElementById('btn-' + id).classList.add('nav-active');
            if(id === 'page-market') renderMarketItems();
        }

        function addWasteItem() {
            const type = document.getElementById('w-type').value;
            const weight = document.getElementById('w-weight').value;
            const loc = document.getElementById('w-location').value.trim() || "Not specified";

            if(!weight || weight <= 0) { alert("කරුණාකර නිවැරදි බරක් දමන්න!"); return; }

            let items = JSON.parse(localStorage.getItem('ecoloop_items')) || [];
            items.unshift({
                id: Date.now(),
                owner: currentUser ? currentUser.name : "Anonymous",
                phone: currentUser ? currentUser.phone : "00000",
                type: type,
                weight: weight + " KG",
                location: loc
            });

            localStorage.setItem('ecoloop_items', JSON.stringify(items));
            alert("🤝 සාර්ථකයි! ඔබේ ඕඩරය සේව් වී මාකට් එකට එකතු වුණා.");
            document.getElementById('w-location').value = "";
            switchPage('page-market');
        }

        function filterMarket(category) {
            currentFilter = category;
            renderMarketItems();
        }

        function renderMarketItems() {
            const list = document.getElementById('market-list');
            list.innerHTML = '';
            let items = JSON.parse(localStorage.getItem('ecoloop_items')) || [];

            if(currentFilter !== 'All') {
                items = items.filter(item => item.type === currentFilter);
            }

            if(items.length === 0) {
                list.innerHTML = `<p class="text-center text-gray-600 text-xs py-10 font-mono">දැනට මෙම වර්ගයේ කිසිදු කුණු තොගයක් නොමැත.</p>`;
                return;
            }

            items.forEach(item => {
                list.innerHTML += `
                    <div class="bg-gray-950 p-4 rounded-xl border border-gray-800 space-y-2 animate-fadeIn">
                        <div class="flex justify-between items-center">
                            <span class="text-xs text-emerald-400 font-bold"><i class="fa-solid fa-user-circle"></i> ${item.owner}</span>
                            <span class="bg-emerald-500/10 text-emerald-400 text-[10px] px-2 py-0.5 rounded-md font-bold">${item.type}</span>
                        </div>
                        <div class="text-xs text-gray-400 flex justify-between">
                            <span>බර: <strong class="text-white">${item.weight}</strong></span>
                            <span>📍 ${item.location}</span>
                        </div>
                        <div class="pt-2 border-t border-gray-900 flex gap-2">
                            <a href="https://wa.me/${item.phone}" target="_blank" class="w-full text-center bg-emerald-500 text-slate-950 text-xs font-black py-2 rounded-lg"><i class="fa-brands fa-whatsapp"></i> ඇමතුමක් ගන්න</a>
                        </div>
                    </div>`;
            });
        }

        function clearAllData() {
            if(confirm("සිරාවටම ඔක්කොම ඕඩර්ස් ටික මකන්නද බන්?")) {
                localStorage.removeItem('ecoloop_items');
                renderMarketItems();
            }
        }
    </script>
</body>
</html>
