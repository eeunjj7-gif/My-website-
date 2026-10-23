<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>iPhone 17 Interactive Screen</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-tap-highlight-color: transparent;
      font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif;
    }
    body {
      background-color: #0d0d0f;
      color: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      overflow: hidden;
    }

    /* iPhone 17 Frame Container */
    .phone-container {
      position: relative;
      width: 390px;
      height: 820px;
      background: #000;
      border-radius: 54px;
      box-shadow: 0 25px 60px rgba(0,0,0,0.8), 0 0 0 12px #28282b, 0 0 0 13px #18181a;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    /* Status Bar */
    .status-bar {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 48px;
      padding: 12px 26px 0 26px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      z-index: 1000;
      font-size: 14px;
      font-weight: 600;
      letter-spacing: -0.2px;
      color: #fff;
    }
    .status-icons {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 12px;
    }

    /* Dynamic Island */
    .dynamic-island {
      position: absolute;
      top: 10px;
      left: 50%;
      transform: translateX(-50%);
      width: 120px;
      height: 35px;
      background-color: #000;
      border-radius: 20px;
      z-index: 1001;
      transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 12px;
      cursor: pointer;
    }
    .dynamic-island.playing {
      width: 200px;
    }
    .di-content {
      display: none;
      align-items: center;
      width: 100%;
      justify-content: space-between;
      color: #fff;
      font-size: 11px;
    }
    .dynamic-island.playing .di-content {
      display: flex;
    }
    .di-left {
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .di-album {
      width: 18px;
      height: 18px;
      border-radius: 4px;
      object-fit: cover;
    }
    .wave-bar {
      display: inline-block;
      width: 2px;
      height: 10px;
      background: #1db954;
      border-radius: 1px;
      animation: wave 1s infinite ease-in-out;
    }
    .wave-bar:nth-child(2) { animation-delay: 0.2s; }
    .wave-bar:nth-child(3) { animation-delay: 0.4s; }
    @keyframes wave {
      0%, 100% { height: 4px; }
      50% { height: 12px; }
    }

    /* Screen Management */
    .screen {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: none;
      flex-direction: column;
      background: #000;
      padding-top: 48px;
      padding-bottom: 24px;
      overflow: hidden;
    }
    .screen.active {
      display: flex;
    }

    /* Lock Screen */
    #lock-screen {
      background: linear-gradient(180deg, rgba(0,0,0,0.1) 0%, rgba(0,0,0,0.6) 100%),
                  url('https://images.unsplash.com/photo-1507525428034-b723cf961d3e?auto=format&fit=crop&w=800&q=80') center/cover;
      justify-content: space-between;
      align-items: center;
      padding-top: 60px;
      padding-bottom: 40px;
      text-align: center;
    }
    .lock-date {
      font-size: 16px;
      font-weight: 600;
      opacity: 0.9;
    }
    .lock-time {
      font-size: 80px;
      font-weight: 700;
      letter-spacing: -2px;
      line-height: 1;
      margin-top: 4px;
    }
    .lock-hint {
      font-size: 13px;
      opacity: 0.8;
      animation: bounce 2s infinite;
    }
    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-6px); }
    }

    /* Home Screen */
    #home-screen {
      background: linear-gradient(180deg, #1c1c1e 0%, #000 100%);
      padding: 60px 20px 20px 20px;
    }
    .widget-container {
      display: flex;
      gap: 14px;
      margin-bottom: 24px;
    }
    .widget {
      flex: 1;
      height: 140px;
      background: rgba(255,255,255,0.1);
      backdrop-filter: blur(20px);
      border-radius: 22px;
      padding: 16px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
    .app-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 18px 12px;
      align-items: start;
    }
    .app-icon-wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 6px;
      cursor: pointer;
    }
    .app-icon {
      width: 62px;
      height: 62px;
      border-radius: 15px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 28px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
      background-size: cover;
      background-position: center;
    }
    .app-label {
      font-size: 11px;
      color: #fff;
      text-align: center;
      font-weight: 500;
    }

    /* App Styles */
    .app-header {
      height: 44px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 16px;
      border-bottom: 1px solid rgba(255,255,255,0.1);
      background: #000;
    }
    .back-btn {
      color: #0a84ff;
      font-size: 15px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    /* Instagram */
    .insta-profile {
      padding: 16px;
    }
    .insta-info {
      display: flex;
      align-items: center;
      gap: 20px;
      margin-bottom: 16px;
    }
    .insta-avatar {
      width: 70px;
      height: 70px;
      border-radius: 50%;
      object-fit: cover;
    }
    .insta-stats {
      display: flex;
      gap: 20px;
      text-align: center;
    }
    .insta-stat-num {
      font-weight: 700;
      font-size: 15px;
    }
    .insta-stat-lbl {
      font-size: 11px;
      color: #8e8e93;
    }
    .insta-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 2px;
      background: #000;
    }
    .insta-grid-item {
      width: 100%;
      aspect-ratio: 1;
      object-fit: cover;
      cursor: pointer;
    }

    /* Spotify */
    .spotify-hero {
      padding: 20px;
      text-align: center;
    }
    .spotify-cover {
      width: 180px;
      height: 180px;
      border-radius: 12px;
      object-fit: cover;
      box-shadow: 0 8px 24px rgba(0,0,0,0.5);
      margin-bottom: 16px;
    }
    .song-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 16px;
      border-bottom: 1px solid rgba(255,255,255,0.05);
      cursor: pointer;
    }
    .song-thumb {
      width: 44px;
      height: 44px;
      border-radius: 6px;
      object-fit: cover;
    }

    /* Notes */
    .note-item {
      padding: 14px 16px;
      border-bottom: 1px solid rgba(255,255,255,0.1);
      cursor: pointer;
    }
    .note-date {
      font-size: 11px;
      color: #8e8e93;
      margin-top: 4px;
    }

    /* Messages */
    .msg-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 16px;
      border-bottom: 1px solid rgba(255,255,255,0.08);
      cursor: pointer;
    }
    .msg-avatar {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      background: #3a3a3c;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 600;
    }

    /* Phone Call Subtitles */
    .call-screen {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      padding: 40px 20px;
      text-align: center;
    }
    .subtitle-box {
      background: rgba(255,255,255,0.15);
      backdrop-filter: blur(10px);
      padding: 12px 18px;
      border-radius: 16px;
      font-size: 14px;
      max-width: 80%;
      margin-top: 20px;
    }

    /* Bottom Home Indicator Bar */
    .home-bar {
      position: absolute;
      bottom: 8px;
      left: 50%;
      transform: translateX(-50%);
      width: 134px;
      height: 5px;
      background: #fff;
      border-radius: 3px;
      z-index: 2000;
      cursor: pointer;
    }
  </style>
</head>
<body>

<div class="phone-container">
  <!-- Status Bar -->
  <div class="status-bar">
    <span id="clock">14:42</span>
    <div class="status-icons">
      <span>5G</span>
      <span>91%</span>
    </div>
  </div>

  <!-- Dynamic Island -->
  <div class="dynamic-island" id="dynamic-island">
    <div class="di-content">
      <div class="di-left">
        <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=100&q=80" class="di-album" alt="album">
        <span>MOYA - Jay Park</span>
      </div>
      <div style="display:flex; gap:2px;">
        <span class="wave-bar"></span>
        <span class="wave-bar"></span>
        <span class="wave-bar"></span>
      </div>
    </div>
  </div>

  <!-- Lock Screen -->
  <div class="screen active" id="lock-screen" onclick="unlockPhone()">
    <div>
      <div class="lock-date">Thursday, October 22</div>
      <div class="lock-time">14:42</div>
    </div>
    <div class="lock-hint">Swipe up or tap to unlock</div>
  </div>

  <!-- Home Screen -->
  <div class="screen" id="home-screen">
    <div class="widget-container">
      <div class="widget">
        <span style="color:#ff3b30; font-weight:600; font-size:12px;">FRIDAY</span>
        <span style="font-size:32px; font-weight:700;">23</span>
        <span style="font-size:11px; color:#8e8e93;">COMEBACK</span>
      </div>
      <div class="widget">
        <span style="font-size:12px; color:#8e8e93;">Seoul</span>
        <span style="font-size:28px; font-weight:600;">18°</span>
        <span style="font-size:11px; color:#8e8e93;">Mostly Sunny</span>
      </div>
    </div>

    <div class="app-grid">
      <div class="app-icon-wrapper" onclick="openApp('insta-screen')">
        <div class="app-icon" style="background: linear-gradient(45deg, #f09433, #bc1888);">📷</div>
        <span class="app-label">Instagram</span>
      </div>
      <div class="app-icon-wrapper" onclick="openApp('spotify-screen')">
        <div class="app-icon" style="background: #1db954;">🎵</div>
        <span class="app-label">Spotify</span>
      </div>
      <div class="app-icon-wrapper" onclick="openApp('notes-screen')">
        <div class="app-icon" style="background: #f1c40f;">📝</div>
        <span class="app-label">메모</span>
      </div>
      <div class="app-icon-wrapper" onclick="openApp('msg-screen')">
        <div class="app-icon" style="background: #2ecc71;">💬</div>
        <span class="app-label">메시지</span>
      </div>
      <div class="app-icon-wrapper" onclick="openApp('call-screen')">
        <div class="app-icon" style="background: #3498db;">📞</div>
        <span class="app-label">전화</span>
      </div>
      <div class="app-icon-wrapper" onclick="openApp('garage-screen')">
        <div class="app-icon" style="background: #e67e22;">🎹</div>
        <span class="app-label">GarageBand</span>
      </div>
    </div>
  </div>

  <!-- Instagram Screen -->
  <div class="screen" id="insta-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">ryullkimm</span>
      <span></span>
    </div>
    <div style="overflow-y:auto; flex:1;">
      <div class="insta-profile">
        <div class="insta-info">
          <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=200&q=80" class="insta-avatar" alt="profile">
          <div class="insta-stats">
            <div><div class="insta-stat-num">10</div><div class="insta-stat-lbl">posts</div></div>
            <div><div class="insta-stat-num">248</div><div class="insta-stat-lbl">followers</div></div>
            <div><div class="insta-stat-num">180</div><div class="insta-stat-lbl">following</div></div>
          </div>
        </div>
        <div style="font-weight:600; font-size:14px;">김률</div>
        <div style="font-size:12px; color:#8e8e93;">istj, south korea</div>
      </div>
      <div class="insta-grid" id="insta-grid"></div>
    </div>
  </div>

  <!-- Instagram Detail Post Screen -->
  <div class="screen" id="insta-post-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('insta-screen')">&lt; Back</span>
      <span style="font-weight:600;">Post</span>
      <span></span>
    </div>
    <div style="padding:16px;">
      <img id="post-img" src="" style="width:100%; border-radius:8px;" alt="post">
      <div style="margin-top:12px; font-weight:600;">ryullkimm</div>
      <div style="font-size:13px; color:#ccc; margin-top:4px;">yo</div>
    </div>
  </div>

  <!-- Spotify Screen -->
  <div class="screen" id="spotify-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">Your Library</span>
      <span></span>
    </div>
    <div style="overflow-y:auto; flex:1;">
      <div class="spotify-hero">
        <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=400&q=80" class="spotify-cover" alt="cover">
        <h2>For Us</h2>
        <div style="font-size:12px; color:#8e8e93; margin-top:4px;">ryul kim • 3 songs</div>
      </div>
      <div class="song-item" onclick="togglePlay()">
        <img src="https://images.unsplash.com/photo-1511671782779-c97d3d27a1d4?w=100&q=80" class="song-thumb" alt="moya">
        <div>
          <div style="font-weight:600; font-size:14px;">MOYA</div>
          <div style="font-size:12px; color:#8e8e93;">Jay Park, LNGSHOT</div>
        </div>
      </div>
      <div class="song-item" onclick="togglePlay()">
        <img src="https://images.unsplash.com/photo-1514525253161-7a46d19cd819?w=100&q=80" class="song-thumb" alt="thingking">
        <div>
          <div style="font-weight:600; font-size:14px;">Thingking</div>
          <div style="font-size:12px; color:#8e8e93;">RYUL of LNGSHOT</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Notes Screen -->
  <div class="screen" id="notes-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">메모</span>
      <span></span>
    </div>
    <div style="overflow-y:auto; flex:1;">
      <div class="note-item">
        <div style="font-weight:600; font-size:14px;">Ann Demeulemeester 빈티지...</div>
        <div class="note-date">2026년 9월 16일 오후 5시 39분</div>
      </div>
      <div class="note-item">
        <div style="font-weight:600; font-size:14px;">Cooking (녹음/후반 작업)</div>
        <div class="note-date">2026년 10월 11일 오전 2시 34분</div>
      </div>
      <div class="note-item">
        <div style="font-weight:600; font-size:14px;">오후에 프로듀서형이랑 작업있음</div>
        <div class="note-date">2026년 10월 17일 오후 11시 34분</div>
      </div>
    </div>
  </div>

  <!-- Messages Screen -->
  <div class="screen" id="msg-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">메시지</span>
      <span></span>
    </div>
    <div style="overflow-y:auto; flex:1;">
      <div class="msg-item"><div class="msg-avatar">성</div><div><strong>성현이</strong><div style="font-size:12px; color:#8e8e93;">오늘 작업실 몇 시에 와?</div></div></div>
      <div class="msg-item"><div class="msg-avatar">건</div><div><strong>건호</strong><div style="font-size:12px; color:#8e8e93;">형 확인했습니다!</div></div></div>
      <div class="msg-item"><div class="msg-avatar">제</div><div><strong>제임스형</strong><div style="font-size:12px; color:#8e8e93;">곡 리스트 확인해봐~</div></div></div>
      <div class="msg-item"><div class="msg-avatar">주</div><div><strong>주훈이</strong><div style="font-size:12px; color:#8e8e93;">오케이 수고했어</div></div></div>
      <div class="msg-item"><div class="msg-avatar">스</div><div><strong>스타일리스트형</strong><div style="font-size:12px; color:#8e8e93;">착장 의상 보냈어요</div></div></div>
      <div class="msg-item"><div class="msg-avatar">P</div><div><strong>PD님</strong><div style="font-size:12px; color:#8e8e93;">내일 미팅 시간 변경건</div></div></div>
      <div class="msg-item"><div class="msg-avatar">프</div><div><strong>프로듀서 형</strong><div style="font-size:12px; color:#8e8e93;">믹스본 피드백 완료!</div></div></div>
    </div>
  </div>

  <!-- Phone Call Screen -->
  <div class="screen" id="call-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">통화</span>
      <span></span>
    </div>
    <div class="call-screen">
      <div>
        <div style="font-size:24px; font-weight:600;">권오율 ♡</div>
        <div style="color:#8e8e93; font-size:14px; margin-top:4px;">Calling... mobile</div>
      </div>
      <div class="subtitle-box">"여보세요? 지금 어디야? 작업 거의 다 끝나가?"</div>
      <div style="width:60px; height:60px; background:#ff3b30; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:24px; cursor:pointer;" onclick="openApp('home-screen')">
        📞
      </div>
    </div>
  </div>

  <!-- GarageBand Screen -->
  <div class="screen" id="garage-screen">
    <div class="app-header">
      <span class="back-btn" onclick="openApp('home-screen')">&lt; Home</span>
      <span style="font-weight:600;">GarageBand</span>
      <span></span>
    </div>
    <div style="flex:1; display:flex; flex-direction:column; justify-content:center; align-items:center; gap:20px; padding:20px;">
      <div style="font-size:18px; font-weight:600;">🎹 Touch Piano</div>
      <div style="display:flex; gap:6px;">
        <div style="width:40px; height:120px; background:#fff; color:#000; border-radius:0 0 6px 6px; display:flex; align-items:flex-end; justify-content:center; padding-bottom:8px; font-weight:600; cursor:pointer;">C</div>
        <div style="width:40px; height:120px; background:#fff; color:#000; border-radius:0 0 6px 6px; display:flex; align-items:flex-end; justify-content:center; padding-bottom:8px; font-weight:600; cursor:pointer;">D</div>
        <div style="width:40px; height:120px; background:#fff; color:#000; border-radius:0 0 6px 6px; display:flex; align-items:flex-end; justify-content:center; padding-bottom:8px; font-weight:600; cursor:pointer;">E</div>
        <div style="width:40px; height:120px; background:#fff; color:#000; border-radius:0 0 6px 6px; display:flex; align-items:flex-end; justify-content:center; padding-bottom:8px; font-weight:600; cursor:pointer;">F</div>
        <div style="width:40px; height:120px; background:#fff; color:#000; border-radius:0 0 6px 6px; display:flex; align-items:flex-end; justify-content:center; padding-bottom:8px; font-weight:600; cursor:pointer;">G</div>
      </div>
    </div>
  </div>

  <!-- Bottom Home Bar -->
  <div class="home-bar" onclick="openApp('home-screen')"></div>
</div>

<script>
  const instaImages = [
    'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=500&q=80',
    'https://images.unsplash.com/photo-1539571696357-5a69c17a67c6?w=500&q=80',
    'https://images.unsplash.com/photo-1517841905240-472988babdf9?w=500&q=80',
    'https://images.unsplash.com/photo-1524504388940-b1c1722653e1?w=500&q=80',
    'https://images.unsplash.com/photo-1501196354995-cbb51c65aaea?w=500&q=80',
    'https://images.unsplash.com/photo-1492562080023-ab3db95bfbce?w=500&q=80',
    'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=500&q=80',
    'https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?w=500&q=80',
    'https://images.unsplash.com/photo-1522075469751-3a6694fb2f61?w=500&q=80',
    'https://images.unsplash.com/photo-1519085360753-af0119f7cbe7?w=500&q=80'
  ];

  // Render Instagram Grid
  const gridContainer = document.getElementById('insta-grid');
  instaImages.forEach((src) => {
    const img = document.createElement('img');
    img.src = src;
    img.className = 'insta-grid-item';
    img.onclick = () => {
      document.getElementById('post-img').src = src;
      openApp('insta-post-screen');
    };
    gridContainer.appendChild(img);
  });

  function unlockPhone() {
    openApp('home-screen');
  }

  function openApp(screenId) {
    document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
    document.getElementById(screenId).classList.add('active');
  }

  let isPlaying = false;
  function togglePlay() {
    isPlaying = !isPlaying;
    const di = document.getElementById('dynamic-island');
    if(isPlaying) {
      di.classList.add('playing');
    } else {
      di.classList.remove('playing');
    }
  }
</script>
</body>
</html>
