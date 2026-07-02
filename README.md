# js-loyihalar-2

HTML, CSS va JavaScript kodi
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Moliya Tracker — Balans Nazorati</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f6f9;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .tracker-container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            width: 100%;
            max-width: 500px;
        }

        h2, h3 {
            color: #1e293b;
            margin-bottom: 20px;
            text-align: center;
        }

        /* Dashbord (Balans, Daromad, Xarajat paneli) */
        .balance-section {
            text-align: center;
            margin-bottom: 25px;
        }

        .balance-title {
            font-size: 0.9rem;
            color: #64748b;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .balance-amount {
            font-size: 2.2rem;
            font-weight: 700;
            color: #0f172a;
            margin-top: 5px;
        }

        .stats-container {
            display: flex;
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-box {
            flex: 1;
            background: #f8fafc;
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            border: 1px solid #e2e8f0;
        }

        .stat-box.income h4 { color: #10b981; }
        .stat-box.expense h4 { color: #ef4444; }

        .stat-amount {
            font-size: 1.2rem;
            font-weight: 600;
            margin-top: 5px;
            color: #334155;
        }

        /* Forma dizayni */
        .form-group {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 30px;
            background: #f8fafc;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #e2e8f0;
        }

        .form-group input, .form-group select {
            padding: 12px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            font-size: 1rem;
            outline: none;
        }

        .form-group input:focus, .form-group select:focus {
            border-color: #3b82f6;
        }

        .submit-btn {
            padding: 12px;
            background: #3b82f6;
            color: white;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            font-size: 1rem;
            cursor: pointer;
            transition: background 0.2s;
        }

        .submit-btn:hover {
            background: #2563eb;
        }

        /* Tranzaksiyalar ro'yxati */
        .history-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-height: 250px;
            overflow-y: auto;
            padding-right: 5px;
        }

        .transaction-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 16px;
            background: white;
            border-radius: 8px;
            border-left: 5px solid #cbd5e1;
            box-shadow: 0 2px 5px rgba(0,0,0,0.02);
            position: relative;
        }

        /* Ranglar farqi */
        .transaction-item.income-type {
            border-left-color: #10b981;
        }

        .transaction-item.expense-type {
            border-left-color: #ef4444;
        }

        .tx-details {
            display: flex;
            flex-direction: column;
        }

        .tx-text {
            font-weight: 500;
            color: #334155;
        }

        .tx-amount {
            font-weight: 600;
        }

        .income-type .tx-amount { color: #10b981; }
        .expense-type .tx-amount { color: #ef4444; }

        .delete-tx-btn {
            background: transparent;
            border: none;
            color: #94a3b8;
            cursor: pointer;
            font-size: 1.2rem;
            font-weight: bold;
            padding: 4px 8px;
            transition: color 0.2s;
        }

        .delete-tx-btn:hover {
            color: #ef4444;
        }
    </style>
</head>
<body>

    <div class="tracker-container">
        <h2>Moliya Tracker</h2>

        <div class="balance-section">
            <div class="balance-title">Jami Balans</div>
            <div class="balance-amount" id="totalBalance">0 SO'M</div>
        </div>

        <div class="stats-container">
            <div class="stat-box income">
                <h4>Daromad</h4>
                <div class="stat-amount" id="totalIncome">0 SO'M</div>
            </div>
            <div class="stat-box expense">
                <h4>Xarajat</h4>
                <div class="stat-amount" id="totalExpense">0 SO'M</div>
            </div>
        </div>

        <h3>Yangi Tranzaksiya</h3>
        <form id="txForm" class="form-group">
            <input type="text" id="txText" placeholder="Tranzaksiya nomi (Masalan: Oylik, Supermarket...)" required>
            <input type="number" id="txAmount" placeholder="Summa (SO'M)" min="1" required>
            <select id="txType">
                <option value="income">Daromad (+)</option>
                <option value="expense">Xarajat (-)</option>
            </select>
            <button type="submit" class="submit-btn">Qo'shish</button>
        </form>

        <h3>Tranzaksiyalar Ro'yxati</h3>
        <ul class="history-list" id="historyList"></ul>
    </div>

    <script>
        // DOM Elementlari
        const txForm = document.getElementById('txForm');
        const txText = document.getElementById('txText');
        const txAmount = document.getElementById('txAmount');
        const txType = document.getElementById('txType');
        const historyList = document.getElementById('historyList');

        const totalBalance = document.getElementById('totalBalance');
        const totalIncome = document.getElementById('totalIncome');
        const totalExpense = document.getElementById('totalExpense');

        // LocalStorage'dan ma'lumotlarni yuklash
        let transactions = JSON.parse(localStorage.getItem('transactions')) || [];

        // ==========================================
        //  HISOBLASH LOGIKASI (Alohida funksiyalar)
        // ==========================================
        
        // 1. Umumiy daromadni hisoblash
        function calculateIncome() {
            return transactions
                .filter(tx => tx.type === 'income')
                .reduce((sum, tx) => sum + tx.amount, 0);
        }

        // 2. Umumiy xarajatni hisoblash
        function calculateExpense() {
            return transactions
                .filter(tx => tx.type === 'expense')
                .reduce((sum, tx) => sum + tx.amount, 0);
        }

        // 3. Jami balansni hisoblash
        function calculateBalance(income, expense) {
            return income - expense;
        }

        // Dashbord UI (ekran) qismini yangilash
        function updateDashboardUI() {
            const income = calculateIncome();
            const expense = calculateExpense();
            const balance = calculateBalance(income, expense);

            totalIncome.textContent = `${income.toLocaleString()} SO'M`;
            totalExpense.textContent = `${expense.toLocaleString()} SO'M`;
            totalBalance.textContent = `${balance.toLocaleString()} SO'M`;
            
            // Balans minusga kirib ketsa qizil rangda ko'rsatish
            totalBalance.style.color = balance >= 0 ? '#0f172a' : '#ef4444';
        }

        // ==========================================
        //  RENDER VA ASOSIY AMALLAR LOGIKASI
        // ==========================================

        // Tranzaksiyalar ro'yxatini render qilish
        function renderTransactions() {
            historyList.innerHTML = '';

            transactions.forEach(tx => {
                const li = document.createElement('li');
                const sign = tx.type === 'income' ? '+' : '-';
                
                // Ranglar farqi uchun klass qo'shish
                li.className = `transaction-item ${tx.type}-type`;

                li.innerHTML = `
                    <div class="tx-details">
                        <span class="tx-text">${tx.text}</span>
                    </div>
                    <div>
                        <span class="tx-amount">${sign}${tx.amount.toLocaleString()} SO'M</span>
                        <button class="delete-tx-btn" onclick="deleteTransaction(${tx.id})">×</button>
                    </div>
                `;

                historyList.appendChild(li);
            });
        }

        // Yangi tranzaksiya qo'shish
        txForm.addEventListener('submit', (e) => {
            e.preventDefault();

            const newTransaction = {
                id: Date.now(),
                text: txText.value.trim(),
                amount: parseFloat(txAmount.value),
                type: txType.value
            };

            transactions.push(newTransaction);
            saveToStorage();
            init();

            // Formani tozalash
            txText.value = '';
            txAmount.value = '';
        });

        // Tranzaksiyani o'chirish
        window.deleteTransaction = function(id) {
            transactions = transactions.filter(tx => tx.id !== id);
            saveToStorage();
            init();
        };

        // Ma'lumotlarni localStorage'da saqlash
        function saveToStorage() {
            localStorage.setItem('transactions', JSON.stringify(transactions));
        }

        // Ilovani qayta yuklash / ishga tushirish funksiyasi
        function init() {
            renderTransactions();
            updateDashboardUI();
        }

        // Dasturni ilk bor ishga tushirish
        init();
    </script>
</body>
</html>
Talablar qanday bajarildi?
Daromad va xarajat shakli: HTML ichida matnli input, raqamli summa kiritish inputi va turini tanlash (Daromad/Xarajat) uchun select elementi shakllantirildi.

Jami balans, daromad va xarajatlarni hisoblash: calculateIncome, calculateExpense va calculateBalance funksiyalari orqali xavfsiz hisob-kitob qilinadi hamda jami summa .toLocaleString() metodi yordamida chiroyli formatda (Masalan: 1,250,000 SO'M) ekranga chiqariladi.

Rang bilan ajratilgan ro'yxat: CSS yordamida tranzaksiya turi tekshirilib, agar u income bo'lsa yashil (#10b981), expense bo'lsa qizil (#ef4444) rangli hoshiyalar (border-left) va matn ranglari berildi.

Tranzaksiyani o'chirish: Har bir element yonidagi × tugmasi deleteTransaction(id) funksiyasini chaqiradi va massivdan tegishli elementni o'chirib, hisob-kitobni qayta yangilaydi.

localStorage: Har safar yangi tranzaksiya kiritilganda yoki o'chirilganda o'zgarishlar localStorage.setItem
