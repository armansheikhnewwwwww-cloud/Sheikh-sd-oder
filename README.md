<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>YT ViewPulse</title>
  <link rel="manifest" href="#" />
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg: #0a0a0f;
      --surface: #12121a;
      --card: #1a1a26;
      --border: #2a2a3e;
      --accent: #ff3d5a;
      --accent2: #7c3aed;
      --green: #00e676;
      --text: #f0f0ff;
      --muted: #6b6b8a;
      --font-display: 'Syne', sans-serif;
      --font-mono: 'Space Mono', monospace;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font-display);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* Background mesh */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background:
        radial-gradient(ellipse 600px 400px at 20% 20%, rgba(124,58,237,0.08) 0%, transparent 70%),
        radial-gradient(ellipse 400px 300px at 80% 80%, rgba(255,61,90,0.06) 0%, transparent 70%);
      pointer-events: none;
      z-index: 0;
    }

    .wrapper { position: relative; z-index: 1; max-width: 1200px; margin: 0 auto; padding: 0 20px 60px; }

    /* Header */
    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 28px 0 24px;
      border-bottom: 1px solid var(--border);
      margin-bottom: 32px;
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .logo-icon {
      width: 36px; height: 36px;
      background: var(--accent);
      border-radius: 8px;
      display: flex; align-items: center; justify-content: center;
      font-size: 18px;
    }

    .logo h1 {
      font-size: 1.4rem;
      font-weight: 800;
      letter-spacing: -0.5px;
    }

    .logo span { color: var(--accent); }

    .live-badge {
      display: flex;
      align-items: center;
      gap: 7px;
      background: rgba(0,230,118,0.1);
      border: 1px solid rgba(0,230,118,0.25);
      padding: 6px 14px;
      border-radius: 100px;
      font-size: 0.72rem;
      font-family: var(--font-mono);
      color: var(--green);
      letter-spacing: 1px;
    }

    .live-dot {
      width: 7px; height: 7px;
      border-radius: 50%;
      background: var(--green);
      animation: pulse 1.5s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.5; transform: scale(0.8); }
    }

    /* API Setup */
    #setup-section {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 32px;
      margin-bottom: 32px;
      animation: fadeUp 0.5s ease both;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    #setup-section h2 {
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 8px;
      color: var(--text);
    }

    #setup-section p {
      font-size: 0.82rem;
      color: var(--muted);
      margin-bottom: 18px;
      line-height: 1.6;
    }

    #setup-section a { color: var(--accent2); text-decoration: none; }
    #setup-section a:hover { text-decoration: underline; }

    .input-row {
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
    }

    input[type="text"], input[type="password"] {
      flex: 1;
      min-width: 200px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 16px;
      color: var(--text);
      font-family: var(--font-mono);
      font-size: 0.8rem;
      outline: none;
      transition: border-color 0.2s;
    }

    input:focus { border-color: var(--accent2); }

    button {
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: 10px;
      padding: 12px 22px;
      font-family: var(--font-display);
      font-weight: 700;
      font-size: 0.85rem;
      cursor: pointer;
      transition: opacity 0.2s, transform 0.15s;
      letter-spacing: 0.3px;
    }

    button:hover { opacity: 0.88; transform: translateY(-1px); }
    button:active { transform: translateY(0); }

    .btn-secondary {
      background: var(--surface);
      border: 1px solid var(--border);
      color: var(--text);
    }

    .btn-green { background: var(--green); color: #000; }

    /* Add Video */
    #add-section {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 24px 28px;
      margin-bottom: 32px;
      display: none;
      animation: fadeUp 0.4s ease both;
    }

    #add-section h2 {
      font-size: 1rem;
      font-weight: 700;
      margin-bottom: 14px;
    }

    /* Stats bar */
    #stats-bar {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 14px;
      margin-bottom: 28px;
      display: none;
    }

    .stat-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 16px 20px;
      animation: fadeUp 0.5s ease both;
    }

    .stat-label {
      font-size: 0.7rem;
      color: var(--muted);
      font-family: var(--font-mono);
      letter-spacing: 1px;
      text-transform: uppercase;
      margin-bottom: 6px;
    }

    .stat-value {
      font-size: 1.5rem;
      font-weight: 800;
      font-family: var(--font-mono);
    }

    .stat-value.red { color: var(--accent); }
    .stat-value.purple { color: #a78bfa; }
    .stat-value.green { color: var(--green); }

    /* Video Grid */
    #videos-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
      gap: 20px;
    }

    .video-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 16px;
      overflow: hidden;
      animation: fadeUp 0.5s ease both;
      transition: border-color 0.2s, transform 0.2s;
      position: relative;
    }

    .video-card:hover {
      border-color: var(--accent2);
      transform: translateY(-3px);
    }

    .video-card .thumb {
      width: 100%;
      aspect-ratio: 16/9;
      background: var(--surface);
      position: relative;
      overflow: hidden;
    }

    .video-card .thumb iframe {
      width: 100%;
      height: 100%;
      border: none;
      display: block;
    }

    .video-card .thumb img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }

    .play-overlay {
      position: absolute;
      inset: 0;
      background: rgba(0,0,0,0.4);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: background 0.2s;
    }

    .play-overlay:hover { background: rgba(0,0,0,0.2); }

    .play-btn {
      width: 52px; height: 52px;
      background: var(--accent);
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 20px;
      box-shadow: 0 4px 20px rgba(255,61,90,0.5);
      transition: transform 0.2s;
    }

    .play-overlay:hover .play-btn { transform: scale(1.1); }

    .video-info {
      padding: 16px 18px 18px;
    }

    .video-title {
      font-size: 0.88rem;
      font-weight: 700;
      margin-bottom: 12px;
      line-height: 1.4;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    .metrics-row {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .metric {
      flex: 1;
      min-width: 80px;
      background: var(--surface);
      border-radius: 8px;
      padding: 10px 12px;
      text-align: center;
    }

    .metric-label {
      font-size: 0.62rem;
      color: var(--muted);
      font-family: var(--font-mono);
      letter-spacing: 0.8px;
      text-transform: uppercase;
      margin-bottom: 4px;
    }

    .metric-value {
      font-size: 1.05rem;
      font-weight: 700;
      font-family: var(--font-mono);
    }

    .metric-value.views { color: var(--accent); }
    .metric-value.likes { color: #f59e0b; }
    .metric-value.comments { color: #60a5fa; }

    .card-actions {
      display: flex;
      gap: 8px;
      padding: 0 18px 14px;
    }

    .btn-small {
      flex: 1;
      padding: 8px 10px;
      font-size: 0.75rem;
      border-radius: 8px;
    }

    .btn-remove {
      background: rgba(255,61,90,0.12);
      border: 1px solid rgba(255,61,90,0.3);
      color: var(--accent);
    }

    .btn-refresh {
      background: rgba(124,58,237,0.12);
      border: 1px solid rgba(124,58,237,0.3);
      color: #a78bfa;
    }

    /* Refresh bar */
    #refresh-bar {
      position: fixed;
      bottom: 0; left: 0; right: 0;
      background: var(--surface);
      border-top: 1px solid var(--border);
      padding: 12px 24px;
      display: none;
      align-items: center;
      justify-content: space-between;
      z-index: 100;
      font-size: 0.78rem;
      font-family: var(--font-mono);
      color: var(--muted);
    }

    .progress-track {
      flex: 1;
      height: 3px;
      background: var(--border);
      border-radius: 10px;
      margin: 0 16px;
      overflow: hidden;
    }

    .progress-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--accent2), var(--accent));
      border-radius: 10px;
      transition: width 1s linear;
    }

    /* Loading */
    .loading {
      display: inline-block;
      width: 14px; height: 14px;
      border: 2px solid var(--border);
      border-top-color: var(--accent);
      border-radius: 50%;
      animation: spin 0.7s linear infinite;
      vertical-align: middle;
    }

    @keyframes spin { to { transform: rotate(360deg); } }

    /* Empty state */
    #empty-state {
      text-align: center;
      padding: 60px 20px;
      display: none;
    }

    #empty-state .icon { font-size: 3rem; margin-bottom: 16px; }
    #empty-state h3 { font-size: 1.1rem; margin-bottom: 8px; color: var(--text); }
    #empty-state p { font-size: 0.82rem; color: var(--muted); }

    .toast {
      position: fixed;
      top: 20px; right: 20px;
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 18px;
      font-size: 0.8rem;
      z-index: 999;
      animation: slideIn 0.3s ease;
      max-width: 280px;
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateX(30px); }
      to { opacity: 1; transform: translateX(0); }
    }

    .toast.success { border-color: var(--green); color: var(--green); }
    .toast.error { border-color: var(--accent); color: var(--accent); }

    /* How to get API key */
    .api-steps {
      background: var(--surface);
      border-radius: 10px;
      padding: 14px 18px;
      margin-top: 14px;
      font-size: 0.78rem;
      color: var(--muted);
      line-height: 1.8;
    }

    .api-steps strong { color: var(--text); }

    @media (max-width: 600px) {
      header { flex-direction: column; gap: 14px; align-items: flex-start; }
      .input-row { flex-direction: column; }
      #videos-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<div class="wrapper">

  <!-- Header -->
  <header>
    <div class="logo">
      <div class="logo-icon">▶</div>
      <h1>YT <span>ViewPulse</span></h1>
    </div>
    <div class="live-badge">
      <div class="live-dot"></div>
      LIVE MONITOR
    </div>
  </header>

  <!-- API Setup -->
  <div id="setup-section">
    <h2>🔑 YouTube API Key Setup</h2>
    <p>
      Real-time view count ke liye YouTube Data API v3 key chahiye.<br>
      Free hai — <a href="https://console.cloud.google.com/" target="_blank">Google Cloud Console</a> se milti hai.
    </p>
    <div class="input-row">
      <input type="password" id="api-key-input" placeholder="Apni YouTube API Key yahan paste karo..." />
      <button onclick="saveApiKey()">Save & Start</button>
    </div>
    <div class="api-steps">
      <strong>API Key kaise milegi? (2 minute ka kaam)</strong><br>
      1. <a href="https://console.cloud.google.com/" target="_blank" style="color:#a78bfa">console.cloud.google.com</a> par jao<br>
      2. New Project banao → APIs & Services → Enable APIs<br>
      3. "YouTube Data API v3" search karo → Enable karo<br>
      4. Credentials → Create API Key → Copy karo<br>
      5. Upar paste karo ✅
    </div>
  </div>

  <!-- Add Video Section -->
  <div id="add-section">
    <h2>➕ Video Add Karo</h2>
    <div class="input-row">
      <input type="text" id="video-url-input" placeholder="YouTube video URL ya Video ID paste karo..." />
      <button class="btn-green" onclick="addVideo()">Add Video</button>
      <button class="btn-secondary" onclick="refreshAll()">🔄 Refresh All</button>
    </div>
  </div>

  <!-- Stats Bar -->
  <div id="stats-bar">
    <div class="stat-card">
      <div class="stat-label">Total Videos</div>
      <div class="stat-value purple" id="stat-total">0</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Total Views</div>
      <div class="stat-value red" id="stat-views">0</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Total Likes</div>
      <div class="stat-value green" id="stat-likes">0</div>
    </div>
    <div class="stat-card">
      <div class="stat-label">Last Updated</div>
      <div class="stat-value" id="stat-time" style="font-size:0.85rem; color:#6b6b8a">--:--</div>
    </div>
  </div>

  <!-- Empty State -->
  <div id="empty-state">
    <div class="icon">📺</div>
    <h3>Koi video nahi hai abhi</h3>
    <p>Upar YouTube video URL paste karo aur Add karo</p>
  </div>

  <!-- Videos Grid -->
  <div id="videos-grid"></div>

</div>

<!-- Refresh Bar -->
<div id="refresh-bar">
  <span id="refresh-label">Next refresh in <strong id="countdown">30</strong>s</span>
  <div class="progress-track">
    <div class="progress-fill" id="progress-fill" style="width:100%"></div>
  </div>
  <button class="btn-secondary btn-small" onclick="refreshAll()" style="width:auto; padding: 6px 14px; font-size:0.75rem">Refresh Now</button>
</div>

<script>
  let apiKey = localStorage.getItem('yt_api_key') || '';
  let videos = JSON.parse(localStorage.getItem('yt_videos') || '[]');
  let refreshTimer = null;
  let countdownTimer = null;
  let countdownVal = 30;

  // Init
  window.onload = () => {
    if (apiKey) {
      showDashboard();
      if (videos.length > 0) {
        renderAll();
        refreshAll();
        startAutoRefresh();
      } else {
        showEmptyState();
      }
    }
  };

  function saveApiKey() {
    const key = document.getElementById('api-key-input').value.trim();
    if (!key) { showToast('API key daalo!', 'error'); return; }
    apiKey = key;
    localStorage.setItem('yt_api_key', key);
    showDashboard();
    showToast('API key save ho gayi! ✅', 'success');
    showEmptyState();
  }

  function showDashboard() {
    document.getElementById('setup-section').style.display = 'none';
    document.getElementById('add-section').style.display = 'block';
    document.getElementById('stats-bar').style.display = 'grid';
    document.getElementById('refresh-bar').style.display = 'flex';
  }

  function extractVideoId(url) {
    url = url.trim();
    // Already an ID (11 chars)
    if (/^[a-zA-Z0-9_-]{11}$/.test(url)) return url;
    // youtu.be/ID
    let m = url.match(/youtu\.be\/([a-zA-Z0-9_-]{11})/);
    if (m) return m[1];
    // youtube.com/watch?v=ID
    m = url.match(/[?&]v=([a-zA-Z0-9_-]{11})/);
    if (m) return m[1];
    // youtube.com/shorts/ID
    m = url.match(/shorts\/([a-zA-Z0-9_-]{11})/);
    if (m) return m[1];
    return null;
  }

  async function addVideo() {
    const input = document.getElementById('video-url-input');
    const url = input.value.trim();
    if (!url) { showToast('URL daalo!', 'error'); return; }

    const id = extractVideoId(url);
    if (!id) { showToast('Valid YouTube URL nahi hai!', 'error'); return; }

    if (videos.find(v => v.id === id)) {
      showToast('Yeh video pehle se hai!', 'error'); return;
    }

    input.value = '';
    showToast('Video fetch ho rahi hai...', 'success');

    const data = await fetchVideoData(id);
    if (!data) return;

    videos.push({ id, ...data });
    saveVideos();
    renderAll();
    updateStats();
    startAutoRefresh();
    document.getElementById('empty-state').style.display = 'none';
  }

  async function fetchVideoData(videoId) {
    try {
      const url = `https://www.googleapis.com/youtube/v3/videos?part=snippet,statistics&id=${videoId}&key=${apiKey}`;
      const res = await fetch(url);
      const data = await res.json();

      if (data.error) {
        showToast('API Error: ' + data.error.message, 'error');
        return null;
      }

      if (!data.items || data.items.length === 0) {
        showToast('Video nahi mili! ID check karo.', 'error');
        return null;
      }

      const item = data.items[0];
      return {
        title: item.snippet.title,
        channel: item.snippet.channelTitle,
        thumbnail: item.snippet.thumbnails?.medium?.url || '',
        views: parseInt(item.statistics.viewCount || 0),
        likes: parseInt(item.statistics.likeCount || 0),
        comments: parseInt(item.statistics.commentCount || 0),
        playing: false
      };
    } catch (e) {
      showToast('Network error!', 'error');
      return null;
    }
  }

  async function refreshAll() {
    if (videos.length === 0) return;
    showToast('Refreshing...', 'success');

    for (let i = 0; i < videos.length; i++) {
      const data = await fetchVideoData(videos[i].id);
      if (data) {
        videos[i] = { id: videos[i].id, playing: videos[i].playing, ...data };
      }
    }

    saveVideos();
    renderAll();
    updateStats();
    resetCountdown();
  }

  function renderAll() {
    const grid = document.getElementById('videos-grid');
    grid.innerHTML = '';

    if (videos.length === 0) {
      showEmptyState(); return;
    }

    document.getElementById('empty-state').style.display = 'none';

    videos.forEach((v, i) => {
      const card = document.createElement('div');
      card.className = 'video-card';
      card.id = `card-${v.id}`;

      card.innerHTML = `
        <div class="thumb">
          ${v.playing
            ? `<iframe src="https://www.youtube.com/embed/${v.id}?autoplay=1" allow="autoplay; encrypted-media" allowfullscreen></iframe>`
            : `<img src="${v.thumbnail}" alt="${v.title}" />
               <div class="play-overlay" onclick="playVideo('${v.id}')">
                 <div class="play-btn">▶</div>
               </div>`
          }
        </div>
        <div class="video-info">
          <div class="video-title">${v.title}</div>
          <div class="metrics-row">
            <div class="metric">
              <div class="metric-label">👁 Views</div>
              <div class="metric-value views">${formatNum(v.views)}</div>
            </div>
            <div class="metric">
              <div class="metric-label">👍 Likes</div>
              <div class="metric-value likes">${formatNum(v.likes)}</div>
            </div>
            <div class="metric">
              <div class="metric-label">💬 Comments</div>
              <div class="metric-value comments">${formatNum(v.comments)}</div>
            </div>
          </div>
        </div>
        <div class="card-actions">
          <button class="btn-small btn-refresh" onclick="refreshSingle('${v.id}')">🔄 Refresh</button>
          <button class="btn-small btn-remove" onclick="removeVideo('${v.id}')">✕ Remove</button>
        </div>
      `;

      grid.appendChild(card);
    });

    updateStats();
  }

  function playVideo(id) {
    const idx = videos.findIndex(v => v.id === id);
    if (idx === -1) return;
    videos[idx].playing = true;
    saveVideos();
    renderAll();
  }

  async function refreshSingle(id) {
    const idx = videos.findIndex(v => v.id === id);
    if (idx === -1) return;
    showToast('Refreshing...', 'success');
    const data = await fetchVideoData(id);
    if (data) {
      videos[idx] = { id, playing: videos[idx].playing, ...data };
      saveVideos();
      renderAll();
      updateStats();
    }
  }

  function removeVideo(id) {
    videos = videos.filter(v => v.id !== id);
    saveVideos();
    renderAll();
    updateStats();
    if (videos.length === 0) showEmptyState();
    showToast('Video remove ho gayi', 'success');
  }

  function updateStats() {
    document.getElementById('stat-total').textContent = videos.length;
    document.getElementById('stat-views').textContent = formatNum(videos.reduce((s, v) => s + v.views, 0));
    document.getElementById('stat-likes').textContent = formatNum(videos.reduce((s, v) => s + v.likes, 0));
    const now = new Date();
    document.getElementById('stat-time').textContent = now.toLocaleTimeString('en-IN', { hour: '2-digit', minute: '2-digit' });
  }

  function formatNum(n) {
    if (n >= 1_000_000) return (n / 1_000_000).toFixed(1) + 'M';
    if (n >= 1_000) return (n / 1_000).toFixed(1) + 'K';
    return n.toString();
  }

  function saveVideos() {
    localStorage.setItem('yt_videos', JSON.stringify(videos));
  }

  function showEmptyState() {
    if (videos.length === 0) {
      document.getElementById('empty-state').style.display = 'block';
    }
  }

  // Auto refresh every 30 seconds
  function startAutoRefresh() {
    if (refreshTimer) return;
    refreshTimer = setInterval(refreshAll, 30000);
    startCountdown();
  }

  function startCountdown() {
    countdownVal = 30;
    if (countdownTimer) clearInterval(countdownTimer);
    countdownTimer = setInterval(() => {
      countdownVal--;
      document.getElementById('countdown').textContent = countdownVal;
      const pct = (countdownVal / 30) * 100;
      document.getElementById('progress-fill').style.width = pct + '%';
      if (countdownVal <= 0) resetCountdown();
    }, 1000);
  }

  function resetCountdown() {
    countdownVal = 30;
    document.getElementById('countdown').textContent = '30';
    document.getElementById('progress-fill').style.width = '100%';
    if (countdownTimer) clearInterval(countdownTimer);
    countdownTimer = null;
    startCountdown();
  }

  function showToast(msg, type = 'success') {
    const t = document.createElement('div');
    t.className = `toast ${type}`;
    t.textContent = msg;
    document.body.appendChild(t);
    setTimeout(() => t.remove(), 2500);
  }
</script>
</body>
</html>
