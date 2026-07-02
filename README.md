# js-loyihalar-2
Rasmlar sifatli chiqishi uchun Unsplash servisining tayyor rasmlaridan foydalanildi.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Foto Galereya</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #111;
            color: #fff;
            padding: 40px 20px;
        }

        h1 {
            text-align: center;
            margin-bottom: 40px;
            font-weight: 300;
            letter-spacing: 2px;
        }

        /* Galereya Setkasi */
        .gallery-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .gallery-item {
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            cursor: pointer;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            height: 200px;
        }

        .gallery-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }

        .gallery-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(255,255,255,0.2);
        }

        .gallery-item:hover img {
            transform: scale(1.08);
        }

        /* Overlay (Lightbox) */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.92);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            
            /* Silliq fade-in/out uchun */
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s ease;
        }

        .overlay.active {
            opacity: 1;
            pointer-events: auto;
        }

        .overlay-content {
            position: relative;
            max-width: 85%;
            max-height: 85%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .overlay-img {
            max-width: 100%;
            max-height: 85vh;
            border-radius: 4px;
            box-shadow: 0 0 30px rgba(255,255,255,0.1);
            
            /* Rasm almashgandagi silliq effekt */
            transition: transform 0.3s ease, opacity 0.3s ease;
        }

        /* Navigatsiya tugmalari */
        .close-btn {
            position: absolute;
            top: -40px;
            right: 0;
            font-size: 35px;
            color: #fff;
            cursor: pointer;
            transition: color 0.2s;
        }

        .nav-btn {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            background: rgba(255, 255, 255, 0.1);
            color: white;
            border: none;
            font-size: 24px;
            padding: 15px 20px;
            cursor: pointer;
            border-radius: 50%;
            transition: background 0.3s, color 0.3s;
            user-select: none;
        }

        .nav-btn:hover {
            background: rgba(255, 255, 255, 0.9);
            color: #000;
        }

        .prev-btn { left: -80px; }
        .next-btn { right: -80px; }

        /* Mobil qurilmalar uchun tugmalarni moslashtirish */
        @media (max-width: 768px) {
            .prev-btn { left: 10px; background: rgba(0,0,0,0.5); }
            .next-btn { right: 10px; background: rgba(0,0,0,0.5); }
            .close-btn { top: 15px; right: 20px; position: fixed; z-index: 1001; }
        }
    </style>
</head>
<body>

    <h1>🌌 Go'zal Tabiat Galereyasi</h1>

    <div class="gallery-grid">
        <div class="gallery-item" data-index="0"><img src="https://images.unsplash.com/photo-1469474968028-56623f02e42e?w=800" alt="Tog'lar"></div>
        <div class="gallery-item" data-index="1"><img src="https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?w=800" alt="Tuman"></div>
        <div class="gallery-item" data-index="2"><img src="https://images.unsplash.com/photo-1501854140801-50d01698950b?w=800" alt="Yashillik"></div>
        <div class="gallery-item" data-index="3"><img src="https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?w=800" alt="O'rmon"></div>
        <div class="gallery-item" data-index="4"><img src="https://images.unsplash.com/photo-1472214222541-d510753a8707?w=800" alt="Vodiy"></div>
        <div class="gallery-item" data-index="5"><img src="https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=800" alt="Quyosh nuri"></div>
    </div>

    <div class="overlay" id="lightbox">
        <div class="overlay-content">
            <span class="close-btn" id="closeBtn">&times;</span>
            <button class="nav-btn prev-btn" id="prevBtn">&#10094;</button>
            <img src="" alt="Katta rasm" class="overlay-img" id="lightboxImg">
            <button class="nav-btn next-btn" id="nextBtn">&#10095;</button>
        </div>
    </div>

<script>
    const galleryItems = document.querySelectorAll('.gallery-item');
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightboxImg');
    const closeBtn = document.getElementById('closeBtn');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');

    let currentIndex = 0;

    // Rasmlar ro'yxatini massiv qilib olish
    const imagesSrc = Array.from(galleryItems).map(item => item.querySelector('img').src);

    // Lightboxni ochish funksiyasi
    function openLightbox(index) {
        currentIndex = index;
        lightboxImg.src = imagesSrc[currentIndex];
        lightbox.classList.add('active');
        document.body.style.overflow = 'hidden'; // Orqa fon skroll bo'lmasligi uchun
    }

    // Lightboxni yopish funksiyasi
    function closeLightbox() {
        lightbox.classList.remove('active');
        document.body.style.overflow = 'auto';
    }

    // Keyingi rasmga o'tish
    function nextImage() {
        currentIndex = (currentIndex + 1) % imagesSrc.length; // Oxiriga kelsa boshiga qaytadi
        updateImage();
    }

    // Oldingi rasmga o'tish
    function prevImage() {
        currentIndex = (currentIndex - 1 + imagesSrc.length) % imagesSrc.length; // Boshida tursa oxiriga o'tadi
        updateImage();
    }

    // Rasmni silliq almashtirish effekti
    function updateImage() {
        lightboxImg.style.opacity = '0';
        lightboxImg.style.transform = 'scale(0.95)';
        
        setTimeout(() => {
            lightboxImg.src = imagesSrc[currentIndex];
            lightboxImg.style.opacity = '1';
            lightboxImg.style.transform = 'scale(1)';
        }, 150);
    }

    // Event Listeners (Hodisalarni tinglash)
    galleryItems.forEach((item, index) => {
        item.addEventListener('click', () => openLightbox(index));
    });

    closeBtn.addEventListener('click', closeLightbox);
    nextBtn.addEventListener('click', nextImage);
    prevBtn.addEventListener('click', prevImage);

    // Overlayning bo'sh joyi bosilganda ham yopilsin
    lightbox.addEventListener('click', (e) => {
        if (e.target === lightbox) closeLightbox();
    });

    // Klaviatura navigatsiyasi (←  →  Escape)
    document.addEventListener('keydown', (e) => {
        if (!lightbox.classList.contains('active')) return; // Faqat overlay ochiqligida ishlaydi

        if (e.key === 'Escape') {
            closeLightbox();
        } else if (e.key === 'ArrowRight') {
            nextImage();
        } else if (e.key === 'ArrowLeft') {
            prevImage();
        }
    });
</script>

</body>
</html>
Belgilangan funksiyalar qanday bajarildi?
6 ta rasm: Unsplash'dan olingan 6 ta tayyor rasm gallery-grid ichiga joylandi.

Overlay ochilishi: Har bir rasm bosilganda lightbox elementiga .active klassi qo'shiladi va katta rasm yuklanadi.

Oldinga/orqaga navigatsiya: &#10094; va &#10095; belgilari yordamida tugmalar yaratildi va cheksiz aylanish tsikli (% operatori orqali) o'rnatildi.

Yopilish: X tugmasi, Escape tugmasi yoki rasmning tashqarisidagi qora maydon bosilganda overlay yopiladi.

CSS Animatsiya: .overlay klassida transition: opacity 0.4s ease orqali silliq paydo bo'lish (fade-in) va yo'qolish (fade-out) ta'minlandi.

Keyboard Navigatsiya: JS'dagi keydown hodisasi orqali ArrowRight (o'ngga), ArrowLeft (chapga) va
