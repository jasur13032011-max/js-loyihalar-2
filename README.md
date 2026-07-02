# js-loyihalar-2
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

Klaviatura shortcutlari: keydown hodisasi yordamida Enter bosilganda natija hisoblanadi, Escape
