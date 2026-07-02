# js-loyihalar-2
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
