# js-loyihalar-2
Mana, siz aytgan barcha shartlarga javob beradigan — chiroyli dizayn, silliq ishlash xususiyati, progress bar, ovoz boshqaruvi hamda Shuffle (tasodifiy) va Repeat (takrorlash) rejimlari mavjud to'liq Musiqa Pleyeri kodi.

Rasmlar va audio fayllar xizmat ko'rsatuvchi ochiq internet manbalaridan (bepul musiqalar) olindi, shuning uchun kodni saqlab brauzerda ochishingiz bilan pleyer to'liq ishlaydi. Kodni bitta faylga (masalan, player.html deb) saqlang:

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interaktiv Musiqa Pleyeri</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #fff;
        }

        .player-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(15px);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.3);
            width: 380px;
            text-align: center;
            border: 1px solid rgba(255,255,255,0.1);
        }

        /* Qo'shiq rasmi */
        .img-area {
            width: 200px;
            height: 200px;
            margin: 0 auto 25px auto;
            border-radius: 50%;
            overflow: hidden;
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
            border: 4px solid rgba(255,255,255,0.2);
            transition: transform 0.5s ease;
        }

        .img-area img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Qo'shiq aylanayotganda animatsiya */
        .playing .img-area {
            animation: rotateAlbum 12s linear infinite;
        }

        @keyframes rotateAlbum {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Qo'shiq ma'lumotlari */
        .song-details {
            margin-bottom: 25px;
        }

        .song-details .name {
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .song-details .artist {
            font-size: 14px;
            color: #ccc;
        }

        /* Progress Bar */
        .progress-area {
            height: 6px;
            width: 100%;
            background: rgba(255,255,255,0.2);
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 8px;
            position: relative;
        }

        .progress-bar {
            height: 100%;
            width: 0%;
            background: #1db954; /* Spotify yashil rangi */
            border-radius: 5px;
            position: relative;
            transition: width 0.1s linear;
        }

        .timer {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: #ddd;
            margin-bottom: 25px;
        }

        /* Boshqaruv tugmalari */
        .controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }

        .controls button {
            background: none;
            border: none;
            color: #fff;
            cursor: pointer;
            font-size: 20px;
            transition: color 0.2s, transform 0.1s;
        }

        .controls button:hover { color: #1db954; }
        .controls button:active { transform: scale(0.95); }

        .controls .play-pause {
            background: #fff;
            color: #333;
            width: 55px;
            height: 55px;
            border-radius: 50%;
            font-size: 22px;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }
        .controls .play-pause:hover { background: #1db954; color: #fff; }

        /* Aktiv rejimlar rangi */
        .controls button.active-mode {
            color: #1db954;
            text-shadow: 0 0 8px rgba(29, 185, 84, 0.6);
        }

        /* Ovoz boshqaruvi (Volume) */
        .volume-area {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            font-size: 14px;
        }

        .volume-area input {
            cursor: pointer;
            accent-color: #1db954;
            width: 100px;
        }
    </style>
</head>
<body>

<div class="player-container" id="playerContainer">
    <div class="img-area">
        <img id="trackImage" src="" alt="Albom rasmi">
    </div>

    <div class="song-details">
        <p class="name" id="trackName">Yuklanmoqda...</p>
        <p class="artist" id="trackArtist">...</p>
    </div>

    <div class="progress-area" id="progressArea">
        <div class="progress-bar" id="progressBar"></div>
    </div>
    <div class="timer">
        <span id="currentTime">0:00</span>
        <span id="duration">0:00</span>
    </div>

    <div class="controls">
        <button id="repeatBtn" title="Takrorlash">🔁</button>
        <button id="prevBtn" style="font-size: 26px;">⏮</button>
        <button id="playPauseBtn" class="play-pause">▶</button>
        <button id="nextBtn" style="font-size: 26px;">⏭</button>
        <button id="shuffleBtn" title="Tasodifiy tartib">🔀</button>
    </div>

    <div class="volume-area">
        <span>🔈</span>
        <input type="range" id="volumeSlider" min="0" max="1" step="0.05" value="0.7">
        <span>🔊</span>
    </div>
</div>

<audio id="mainAudio" src=""></audio>

<script>
    // Qo'shiqlar bazasi
    const playlist = [
        {
            name: "Lost in the City Lights",
            artist: "Cosmo Sheldrake",
            img: "https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=400",
            src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3"
        },
        {
            name: "Forest Echoes",
            artist: "Nature Ambient",
            img: "https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=400",
            src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3"
        },
        {
            name: "Midnight Drive",
            artist: "Lofi Beats",
            img: "https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=400",
            src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3"
        },
        {
            name: "Summer Chill",
            artist: "Tropical Vibe",
            img: "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=400",
            src: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-6.mp3"
        }
    ];

    let trackIndex = 0;
    let isPlaying = false;
    let isRepeat = false;
    let isShuffle = false;

    const playerContainer = document.getElementById('playerContainer');
    const mainAudio = document.getElementById('mainAudio');
    const trackImage = document.getElementById('trackImage');
    const trackName = document.getElementById('trackName');
    const trackArtist = document.getElementById('trackArtist');
    
    const playPauseBtn = document.getElementById('playPauseBtn');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const repeatBtn = document.getElementById('repeatBtn');
    const shuffleBtn = document.getElementById('shuffleBtn');
    
    const progressArea = document.getElementById('progressArea');
    const progressBar = document.getElementById('progressBar');
    const currentTimeText = document.getElementById('currentTime');
    const durationText = document.getElementById('duration');
    const volumeSlider = document.getElementById('volumeSlider');

    // Dastlabki yuklash
    window.addEventListener('DOMContentLoaded', () => {
        loadTrack(trackIndex);
    });

    // Qo'shiqni yuklash funksiyasi
    function loadTrack(index) {
        const track = playlist[index];
        trackName.innerText = track.name;
        trackArtist.innerText = track.artist;
        trackImage.src = track.img;
        mainAudio.src = track.src;
    }

    // Play / Pause boshqaruvi
    function togglePlay() {
        if (isPlaying) {
            pauseTrack();
        } else {
            playTrack();
        }
    }

    function playTrack() {
        isPlaying = true;
        playPauseBtn.innerText = "⏸";
        playerContainer.classList.add('playing');
        mainAudio.play();
    }

    function pauseTrack() {
        isPlaying = false;
        playPauseBtn.innerText = "▶";
        playerContainer.classList.remove('playing');
        mainAudio.pause();
    }

    // Keyingi va oldingi qo'shiqqa o'tish
    function nextTrack() {
        if (isShuffle) {
            // Tasodifiy rejim yoqilgan bo'lsa
            let randomIndex;
            do {
                randomIndex = Math.floor(Math.random() * playlist.length);
            } while (randomIndex === trackIndex);
            trackIndex = randomIndex;
        } else {
            // Oddiy tartibda o'tish
            trackIndex = (trackIndex + 1) % playlist.length;
        }
        loadTrack(trackIndex);
        playTrack();
    }

    function prevTrack() {
        trackIndex = (trackIndex - 1 + playlist.length) % playlist.length;
        loadTrack(trackIndex);
        playTrack();
    }

    // Progress bar va vaqtni yangilash
    mainAudio.addEventListener('timeupdate', (e) => {
        const currentTime = e.target.currentTime;
        const duration = e.target.duration;
        
        if (duration) {
            // Progress barni surish
            const progressWidth = (currentTime / duration) * 100;
            progressBar.style.width = `${progressWidth}%`;

            // Vaqtni hisoblash (Joriy vaqt)
            let currentMin = Math.floor(currentTime / 60);
            let currentSec = Math.floor(currentTime % 60);
            if (currentSec < 10) currentSec = `0${currentSec}`;
            currentTimeText.innerText = `${currentMin}:${currentSec}`;

            // Umumiy vaqt (Duration)
            let totalMin = Math.floor(duration / 60);
            let totalSec = Math.floor(duration % 60);
            if (totalSec < 10) totalSec = `0${totalSec}`;
            durationText.innerText = `${totalMin}:${totalSec}`;
        }
    });

    // Progress barni bosib o'tkazish (Click orqali)
    progressArea.addEventListener('click', (e) => {
        const progressWidthval = progressArea.clientWidth; 
        const clickedOffsetX = e.offsetX; 
        const songDuration = mainAudio.duration; 

        if (songDuration) {
            mainAudio.currentTime = (clickedOffsetX / progressWidthval) * songDuration;
            if (!isPlaying) playTrack();
        }
    });

    // Ovoz balandligini boshqarish
    volumeSlider.addEventListener('input', (e) => {
        mainAudio.volume = e.target.value;
    });

    // Repeat rejimini yoqish/o'chirish
    repeatBtn.addEventListener('click', () => {
        isRepeat = !isRepeat;
        if (isRepeat) {
            isShuffle = false;
            shuffleBtn.classList.remove('active-mode');
            repeatBtn.classList.add('active-mode');
        } else {
            repeatBtn.classList.remove('active-mode');
        }
    });

    // Shuffle rejimini yoqish/o'chirish
    shuffleBtn.addEventListener('click', () => {
        isShuffle = !isShuffle;
        if (isShuffle) {
            isRepeat = false;
            repeatBtn.classList.remove('active-mode');
            shuffleBtn.classList.add('active-mode');
        } else {
            shuffleBtn.classList.remove('active-mode');
        }
    });

    // Qo'shiq tugaganda nima bo'lishi
    mainAudio.addEventListener('ended', () => {
        if (isRepeat) {
            mainAudio.currentTime = 0;
            playTrack();
        } else {
            nextTrack();
        }
    });

    // Tugma hodisalari
    playPauseBtn.addEventListener('click', togglePlay);
    nextBtn.addEventListener('click', nextTrack);
    prevBtn.addEventListener('click', prevTrack);
</script>

</body>
</html>
Kod qanday shartlarni bajardi?
Play / Pause: Markazdagi doira shaklidagi tugma qo'shiqni qo'yadi (mainAudio.play()) va to'xtatadi.
