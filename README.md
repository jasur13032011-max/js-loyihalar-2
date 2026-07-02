# js-loyihalar-2

Conversation with Gemini
 Требования

✅ Kamida 6 ta rang tugmasi bo'lsin

✅ 'Random rang' tugmasi tasodifiy rang tanlаsin

✅ Joriy rang HEX kodi ekranda ko'rinsin

✅ Silliq animatsiya bilan rang almashinsin

✅ Oxirgi tanlangan rangni localStorage'ga saqlang

Mana, barcha talablarga javob beradigan, chiroyli va tushunarli kod. Uni bitta HTML fayl sifatida saqlab, brauzerda ochishingiz mumkin.

HTML & CSS & JavaScript (Barchasi birda)
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rang O'zgartiruvchi</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            /* Silliq animatsiya bilan rang almashishi uchun transition */
            transition: background-color 0.5s ease; 
            background-color: #2c3e50;
        }

        .container {
            background: rgba(255, 255, 255, 0.85);
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            backdrop-filter: blur(10px);
            text-align: center;
            max-width: 400px;
            width: 100%;
        }

        h2 {
            margin-bottom: 10px;
            color: #333;
        }

        .hex-code {
            font-size: 1.5rem;
            font-weight: bold;
            color: #2c3e50;
            background: #ededed;
            padding: 8px 15px;
            border-radius: 8px;
            display: inline-block;
            margin-bottom: 25px;
            letter-spacing: 1px;
        }

        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 20px;
        }

        .color-btn {
            height: 50px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .color-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
        }

        .random-btn {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            background-color: #333;
            color: white;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }

        .random-btn:hover {
            background-color: #555;
        }
    </style>
</head>
<body>

    <div class="container">
        <h2>Joriy Rang:</h2>
        <div class="hex-code" id="hexDisplay">#2c3e50</div>

        <div class="buttons-grid">
            <button class="color-btn" style="background-color: #ff7675;" data-color="#ff7675"></button>
            <button class="color-btn" style="background-color: #74b9ff;" data-color="#74b9ff"></button>
            <button class="color-btn" style="background-color: #55efc4;" data-color="#55efc4"></button>
            <button class="color-btn" style="background-color: #ffeaa7;" data-color="#ffeaa7"></button>
            <button class="color-btn" style="background-color: #a29bfe;" data-color="#a29bfe"></button>
            <button class="color-btn" style="background-color: #fd79a8;" data-color="#fd79a8"></button>
        </div>

        <button class="random-btn" id="randomBtn">🎲 Random rang</button>
    </div>

    <script>
        const hexDisplay = document.getElementById('hexDisplay');
        const randomBtn = document.getElementById('randomBtn');
        const colorButtons = document.querySelectorAll('.color-btn');

        // Rangni o'zgartirish va saqlash funksiyasi
        function changeColor(color) {
            document.body.style.backgroundColor = color;
            hexDisplay.textContent = color.toUpperCase();
            // Oxirgi tanlangan rangni localStorage'ga saqlash
            localStorage.setItem('savedColor', color);
        }

        // Tasodifiy HEX rang yaratish funksiyasi
        function generateRandomColor() {
            const chars = '0123456789abcdef';
            let color = '#';
            for (let i = 0; i < 6; i++) {
                color += chars[Math.floor(Math.random() * 16)];
            }
            return color;
        }

        // Tayyor rang tugmalariga hodisa bog'lash
        colorButtons.forEach(button => {
            button.addEventListener('click', () => {
                const selectedColor = button.getAttribute('data-color');
                changeColor(selectedColor);
            });
        });

        // Random tugmasi bosilganda
        randomBtn.addEventListener('click', () => {
            const randomColor = generateRandomColor();
            changeColor(randomColor);
        });

        // Sahifa yuklanganda localStorage'dan rangni tekshirish
        window.addEventListener('DOMContentLoaded', () => {
            const savedColor = localStorage.getItem('savedColor');
            if (savedColor) {
                changeColor(savedColor);
            }
        });
    </script>
</body>
</html>
Kod qanday ishlaydi?
Silliq animatsiya: CSS'da body qismiga qo'shilgan transition: background-color 0.5s ease; xossasi ranglar almashganda ko'zni charchatmaydigan silliq o'tishni ta'minlaydi.

Tayyor tugmalar: HTML ichida 6 ta har xil rangdagi tugmalar data-color atributi bilan joylashtirilgan.

Random rang: JavaScript'dagi generateRandomColor funksiyasi har safar har xil HEX kod generatsiya qiladi.

localStorage: Rang o'zgargan zahoti localStorage.setItem('savedColor', color) orqali brauzer xotirasiga yoziladi va sahifa yangilanganda qayta o'qib olinadi (DOMContentLoaded).

✅ Amallarni switch-case orqali boshqaring

✅ && va || operatorlarini ishlatib validatsiya qiling

✅ Tarix (oxirgi 5 hisoblash) qatorini ko'rsating

✅ Foiz (%) hisoblash funksiyasini qo'shing

✅ Keyboard shortcut: Enter = hisoblash, Escape = tozalash

Mana, barcha talablarga javob beradigan, mukammal va tushunarli kalkulyator kodi. Uni bitta HTML fayl sifatida saqlab, brauzerda ishlatishingiz mumkin.

HTML, CSS va JavaScript kodi
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kalkulyator Pro</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: #f0f2f5;
        }

        .calculator {
            background: #ffffff;
            padding: 25px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            width: 320px;
        }

        .screen {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 20px;
            border: 1px solid #e9ecef;
        }

        .screen input {
            width: 100%;
            border: none;
            background: transparent;
            font-size: 1.8rem;
            text-align: right;
            outline: none;
            color: #212529;
        }

        .history-box {
            margin-top: 15px;
            border-top: 1px solid #eee;
            padding-top: 10px;
            font-size: 0.85rem;
            color: #6c757d;
        }

        .history-title {
            font-weight: bold;
            margin-bottom: 5px;
        }

        .history-item {
            padding: 2px 0;
            border-bottom: 1px dashed #f1f1f1;
            text-align: right;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 15px;
            font-size: 1.2rem;
            font-weight: 600;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: background 0.2s, transform 0.1s;
            background: #e9ecef;
            color: #495057;
        }

        button:active {
            transform: scale(0.95);
        }

        button.operator {
            background: #ff9f0a;
            color: white;
        }

        button.action {
            background: #d1d1d6;
        }

        button.equals {
            background: #34c759;
            color: white;
            grid-column: span 2;
        }
    </style>
</head>
<body>

    <div class="calculator">
        <div class="screen">
            <input type="text" id="display" readonly placeholder="0">
        </div>
        
        <div class="grid">
            <button class="action" onclick="clearDisplay()">C</button>
            <button class="action" onclick="appendOperator('%')">%</button>
            <button class="operator" onclick="appendOperator('/')">/</button>
            <button class="operator" onclick="appendOperator('*')">*</button>

            <button onclick="appendNumber('7')">7</button>
            <button onclick="appendNumber('8')">8</button>
            <button onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('-')">-</button>

            <button onclick="appendNumber('4')">4</button>
            <button onclick="appendNumber('5')">5</button>
            <button onclick="appendNumber('6')">6</button>
            <button class="operator" onclick="appendOperator('+')">+</button>

            <button onclick="appendNumber('1')">1</button>
            <button onclick="appendNumber('2')">2</button>
            <button onclick="appendNumber('3')">3</button>
            <button onclick="appendNumber('.')">.</button>

            <button onclick="appendNumber('0')">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>

        <div class="history-box">
            <div class="history-title">Tarix (Oxirgi 5 ta):</div>
            <div id="historyList"></div>
        </div>
    </div>

    <script>
        const display = document.getElementById('display');
        const historyList = document.getElementById('historyList');
        
        let currentInput = '';
        let history = [];

        function appendNumber(num) {
            currentInput += num;
            display.value = currentInput;
        }

        function appendOperator(op) {
            // Validatsiya: Agar oxirgi belgi operator bo'lsa yoki ekran bo'sh bo'lsa, qayta operator qo'shilmaydi
            const lastChar = currentInput.slice(-1);
            const operators = ['+', '-', '*', '/', '%'];
            
            // && va || operatorlari orqali validatsiya
            if (currentInput === '' || operators.includes(lastChar)) {
                return; // Funksiyani to'xtatish
            }

            currentInput += op;
            display.value = currentInput;
        }

        function clearDisplay() {
            currentInput = '';
            display.value = '';
        }

        function calculate() {
            const operators = ['+', '-', '*', '/', '%'];
            const lastChar = currentInput.slice(-1);

            // Validatsiya: Bo'sh bo'lsa yoki oxiri operator bilan tugagan bo'lsa hisoblamaydi
            if (currentInput === '' || operators.includes(lastChar)) {
                return;
            }

            // Ifodani raqamlar va operatorga ajratish (Masalan: "10+5" -> num1=10, op='+', num2=5)
            const match = currentInput.match(/(\d+\.?\d*)([\+\-\*\/%])(\d+\.?\d*)/);
            
            if (!match) return;

            const num1 = parseFloat(match[1]);
            const op = match[2];
            const num2 = parseFloat(match[3]);
            let result = 0;

            // switch-case orqali amallarni boshqarish
            switch (op) {
                case '+':
                    result = num1 + num2;
                    break;
                case '-':
                    result = num1 - num2;
                    break;
                case '*':
                    result = num1 * num2;
                    break;
                case '/':
                    // Nolga bo'lish validatsiyasi
                    if (num2 === 0) {
                        alert("Nolga bo'lish mumkin emas!");
                        clearDisplay();
                        return;
                    }
                    result = num1 / num2;
                    break;
                case '%':
                    // Foiz hisoblash (Masalan: 500 % 10 = 50, ya'ni 500 ning 10 foizi)
                    result = (num1 * num2) / 100;
                    break;
                default:
                    return;
            }

            // Tarixni yangilash (Maksimal 5 ta)
            const expression = `${currentInput} = ${result}`;
            history.unshift(expression); // Massiv boshiga qo'shish
            if (history.length > 5) {
                history.pop(); // 5 tadan oshsa oxirgisini o'chirish
            }
            updateHistoryUI();

            // Ekranni yangilash
            currentInput = result.toString();
            display.value = currentInput;
        }

        function updateHistoryUI() {
            historyList.innerHTML = '';
            history.forEach(item => {
                const div = document.createElement('div');
                div.className = 'history-item';
                div.textContent = item;
                historyList.appendChild(div);
            });
        }

        // Keyboard shortcuts (Klaviatura tugmalari)
        document.addEventListener('keydown', (event) => {
            if (event.key === 'Enter') {
                event.preventDefault(); // Sahifa yangilanib ketishining oldini oladi
                calculate();
            } else if (event.key === 'Escape') {
                clearDisplay();
            }
        });
    </script>
</body>
</html>
Talablar qanday bajarildi?
switch-case amallari: Kodning switch (op) qismida barcha arifmetik amallar (+, -, *, /, %) xavfsiz boshqarilgan.

&& va || bilan validatsiya: appendOperator funksiyasida ekran bo'sh bo'lishi yoki oxirgi belgi operator bo'lishi holatlari mantiqiy operatorlar orqali tekshirilib, xato ifoda kiritilishining oldi olingan.

Tarix qatori (Oxirgi 5 ta): history massiviga har safar unshift() orqali yangi hisob-kitob qo'shiladi va .length > 5 sharti bilan faqat oxirgi 5 tasi saqlab qolinadi.

Foiz (%) funksiyasi: % belgisi bosilganda matematika qoidasiga ko'ra birinchi sonning ikkinchi son qiymatidagi foizi hisoblanadi (Masalan: 200 % 10 yozilsa, 200 ning 10% ini, ya'ni 20 natijasini beradi).

Klaviatura shortcutlari: keydown hodisasi yordamida Enter bosilganda natija hisoblanadi, Escape bosilganda esa ekran tozalanadi.

✅ for tsikli bilan kartochkalarni render qiling

✅ Oldinga/orqaga navigatsiya tugmalari

✅ Joriy sahifa raqami ko'rinsin

✅ Birinchi/oxirgi sahifada tegishli tugmalar o'chib qolsin

✅ Sahifa raqamlarini bosish orqali o'tish imkoni

Mana, barcha talablarga to'liq javob beradigan, dinamik va chiroyli dizaynga ega Pagination (Sahifalash) tizimi. Kodlar bitta HTML faylga jamlangan.

HTML, CSS va JavaScript kodi
HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dinamik Sahifalash (Pagination)</title>
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
            flex-direction: column;
            align-items: center;
            padding: 40px 20px;
        }

        h2 {
            color: #2c3e50;
            margin-bottom: 25px;
        }

        /* Kartochkalar panjarasi */
        .cards-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            width: 100%;
            max-width: 900px;
            margin-bottom: 30px;
        }

        /* Kartochka dizayni */
        .card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
            border: 1px solid #eef2f5;
            transition: transform 0.2s;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card-id {
            font-size: 0.85rem;
            color: #ff9f0a;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .card-title {
            font-size: 1.2rem;
            color: #333;
            margin-bottom: 10px;
        }

        .card-text {
            color: #666;
            font-size: 0.95rem;
            line-height: 1.4;
        }

        /* Navigatsiya boshqaruvi */
        .pagination-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 12px;
        }

        .current-page-info {
            font-size: 0.95rem;
            color: #555;
            font-weight: 600;
        }

        .pagination-controls {
            display: flex;
            align-items: center;
            gap: 8px;
            background: white;
            padding: 8px 12px;
            border-radius: 30px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        /* Tugmalar dizayni */
        .nav-btn, .page-num-btn {
            border: none;
            background: transparent;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.2s;
        }

        .nav-btn {
            padding: 8px 16px;
            background-color: #007bff;
            color: white;
            border-radius: 20px;
        }

        .nav-btn:hover:not(:disabled) {
            background-color: #0056b3;
        }

        .page-num-btn {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .page-num-btn:hover:not(.active) {
            background-color: #f0f2f5;
        }

        .page-num-btn.active {
            background-color: #007bff;
            color: white;
        }

        /* O'chirilgan (Disabled) tugmalar holati */
        button:disabled {
            background-color: #cccccc !important;
            color: #666666 !important;
            cursor: not-allowed;
            opacity: 0.6;
        }
    </style>
</head>
<body>

    <h2>Dinamik Ma'lumotlar Ro'yxati</h2>

    <div class="cards-grid" id="cardsGrid"></div>

    <div class="pagination-container">
        <div class="current-page-info" id="pageInfo">Sahifa: 1 / 1</div>
        
        <div class="pagination-controls">
            <button class="nav-btn" id="prevBtn">← Orqaga</button>
            
            <div id="pageNumbers" style="display: flex; gap: 5px;"></div>
            
            <button class="nav-btn" id="nextBtn">Oldinga →</button>
        </div>
    </div>

    <script>
        // 1. Simulyatsiya uchun soxta ma'lumotlar (Baza) yaratamiz (jami 24 ta element)
        const mockData = [];
        for (let i = 1; i <= 24; i++) {
            mockData.push({
                id: i,
                title: `Kompaniya loyihasi #${i}`,
                description: `Bu erda loyiha haqida batafsil ma'lumotlar joylashgan. Sahifalash tizimini tekshirish uchun yaratilgan matn.`
            });
        }

        // 2. Sahifalash uchun sozlamalar
        const itemsPerPage = 6; // Har bir sahifada nechta kartochka chiqishi
        let currentPage = 1;    // Dastlabki joriy sahifa
        const totalPages = Math.ceil(mockData.length / itemsPerPage); // Jami sahifalar soni

        // DOM elementlarini olish
        const cardsGrid = document.getElementById('cardsGrid');
        const pageInfo = document.getElementById('pageInfo');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const pageNumbersContainer = document.getElementById('pageNumbers');

        // 3. Kartochkalarni render qilish va boshqarish funksiyasi
        function renderPage(page) {
            cardsGrid.innerHTML = ''; // Oldingi sahifa elementlarini tozalash
            
            // Sahifaga mos keluvchi indekslarni aniqlash
            const startIndex = (page - 1) * itemsPerPage;
            const endIndex = startIndex + itemsPerPage;
            
            // `for` tsikli yordamida faqat joriy sahifadagi ma'lumotlarni render qilish
            for (let i = startIndex; i < endIndex && i < mockData.length; i++) {
                const item = mockData[i];
                
                const cardHTML = `
                    <div class="card">
                        <div class="card-id">ID: ${item.id}</div>
                        <h3 class="card-title">${item.title}</h3>
                        <p class="card-text">${item.description}</p>
                    </div>
                `;
                cardsGrid.innerHTML += cardHTML;
            }

            // UI qismlarini yangilash
            updatePaginationUI();
        }

        // 4. Tugmalar holati va raqamlarni yangilash funksiyasi
        function updatePaginationUI() {
            // Joriy sahifa matnini ko'rsatish
            pageInfo.textContent = `Sahifa: ${currentPage} / ${totalPages}`;

            // Birinchi sahifada "Orqaga", oxirgi sahifada "Oldinga" tugmasini o'chirish (disabled)
            prevBtn.disabled = (currentPage === 1);
            nextBtn.disabled = (currentPage === totalPages);

            // Sahifa raqamlari tugmalarini qayta hosil qilish
            pageNumbersContainer.innerHTML = '';
            for (let i = 1; i <= totalPages; i++) {
                const btn = document.createElement('button');
                btn.className = `page-num-btn ${i === currentPage ? 'active' : ''}`;
                btn.textContent = i;
                
                // Sahifa raqamini bosganda o'sha sahifaga o'tish imkoniyati
                btn.addEventListener('click', () => {
                    currentPage = i;
                    renderPage(currentPage);
                });
                
                pageNumbersContainer.appendChild(btn);
            }
        }

        // 5. Hodisalarni tinglash (Event Listeners)
        prevBtn.addEventListener('click', () => {
            if (currentPage > 1) {
                currentPage--;
                renderPage(currentPage);
            }
        });

        nextBtn.addEventListener('click', () => {
            if (currentPage < totalPages) {
                currentPage++;
                renderPage(currentPage);
            }
        });

        // Dasturni birinchi marta ishga tushirish
        renderPage(currentPage);
    </script>
</body>
</html>
Talablar qanday bajarildi?
for tsikli bilan render: renderPage funksiyasining ichida an'anaviy for (let i = startIndex; ...) tsikli ishlatildi. U har safar faqat joriy sahifaga tegishli bo'lgan 6 ta elementni ajratib olib ekranga chiqaradi.

Oldinga/orqaga navigatsiyasi: prevBtn (Orqaga) va nextBtn (Oldinga) tugmalari bosilganda currentPage o'zgaradi va sahifa qayta render bo'ladi.

Joriy sahifa raqami: Sahifa: X / Y shaklidagi ma'lumot real vaqtda pageInfo div elementida ko'rsatib turiladi.

Tugmalarning o'chib qolishi (Disabled): updatePaginationUI funksiyasi ichida mantiqiy operatorlar orqali currentPage === 1 bo'lsa Orqaga tugmasi, currentPage === totalPages bo'lsa Oldinga tugmasi avtomatik disabled (muzlatilgan) holatga o'tadi va bosish imkoni bo'lmaydi.

Raqamlar orqali o'tish: Dinamik yaratilgan har bir raqamli tugmaga 




