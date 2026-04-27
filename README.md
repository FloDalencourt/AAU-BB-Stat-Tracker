<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>🏀 Stat Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;700;900&family=Barlow:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --orange: #FF6B35;
    --orange2: #F7931E;
    --gold: #FFD700;
    --green: #4CAF50;
    --green2: #81C784;
    --blue: #2196F3;
    --purple: #9C27B0;
    --cyan: #00BCD4;
    --red: #F44336;
    --gray: #9E9E9E;
    --bg: #0a0f1e;
    --card: #111827;
    --border: #1e293b;
    --muted: #64748b;
    --text2: #94a3b8;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  body {
    background: var(--bg);
    color: #fff;
    font-family: 'Barlow', sans-serif;
    max-width: 480px;
    margin: 0 auto;
    min-height: 100vh;
    overflow-x: hidden;
  }
  h1,h2,h3 { font-family: 'Barlow Condensed', sans-serif; }

  /* HEADER */
  .header {
    display: flex; justify-content: space-between; align-items: center;
    padding: 14px 16px;
    background: var(--card);
    border-bottom: 1px solid var(--border);
    position: sticky; top: 0; z-index: 100;
  }
  .header-left { display: flex; align-items: center; gap: 10px; }
  .ball { font-size: 22px; }
  .player-name {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 18px; font-weight: 700;
    cursor: pointer; letter-spacing: 0.5px;
  }
  .player-name:hover { color: var(--orange); }
  #nameInput {
    background: transparent; border: none;
    border-bottom: 2px solid var(--orange);
    color: #fff; font-size: 16px; font-weight: 700;
    font-family: 'Barlow Condensed', sans-serif;
    outline: none; width: 160px;
  }
  .nav { display: flex; gap: 6px; align-items: center; }
  .nav-btn {
    background: transparent; border: 1px solid #2a3a5e;
    color: var(--text2); padding: 6px 12px;
    border-radius: 8px; cursor: pointer; font-size: 13px;
    font-family: 'Barlow', sans-serif;
    transition: all 0.15s;
  }
  .nav-btn:hover { background: var(--border); color: #fff; }
  .nav-btn.active { background: var(--border); color: #fff; border-color: var(--orange); }
  .nav-btn.live {
    background: rgba(255,107,53,0.15); color: var(--orange);
    border-color: var(--orange); font-weight: 700; font-size: 11px;
    animation: pulse-border 1.5s infinite;
  }
  @keyframes pulse-border {
    0%,100% { box-shadow: 0 0 0 0 rgba(255,107,53,0.4); }
    50% { box-shadow: 0 0 0 4px rgba(255,107,53,0); }
  }

  /* VIEWS */
  .view { display: none; padding: 14px; }
  .view.active { display: block; }

  /* CARDS */
  .card {
    background: var(--card); border-radius: 14px;
    padding: 18px; margin-bottom: 14px;
    border: 1px solid var(--border);
  }
  .card-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 13px; font-weight: 700; color: var(--text2);
    text-transform: uppercase; letter-spacing: 1.5px;
    margin-bottom: 14px;
  }
  .empty { color: var(--muted); text-align: center; padding: 12px 0; font-size: 14px; }

  /* AVG GRID */
  .avg-grid { display: flex; gap: 6px; flex-wrap: wrap; justify-content: space-between; }
  .avg-item { display: flex; flex-direction: column; align-items: center; flex: 1; min-width: 50px; }
  .avg-val { font-family: 'Barlow Condensed', sans-serif; font-size: 28px; font-weight: 900; color: var(--orange); line-height: 1; }
  .avg-label { font-size: 9px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; margin-top: 3px; }
  .games-played { color: var(--muted); font-size: 12px; text-align: center; margin-top: 12px; }

  /* INPUTS */
  input[type="text"] {
    width: 100%; padding: 11px 13px; border-radius: 10px;
    background: #1e293b; border: 1px solid #2a3a5e;
    color: #fff; font-size: 15px; margin-bottom: 10px;
    outline: none; font-family: 'Barlow', sans-serif;
    transition: border-color 0.2s;
  }
  input[type="text"]:focus { border-color: var(--orange); }

  /* BUTTONS */
  .btn { border: none; border-radius: 10px; cursor: pointer; font-family: 'Barlow Condensed', sans-serif; font-weight: 700; letter-spacing: 0.5px; transition: transform 0.1s, opacity 0.1s; }
  .btn:active { transform: scale(0.96); opacity: 0.9; }
  .btn-primary { background: var(--orange); color: #fff; padding: 13px 20px; font-size: 17px; width: 100%; }
  .btn-danger { background: var(--red); color: #fff; padding: 12px 18px; font-size: 15px; flex: 1; }
  .btn-undo { background: #1e293b; color: var(--text2); border: 1px solid #2a3a5e; padding: 12px 18px; font-size: 15px; }
  .btn-undo:disabled { opacity: 0.35; cursor: not-allowed; }
  .btn-row { display: flex; gap: 10px; }

  /* LIVE GAME */
  .game-header {
    display: flex; justify-content: space-between; align-items: center;
    background: var(--card); border-radius: 14px; padding: 14px 16px;
    margin-bottom: 10px; border: 1px solid var(--border);
  }
  .game-vs { font-family: 'Barlow Condensed', sans-serif; font-size: 20px; font-weight: 700; }
  .game-date { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .game-score { font-family: 'Barlow Condensed', sans-serif; font-size: 56px; font-weight: 900; color: var(--orange); line-height: 1; }

  .quick-stats {
    display: flex; gap: 4px; flex-wrap: wrap;
    background: var(--card); border-radius: 12px; padding: 10px 10px;
    margin-bottom: 10px; border: 1px solid var(--border);
  }
  .quick-stat { display: flex; flex-direction: column; align-items: center; flex: 1; min-width: 40px; }
  .quick-val { font-family: 'Barlow Condensed', sans-serif; font-size: 20px; font-weight: 800; line-height: 1; }
  .quick-label { font-size: 8px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; margin-top: 2px; }

  .ft-line { text-align: center; font-size: 12px; color: var(--muted); margin-bottom: 10px; letter-spacing: 0.5px; }

  /* STAT GRID */
  .stat-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 10px; }
  .stat-btn {
    background: var(--card); border: 2px solid;
    border-radius: 12px; padding: 15px 6px;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 15px; font-weight: 800; cursor: pointer;
    letter-spacing: 1px; transition: transform 0.08s, background 0.1s;
    line-height: 1;
  }
  .stat-btn:active { transform: scale(0.93); }
  .stat-btn:hover { filter: brightness(1.15); }

  /* LOG */
  .log-box { background: var(--card); border-radius: 12px; padding: 12px; border: 1px solid var(--border); margin-top: 10px; }
  .log-title { font-size: 10px; color: #475569; text-transform: uppercase; letter-spacing: 1.5px; margin-bottom: 8px; font-family: 'Barlow Condensed', sans-serif; }
  .log-entry { display: flex; justify-content: space-between; padding: 5px 0; border-bottom: 1px solid var(--border); font-size: 13px; }
  .log-entry:last-child { border-bottom: none; }
  .log-time { color: #475569; font-size: 11px; }

  /* HISTORY */
  .section-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 16px; font-weight: 700; color: var(--text2);
    text-transform: uppercase; letter-spacing: 1.5px; margin-bottom: 12px;
  }
  .history-card { background: var(--card); border-radius: 14px; padding: 14px; margin-bottom: 10px; border: 1px solid var(--border); }
  .history-top { display: flex; align-items: center; margin-bottom: 10px; }
  .history-opp { font-family: 'Barlow Condensed', sans-serif; font-size: 18px; font-weight: 700; flex: 1; }
  .history-date { color: var(--muted); font-size: 12px; margin-right: 8px; }
  .delete-btn { background: transparent; border: none; color: #475569; cursor: pointer; font-size: 16px; padding: 0 4px; }
  .delete-btn:hover { color: var(--red); }
  .history-stats { display: flex; gap: 4px; flex-wrap: wrap; margin-bottom: 8px; }
  .hist-stat { display: flex; flex-direction: column; align-items: center; flex: 1; min-width: 38px; }
  .hist-val { font-family: 'Barlow Condensed', sans-serif; font-size: 22px; font-weight: 800; color: var(--orange); line-height: 1; }
  .hist-label { font-size: 8px; color: var(--muted); letter-spacing: 1px; text-transform: uppercase; margin-top: 2px; }
  .history-ft { font-size: 11px; color: #475569; border-top: 1px solid var(--border); padding-top: 8px; }

  /* FLASH */
  #flash {
    position: fixed; top: 0; left: 0; right: 0; bottom: 0;
    display: flex; align-items: center; justify-content: center;
    pointer-events: none; z-index: 999; opacity: 0;
    transition: opacity 0.1s;
  }
  #flash.show { opacity: 1; }
  #flash span {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 72px; font-weight: 900; color: var(--orange);
    text-shadow: 0 0 60px rgba(255,107,53,0.9);
  }

  /* RESUME CARD */
  .live-pts { font-family: 'Barlow Condensed', sans-serif; font-size: 52px; font-weight: 900; color: var(--orange); margin: 6px 0; }
  .vs-text { color: var(--text2); margin-bottom: 8px; font-size: 14px; }

  /* SETUP NOTICE */
  .setup-box {
    background: #1a1f2e; border: 1px solid #2a3a5e;
    border-radius: 12px; padding: 14px; margin-bottom: 14px;
    font-size: 13px; color: var(--text2); line-height: 1.6;
  }
  .setup-box a { color: var(--orange); }
  .setup-box strong { color: #fff; }

  /* STORAGE MODE BADGE */
  .storage-badge {
    display: inline-flex; align-items: center; gap: 5px;
    background: #1e293b; border-radius: 6px; padding: 4px 10px;
    font-size: 11px; color: var(--muted); margin-bottom: 14px;
  }
  .storage-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--green); }
  .storage-dot.local { background: var(--gold); }

  @media (max-width: 360px) {
    .stat-btn { font-size: 13px; padding: 12px 4px; }
    .avg-val { font-size: 22px; }
  }
</style>
</head>
<body>

<!-- FLASH -->
<div id="flash"><span id="flashText"></span></div>

<!-- HEADER -->
<div class="header">
  <div class="header-left">
    <span class="ball">🏀</span>
    <span class="player-name" id="playerNameDisplay" onclick="editName()">Loading…</span>
    <input type="text" id="nameInput" style="display:none;width:150px;margin:0" onblur="saveName()" onkeydown="if(event.key==='Enter')saveName()">
  </div>
  <div class="nav">
    <button class="nav-btn active" id="navHome" onclick="showView('home')">🏠</button>
    <button class="nav-btn" id="navHistory" onclick="showView('history')">📋</button>
    <button class="nav-btn live" id="navLive" style="display:none" onclick="showView('game')">🔴 LIVE</button>
  </div>
</div>

<!-- HOME VIEW -->
<div class="view active" id="view-home">
  <div class="card">
    <div class="card-title">Season Averages</div>
    <div id="avgContent"><p class="empty">No games yet — start tracking!</p></div>
    <p class="games-played" id="gamesPlayed"></p>
  </div>

  <div id="liveCard" style="display:none" class="card">
    <div class="card-title">Game In Progress</div>
    <p class="vs-text" id="liveCardVs"></p>
    <div class="live-pts" id="liveCardPts">0</div>
    <div class="btn-row" style="margin-top:10px">
      <button class="btn btn-primary" style="font-size:15px" onclick="showView('game')">Resume Tracking</button>
      <button class="btn btn-danger" onclick="endGame()">End Game</button>
    </div>
  </div>

  <div id="newGameCard" class="card">
    <div class="card-title">New Game</div>
    <input type="text" id="opponentInput" placeholder="Opponent name (optional)">
    <button class="btn btn-primary" onclick="startGame()">🏀 Start Game</button>
  </div>

  <div class="setup-box" id="storageNotice" style="display:none">
    ⚡ <strong>Local mode:</strong> Stats are saved on this device only.
    To share live stats with others, see the
    <a href="#" onclick="showView('setup');return false">Setup Guide</a>.
  </div>
</div>

<!-- GAME VIEW -->
<div class="view" id="view-game">
  <div class="game-header">
    <div>
      <div class="game-vs" id="gameVs">vs. —</div>
      <div class="game-date" id="gameDate"></div>
    </div>
    <div class="game-score" id="gamePts">0</div>
  </div>

  <div class="quick-stats" id="quickStats"></div>
  <div class="ft-line" id="ftLine"></div>

  <div class="stat-grid" id="statGrid"></div>

  <div class="btn-row" style="margin-bottom:10px">
    <button class="btn btn-undo" id="undoBtn" onclick="undoLast()" disabled>↩ Undo</button>
    <button class="btn btn-danger" onclick="endGame()">End Game</button>
  </div>

  <div class="log-box" id="logBox" style="display:none">
    <div class="log-title">Recent Actions</div>
    <div id="logEntries"></div>
  </div>
</div>

<!-- HISTORY VIEW -->
<div class="view" id="view-history">
  <h2 class="section-title">Game History</h2>
  <div id="historyList"></div>
</div>

<!-- SETUP VIEW -->
<div class="view" id="view-setup">
  <h2 class="section-title">Share Your Tracker</h2>
  <div class="card">
    <div class="card-title">Step 1 — Create a Free Firebase Database</div>
    <p style="font-size:13px;color:var(--text2);line-height:1.7;margin-bottom:10px">
      1. Go to <strong><a href="https://console.firebase.google.com" target="_blank" style="color:var(--orange)">console.firebase.google.com</a></strong><br>
      2. Click <strong>"Add project"</strong> → give it any name → click through<br>
      3. In the left menu, click <strong>"Realtime Database"</strong><br>
      4. Click <strong>"Create Database"</strong> → choose any location → start in <strong>test mode</strong><br>
      5. Copy the database URL — it looks like:<br>
      <code style="background:#1e293b;padding:2px 6px;border-radius:4px;font-size:12px">https://YOUR-PROJECT-default-rtdb.firebaseio.com</code>
    </p>
  </div>
  <div class="card">
    <div class="card-title">Step 2 — Paste Your Database URL</div>
    <input type="text" id="firebaseUrl" placeholder="https://your-project-default-rtdb.firebaseio.com">
    <button class="btn btn-primary" onclick="saveFirebaseUrl()">Save & Enable Sharing</button>
    <p style="font-size:12px;color:var(--muted);margin-top:10px">
      After saving, share this HTML file with anyone — they'll all see the same live stats.
    </p>
  </div>
  <div class="card" id="currentDbCard" style="display:none">
    <div class="card-title">Current Database</div>
    <p style="font-size:13px;color:var(--green);word-break:break-all" id="currentDbUrl"></p>
    <button class="btn" style="background:#1e293b;color:var(--red);margin-top:10px;padding:8px 14px;font-size:13px" onclick="clearFirebaseUrl()">Remove & Use Local Storage</button>
  </div>
</div>

<script>
// ─── STAT BUTTONS CONFIG ────────────────────────────────
const STAT_BUTTONS = [
  { key: "points2",     label: "2 PTS",    color: "#FF6B35" },
  { key: "points3",     label: "3 PTS",    color: "#F7931E" },
  { key: "ft",          label: "FT",       color: "#FFD700" },
  { key: "ftMiss",      label: "FT MISS",  color: "#9E9E9E" },
  { key: "offRebounds", label: "OFF REB",  color: "#4CAF50" },
  { key: "defRebounds", label: "DEF REB",  color: "#81C784" },
  { key: "assists",     label: "AST",      color: "#2196F3" },
  { key: "steals",      label: "STL",      color: "#9C27B0" },
  { key: "blocks",      label: "BLK",      color: "#00BCD4" },
  { key: "turnovers",   label: "TO",       color: "#F44336" },
  { key: "fouls",       label: "FOUL",     color: "#FF5722" },
];

// ─── STATE ──────────────────────────────────────────────
let state = {
  playerName: "My Daughter",
  games: [],
  currentGame: null,
};
let firebaseUrl = null;
let firebasePollTimer = null;

// ─── STORAGE ────────────────────────────────────────────
function getFirebaseUrl() {
  return localStorage.getItem("bball_fb_url") || null;
}
function saveFirebaseUrl() {
  const url = document.getElementById("firebaseUrl").value.trim().replace(/\/$/, "");
  if (!url.startsWith("https://")) {
    alert("Please enter a valid Firebase URL starting with https://");
    return;
  }
  localStorage.setItem("bball_fb_url", url);
  firebaseUrl = url;
  alert("✅ Firebase connected! Share this HTML file with others and they'll see live stats.");
  renderSetup();
  loadState();
}
function clearFirebaseUrl() {
  localStorage.removeItem("bball_fb_url");
  firebaseUrl = null;
  renderSetup();
}

async function loadState() {
  firebaseUrl = getFirebaseUrl();
  if (firebaseUrl) {
    try {
      const res = await fetch(firebaseUrl + "/bball.json");
      const data = await res.json();
      if (data) { state = data; }
    } catch(e) { loadLocal(); }
    startPolling();
  } else {
    loadLocal();
    document.getElementById("storageNotice").style.display = "block";
  }
  renderAll();
}

function loadLocal() {
  const raw = localStorage.getItem("bball_state");
  if (raw) { try { state = JSON.parse(raw); } catch(e) {} }
}

async function saveState() {
  firebaseUrl = getFirebaseUrl();
  if (firebaseUrl) {
    try {
      await fetch(firebaseUrl + "/bball.json", {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(state),
      });
    } catch(e) { localStorage.setItem("bball_state", JSON.stringify(state)); }
  } else {
    localStorage.setItem("bball_state", JSON.stringify(state));
  }
  renderAll();
}

function startPolling() {
  if (firebasePollTimer) clearInterval(firebasePollTimer);
  firebasePollTimer = setInterval(async () => {
    try {
      const res = await fetch(firebaseUrl + "/bball.json");
      const data = await res.json();
      if (data) { state = data; renderAll(); }
    } catch(e) {}
  }, 4000);
}

// ─── GAME LOGIC ─────────────────────────────────────────
function emptyGame(opponent) {
  return {
    id: Date.now(),
    date: new Date().toLocaleDateString(),
    opponent: opponent || "Opponent",
    points: 0,
    offRebounds: 0,
    defRebounds: 0,
    assists: 0,
    steals: 0,
    blocks: 0,
    turnovers: 0,
    fouls: 0,
    ftMade: 0,
    ftAttempts: 0,
    fgMade: 0,
    fgAttempts: 0,
    log: [],
  };
}

function startGame() {
  const opp = document.getElementById("opponentInput").value.trim();
  state.currentGame = emptyGame(opp);
  document.getElementById("opponentInput").value = "";
  saveState();
  showView("game");
}

function endGame() {
  if (!state.currentGame) return;
  state.games = [state.currentGame, ...(state.games || [])];
  state.currentGame = null;
  saveState();
  showView("history");
}

function recordStat(key) {
  const g = state.currentGame;
  if (!g) return;
  const entry = { key, time: new Date().toLocaleTimeString([], {hour:'2-digit',minute:'2-digit'}) };

  if (key === "points2") { g.points += 2; g.fgMade++; g.fgAttempts++; }
  else if (key === "points3") { g.points += 3; g.fgMade++; g.fgAttempts++; }
  else if (key === "ft") { g.points++; g.ftMade++; g.ftAttempts++; }
  else if (key === "ftMiss") { g.ftAttempts++; }
  else { g[key] = (g[key] || 0) + 1; }

  g.log = [entry, ...(g.log || [])];
  saveState();

  const btn = STAT_BUTTONS.find(b => b.key === key);
  showFlash(btn?.label || key);
}

function undoLast() {
  const g = state.currentGame;
  if (!g || !g.log || g.log.length === 0) return;
  const [last, ...rest] = g.log;
  const key = last.key;

  if (key === "points2") { g.points = Math.max(0,g.points-2); g.fgMade = Math.max(0,g.fgMade-1); g.fgAttempts = Math.max(0,g.fgAttempts-1); }
  else if (key === "points3") { g.points = Math.max(0,g.points-3); g.fgMade = Math.max(0,g.fgMade-1); g.fgAttempts = Math.max(0,g.fgAttempts-1); }
  else if (key === "ft") { g.points = Math.max(0,g.points-1); g.ftMade = Math.max(0,g.ftMade-1); g.ftAttempts = Math.max(0,g.ftAttempts-1); }
  else if (key === "ftMiss") { g.ftAttempts = Math.max(0,g.ftAttempts-1); }
  else { g[key] = Math.max(0,(g[key]||0)-1); }

  g.log = rest;
  saveState();
}

function deleteGame(id) {
  if (!confirm("Delete this game?")) return;
  state.games = state.games.filter(g => g.id !== id);
  saveState();
}

// ─── NAME EDIT ──────────────────────────────────────────
function editName() {
  const disp = document.getElementById("playerNameDisplay");
  const inp = document.getElementById("nameInput");
  disp.style.display = "none";
  inp.style.display = "inline-block";
  inp.value = state.playerName;
  inp.focus();
}
function saveName() {
  const inp = document.getElementById("nameInput");
  const disp = document.getElementById("playerNameDisplay");
  const val = inp.value.trim();
  if (val) state.playerName = val;
  inp.style.display = "none";
  disp.style.display = "inline";
  saveState();
}

// ─── FLASH ──────────────────────────────────────────────
let flashTimer = null;
function showFlash(text) {
  const el = document.getElementById("flash");
  document.getElementById("flashText").textContent = text;
  el.classList.add("show");
  if (flashTimer) clearTimeout(flashTimer);
  flashTimer = setTimeout(() => el.classList.remove("show"), 500);
}

// ─── VIEW NAVIGATION ────────────────────────────────────
function showView(name) {
  document.querySelectorAll(".view").forEach(v => v.classList.remove("active"));
  document.querySelectorAll(".nav-btn").forEach(b => b.classList.remove("active"));
  document.getElementById("view-" + name).classList.add("active");
  const nb = document.getElementById("nav" + name.charAt(0).toUpperCase() + name.slice(1));
  if (nb) nb.classList.add("active");
}

// ─── RENDER ─────────────────────────────────────────────
function renderAll() {
  const g = state.currentGame;
  const games = state.games || [];

  // Header name
  document.getElementById("playerNameDisplay").textContent = (state.playerName || "Player") + " ✏️";

  // Live nav button
  document.getElementById("navLive").style.display = g ? "inline-flex" : "none";

  // ── HOME ──
  const gp = games.length;
  if (gp === 0) {
    document.getElementById("avgContent").innerHTML = '<p class="empty">No games yet — start tracking!</p>';
    document.getElementById("gamesPlayed").textContent = "";
  } else {
    const tot = games.reduce((a,gm) => ({
      pts: a.pts + (gm.points||0),
      oreb: a.oreb + (gm.offRebounds||0),
      dreb: a.dreb + (gm.defRebounds||gm.rebounds||0),
      ast: a.ast + (gm.assists||0),
      stl: a.stl + (gm.steals||0),
      blk: a.blk + (gm.blocks||0),
    }), {pts:0,oreb:0,dreb:0,ast:0,stl:0,blk:0});
    const avg = k => (tot[k]/gp).toFixed(1);
    const reb = (tot.oreb+tot.dreb)/gp;
    const items = [
      {l:"PPG",v:avg("pts")},{l:"REB",v:reb.toFixed(1)},
      {l:"OREB",v:avg("oreb")},{l:"DREB",v:avg("dreb")},
      {l:"APG",v:avg("ast")},{l:"SPG",v:avg("stl")},{l:"BPG",v:avg("blk")},
    ];
    document.getElementById("avgContent").innerHTML =
      '<div class="avg-grid">' +
      items.map(i=>`<div class="avg-item"><span class="avg-val">${i.v}</span><span class="avg-label">${i.l}</span></div>`).join("") +
      "</div>";
    document.getElementById("gamesPlayed").textContent = `${gp} game${gp!==1?"s":""} recorded`;
  }

  // Live card on home
  if (g) {
    document.getElementById("liveCard").style.display = "block";
    document.getElementById("newGameCard").style.display = "none";
    document.getElementById("liveCardVs").textContent = `vs. ${g.opponent} — ${g.date}`;
    document.getElementById("liveCardPts").textContent = g.points + " PTS";
  } else {
    document.getElementById("liveCard").style.display = "none";
    document.getElementById("newGameCard").style.display = "block";
  }

  // ── GAME VIEW ──
  if (g) {
    document.getElementById("gameVs").textContent = "vs. " + g.opponent;
    document.getElementById("gameDate").textContent = g.date;
    document.getElementById("gamePts").textContent = g.points;

    const qs = [
      {l:"OREB",v:g.offRebounds||0},
      {l:"DREB",v:g.defRebounds||0},
      {l:"AST",v:g.assists||0},
      {l:"STL",v:g.steals||0},
      {l:"BLK",v:g.blocks||0},
      {l:"TO",v:g.turnovers||0},
      {l:"FOULS",v:g.fouls||0},
    ];
    document.getElementById("quickStats").innerHTML = qs.map(s=>
      `<div class="quick-stat"><span class="quick-val">${s.v}</span><span class="quick-label">${s.l}</span></div>`
    ).join("");

    const fgPct = g.fgAttempts > 0 ? ` (${Math.round(g.fgMade/g.fgAttempts*100)}%)` : "";
    document.getElementById("ftLine").textContent =
      `FT: ${g.ftMade}/${g.ftAttempts}  ·  FG: ${g.fgMade}/${g.fgAttempts}${fgPct}`;

    // Build stat buttons once
    const grid = document.getElementById("statGrid");
    if (!grid.dataset.built) {
      grid.innerHTML = STAT_BUTTONS.map(btn =>
        `<button class="stat-btn" style="border-color:${btn.color};color:${btn.color}"
          onclick="recordStat('${btn.key}')">${btn.label}</button>`
      ).join("");
      grid.dataset.built = "1";
    }

    document.getElementById("undoBtn").disabled = !g.log || g.log.length === 0;

    const log = g.log || [];
    if (log.length > 0) {
      document.getElementById("logBox").style.display = "block";
      document.getElementById("logEntries").innerHTML = log.slice(0,8).map(e => {
        const btn = STAT_BUTTONS.find(b=>b.key===e.key);
        return `<div class="log-entry">
          <span style="color:${btn?.color||'#fff'}">${btn?.label||e.key}</span>
          <span class="log-time">${e.time}</span>
        </div>`;
      }).join("");
    } else {
      document.getElementById("logBox").style.display = "none";
    }
  }

  // ── HISTORY ──
  const hl = document.getElementById("historyList");
  if (games.length === 0) {
    hl.innerHTML = '<div class="card"><p class="empty">No completed games yet.</p></div>';
  } else {
    hl.innerHTML = games.map(gm => {
      const oreb = gm.offRebounds||0;
      const dreb = gm.defRebounds||gm.rebounds||0;
      const reb = oreb+dreb;
      const fgPct = gm.fgAttempts>0 ? ` (${Math.round(gm.fgMade/gm.fgAttempts*100)}%)` : "";
      const stats = [
        {l:"PTS",v:gm.points},{l:"REB",v:reb},{l:"OREB",v:oreb},
        {l:"DREB",v:dreb},{l:"AST",v:gm.assists||0},{l:"STL",v:gm.steals||0},
        {l:"BLK",v:gm.blocks||0},{l:"TO",v:gm.turnovers||0},
      ];
      return `<div class="history-card">
        <div class="history-top">
          <span class="history-opp">vs. ${gm.opponent}</span>
          <span class="history-date">${gm.date}</span>
          <button class="delete-btn" onclick="deleteGame(${gm.id})">✕</button>
        </div>
        <div class="history-stats">
          ${stats.map(s=>`<div class="hist-stat"><span class="hist-val">${s.v}</span><span class="hist-label">${s.l}</span></div>`).join("")}
        </div>
        <div class="history-ft">FT: ${gm.ftMade||0}/${gm.ftAttempts||0} · FG: ${gm.fgMade||0}/${gm.fgAttempts||0}${fgPct} · Fouls: ${gm.fouls||0}</div>
      </div>`;
    }).join("");
  }
}

function renderSetup() {
  const url = getFirebaseUrl();
  document.getElementById("currentDbCard").style.display = url ? "block" : "none";
  if (url) {
    document.getElementById("currentDbUrl").textContent = url;
    document.getElementById("firebaseUrl").value = url;
  }
}

// Add setup nav button dynamically
const nav = document.querySelector(".nav");
const setupBtn = document.createElement("button");
setupBtn.className = "nav-btn";
setupBtn.id = "navSetup";
setupBtn.textContent = "⚙️";
setupBtn.onclick = () => { showView("setup"); renderSetup(); };
nav.appendChild(setupBtn);

// ─── INIT ───────────────────────────────────────────────
loadState();
</script>
</body>
</html>
