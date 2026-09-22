<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cari Takip Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen font-sans">

    <!-- ANA KAPSAYICI -->
    <div id="app" class="max-w-md mx-auto min-h-screen bg-slate-900 shadow-2xl flex flex-col relative border-x border-slate-800">
        
        <!-- ================= 1. SAYFA: ANA SAYFA ================= -->
        <div id="page-home" class="flex flex-col flex-1">
            <!-- Üst Bar -->
            <header class="bg-slate-900/90 backdrop-blur sticky top-0 z-20 border-b border-slate-800 px-4 py-3 flex items-center justify-between">
                <button onclick="openAddCustomerModal()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3 py-1.5 rounded-lg text-sm font-medium flex items-center gap-1.5 transition shadow-lg shadow-emerald-900/20">
                    <i class="fa-solid fa-plus"></i> Müşteri Ekle
                </button>
                <h1 class="text-base font-bold text-slate-200 tracking-wide">Anasayfa</h1>
                <div class="relative">
                    <button onclick="toggleSortMenu()" class="text-slate-300 hover:text-white px-2 py-1.5 rounded-lg text-sm bg-slate-800 border border-slate-700 flex items-center gap-1">
                        <i class="fa-solid fa-filter text-xs"></i> Sırala
                    </button>
                    <!-- Sıralama Menüsü -->
                    <div id="sort-menu" class="hidden absolute right-0 mt-2 w-48 bg-slate-800 border border-slate-700 rounded-xl shadow-2xl py-1 z-30">
                        <button onclick="setSort('debt-desc')" class="w-full text-left px-4 py-2 text-xs text-slate-300 hover:bg-slate-700 hover:text-white">Borca Göre (Yüksek > Düşük)</button>
                        <button onclick="setSort('debt-asc')" class="w-full text-left px-4 py-2 text-xs text-slate-300 hover:bg-slate-700 hover:text-white">Borca Göre (Düşük > Yüksek)</button>
                        <button onclick="setSort('name-asc')" class="w-full text-left px-4 py-2 text-xs text-slate-300 hover:bg-slate-700 hover:text-white">İsme Göre (A > Z)</button>
                    </div>
                </div>
            </header>

            <!-- Arama Çubuğu -->
            <div class="p-4 pb-2">
                <div class="relative">
                    <i class="fa-solid fa-search absolute left-3.5 top-3 text-slate-500 text-sm"></i>
                    <input type="text" id="search-input" oninput="renderCustomers()" placeholder="Müşteri ara..." class="w-full bg-slate-950 border border-slate-800 rounded-xl pl-10 pr-4 py-2.5 text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:border-emerald-500">
                </div>
            </div>

            <!-- Müşteri Listesi -->
            <div class="flex-1 px-4 py-2 space-y-2.5 overflow-y-auto pb-6" id="customer-list">
                <!-- Javascript ile doldurulacak -->
            </div>
        </div>

        <!-- ================= 2. SAYFA: MÜŞTERİ DETAY SAYFASI ================= -->
        <div id="page-detail" class="hidden flex flex-col flex-1 bg-slate-900">
            <!-- Üst Bar -->
            <header class="bg-slate-900/90 backdrop-blur sticky top-0 z-20 border-b border-slate-800 px-4 py-3 flex items-center justify-between">
                <button onclick="goHome()" class="text-slate-400 hover:text-white px-2 py-1 rounded-lg text-sm flex items-center gap-1 transition">
                    <i class="fa-solid fa-arrow-left"></i> Geri
                </button>
                <h1 id="detail-customer-name" class="text-base font-bold text-slate-200">Müşteri Adı</h1>
                <div class="w-12"></div> <!-- Hizalama için boşluk -->
            </header>

            <!-- Müşteri Özet Kartı -->
            <div class="p-4">
                <div class="bg-gradient-to-br from-slate-800 to-slate-850 border border-slate-750 rounded-2xl p-4 text-center shadow-lg">
                    <span class="text-xs text-slate-400 font-medium uppercase tracking-wider">Güncel Bakiye</span>
                    <div id="detail-customer-balance" class="text-2xl font-black mt-1 text-emerald-400">0.00 ₺</div>
                </div>
            </div>

            <!-- 4 Ana Buton (Çizimdeki Tasarım) -->
            <div class="px-4 grid grid-cols-2 gap-2.5">
                <button onclick="openTransactionModal('odeme')" class="bg-red-500/10 border border-red-500/30 hover:bg-red-500/20 text-red-400 p-3 rounded-xl font-semibold text-sm flex items-center justify-center gap-2 transition">
                    <i class="fa-solid fa-arrow-up-right-from-square"></i> Ödeme
                </button>
                <button onclick="openTransactionModal('tahsilat')" class="bg-emerald-500/10 border border-emerald-500/30 hover:bg-emerald-500/20 text-emerald-400 p-3 rounded-xl font-semibold text-sm flex items-center justify-center gap-2 transition">
                    <i class="fa-solid fa-arrow-down-left-and-arrow-right-to-center"></i> Tahsilat
                </button>
                <button onclick="switchTab('summary')" id="btn-tab-summary" class="bg-slate-800 border border-slate-700 hover:bg-slate-750 text-slate-200 p-3 rounded-xl font-semibold text-sm flex items-center justify-center gap-2 transition">
                    <i class="fa-solid fa-file-lines text-sky-400"></i> Hesap Özeti
                </button>
                <button onclick="shareWhatsApp()" class="bg-emerald-600 hover:bg-emerald-500 text-white p-3 rounded-xl font-semibold text-sm flex items-center justify-center gap-2 transition shadow-md shadow-emerald-900/30">
                    <i class="fa-brands fa-whatsapp text-lg"></i> Paylaş - WhatsApp
                </button>
            </div>

            <!-- İçerik Alanı (Hesap Özeti / Hareketler Listesi) -->
            <div class="flex-1 px-4 mt-4 overflow-y-auto pb-6">
                <h3 class="text-xs font-semibold text-slate-400 uppercase tracking-wider mb-2">İşlem Geçmişi</h3>
                <div id="transaction-list" class="space-y-2">
                    <!-- İşlemler buraya gelecek -->
                </div>
            </div>
        </div>

    </div>

    <!-- MÜŞTERİ EKLE MODAL -->
    <div id="modal-add-customer" class="hidden fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 w-full max-w-sm rounded-2xl p-5 shadow-2xl">
            <h3 class="text-base font-bold text-slate-100 mb-4">Yeni Müşteri Ekle</h3>
            <input type="text" id="new-customer-name" placeholder="Müşteri Adı Soyadı (Örn: Ahmet)" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:border-emerald-500 mb-4">
            <div class="flex gap-2.5">
                <button onclick="closeAddCustomerModal()" class="flex-1 bg-slate-800 hover:bg-slate-700 text-slate-300 py-2.5 rounded-xl text-sm font-medium transition">İptal</button>
                <button onclick="addCustomer()" class="flex-1 bg-emerald-600 hover:bg-emerald-500 text-white py-2.5 rounded-xl text-sm font-medium transition">Kaydet</button>
            </div>
        </div>
    </div>

    <!-- İŞLEM EKLE / DÜZENLE MODAL (Ödeme veya Tahsilat) -->
    <div id="modal-transaction" class="hidden fixed inset-0 bg-black/70 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 w-full max-w-sm rounded-2xl p-5 shadow-2xl">
            <h3 id="tx-modal-title" class="text-base font-bold text-slate-100 mb-4">İşlem Ekle</h3>
            <input type="hidden" id="tx-edit-id">
            <div class="space-y-3 mb-4">
                <div>
                    <label class="text-xs text-slate-400 mb-1 block">İşlem Türü</label>
                    <select id="tx-type" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 focus:outline-none focus:border-emerald-500">
                        <option value="odeme">Ödeme (Borç Ekle)</option>
                        <option value="tahsilat">Tahsilat (Para Al)</option>
                    </select>
                </div>
                <div>
                    <label class="text-xs text-slate-400 mb-1 block">Tutar (₺)</label>
                    <input type="number" id="tx-amount" placeholder="0.00" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:border-emerald-500">
                </div>
                <div>
                    <label class="text-xs text-slate-400 mb-1 block">Açıklama</label>
                    <input type="text" id="tx-desc" placeholder="İşlem açıklaması..." class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-2.5 text-sm text-slate-100 placeholder-slate-500 focus:outline-none focus:border-emerald-500">
                </div>
            </div>
            <div class="flex gap-2.5">
                <button onclick="closeTransactionModal()" class="flex-1 bg-slate-800 hover:bg-slate-700 text-slate-300 py-2.5 rounded-xl text-sm font-medium transition">İptal</button>
                <button onclick="saveTransaction()" class="flex-1 bg-emerald-600 hover:bg-emerald-500 text-white py-2.5 rounded-xl text-sm font-medium transition">Kaydet</button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT MİMARİSİ -->
    <script>
        // Başlangıç Verileri (Çizimindeki Ahmet, Mehmet ve Canım örnekleri)
        let customers = JSON.parse(localStorage.getItem('cari_customers')) || [
            { id: 1, name: 'Ahmet', transactions: [{ id: 101, type: 'odeme', amount: 2800, desc: 'Açılış Borcu', date: '22.09.2026 10:00' }] },
            { id: 2, name: 'Mehmet', transactions: [{ id: 102, type: 'odeme', amount: 800, desc: 'Açılış Borcu', date: '22.09.2026 10:05' }] },
            { id: 3, name: 'Canım', transactions: [{ id: 103, type: 'odeme', amount: 4000, desc: 'Açılış Borcu', date: '22.09.2026 10:10' }] }
        ];

        let currentCustomerId = null;
        let currentSort = 'debt-desc';

        function saveData() {
            localStorage.setItem('cari_customers', JSON.stringify(customers));
        }

        function getCustomerBalance(customer) {
            return customer.transactions.reduce((acc, t) => {
                return t.type === 'odeme' ? acc + t.amount : acc - t.amount;
            }, 0);
        }

        // Sayfa Yönetimi
        function goHome() {
            document.getElementById('page-home').classList.remove('hidden');
            document.getElementById('page-detail').classList.add('hidden');
            currentCustomerId = null;
            renderCustomers();
        }

        function openDetail(id) {
            currentCustomerId = id;
            document.getElementById('page-home').classList.add('hidden');
            document.getElementById('page-detail').classList.remove('hidden');
            renderDetail();
        }

        // Sıralama Menüsü
        function toggleSortMenu() {
            const menu = document.getElementById('sort-menu');
            menu.classList.toggle('hidden');
        }

        function setSort(type) {
            currentSort = type;
            document.getElementById('sort-menu').classList.add('hidden');
            renderCustomers();
        }

        // Müşteri Listeleme
        function renderCustomers() {
            const listEl = document.getElementById('customer-list');
            const searchVal = document.getElementById('search-input').value.toLowerCase();
            
            let filtered = customers.filter(c => c.name.toLowerCase().includes(searchVal));

            // Sıralama Mantığı
            filtered.sort((a, b) => {
                let balA = getCustomerBalance(a);
                let balB = getCustomerBalance(b);
                if (currentSort === 'debt-desc') return balB - balA;
                if (currentSort === 'debt-asc') return balA - balB;
                if (currentSort === 'name-asc') return a.name.localeCompare(b.name);
            });

            if (filtered.length === 0) {
                listEl.innerHTML = `<div class="text-center py-10 text-slate-500 text-sm">Müşteri bulunamadı.</div>`;
                return;
            }

            listEl.innerHTML = filtered.map(c => {
                let bal = getCustomerBalance(c);
                return `
                    <div onclick="openDetail(${c.id})" class="bg-slate-900 border border-slate-800 hover:border-slate-700 p-4 rounded-2xl flex items-center justify-between cursor-pointer transition shadow-sm">
                        <div class="flex items-center gap-3">
                            <div class="w-10 h-10 rounded-xl bg-slate-800 flex items-center justify-center text-slate-300 font-bold text-sm">
                                ${c.name.charAt(0).toUpperCase()}
                            </div>
                            <span class="font-semibold text-sm text-slate-200">${c.name}</span>
                        </div>
                        <div class="text-right">
                            <span class="text-sm font-bold ${bal >= 0 ? 'text-red-400' : 'text-emerald-400'}">${bal.toLocaleString('tr-TR')} ₺</span>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Müşteri Detay Render
        function renderDetail() {
            const customer = customers.find(c => c.id === currentCustomerId);
            if (!customer) return;

            document.getElementById('detail-customer-name').innerText = customer.name;
            let bal = getCustomerBalance(customer);
            
            const balEl = document.getElementById('detail-customer-balance');
            balEl.innerText = bal.toLocaleString('tr-TR') + ' ₺';
            balEl.className = `text-2xl font-black mt-1 ${bal >= 0 ? 'text-red-400' : 'text-emerald-400'}`;

            const txListEl = document.getElementById('transaction-list');
            if (customer.transactions.length === 0) {
                txListEl.innerHTML = `<div class="text-center py-8 text-slate-500 text-xs">Henüz işlem bulunmuyor.</div>`;
                return;
            }

            txListEl.innerHTML = customer.transactions.slice().reverse().map(t => {
                let isOdeme = t.type === 'odeme';
                return `
                    <div class="bg-slate-900 border border-slate-800 p-3 rounded-xl flex items-center justify-between">
                        <div>
                            <div class="flex items-center gap-2">
                                <span class="text-xs font-bold px-2 py-0.5 rounded-md ${isOdeme ? 'bg-red-500/10 text-red-400' : 'bg-emerald-500/10 text-emerald-400'}">
                                    ${isOdeme ? 'Ödeme' : 'Tahsilat'}
                                </span>
                                <span class="text-xs text-slate-400">${t.date}</span>
                            </div>
                            <p class="text-xs text-slate-300 mt-1">${t.desc || 'Açıklama yok'}</p>
                        </div>
                        <div class="flex items-center gap-3">
                            <span class="text-sm font-bold ${isOdeme ? 'text-red-400' : 'text-emerald-400'}">
                                ${isOdeme ? '+' : '-'}${t.amount.toLocaleString('tr-TR')} ₺
                            </span>
                            <div class="flex gap-1">
                                <button onclick="editTransaction(${t.id})" class="text-slate-500 hover:text-slate-300 p-1 text-xs"><i class="fa-solid fa-pen"></i></button>
                                <button onclick="deleteTransaction(${t.id})" class="text-slate-500 hover:text-red-400 p-1 text-xs"><i class="fa-solid fa-trash"></i></button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Müşteri Ekle Modal
        function openAddCustomerModal() {
            document.getElementById('modal-add-customer').classList.remove('hidden');
            document.getElementById('new-customer-name').value = '';
        }
        function closeAddCustomerModal() {
            document.getElementById('modal-add-customer').classList.add('hidden');
        }
        function addCustomer() {
            const name = document.getElementById('new-customer-name').value.trim();
            if (!name) return alert('Lütfen müşteri adı girin.');
            
            customers.push({ id: Date.now(), name, transactions: [] });
            saveData();
            closeAddCustomerModal();
            renderCustomers();
        }

        // İşlem Ekle / Düzenle Modal
        function openTransactionModal(type, tx = null) {
            document.getElementById('modal-transaction').classList.remove('hidden');
            if (tx) {
                document.getElementById('tx-modal-title').innerText = 'İşlemi Düzenle';
                document.getElementById('tx-edit-id').value = tx.id;
                document.getElementById('tx-type').value = tx.type;
                document.getElementById('tx-amount').value = tx.amount;
                document.getElementById('tx-desc').value = tx.desc;
            } else {
                document.getElementById('tx-modal-title').innerText = 'Yeni İşlem Ekle';
                document.getElementById('tx-edit-id').value = '';
                document.getElementById('tx-type').value = type;
                document.getElementById('tx-amount').value = '';
                document.getElementById('tx-desc').value = '';
            }
        }
        function closeTransactionModal() {
            document.getElementById('modal-transaction').classList.add('hidden');
        }

        function saveTransaction() {
            const customer = customers.find(c => c.id === currentCustomerId);
            if (!customer) return;

            const editId = document.getElementById('tx-edit-id').value;
            const type = document.getElementById('tx-type').value;
            const amount = parseFloat(document.getElementById('tx-amount').value);
            const desc = document.getElementById('tx-desc').value.trim();

            if (!amount || amount <= 0) return alert('Lütfen geçerli bir tutar girin.');

            const now = new Date();
            const dateStr = now.toLocaleDateString('tr-TR') + ' ' + now.toLocaleTimeString('tr-TR', {hour: '2-digit', minute:'2-digit'});

            if (editId) {
                // Güncelleme
                const tx = customer.transactions.find(t => t.id == editId);
                if (tx) {
                    tx.type = type;
                    tx.amount = amount;
                    tx.desc = desc;
                }
            } else {
                // Yeni Ekleme
                customer.transactions.push({
                    id: Date.now(),
                    type,
                    amount,
                    desc,
                    date: dateStr
                });
            }

            saveData();
            closeTransactionModal();
            renderDetail();
        }

        function editTransaction(txId) {
            const customer = customers.find(c => c.id === currentCustomerId);
            const tx = customer.transactions.find(t => t.id === txId);
            if (tx) openTransactionModal(tx.type, tx);
        }

        function deleteTransaction(txId) {
            if (!confirm('Bu işlemi silmek istediğinize emin misiniz?')) return;
            const customer = customers.find(c => c.id === currentCustomerId);
            customer.transactions = customer.transactions.filter(t => t.id !== txId);
            saveData();
            renderDetail();
        }

        // WhatsApp Paylaşım
        function shareWhatsApp() {
            const customer = customers.find(c => c.id === currentCustomerId);
            if (!customer) return;
            let bal = getCustomerBalance(customer);
            let text = `Sayın ${customer.name}, güncel hesap özetiniz:\nToplam Bakiye: ${bal.toLocaleString('tr-TR')} ₺\n\nİşlem Geçmişi:\n`;
            customer.transactions.forEach(t => {
                text += `- ${t.date}: ${t.type === 'odeme' ? 'Ödeme' : 'Tahsilat'} ${t.amount}₺ (${t.desc || '- '})\n`;
            });
            window.open(`https://wa.me/?text=${encodeURIComponent(text)}`, '_blank');
        }

        // İlk Çalıştırma
        renderCustomers();
    </script>
</body>
</html>
