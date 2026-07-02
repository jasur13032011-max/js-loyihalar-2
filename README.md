# js-loyihalar-2
Barcha kodlar bitta admin.html fayliga joylashtirilgan. Shunchaki faylni saqlang va brauzerda oching.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Admin Panel</title>
    <style>
        /* --- CSS O'ZGARUVCHILARI (Light/Dark Mode uchun) --- */
        :root {
            --bg-color: #f4f6f9;
            --panel-bg: #ffffff;
            --text-color: #333333;
            --text-muted: #7f8c8d;
            --sidebar-bg: #2c3e50;
            --sidebar-text: #ffffff;
            --primary: #3498db;
            --success: #2ecc71;
            --danger: #e74c3c;
            --border-color: #e0e0e0;
        }

        [data-theme="dark"] {
            --bg-color: #121212;
            --panel-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --text-muted: #a0a0a0;
            --sidebar-bg: #0d1b2a;
            --sidebar-text: #e0e0e0;
            --border-color: #333333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            transition: background 0.3s ease, color 0.3s ease;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            min-height: 100vh;
        }

        /* --- SIDEBAR NAVIGATSIYA --- */
        .sidebar {
            width: 260px;
            background-color: var(--sidebar-bg);
            color: var(--sidebar-text);
            display: flex;
            flex-direction: column;
            padding: 20px 0;
            position: fixed;
            height: 100vh;
            z-index: 100;
            transition: transform 0.3s ease;
        }

        .sidebar-brand {
            font-size: 22px;
            font-weight: bold;
            text-align: center;
            margin-bottom: 30px;
            padding: 0 20px;
            color: var(--primary);
        }

        .sidebar-menu {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .sidebar-menu li {
            padding: 15px 25px;
            cursor: pointer;
            font-size: 16px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .sidebar-menu li:hover, .sidebar-menu li.active {
            background-color: rgba(255, 255, 255, 0.1);
            border-left: 4px solid var(--primary);
        }

        /* --- ASOSIY MAZMUN --- */
        .main-content {
            margin-left: 260px;
            flex: 1;
            padding: 30px;
        }

        /* Top Bar */
        .topbar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border-color);
        }

        .toggle-btn {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            color: var(--text-color);
            display: none;
        }

        .theme-switch {
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            color: var(--text-color);
            font-weight: bold;
        }

        /* Sahifalar boshqaruvi */
        .page {
            display: none;
        }

        .page.active {
            display: block;
        }

        /* --- DASHBOARD SAHIFASI --- */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: var(--panel-bg);
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            border: 1px solid var(--border-color);
        }

        .stat-card h3 {
            font-size: 14px;
            color: var(--text-muted);
            text-transform: uppercase;
            margin-bottom: 10px;
        }

        .stat-card .value {
            font-size: 28px;
            font-weight: bold;
        }

        /* --- JADVALLAR VA FORMALAR --- */
        .panel {
            background: var(--panel-bg);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.02);
            border: 1px solid var(--border-color);
            margin-bottom: 20px;
        }

        .form-inline {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 20px;
        }

        input, select {
            padding: 10px 15px;
            border: 1px solid var(--border-color);
            background: var(--panel-bg);
            color: var(--text-color);
            border-radius: 5px;
            font-size: 14px;
            flex: 1;
            min-width: 150px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: bold;
            font-size: 14px;
        }

        .btn-primary { background: var(--primary); color: white; }
        .btn-success { background: var(--success); color: white; }
        .btn-danger { background: var(--danger); color: white; }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid var(--border-color);
        }

        th { background: rgba(0,0,0,0.02); color: var(--text-muted); }

        .actions-btn {
            padding: 5px 10px;
            font-size: 12px;
            margin-right: 5px;
        }

        .empty-msg {
            text-align: center;
            color: var(--text-muted);
            padding: 20px;
            font-style: italic;
        }

        /* --- MOBIL RESPONSIVE --- */
        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
            }
            .sidebar.open {
                transform: translateX(0);
            }
            .main-content {
                margin-left: 0;
                padding: 15px;
            }
            .toggle-btn {
                display: block;
            }
        }
    </style>
</head>
<body>

    <div class="sidebar" id="sidebar">
        <div class="sidebar-brand">ADMIN PANEL</div>
        <ul class="sidebar-menu">
            <li class="active" onclick="switchPage('dashboard')">📊 Dashboard</li>
            <li onclick="switchPage('users')">👥 Foydalanuvchilar</li>
            <li onclick="switchPage('products')">📦 Mahsulotlar</li>
        </ul>
    </div>

    <div class="main-content">
        <div class="topbar">
            <button class="toggle-btn" onclick="toggleSidebar()">☰</button>
            <h2 id="pageTitle">Dashboard</h2>
            <button class="theme-switch" onclick="toggleTheme()" id="themeBtn">🌙 Tun</button>
        </div>

        <div id="dashboardPage" class="page active">
            <div class="stats-grid">
                <div class="stat-card">
                    <h3>Jami Foydalanuvchilar</h3>
                    <div class="value" id="dashUsersCount">0</div>
                </div>
                <div class="stat-card">
                    <h3>Jami Mahsulotlar</h3>
                    <div class="value" id="dashProductsCount">0</div>
                </div>
                <div class="stat-card">
                    <h3>Ombordagi jami mahsulot soni</h3>
                    <div class="value" id="dashTotalStock">0</div>
                </div>
            </div>
        </div>

        <div id="usersPage" class="page">
            <div class="panel">
                <h3 id="formTitle" style="margin-bottom: 15px;">Yangi foydalanuvchi qo'shish</h3>
                <form id="userForm" class="form-inline">
                    <input type="hidden" id="userId">
                    <input type="text" id="userName" placeholder="Ism Familiya" required>
                    <input type="email" id="userEmail" placeholder="Email manzil" required>
                    <button type="submit" class="btn-primary" id="userSubmitBtn">Qo'shish</button>
                    <button type="button" class="btn-danger" style="display:none;" id="cancelEditBtn" onclick="resetUserForm()">Bekor qilish</button>
                </form>
            </div>
            <div class="panel" style="overflow-x: auto;">
                <h3>Foydalanuvchilar ro'yxati</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Ism Familiya</th>
                            <th>Email</th>
                            <th>Amallar</th>
                        </tr>
                    </thead>
                    <tbody id="usersTableBody"></tbody>
                </table>
                <div id="usersEmpty" class="empty-msg">Foydalanuvchilar mavjud emas.</div>
            </div>
        </div>

        <div id="productsPage" class="page">
            <div class="panel">
                <h3>Qidiruv va Saralash</h3>
                <div class="form-inline" style="margin-top: 15px;">
                    <input type="text" id="prodSearch" placeholder="Mahsulot nomini yozing..." oninput="renderProducts()">
                    <select id="prodFilter" onchange="renderProducts()">
                        <option value="all">Barcha kategoriyalar</option>
                        <option value="Elektronika">Elektronika</option>
                        <option value="Kiyimlar">Kiyimlar</option>
                        <option value="Kitoblar">Kitoblar</option>
                    </select>
                </div>
            </div>
            <div class="panel" style="overflow-x: auto;">
                <h3>Mahsulotlar ombori</h3>
                <table>
                    <thead>
                        <tr>
                            <th>Nomi</th>
                            <th>Kategoriya</th>
                            <th>Miqdori (Soni)</th>
                        </tr>
                    </thead>
                    <tbody id="productsTableBody"></tbody>
                </table>
                <div id="productsEmpty" class="empty-msg">Mahsulot topilmadi.</div>
            </div>
        </div>
    </div>

<script>
    // --- BAZAVIY MA'LUMOTLAR (LocalStorage yoki Default) ---
    let users = JSON.parse(localStorage.getItem('admin_users')) || [
        { id: '1', userName: 'Anvar Aliyev', userEmail: 'anvar@gmail.com' },
        { id: '2', userName: 'Dilnoza Karimova', userEmail: 'dilnoza@mail.ru' }
    ];

    let products = JSON.parse(localStorage.getItem('admin_products')) || [
        { name: 'iPhone 15 Pro', category: 'Elektronika', stock: 12 },
        { name: 'T-Shirt Sport', category: 'Kiyimlar', stock: 45 },
        { name: 'JavaScript Qo\'llanma', category: 'Kitoblar', stock: 7 },
        { name: 'MacBook Air M3', category: 'Elektronika', stock: 5 }
    ];

    let isEditing = false;

    // --- DARK / LIGHT MODE ---
    function toggleTheme() {
        const currentTheme = document.documentElement.getAttribute('data-theme');
        const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
        document.documentElement.setAttribute('data-theme', newTheme);
        localStorage.setItem('admin_theme', newTheme);
        document.getElementById('themeBtn').innerText = newTheme === 'dark' ? '☀️ Kun' : '🌙 Tun';
    }
    // Dastlabki mavzuni tiklash
    if(localStorage.getItem('admin_theme') === 'dark') {
        document.documentElement.setAttribute('data-theme', 'dark');
        document.getElementById('themeBtn').innerText = '☀️ Kun';
    }

    // --- SIDEBAR VA NAVIGATSIYA ---
    function switchPage(pageId) {
        document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
        document.querySelectorAll('.sidebar-menu li').forEach(li => li.classList.remove('active'));
        
        document.getElementById(pageId + 'Page').classList.add('active');
        event.currentTarget.classList.add('active');
        
        // Sarlavhani o'zgartirish
        const titles = { dashboard: '📊 Dashboard', users: '👥 Foydalanuvchilar', products: '📦 Mahsulotlar' };
        document.getElementById('pageTitle').innerText = titles[pageId];

        // Mobil qurilmada sidebar yopilishi uchun
        document.getElementById('sidebar').classList.remove('open');
        
        if(pageId === 'dashboard') updateDashboard();
    }

    function toggleSidebar() {
        document.getElementById('sidebar').classList.toggle('open');
    }

    // --- DASHBOARD OPERATSIYALARI ---
    function updateDashboard() {
        document.getElementById('dashUsersCount').innerText = users.length;
        document.getElementById('dashProductsCount').innerText = products.length;
        const totalStock = products.reduce((sum, item) => sum + parseInt(item.stock), 0);
        document.getElementById('dashTotalStock').innerText = totalStock;
    }

    // --- FOYDALANUVCHILAR CRUD MATRITSA ---
    const userForm = document.getElementById('userForm');
    
    function renderUsers() {
        const tbody = document.getElementById('usersTableBody');
        tbody.innerHTML = '';
        if(users.length === 0) {
            document.getElementById('usersEmpty').style.display = 'block';
        } else {
            document.getElementById('usersEmpty').style.display = 'none';
            users.forEach(user => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${user.userName}</td>
                    <td>${user.userEmail}</td>
                    <td>
                        <button class="actions-btn btn-success" onclick="editUser('${user.id}')">✏️</button>
                        <button class="actions-btn btn-danger" onclick="deleteUser('${user.id}')">🗑️</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }
        localStorage.setItem('admin_users', JSON.stringify(users));
        updateDashboard();
    }

    userForm.addEventListener('submit', (e) => {
        e.preventDefault();
        const id = document.getElementById('userId').value;
        const name = document.getElementById('userName').value.trim();
        const email = document.getElementById('userEmail').value.trim();

        if(isEditing) {
            users = users.map(u => u.id === id ? { ...u, userName: name, userEmail: email } : u);
            resetUserForm();
        } else {
            users.push({ id: Date.now().toString(), userName: name, userEmail: email });
        }
        userForm.reset();
        renderUsers();
    });

    function editUser(id) {
        const user = users.find(u => u.id === id);
        if(!user) return;
        document.getElementById('userId').value = user.id;
        document.getElementById('userName').value = user.userName;
        document.getElementById('userEmail').value = user.userEmail;
        
        isEditing = true;
        document.getElementById('formTitle').innerText = "Foydalanuvchini tahrirlash";
        document.getElementById('userSubmitBtn').innerText = "Saqlash";
        document.getElementById('cancelEditBtn').style.display = 'inline-block';
    }

    function deleteUser(id) {
        if(confirm("Ushbu foydalanuvchini o'chirishni tasdiqlaysizmi?")) {
            users = users.filter(u => u.id !== id);
            if(document.getElementById('userId').value === id) resetUserForm();
            renderUsers();
        }
    }

    function resetUserForm() {
        isEditing = false;
        userForm.reset();
        document.getElementById('userId').value = '';
        document.getElementById('formTitle').innerText = "Yangi foydalanuvchi qo'shish";
        document.getElementById('userSubmitBtn').innerText = "Qo'shish";
        document.getElementById('cancelEditBtn').style.display = 'none';
    }

    // --- MAHSULOTLAR (QIDIRUV VA FILTR) ---
    function renderProducts() {
        const tbody = document.getElementById('productsTableBody');
        const searchValue = document.getElementById('prodSearch').value.toLowerCase();
        const filterValue = document.getElementById('prodFilter').value;
        
        tbody.innerHTML = '';

        const filtered = products.filter(p => {
            const matchesSearch = p.name.toLowerCase().includes(searchValue);
            const matchesFilter = filterValue === 'all' || p.category === filterValue;
            return matchesSearch && matchesFilter;
        });

        if(filtered.length === 0) {
            document.getElementById('productsEmpty').style.display = 'block';
        } else {
            document.getElementById('productsEmpty').style.display = 'none';
            filtered.forEach(p => {
                const tr = document.createElement('tr');
                tr.innerHTML = `<td>${p.name}</td><td>${p.category}</td><td><b>${p.stock} ta</b></td>`;
                tbody.appendChild(tr);
            });
        }
        localStorage.setItem('admin_products', JSON.stringify(products));
        updateDashboard();
    }

    // Dasturni dastlabki ishga tushirish
    updateDashboard();
    renderUsers();
    renderProducts();
</script>
</body>
</html>
Kod tarkibidagi asosiy yechimlar:
Sidebar navigatsiya: Chap qismdagi menyular orqali sahifalar (Dashboard, Foydalanuvchilar, Mahsulotlar) o'rtasida display: none / block mantiqiy ssenariysi yordamida silliq o'tiladi.

Dashboard: Umumiy dinamik hisoblagichlar joylashtirilgan. Foydalanuvchi qo'shilganda yoki o'chirilganda, u yerdagi raqamlar ham real vaqtda o'zgaradi.

To'liq CRUD (Foydalanuvchilar): Jadvalda ko'rsatish, yangi qo'shish, o'chirish (confirm orqali) va tahrirlash (ma'lumotlarni formaga qayta yuklash) funksiyalari to'liq ishlaydi.

Qidiruv va Filtr (Mahsulotlar): Real vaqt rejimida ham inputga yozish, ham select (kategoriya) elementini o'zgartirish orqali kombinatsiyalashgan qidiruv tizimi ishlaydi.

Light/Dark Mode: CSS o'zgaruvchilari (--bg-color, --panel-bg) yordamida tugma bosilganda sahifa ranglari bir lahzada silliq almashadi.

Xotira (localStorage): Kiritilgan barcha o'zgarishlar va tanlangan rejim brauzer xotirasida mukammal saqlanib qoladi.
