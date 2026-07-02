# js-loyihalar-2
Mana, siz aytgan barcha shartlarga javob beradigan — 5 ta slayd, navigatsiya tugmalari, pastki nuqtalar (dots), 3 soniyalik avtomatik o'tish (va foydalanuvchi aralashganda to'xtash) funksiyasiga ega, silliq CSS animatsiyali Karusel (Slider) kodi.

Buni bitta faylga (masalan, slider.html) saqlab, brauzerda ishlatishingiz mumkin:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Silliq Karusel Slider</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f0f2f5;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        h1 {
            margin-bottom: 20px;
            color: #333;
        }

        /* Slider asosiy konteyneri */
        .slider-container {
            position: relative;
            width: 800px;
            max-width: 95%;
            height: 450px;
            overflow: hidden;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.15);
        }

        /* Slaydlar liniyasi */
        .slider-wrapper {
            display: flex;
            width: 100%;
            height: 100%;
            /* Silliq o'tish effekti */
            transition: transform 0.6s cubic-bezier(0.25, 1, 0.5, 1);
        }

        /* Har bir slayd */
        .slide {
            min-width: 100%;
            height: 100%;
            position: relative;
        }

        .slide img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Slayd ustidagi matn */
        .slide-caption {
            position: absolute;
            bottom: 40px;
            left: 40px;
            color: white;
            background: rgba(0, 0, 0, 0.6);
            padding: 15px 25px;
            border-radius: 8px;
            backdrop-filter: blur(5px);
        }

        .slide-caption h3 {
            font-size: 24px;
            margin-bottom: 5px;
        }

        /* Oldinga/orqaga tugmalari */
        .nav-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 255, 255, 0.3);
            color: white;
            border: none;
            font-size: 24px;
            padding: 15px 20px;
            cursor: pointer;
            border-radius: 50%;
            transition: background 0.3s, transform 0.2s;
            backdrop-filter: blur(2px);
            user-select: none;
            z-index: 10;
        }

        .nav-btn:hover {
            background: rgba(255, 255, 255, 0.8);
            color: #333;
        }

        .nav-btn:active {
            transform: translateY(-50%) scale(0.95);
        }

        .prev-btn { left: 20px; }
        .next-btn { right: 20px; }

        /* Pastki nuqtalar (Dots) */
        .dots-container {
            display: flex;
            gap: 10px;
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 10;
        }

        .dot {
            width: 12px;
            height: 12px;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            cursor: pointer;
            transition: background 0.3s, transform 0.3s, border-radius 0.3s;
        }

        .dot.active {
            background: #fff;
            transform: scale(1.2);
            width: 30px; /* Aktiv nuqtani cho'zish effekti */
            border-radius: 6px;
        }
    </style>
</head>
<body>

    <h1>Interaktiv Foto Slider</h1>

    <div class="slider-container">
        <div class="slider-wrapper" id="sliderWrapper">
            <div class="slide">
                <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?w=1000" alt="Slayd 1">
                <div class="slide-caption">
                    <h3>Yosemite vodiysi</h3>
                    <p>Tabiatning eng go'zal mo'jizalaridan biri.</p>
                </div>
            </div>
            <div class="slide">
                <img src="https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=1000" alt="Slayd 2">
                <div class="slide-caption">
                    <h3>Yashil tepaliklar</h3>
                    <p>Ertalabki tuman va toza havo.</p>
                </div>
            </div>
            <div class="slide">
                <img src="https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?w=1000" alt="Slayd 3">
                <div class="slide-caption">
                    <h3>Sirlardan to'la o'rmon</h3>
                    <p>Asriy daraxtlar orasidagi silliq yo'lak.</p>
                </div>
            </div>
            <div class="slide">
                <img src="https://images.unsplash.com/photo-1472214222541-d510753a8707?w=1000" alt="Slayd 4">
                <div class="slide-caption">
                    <h3>Tinch vodiy</h3>
                    <p>Quyosh botayotgan paytdagi go'zallik.</p>
                </div>
            </div>
            <div class="slide">
                <img src="https://images.unsplash.com/photo-1513836279014-a89f7a76ae86?w=1000" alt="Slayd 5">
                <div class="slide-caption">
                    <h3>Katta o'rmonlar</h3>
                    <p>Tabiat bilan uyg'unlik hissi.</p>
                </div>
            </div>
        </div>

        <button class="nav-btn prev-btn" id="prevBtn">&#10094;</button>
        <button class="nav-btn next-btn" id="nextBtn">&#10095;</button>

        <div class="dots-container" id="dotsContainer"></div>
    </div>

<script>
    const wrapper = document.getElementById('sliderWrapper');
    const slides = document.querySelectorAll('.slide');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const dotsContainer = document.getElementById('dotsContainer');

    let currentIndex = 0;
    const totalSlides = slides.length;
    let autoPlayInterval;

    // 1. Dinamik ravishda nuqtalarni (dots) yaratish
    function createDots() {
        for (let i = 0; i < totalSlides; i++) {
            const dot = document.createElement('div');
            dot.classList.add('dot');
            if (i === 0) dot.classList.add('active');
            dot.addEventListener('click', () => {
                stopAutoPlay(); // Foydalanuvchi bosganda avtomatik o'tish to'xtaydi
                goToSlide(i);
            });
            dotsContainer.appendChild(dot);
        }
    }

    // 2. Slayderni kerakli indeksga surish funksiyasi
    function goToSlide(index) {
        currentIndex = index;
        
        // Agar oxiridan o'tib ketsa boshiga qaytaradi
        if (currentIndex >= totalSlides) currentIndex = 0;
        // Agar birinchisidan orqaga o'tsa oxiriga qaytaradi
        if (currentIndex < 0) currentIndex = totalSlides - 1;

        // CSS transform orqali surish
        wrapper.style.transform = `translateX(-${currentIndex * 100}%)`;
        
        updateDots();
    }

    // 3. Aktiv nuqtani yangilash
    function updateDots() {
        const dots = document.querySelectorAll('.dot');
        dots.forEach((dot, index) => {
            if (index === currentIndex) {
                dot.classList.add('active');
            } else {
                dot.classList.remove('active');
            }
        });
    }

    // 4. Avtomatik o'tishni boshlash (Har 3 soniyada)
    function startAutoPlay() {
        autoPlayInterval = setInterval(() => {
            goToSlide(currentIndex + 1);
        }, 3000);
    }

    // 5. Avtomatik o'tishni to'xtatish (Foydalanuvchi aralashganda)
    function stopAutoPlay() {
        clearInterval(autoPlayInterval);
    }

    // Tugmalar hodisalari
    nextBtn.addEventListener('click', () => {
        stopAutoPlay(); // Foydalanuvchi bosganda to'xtatadi
        goToSlide(currentIndex + 1);
    });

    prevBtn.addEventListener('click', () => {
        stopAutoPlay(); // Foydalanuvchi bosganda to'xtatadi
        goToSlide(currentIndex - 1);
    });

    // Ishga tushirish
    createDots();
    startAutoPlay();
</script>

</body>
</html>
Kod qanday shartlarni bajardi?
5 ta slayd: Unsplash platformasidan olingan 5 ta yuqori sifatli rasm va ularning har biriga mos sarlavhalar (slide-caption) joylashtirildi.

Navigatsiya tugmalari: Chap va o'ng tomonda joylashgan orqaga (&#10094;) va oldinga (&#10095;) tugmalari slaydlarni ketma-ket almashtiradi.

Pastki nuqtalar (Dots): Slaydlar soniga qarab JS orqali dinamik nuqtalar yaratiladi. Har bir nuqta bosilganda mos slaydga to'g'ridan-to'g'ri o'tiladi.

Har 3 soniyada avtomatik o'tish: setInterval yordamida dastur yoqilishi bilan har 3000 millisekundda keyingi slaydga o'tish mexanizmi ishlaydi.

Foydalanuvchi bosganda avtomatik to'xtash: Agar foydalanuvchi oldinga/orqaga tugmalarini yoki pastki nuqtalardan birini bossa, stopAutoPlay() funksiyasi ishga tushadi va clearInterval yordamida avtomatik o'tish butunlay to'xtatiladi.

Silliq CSS transition: .slider-wrapper klassidagi transition: transform 0.6s cubic-bezier(...)
