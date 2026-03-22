<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>FLUX — Influencer Command Center</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,700;1,300&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #080810;
    --surface: #0f0f1a;
    --surface2: #161625;
    --border: rgba(255,255,255,0.07);
    --coral: #FF3D5A;
    --coral-dim: rgba(255,61,90,0.15);
    --gold: #F5C842;
    --mint: #3DFFC0;
    --mint-dim: rgba(61,255,192,0.12);
    --text: #F0EEF8;
    --muted: rgba(240,238,248,0.45);
    --font-display: 'Bebas Neue', sans-serif;
    --font-body: 'DM Sans', sans-serif;
    --font-mono: 'JetBrains Mono', monospace;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Ambient background glow */
  body::before {
    content: '';
    position: fixed;
    top: -200px; left: -200px;
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(255,61,90,0.08) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }
  body::after {
    content: '';
    position: fixed;
    bottom: -200px; right: -100px;
    width: 500px; height: 500px;
    background: radial-gradient(circle, rgba(61,255,192,0.07) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }

  /* ── TOPBAR ── */
  .topbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 36px;
    border-bottom: 1px solid var(--border);
    position: relative;
    z-index: 10;
  }
  .logo {
    font-family: var(--font-display);
    font-size: 2rem;
    letter-spacing: 0.12em;
    color: var(--text);
  }
  .logo span { color: var(--coral); }

  .profile-pill {
    display: flex;
    align-items: center;
    gap: 12px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 40px;
    padding: 8px 16px 8px 8px;
    cursor: pointer;
    transition: border-color 0.2s;
  }
  .profile-pill:hover { border-color: var(--coral); }
  .avatar {
    width: 36px; height: 36px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--coral), var(--gold));
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 0.85rem;
  }
  .profile-pill .name { font-size: 0.85rem; font-weight: 500; }
  .profile-pill .handle { font-size: 0.72rem; color: var(--muted); font-family: var(--font-mono); }

  .topbar-right { display: flex; align-items: center; gap: 16px; }
  .platform-badge {
    display: flex; align-items: center; gap: 6px;
    background: var(--surface2); border: 1px solid var(--border);
    border-radius: 20px; padding: 6px 14px;
    font-size: 0.78rem; font-family: var(--font-mono);
    color: var(--muted);
  }
  .platform-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--mint); box-shadow: 0 0 8px var(--mint); }

  /* ── MAIN LAYOUT ── */
  .main {
    display: grid;
    grid-template-columns: 1fr 320px;
    grid-template-rows: auto auto auto;
    gap: 20px;
    padding: 28px 36px;
    position: relative;
    z-index: 1;
    max-width: 1400px;
  }

  /* ── STAT CARDS ── */
  .stats-row {
    grid-column: 1 / -1;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 22px 24px;
    position: relative;
    overflow: hidden;
    transition: transform 0.2s, border-color 0.2s;
    cursor: default;
  }
  .stat-card:hover { transform: translateY(-3px); border-color: rgba(255,255,255,0.15); }
  .stat-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
  }
  .stat-card.coral::before { background: linear-gradient(90deg, var(--coral), transparent); }
  .stat-card.gold::before  { background: linear-gradient(90deg, var(--gold), transparent); }
  .stat-card.mint::before  { background: linear-gradient(90deg, var(--mint), transparent); }
  .stat-card.purple::before { background: linear-gradient(90deg, #9B5DE5, transparent); }

  .stat-label { font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.12em; color: var(--muted); margin-bottom: 10px; }
  .stat-value { font-family: var(--font-display); font-size: 2.4rem; line-height: 1; letter-spacing: 0.03em; }
  .stat-card.coral .stat-value { color: var(--coral); }
  .stat-card.gold  .stat-value { color: var(--gold); }
  .stat-card.mint  .stat-value { color: var(--mint); }
  .stat-card.purple .stat-value { color: #9B5DE5; }

  .stat-delta {
    margin-top: 8px;
    font-size: 0.78rem;
    font-family: var(--font-mono);
    display: flex; align-items: center; gap: 4px;
  }
  .delta-up   { color: var(--mint); }
  .delta-down { color: var(--coral); }

  /* ── CHART ── */
  .chart-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 28px;
    grid-column: 1;
  }
  .card-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 24px;
  }
  .card-title {
    font-family: var(--font-display);
    font-size: 1.4rem;
    letter-spacing: 0.06em;
  }
  .period-tabs {
    display: flex; gap: 4px;
    background: var(--surface2);
    border-radius: 8px;
    padding: 3px;
  }
  .period-tab {
    padding: 5px 12px;
    font-size: 0.75rem;
    font-family: var(--font-mono);
    border-radius: 6px;
    border: none;
    background: transparent;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.2s;
  }
  .period-tab.active { background: var(--coral); color: #fff; }
  .period-tab:hover:not(.active) { color: var(--text); }

  .chart-wrap { height: 240px; position: relative; }

  /* ── HASHTAG PANEL ── */
  .hashtag-panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 24px;
    grid-column: 2;
    grid-row: 2 / 4;
  }
  .hashtag-list { display: flex; flex-direction: column; gap: 10px; margin-top: 4px; }
  .hashtag-item {
    display: flex; align-items: center; justify-content: space-between;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 11px 14px;
    cursor: pointer;
    transition: all 0.2s;
    position: relative;
    overflow: hidden;
  }
  .hashtag-item:hover { border-color: var(--coral); }
  .hashtag-item .bar {
    position: absolute; left: 0; top: 0; bottom: 0;
    background: var(--coral-dim);
    transition: width 0.6s ease;
    border-radius: 10px;
  }
  .hashtag-item .tag-name {
    font-family: var(--font-mono);
    font-size: 0.82rem;
    color: var(--coral);
    position: relative;
    z-index: 1;
  }
  .hashtag-item .tag-meta {
    display: flex; align-items: center; gap: 8px;
    position: relative; z-index: 1;
  }
  .tag-posts { font-size: 0.72rem; color: var(--muted); font-family: var(--font-mono); }
  .tag-trend {
    font-size: 0.7rem;
    padding: 2px 7px;
    border-radius: 20px;
    font-weight: 600;
    letter-spacing: 0.04em;
  }
  .tag-trend.hot    { background: rgba(255,61,90,0.2); color: var(--coral); }
  .tag-trend.rising { background: rgba(245,200,66,0.2); color: var(--gold); }
  .tag-trend.new    { background: rgba(61,255,192,0.15); color: var(--mint); }

  .search-input-wrap {
    position: relative;
    margin-bottom: 16px;
  }
  .search-input-wrap input {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 14px 10px 36px;
    color: var(--text);
    font-family: var(--font-body);
    font-size: 0.85rem;
    outline: none;
    transition: border-color 0.2s;
  }
  .search-input-wrap input::placeholder { color: var(--muted); }
  .search-input-wrap input:focus { border-color: var(--coral); }
  .search-icon {
    position: absolute; left: 12px; top: 50%; transform: translateY(-50%);
    color: var(--muted); font-size: 0.85rem; pointer-events: none;
  }

  /* ── FEED ── */
  .feed-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 28px;
    grid-column: 1;
  }
  .feed-list { display: flex; flex-direction: column; gap: 12px; }
  .feed-item {
    display: flex; align-items: flex-start; gap: 14px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 14px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .feed-item:hover { border-color: rgba(255,255,255,0.14); transform: translateX(4px); }
  .feed-thumb {
    width: 52px; height: 52px;
    border-radius: 10px;
    flex-shrink: 0;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.4rem;
  }
  .feed-body { flex: 1; min-width: 0; }
  .feed-caption { font-size: 0.85rem; line-height: 1.45; margin-bottom: 8px; }
  .feed-caption .hashtag { color: var(--coral); font-weight: 500; }
  .feed-stats { display: flex; gap: 16px; }
  .feed-stat { font-size: 0.75rem; color: var(--muted); font-family: var(--font-mono); display: flex; align-items: center; gap: 4px; }
  .feed-time { font-size: 0.72rem; color: var(--muted); font-family: var(--font-mono); white-space: nowrap; margin-left: auto; flex-shrink: 0; }

  /* ── HINTS ── */
  .hints-row {
    grid-column: 1 / -1;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
  }
  .hint-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 22px;
    display: flex; gap: 16px;
    align-items: flex-start;
    transition: all 0.2s;
    cursor: default;
  }
  .hint-card:hover { transform: translateY(-3px); border-color: rgba(255,255,255,0.14); }
  .hint-icon {
    width: 42px; height: 42px; border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 1.2rem;
    flex-shrink: 0;
  }
  .hint-icon.a { background: var(--coral-dim); }
  .hint-icon.b { background: rgba(245,200,66,0.15); }
  .hint-icon.c { background: var(--mint-dim); }
  .hint-icon.d { background: rgba(155,93,229,0.15); }
  .hint-icon.e { background: rgba(61,150,255,0.15); }
  .hint-icon.f { background: rgba(255,150,61,0.15); }
  .hint-title { font-weight: 600; font-size: 0.9rem; margin-bottom: 5px; }
  .hint-body { font-size: 0.8rem; color: var(--muted); line-height: 1.5; }
  .hint-impact {
    margin-top: 10px;
    font-size: 0.72rem;
    font-family: var(--font-mono);
    color: var(--mint);
    letter-spacing: 0.04em;
  }

  /* ── SECTION TITLE ── */
  .section-label {
    grid-column: 1 / -1;
    display: flex; align-items: center; gap: 12px;
    font-family: var(--font-display);
    font-size: 1rem;
    letter-spacing: 0.14em;
    color: var(--muted);
    text-transform: uppercase;
    margin-top: 4px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(18px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  .stat-card, .chart-card, .hashtag-panel, .feed-card, .hint-card {
    animation: fadeUp 0.5s ease both;
  }
  .stat-card:nth-child(1) { animation-delay: 0.05s; }
  .stat-card:nth-child(2) { animation-delay: 0.10s; }
  .stat-card:nth-child(3) { animation-delay: 0.15s; }
  .stat-card:nth-child(4) { animation-delay: 0.20s; }
  .chart-card  { animation-delay: 0.25s; }
  .hashtag-panel { animation-delay: 0.30s; }
  .feed-card   { animation-delay: 0.35s; }
  .hint-card   { animation-delay: 0.40s; }

  /* scrollbar */
  ::-webkit-scrollbar { width: 5px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--surface2); border-radius: 4px; }

  @media (max-width: 900px) {
    .main { grid-template-columns: 1fr; }
    .stats-row { grid-template-columns: repeat(2, 1fr); }
    .hints-row { grid-template-columns: 1fr; }
    .hashtag-panel { grid-column: 1; grid-row: auto; }
    .chart-card, .feed-card { grid-column: 1; }
  }
</style>
</head>
<body>

<!-- TOPBAR -->
<header class="topbar">
  <div class="logo">FLUX<span>.</span></div>
  <div class="topbar-right">
    <div class="platform-badge">
      <span class="platform-dot"></span>
      Live Data
    </div>
    <div class="profile-pill">
      <div class="avatar">AK</div>
      <div>
        <div class="name">Ava Kim</div>
        <div class="handle">@ava.creates</div>
      </div>
    </div>
  </div>
</header>

<!-- MAIN GRID -->
<div class="main">

  <!-- STATS ROW -->
  <div class="stats-row">
    <div class="stat-card coral">
      <div class="stat-label">Total Followers</div>
      <div class="stat-value" id="stat-followers">248K</div>
      <div class="stat-delta delta-up">▲ +3,412 this week</div>
    </div>
    <div class="stat-card gold">
      <div class="stat-label">Avg Engagement</div>
      <div class="stat-value">6.8%</div>
      <div class="stat-delta delta-up">▲ +0.4% vs last week</div>
    </div>
    <div class="stat-card mint">
      <div class="stat-label">Reach / Post</div>
      <div class="stat-value">42K</div>
      <div class="stat-delta delta-down">▼ –1.2K vs avg</div>
    </div>
    <div class="stat-card purple">
      <div class="stat-label">Profile Visits</div>
      <div class="stat-value">18K</div>
      <div class="stat-delta delta-up">▲ +22% this week</div>
    </div>
  </div>

  <!-- GROWTH CHART -->
  <div class="chart-card">
    <div class="card-header">
      <div class="card-title">FOLLOWER GROWTH</div>
      <div class="period-tabs">
        <button class="period-tab active" onclick="switchPeriod(this,'7d')">7D</button>
        <button class="period-tab" onclick="switchPeriod(this,'30d')">30D</button>
        <button class="period-tab" onclick="switchPeriod(this,'90d')">90D</button>
      </div>
    </div>
    <div class="chart-wrap">
      <canvas id="growthChart"></canvas>
    </div>
  </div>

  <!-- HASHTAG PANEL -->
  <div class="hashtag-panel">
    <div class="card-header" style="margin-bottom:16px;">
      <div class="card-title">TRENDING TAGS</div>
    </div>
    <div class="search-input-wrap">
      <span class="search-icon">#</span>
      <input type="text" id="tagSearch" placeholder="Search hashtags..." oninput="filterTags()"/>
    </div>
    <div class="hashtag-list" id="hashtagList">
      <!-- filled by JS -->
    </div>
  </div>

  <!-- RECENT FEED -->
  <div class="feed-card">
    <div class="card-header">
      <div class="card-title">RECENT POSTS</div>
    </div>
    <div class="feed-list" id="feedList">
      <!-- filled by JS -->
    </div>
  </div>

  <!-- HINTS -->
  <div class="section-label">Growth Hints</div>
  <div class="hints-row" id="hintsRow">
    <!-- filled by JS -->
  </div>

</div>

<script>
// ── DATA ──────────────────────────────────────────────────────────────────────

const chartData = {
  '7d': {
    labels: ['Mon','Tue','Wed','Thu','Fri','Sat','Sun'],
    data:   [244200, 244900, 245400, 245100, 246300, 247100, 248000]
  },
  '30d': {
    labels: ['W1','W2','W3','W4'],
    data:   [230000, 235000, 241000, 248000]
  },
  '90d': {
    labels: ['Jan','Feb','Mar'],
    data:   [198000, 224000, 248000]
  }
};

const hashtags = [
  { tag: '#contentcreator', posts: '4.2M', heat: 92, trend: 'hot' },
  { tag: '#aestheticlife',   posts: '2.8M', heat: 78, trend: 'hot' },
  { tag: '#dailyvlog',       posts: '1.9M', heat: 65, trend: 'rising' },
  { tag: '#moodboard2026',   posts: '840K', heat: 55, trend: 'new' },
  { tag: '#ugccreator',      posts: '3.1M', heat: 87, trend: 'hot' },
  { tag: '#slowliving',      posts: '1.1M', heat: 48, trend: 'rising' },
  { tag: '#minimalistlife',  posts: '2.3M', heat: 60, trend: 'rising' },
  { tag: '#digitalart2026',  posts: '560K', heat: 40, trend: 'new' },
];

const feedItems = [
  { emoji: '🌿', bg: '#1a2b1e', caption: 'Morning routine reset ✨ New video up — link in bio!', tags: ['#morningroutine', '#wellness'], likes: '12.4K', comments: '348', shares: '912', time: '2h ago' },
  { emoji: '📸', bg: '#1a1a2b', caption: 'Behind the lens 🎞️ How I edit my photos for that dreamy look', tags: ['#photography', '#aestheticlife'], likes: '9.8K', comments: '215', shares: '640', time: '1d ago' },
  { emoji: '☕', bg: '#2b1e14', caption: 'Slow mornings are my superpower. No apologies.', tags: ['#slowliving', '#contentcreator'], likes: '22.1K', comments: '780', shares: '3.1K', time: '2d ago' },
  { emoji: '🎨', bg: '#1e1428', caption: 'New collab dropping soon… can you guess who? 👀', tags: ['#ugccreator', '#collab'], likes: '31K', comments: '1.4K', shares: '5.8K', time: '4d ago' },
];

const hints = [
  { icon: '⏱️', cls: 'a', title: 'Post at Peak Hours', body: 'Your audience is most active between 7–9 PM. Posting in this window can lift reach by up to 40%.', impact: '↑ +35% reach potential' },
  { icon: '🔁', cls: 'b', title: 'Reels Over Carousels', body: 'Reels are getting 2× the organic reach right now. Try converting your top carousels into short videos.', impact: '↑ +2× organic reach' },
  { icon: '#',  cls: 'c', title: 'Mix Hashtag Sizes', body: 'Combine 2 mega tags (1M+), 3 mid-tier (100K–500K), and 5 niche (<50K) per post for best discovery.', impact: '↑ +28% discoverability' },
  { icon: '💬', cls: 'd', title: 'Reply in First Hour', body: 'Replying to comments within 60 min of posting signals high engagement to the algorithm.', impact: '↑ +20% algorithm boost' },
  { icon: '📌', cls: 'e', title: 'Pin Your Best Post', body: 'Your collab post has 31K likes — pin it to your profile to convert profile visitors to followers.', impact: '↑ +15% follow rate' },
  { icon: '🤝', cls: 'f', title: 'Collab Post Strategy', body: 'You have high engagement on collabs. Aim for one collab per week with accounts in the 50K–300K range.', impact: '↑ +500–2K new followers/wk' },
];

// ── CHART ─────────────────────────────────────────────────────────────────────

const ctx = document.getElementById('growthChart').getContext('2d');

const gradient = ctx.createLinearGradient(0, 0, 0, 240);
gradient.addColorStop(0,   'rgba(255,61,90,0.25)');
gradient.addColorStop(1,   'rgba(255,61,90,0)');

let chart = new Chart(ctx, {
  type: 'line',
  data: {
    labels: chartData['7d'].labels,
    datasets: [{
      label: 'Followers',
      data: chartData['7d'].data,
      borderColor: '#FF3D5A',
      borderWidth: 2.5,
      pointBackgroundColor: '#FF3D5A',
      pointRadius: 4,
      pointHoverRadius: 6,
      fill: true,
      backgroundColor: gradient,
      tension: 0.42,
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    interaction: { mode: 'index', intersect: false },
    plugins: {
      legend: { display: false },
      tooltip: {
        backgroundColor: '#161625',
        borderColor: 'rgba(255,255,255,0.1)',
        borderWidth: 1,
        titleColor: '#F0EEF8',
        bodyColor: '#FF3D5A',
        titleFont: { family: 'DM Sans', size: 12 },
        bodyFont: { family: 'JetBrains Mono', size: 13 },
        callbacks: {
          label: ctx => '  ' + ctx.parsed.y.toLocaleString() + ' followers'
        }
      }
    },
    scales: {
      x: {
        grid: { color: 'rgba(255,255,255,0.05)' },
        ticks: { color: 'rgba(240,238,248,0.45)', font: { family: 'JetBrains Mono', size: 11 } },
        border: { display: false }
      },
      y: {
        grid: { color: 'rgba(255,255,255,0.05)' },
        ticks: {
          color: 'rgba(240,238,248,0.45)',
          font: { family: 'JetBrains Mono', size: 11 },
          callback: v => (v/1000).toFixed(0) + 'K'
        },
        border: { display: false }
      }
    }
  }
});

function switchPeriod(btn, period) {
  document.querySelectorAll('.period-tab').forEach(t => t.classList.remove('active'));
  btn.classList.add('active');
  chart.data.labels = chartData[period].labels;
  chart.data.datasets[0].data = chartData[period].data;
  chart.update('active');
}

// ── HASHTAGS ──────────────────────────────────────────────────────────────────

function renderTags(list) {
  const el = document.getElementById('hashtagList');
  el.innerHTML = list.map(h => `
    <div class="hashtag-item" onclick="copyTag('${h.tag}')">
      <div class="bar" style="width:${h.heat}%"></div>
      <span class="tag-name">${h.tag}</span>
      <div class="tag-meta">
        <span class="tag-posts">${h.posts}</span>
        <span class="tag-trend ${h.trend}">${h.trend.toUpperCase()}</span>
      </div>
    </div>
  `).join('');
}

function filterTags() {
  const q = document.getElementById('tagSearch').value.toLowerCase();
  renderTags(hashtags.filter(h => h.tag.includes(q)));
}

function copyTag(tag) {
  navigator.clipboard?.writeText(tag);
  // flash
  const items = document.querySelectorAll('.hashtag-item');
  items.forEach(i => { if(i.innerText.includes(tag.slice(1))) {
    i.style.borderColor='var(--mint)';
    setTimeout(()=>i.style.borderColor='',600);
  }});
}

renderTags(hashtags);

// ── FEED ──────────────────────────────────────────────────────────────────────

document.getElementById('feedList').innerHTML = feedItems.map(f => `
  <div class="feed-item">
    <div class="feed-thumb" style="background:${f.bg}">${f.emoji}</div>
    <div class="feed-body">
      <div class="feed-caption">
        ${f.caption} ${f.tags.map(t=>`<span class="hashtag">${t}</span>`).join(' ')}
      </div>
      <div class="feed-stats">
        <span class="feed-stat">❤️ ${f.likes}</span>
        <span class="feed-stat">💬 ${f.comments}</span>
        <span class="feed-stat">↗ ${f.shares}</span>
      </div>
    </div>
    <div class="feed-time">${f.time}</div>
  </div>
`).join('');

// ── HINTS ─────────────────────────────────────────────────────────────────────

document.getElementById('hintsRow').innerHTML = hints.map(h => `
  <div class="hint-card">
    <div class="hint-icon ${h.cls}">${h.icon}</div>
    <div>
      <div class="hint-title">${h.title}</div>
      <div class="hint-body">${h.body}</div>
      <div class="hint-impact">${h.impact}</div>
    </div>
  </div>
`).join('');

</script>
</body>
</html>
