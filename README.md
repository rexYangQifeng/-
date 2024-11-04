<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>App 主畫面</title>
  <style>
    /* 基本樣式重置 */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body, html {
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #f5f5f5;
    }

    .background {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: url('/mnt/data/462534400_1194419031623857_9033150623048558032_n.jpg') no-repeat center center;
      background-size: cover;
      z-index: -1;
    }

    .container {
      width: 100%;
      max-width: 400px;
      padding: 20px;
      text-align: center;
      background-color: rgba(255, 255, 255, 0.8);
      border-radius: 10px;
      display: none;
    }

    .container.active {
      display: block;
    }

    .btn {
      width: 100%;
      padding: 15px;
      margin: 10px 0;
      font-size: 16px;
      color: #333;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s;
      background-color: #e0e0e0;
    }

    .btn-secondary {
      width: 100%;
      padding: 15px;
      margin: 10px 0;
      font-size: 18px;
      color: #fff;
      background-color: #888;
      border-radius: 5px;
      cursor: pointer;
    }

    .side-menu, .brand-container {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .side-menu {
      position: fixed;
      left: 20px;
      top: 50px;
      background-color: #555;
      padding: 10px;
      border-radius: 5px;
    }

    .side-menu button {
      width: 80px;
      padding: 10px;
      background-color: #888;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    .brand-container {
      margin-top: 50px;
    }

    .brand-button {
      width: 100px;
      padding: 15px;
      font-size: 16px;
      color: #333;
      background-color: #e0e0e0;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin: 5px;
    }

    .back-button {
      position: absolute;
      bottom: 20px;
      right: 20px;
      background-color: #888;
      color: #fff;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    /* 相機預覽和照片區域 */
    #cameraPreview {
      display: none;
      position: relative;
      width: 100%;
      max-width: 400px;
      height: 300px;
      background-color: #000;
      margin-top: 20px;
    }

    video, canvas {
      width: 100%;
      height: 100%;
      border-radius: 10px;
    }

    #takePhotoButton {
      margin-top: 10px;
      padding: 10px;
      font-size: 16px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    #photoResult {
      margin-top: 20px;
    }
  </style>
</head>
<body>

<div class="background"></div>

<div id="homeScreen" class="container active">
  <h2>服裝限時特價</h2>
  <button class="btn btn-teach" onclick="showTutorial()">使用教學</button>
  <button class="btn btn-login" onclick="showLoginScreen()">登入</button>
  <button class="btn btn-translate" onclick="translate()">翻譯</button>
</div>

<div id="loginScreen" class="container">
  <h2>請選擇服裝對象</h2>
  <button class="btn-secondary" onclick="selectClothing('男裝')">男裝</button>
  <button class="btn-secondary" onclick="selectClothing('女裝')">女裝</button>
  <button class="btn-secondary" onclick="selectClothing('中性')">中性</button>
  <button class="btn" onclick="showHomeScreen()">返回主頁</button>
</div>

<div id="brandScreen" class="container">
  <div class="side-menu">
    <button>項目</button>
    <button>品牌</button>
    <button>類型</button>
    <button>材質</button>
    <button>特價</button>
    <button>產地</button>
    <button>價格</button>
  </div>
  <div class="brand-container">
    <h2>品牌</h2>
    <button class="brand-button" onclick="startCamera()">愛迪達</button>
    <button class="brand-button" onclick="startCamera()">巴黎精品</button>
    <button class="brand-button" onclick="startCamera()">NIKE</button>
    <button class="brand-button" onclick="startCamera()">PUMA</button>
  </div>
  <button class="back-button" onclick="showLoginScreen()">返回主頁</button>
</div>

<!-- 相機預覽區域 -->
<div id="cameraPreview">
  <video id="video" autoplay playsinline></video>
  <button id="takePhotoButton" onclick="takePhoto()">拍照</button>
</div>

<!-- 照片顯示區域 -->
<div id="photoResult" class="container">
  <h2>拍攝結果</h2>
  <canvas id="canvas"></canvas>
  <button class="btn" onclick="showHomeScreen()">返回主頁</button>
</div>

<script>
  function showTutorial() {
    alert("這是使用教學。");
  }

  function translate() {
    alert("前往翻譯頁面。");
  }

  function showLoginScreen() {
    document.getElementById('homeScreen').classList.remove('active');
    document.getElementById('loginScreen').classList.add('active');
    document.getElementById('brandScreen').classList.remove('active');
    document.getElementById('cameraPreview').style.display = 'none';
    document.getElementById('photoResult').style.display = 'none';
  }

  function showHomeScreen() {
    document.getElementById('loginScreen').classList.remove('active');
    document.getElementById('homeScreen').classList.add('active');
    document.getElementById('photoResult').style.display = 'none';
  }

  function selectClothing(type) {
    document.getElementById('loginScreen').classList.remove('active');
    document.getElementById('brandScreen').classList.add('active');
  }

  async function startCamera() {
    document.getElementById('brandScreen').classList.remove('active');
    document.getElementById('cameraPreview').style.display = 'block';

    const video = document.getElementById('video');
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ video: true });
      video.srcObject = stream;
    } catch (error) {
      alert("無法啟動相機：" + error.message);
    }
  }

  function takePhoto() {
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const context = canvas.getContext('2d');
    canvas.width = video.videoWidth;
    canvas.height = video.videoHeight;

    // 將影片的當前畫面繪製到 canvas 上
    context.drawImage(video, 0, 0, canvas.width, canvas.height);

    // 顯示拍攝結果
    document.getElementById('cameraPreview').style.display = 'none';
    document.getElementById('photoResult').style.display = 'block';
  }
</script>

</body>
</html>

