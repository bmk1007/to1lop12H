<!DOCTYPE html>
<html lang="vi">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>to1lop12h</title>


    <meta name="description"
        content="to1lop12H - Trang web nghe nhạc riêng của lớp 12H. Nơi lưu giữ những bài hát kỷ niệm, remix yêu thích và khoảnh khắc thanh xuân.">

    <meta name="keywords" content="to1lop12H, lớp 12H, nghe nhạc, music player, remix, kỷ niệm">

    <meta name="author" content="to1lop12H">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            background: #121212;
            color: white;
            display: flex;
            height: 100vh;
        }

        /* Sidebar */
        .sidebar {
            width: 220px;
            background: #000;
            padding: 20px;
        }

        .sidebar h2 {
            color: #1DB954;
            margin-bottom: 20px;
        }

        .sidebar ul {
            list-style: none;
        }

        .sidebar li {
            padding: 10px 0;
            cursor: pointer;
            color: #b3b3b3;
        }

        .sidebar li:hover {
            color: white;
        }

        /* Main */
        .main {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
        }

        .song {
            display: flex;
            align-items: center;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            margin-bottom: 5px;
        }

        .song img {
            width: 50px;
            height: 50px;
            border-radius: 5px;
            margin-right: 10px;
        }

        .song:hover {
            background: #282828;
        }

        .song.active {
            background: #6a3f9b;
        }

        /* Player */
        .player {
            position: fixed;
            bottom: 0;
            left: 220px;
            right: 0;
            background: #181818;
            padding: 15px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .now {
            display: flex;
            align-items: center;
        }

        .now img {
            width: 50px;
            height: 50px;
            margin-right: 10px;
        }

        .controls {
            display: flex;
            align-items: center;
        }

        .controls button {
            background: none;
            border: none;
            color: white;
            font-size: 20px;
            margin: 0 10px;
            cursor: pointer;
        }

        .controls button:hover {
            color: #b91d9f;
        }

        .progress-container {
            width: 300px;
            height: 5px;
            background: #333;
            border-radius: 5px;
            cursor: pointer;
            margin: 0 10px;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: #1DB954;
            border-radius: 5px;
        }

        .time {
            font-size: 12px;
            margin: 0 5px;
        }

        .volume {
            width: 100px;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .sidebar {
                display: none;
            }

            .player {
                left: 0;
            }
        }
    </style>
</head>

<body>

    <div class="sidebar">
        <h2>to1lop12H</h2>
        <ul>
            <li>Home</li>
            <li>tìm kiếm</li>
            <li>Your Library</li>
        </ul>
    </div>

    <div class="main">
        <h1>Playlist của bạn</h1>
        <div id="songList"></div>
        <h1>miêu tả</h1>
        <p>
            Đây là trang web do nhóm 1 lớp 12H trường tt DGNN DGTT Gia lộc làm.nhóm được chủ trì bởi nhóm trưởng Bùi Quý
            Mạnh cùng các thành viên khác:
        </p>
        <p1>Bùi Minh Kế;
        </p1>
        <p2>Nguyễn Đức Huy;
        </p2>
        <p3>Tăng Hữu Long;
        </p3>
        <p4>Ngô Thế Hưng
        </p4>

    </div>

    <div class="player">
        <div class="now">
            <img id="cover" src="">
            <div>
                <div id="title">Chưa phát</div>
                <div id="artist" style="font-size:12px;color:#aaa;"></div>
            </div>
        </div>

        <div class="controls">
            <button onclick="prevSong()">⏮</button>
            <button onclick="togglePlay()" id="playBtn">▶</button>
            <button onclick="nextSong()">⏭</button>

            <span class="time" id="currentTime">0:00</span>
            <div class="progress-container" id="progressContainer">
                <div class="progress" id="progress"></div>
            </div>
            <span class="time" id="duration">0:00</span>
        </div>

        <input type="range" min="0" max="1" step="0.01" value="1" class="volume" id="volume">
    </div>

    <audio id="audio"></audio>

    <script>
        const songs = [
            { title: "Mở Lối Cho Em (Ss x Am Remix)", artist: "Lương Quý Tuấn", src: "https://res.cloudinary.com/dne9zjvt2/video/upload/v1771910854/M%E1%BB%9F_L%E1%BB%91i_Cho_Em_Ss_x_Am_Remix_yenlwl.mp3", cover: "l dũng.jpg" },
            { title: "Thay Tôi Yêu Cô Ấy (Style Huy PT Remix)", artist: "Thanh Hưng", src: "https://res.cloudinary.com/dne9zjvt2/video/upload/v1771910835/Thay_T%C3%B4i_Y%C3%AAu_C%C3%B4_%E1%BA%A4y_Style_Huy_PT_Remix_izm8xu.mp3", cover: "cô gái tôi yêu.jpg" },
            { title: "Trả Cho Anh Remix x In The Club", artist: "Nguyễn Thạc Bảo Ngọc x QKhanh", src: "https://res.cloudinary.com/dne9zjvt2/video/upload/v1771910815/Tr%E1%BA%A3_Cho_Anh_Remix_x_In_The_Club_ev7ucp.mp3", cover: "2.jpg" },
            { title: "doạn tuyệt nàng đi remix", artist: "PHÁT HUY T4 x DOMINO REMIX", src: "https://res.cloudinary.com/dne9zjvt2/video/upload/v1771910896/%C4%90O%E1%BA%A0N_TUY%E1%BB%86T_N%C3%80NG_%C4%90I_REMIX_mswaar.mp3", cover: "3.jpg" },
            { title: "KHÔNG AI NÓI CHIA TAY", artist: "không nhớ:))", src: "https://res.cloudinary.com/ dne9zjvt2/video/upload/v1771910883/Không_Ai_ nói_Chia_Tay_Remix_Bản_Hot_TikTok_idxmaw.mp3", cover: "1.jpg" }
        ]

        let currentSong = 0;

        const audio = document.getElementById("audio");
        const songList = document.getElementById("songList");
        const cover = document.getElementById("cover");
        const title = document.getElementById("title");
        const artist = document.getElementById("artist");
        const playBtn = document.getElementById("playBtn");
        const progress = document.getElementById("progress");
        const progressContainer = document.getElementById("progressContainer");
        const currentTimeEl = document.getElementById("currentTime");
        const durationEl = document.getElementById("duration");
        const volume = document.getElementById("volume");

        function formatTime(time) {
            const minutes = Math.floor(time / 60);
            const seconds = Math.floor(time % 60);
            return minutes + ":" + (seconds < 10 ? "0" + seconds : seconds);
        }

        function loadSong(index) {
            const song = songs[index];
            audio.src = song.src;
            cover.src = song.cover;
            title.innerText = song.title;
            artist.innerText = song.artist;

            document.querySelectorAll(".song").forEach(el => el.classList.remove("active"));
            document.querySelectorAll(".song")[index].classList.add("active");
        }

        function togglePlay() {
            if (audio.paused) {
                audio.play();
                playBtn.innerText = "⏸";
            } else {
                audio.pause();
                playBtn.innerText = "▶";
            }
        }

        function nextSong() {
            currentSong = (currentSong + 1) % songs.length;
            loadSong(currentSong);
            audio.play();
            playBtn.innerText = "⏸";
        }

        function prevSong() {
            currentSong = (currentSong - 1 + songs.length) % songs.length;
            loadSong(currentSong);
            audio.play();
            playBtn.innerText = "⏸";
        }

        audio.addEventListener("loadedmetadata", () => {
            durationEl.innerText = formatTime(audio.duration);
        });

        audio.addEventListener("timeupdate", () => {
            progress.style.width = (audio.currentTime / audio.duration) * 100 + "%";
            currentTimeEl.innerText = formatTime(audio.currentTime);
        });

        progressContainer.addEventListener("click", (e) => {
            const width = progressContainer.clientWidth;
            const clickX = e.offsetX;
            audio.currentTime = (clickX / width) * audio.duration;
        });

        audio.addEventListener("ended", nextSong);

        volume.addEventListener("input", () => {
            audio.volume = volume.value;
        });

        function renderSongs() {
            songs.forEach((song, index) => {
                const div = document.createElement("div");
                div.classList.add("song");
                div.innerHTML = `
            <img src="${song.cover}">
            <div>
                <div>${song.title}</div>
                <div style="font-size:12px;color:#aaa;">${song.artist}</div>
            </div>
        `;
                div.onclick = () => {
                    currentSong = index;
                    loadSong(index);
                    audio.play();
                    playBtn.innerText = "⏸";
                };
                songList.appendChild(div);
            });
        }

        renderSongs();
        loadSong(currentSong);
    </script>

</body>

</html>
