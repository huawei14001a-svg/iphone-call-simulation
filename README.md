<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>iPhone Call Simulation</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  html, body { width: 100%; height: 100%; overflow: hidden; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background: black; }

  /* Экран звонка */
  #callScreen {
    width: 100%;
    height: 100%;
    position: relative;
    background: radial-gradient(circle at center, #000 0%, #111 100%);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    transition: opacity 0.5s;
  }

  #callerInfo {
    text-align: center;
    margin-bottom: 100px;
  }

  #callerInfo h1 {
    font-size: 48px;
    font-weight: 600;
  }

  #callerInfo p {
    font-size: 24px;
    margin-top: 10px;
    color: #ccc;
  }

  /* Кнопки звонка как на iPhone */
  #buttons {
    display: flex;
    gap: 60px;
  }

  .callBtn {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 18px;
    color: white;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 8px 20px rgba(0,0,0,0.5);
    transition: transform 0.2s;
  }

  .callBtn:active { transform: scale(0.9); }

  #answerBtn { background: #4cd964; }
  #declineBtn { background: #ff3b30; }

  /* Видео и камера */
  #videoContainer {
    display: none;
    width: 100%;
    height: 100%;
    position: relative;
    overflow: hidden;
    background: black;
  }

  #mainVideo {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  #camera {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 180px;
    height: 180px;
    border-radius: 15px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
    transform: translateY(40px); /* смещено вниз на 40px */
  }

  #camera video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>
</head>
<body>

<!-- Экран звонка -->
<div id="callScreen">
  <div id="callerInfo">
    <h1>Иван Иванов</h1>
    <p>Входящий звонок</p>
  </div>
  <div id="buttons">
    <div class="callBtn" id="declineBtn">Отклонить</div>
    <div class="callBtn" id="answerBtn">Ответить</div>
  </div>
</div>

<!-- Видео и камера -->
<div id="videoContainer">
  <video id="mainVideo" playsinline></video>
  <div id="camera">
    <video id="cameraVideo" autoplay muted playsinline></video>
  </div>
</div>

<script>
const callScreen = document.getElementById('callScreen');
const videoContainer = document.getElementById('videoContainer');
const mainVideo = document.getElementById('mainVideo');
const cameraVideo = document.getElementById('cameraVideo');

const answerBtn = document.getElementById('answerBtn');
const declineBtn = document.getElementById('declineBtn');

// Ссылка на видео с Google Drive
const videoURL = "https://drive.google.com/uc?export=download&id=184a2FpTODAP4Xda906V2y8Dy32Hm9UKq";

answerBtn.addEventListener('click', async () => {
  // Скрываем экран звонка
  callScreen.style.opacity = 0;
  setTimeout(() => callScreen.style.display = 'none', 500);
  videoContainer.style.display = 'block';

  // Подставляем видео и запускаем
  mainVideo.src = videoURL;
  mainVideo.play();

  // Запускаем камеру
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: false });
    cameraVideo.srcObject = stream;
  } catch(err) {
    console.error('Ошибка доступа к камере:', err);
  }

  // Закрываем Mini App после окончания видео
  mainVideo.addEventListener('ended', () => {
    if (window.TelegramWebApp) {
      Telegram.WebApp.close();
    } else {
      window.close();
    }
  });
});

// Кнопка отклонения звонка
declineBtn.addEventListener('click', () => {
  callScreen.style.opacity = 0;
  setTimeout(() => callScreen.style.display = 'none', 500);
});
</script>

</body>
</html>
