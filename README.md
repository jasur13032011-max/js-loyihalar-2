# js-loyihalar-2
Mana, siz aytgan barcha funksiyalarga ega (film qo'shish, o'chirish, localStorage bilan ishlash, real vaqtda qidirish va janr bo'yicha filtrlash) to'liq HTML, CSS va JavaScript kodi.

Barcha kodlarni bitta faylga (masalan, index.html deb nomlab) saqlab, brauzerda ochishingiz mumkin.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kinolar Olami</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f7f6;
            color: #333;
            padding: 20px;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
        }

        h1 {
            text-align: center;
            margin-bottom: 30px;
            color: #2c3e50;
        }

        /* Forma va boshqaruv bo'limi */
        .controls-section {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 40px;
        }

        @media (max-width: 768px) {
            .controls-section {
                grid-template-columns: 1fr;
            }
        }

        .card-form, .filter-box {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }

        .form-group input, .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
        }

        button {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            font-weight: bold;
            transition: background 0.3s;
        }

        button:hover {
            background-color: #27ae60;
        }

        /* Filmlar kartochkalari */
        .movies-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
        }

        .movie-card {
            background: white;
            border-radius: 8px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            border-top: 5px solid #3498db;
        }

        .movie-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
            color: #2c3e50;
        }

        .movie-info {
            font-size: 14px;
            color: #7f8c8d;
            margin-bottom: 5px;
        }

        .movie-rating {
            display: inline-block;
            background: #f1c40f;
            color: #fff;
            padding: 2px 8px;
            border-radius: 4px;
            font-weight: bold;
            font-size: 12px;
            margin-top: 10px;
            align-self: flex-start;
        }

        .delete-btn {
            background-color: #e74c3c;
            margin-top: 15px;
            padding: 8px;
            font-size: 14px;
        }

        .delete-btn:hover {
            background-color: #c0392b;
        }

        .no-movies {
            grid-column: 1 / -1;
            text-align: center;
            color: #7f8c8d;
            font-size: 18px;
            padding: 40px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🎬 Kinolar Katalogi</h1>

    <div class="controls-section">
        <div class="card-form">
            <h3>Yangi film qo'shish</h3>
            <form id="movieForm" style="margin-top: 15px;">
                <div class="form-group">
                    <label>Film nomi</label>
                    <input type="text" id="title" required placeholder="Masalan: Interstellar">
                </div>
                <div class="form-group">
                    <label>Janri</label>
                    <select id="genre" required>
                        <option value="">Janrni tanlang</option>
                        <option value="Fantastika">Fantastika</option>
                        <option value="Boeyevik">Boeyevik</option>
                        <option value="Drama">Drama</option>
                        <option value="Komediya">Komediya</option>
                        <option value="Daxshat">Daxshat</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Yili</label>
                    <input type="number" id="year" min="1800" max="2026" required placeholder="Masalan: 2014">
                </div>
                <div class="form-group">
                    <label>Reyting (IMDb)</label>
                    <input type="number" id="rating" min="0" max="10" step="0.1" required placeholder="Masalan: 8.6">
                </div>
                <button type="submit">Qo'shish</button>
            </form>
        </div>

        <div class="filter-box">
            <h3>Qidiruv va Filtrlash</h3>
            <div class="form-group" style="margin-top: 15px;">
                <label>Nomi bo'yicha qidirish (Real vaqtda)</label>
                <input type="text" id="searchInp" placeholder="Film nomini yozing...">
            </div>
            <div class="form-group">
                <label>Janr bo'yicha saralash</label>
                <select id="filterGenre">
                    <option value="all">Barcha janrlar</option>
                    <option value="Fantastika">Fantastika</option>
                    <option value="Boeyevik">Boeyevik</option>
                    <option value="Drama">Drama</option>
                    <option value="Komediya">Komediya</option>
                    <option value="Daxshat">Daxshat</option>
                </select>
            </div>
        </div>
    </div>

    <div class="movies-grid" id="moviesGrid"></div>
</div>

<script>
    // LocalStorage'dan ma'lumotlarni olish yoki bo'sh massiv yaratish
    let movies = JSON.parse(localStorage.getItem('movies')) || [];

    const movieForm = document.getElementById('movieForm');
    const moviesGrid = document.getElementById('moviesGrid');
    const searchInp = document.getElementById('searchInp');
    const filterGenre = document.getElementById('filterGenre');

    // Filmlarni ekranga chiqarish funksiyasi
    function displayMovies(moviesToRender) {
        moviesGrid.innerHTML = '';

        if (moviesToRender.length === 0) {
            moviesGrid.innerHTML = '<div class="no-movies">Filmlar topilmadi yoki hali qo\'shilmagan.</div>';
            return;
        }

        moviesToRender.forEach(movie => {
            const card = document.createElement('div');
            card.className = 'movie-card';
            card.innerHTML = `
                <div>
                    <div class="movie-title">${movie.title}</div>
                    <div class="movie-info"><strong>Janr:</strong> ${movie.genre}</div>
                    <div class="movie-info"><strong>Yil:</strong> ${movie.year}</div>
                    <div class="movie-rating">★ ${movie.rating}</div>
                </div>
                <button class="delete-btn" onclick="deleteMovie(${movie.id})">O'chirish</button>
            `;
            moviesGrid.appendChild(card);
        });
    }

    // Yangi film qo'shish
    movieForm.addEventListener('submit', (e) => {
        e.preventDefault();

        const newMovie = {
            id: Date.now(), // Noyob ID yaratish uchun vaqtdan foydalanamiz
            title: document.getElementById('title').value,
            genre: document.getElementById('genre').value,
            year: document.getElementById('year').value,
            rating: document.getElementById('rating').value
        };

        movies.push(newMovie);
        saveAndRender();
        movieForm.reset(); // Formani tozalash
    });

    // Filmni o'chirish
    function deleteMovie(id) {
        movies = movies.filter(movie => movie.id !== id);
        saveAndRender();
    }

    // Ma'lumotni saqlash va yangilash
    function saveAndRender() {
        localStorage.setItem('movies', JSON.stringify(movies));
        filterAndSearchMovies();
    }

    // Real vaqtda Qidiruv va Filtrlash funksiyasi
    function filterAndSearchMovies() {
        const searchText = searchInp.value.toLowerCase();
        const selectedGenre = filterGenre.value;

        const filtered = movies.filter(movie => {
            const matchesSearch = movie.title.toLowerCase().includes(searchText);
            const matchesGenre = selectedGenre === 'all' || movie.genre === selectedGenre;
            return matchesSearch && matchesGenre;
        });

        displayMovies(filtered);
    }

    // Hodisalarni tinglash (Qidiruv va Filtr uchun)
    searchInp.addEventListener('input', filterAndSearchMovies);
    filterGenre.addEventListener('change', filterAndSearchMovies);

    // Dastur ilk bor yuklanganda filmlarni ko'rsatish
    displayMovies(movies);
</script>

</body>
</html>
Kod qanday ishlaydi?
Kartochka ko'rinishi: CSS Grid yordamida filmlar chiroyli va moslashuvchan (responsive) kartochka shaklida chiqadi.

LocalStorage: Har safar film qo'shilganda yoki o'chirilganda localStorage.setItem() ishlaydi. Sahifa yangilansa ham ma'lumotlar o'chib ketmaydi.

Real vaqtda qidiruv: Qidiruv maydoniga yozishingiz bilan input hodisasi (event) ishga tushadi va har bir harfda ro'yxatni saralab boradi.

Filtrlash: Janrni o'zgartirganingizda qidiruv matni bilan birgalikda ishlaydigan mukammal kombinatsiyalangan filtr yaratilgan.
