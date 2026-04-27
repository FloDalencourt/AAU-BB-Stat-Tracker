
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>🏀 Stat Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@400;700;900&family=Barlow:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --orange:#FF6B35;--gold:#FFD700;--green:#4CAF50;--blue:#2196F3;
    --purple:#9C27B0;--cyan:#00BCD4;--red:#F44336;--gray:#9E9E9E;
    --bg:#0a0f1e;--card:#111827;--border:#1e293b;--muted:#64748b;--text2:#94a3b8;
  }
  *{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
  body{background:var(--bg);color:#fff;font-family:'Barlow',sans-serif;max-width:480px;margin:0 auto;min-height:100vh;overflow-x:hidden}
  h1,h2,h3{font-family:'Barlow Condensed',sans-serif}

  .header{display:flex;justify-content:space-between;align-items:center;padding:14px 16px;background:var(--card);border-bottom:1px solid var(--border);position:sticky;top:0;z-index:100}
  .header-left{display:flex;align-items:center;gap:10px}
  .ball{font-size:22px}
  .player-name{font-family:'Barlow Condensed',sans-serif;font-size:18px;font-weight:700;cursor:pointer;letter-spacing:.5px}
  .player-name:hover{color:var(--orange)}
  #nameInput{background:transparent;border:none;border-bottom:2px solid var(--orange);color:#fff;font-size:16px;font-weight:700;font-family:'Barlow Condensed',sans-serif;outline:none;width:160px}
  .nav{display:flex;gap:6px;align-items:center}
  .nav-btn{background:transparent;border:1px solid #2a3a5e;color:var(--text2);padding:6px 12px;border-radius:8px;cursor:pointer;font-size:13px;font-family:'Barlow',sans-serif;transition:all .15s}
  .nav-btn:hover{background:var(--border);color:#fff}
  .nav-btn.active{background:var(--border);color:#fff;border-color:var(--orange)}
  .nav-btn.live{background:rgba(255,107,53,.15);color:var(--orange);border-color:var(--orange);font-weight:700;font-size:11px;animation:pulse-border 1.5s infinite}
  @keyframes pulse-border{0%,100%{box-shadow:0 0 0 0 rgba(255,107,53,.4)}50%{box-shadow:0 0 0 4px rgba(255,107,53,0)}}

  .view{display:none;padding:14px}
  .view.active{display:block}

  .card{background:var(--card);border-radius:14px;padding:18px;margin-bottom:14px;border:1px solid var(--border)}
  .card-title{font-family:'Barlow Condensed',sans-serif;font-size:13px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:14px}
  .empty{color:var(--muted);text-align:center;padding:12px 0;font-size:14px}

  .avg-grid{display:flex;gap:6px;flex-wrap:wrap;justify-content:space-between}
  .avg-item{display:flex;flex-direction:column;align-items:center;flex:1;min-width:50px}
  .avg-val{font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;color:var(--orange);line-height:1}
  .avg-label{font-size:9px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;margin-top:3px}
  .games-played{color:var(--muted);font-size:12px;text-align:center;margin-top:12px}

  input[type="text"]{width:100%;padding:11px 13px;border-radius:10px;background:#1e293b;border:1px solid #2a3a5e;color:#fff;font-size:15px;margin-bottom:10px;outline:none;font-family:'Barlow',sans-serif;transition:border-color .2s}
  input[type="text"]:focus{border-color:var(--orange)}

  .btn{border:none;border-radius:10px;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:700;letter-spacing:.5px;transition:transform .1s,opacity .1s}
  .btn:active{transform:scale(.96);opacity:.9}
  .btn-primary{background:var(--orange);color:#fff;padding:13px 20px;font-size:17px;width:100%}
  .btn-danger{background:var(--red);color:#fff;padding:12px 18px;font-size:15px;flex:1}
  .btn-undo{background:#1e293b;color:var(--text2);border:1px solid #2a3a5e;padding:12px 18px;font-size:15px}
  .btn-undo:disabled{opacity:.35;cursor:not-allowed}
  .btn-row{display:flex;gap:10px}
  .btn-share{background:rgba(33,150,243,.15);color:var(--blue);border:1.5px solid var(--blue);padding:10px 16px;font-size:14px;border-radius:10px;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;letter-spacing:.5px;transition:all .15s;white-space:nowrap}
  .btn-share:hover{background:rgba(33,150,243,.25)}

  /* TIMER */
  .timer-bar{background:var(--card);border-radius:14px;padding:14px 16px;margin-bottom:10px;border:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;gap:12px}
  .timer-display{font-family:'Barlow Condensed',sans-serif;font-size:38px;font-weight:900;line-height:1;letter-spacing:2px;min-width:110px;transition:color .3s}
  .timer-display.running{color:var(--green)}
  .timer-display.paused{color:var(--orange)}
  .timer-display.stopped{color:var(--muted)}
  .timer-label{font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:1.5px;margin-top:3px}
  .timer-controls{display:flex;flex-direction:column;gap:7px;align-items:flex-end}
  .timer-btn{border:none;border-radius:9px;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;font-size:14px;letter-spacing:.5px;padding:9px 18px;transition:transform .1s;white-space:nowrap}
  .timer-btn:active{transform:scale(.94)}
  .timer-btn.start{background:rgba(76,175,80,.2);color:var(--green);border:1.5px solid var(--green)}
  .timer-btn.sub-out{background:rgba(255,107,53,.2);color:var(--orange);border:1.5px solid var(--orange)}
  .timer-btn.reset{background:rgba(158,158,158,.1);color:var(--muted);border:1.5px solid #2a3a5e}
  .timer-status{font-size:11px;font-weight:600;letter-spacing:.5px;padding:3px 9px;border-radius:6px;align-self:flex-start;margin-top:2px}
  .timer-status.on-court{background:rgba(76,175,80,.15);color:var(--green)}
  .timer-status.on-bench{background:rgba(33,150,243,.15);color:var(--blue)}

  .game-header{display:flex;justify-content:space-between;align-items:center;background:var(--card);border-radius:14px;padding:14px 16px;margin-bottom:10px;border:1px solid var(--border)}
  .game-vs{font-family:'Barlow Condensed',sans-serif;font-size:20px;font-weight:700}
  .game-date{font-size:12px;color:var(--muted);margin-top:2px}
  .game-score{font-family:'Barlow Condensed',sans-serif;font-size:56px;font-weight:900;color:var(--orange);line-height:1}

  .quick-stats{display:flex;gap:4px;flex-wrap:wrap;background:var(--card);border-radius:12px;padding:10px;margin-bottom:10px;border:1px solid var(--border)}
  .quick-stat{display:flex;flex-direction:column;align-items:center;flex:1;min-width:40px}
  .quick-val{font-family:'Barlow Condensed',sans-serif;font-size:20px;font-weight:800;line-height:1}
  .quick-label{font-size:8px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;margin-top:2px}
  .ft-line{text-align:center;font-size:12px;color:var(--muted);margin-bottom:10px;letter-spacing:.5px}

  .stat-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px;margin-bottom:10px}
  .stat-btn{background:var(--card);border:2px solid;border-radius:12px;padding:15px 6px;font-family:'Barlow Condensed',sans-serif;font-size:15px;font-weight:800;cursor:pointer;letter-spacing:1px;transition:transform .08s;line-height:1}
  .stat-btn:active{transform:scale(.93)}
  .stat-btn:hover{filter:brightness(1.15)}

  .log-box{background:var(--card);border-radius:12px;padding:12px;border:1px solid var(--border);margin-top:10px}
  .log-title{font-size:10px;color:#475569;text-transform:uppercase;letter-spacing:1.5px;margin-bottom:8px;font-family:'Barlow Condensed',sans-serif}
  .log-entry{display:flex;justify-content:space-between;padding:5px 0;border-bottom:1px solid var(--border);font-size:13px}
  .log-entry:last-child{border-bottom:none}
  .log-time{color:#475569;font-size:11px}

  .section-title{font-family:'Barlow Condensed',sans-serif;font-size:16px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1.5px;margin-bottom:12px}
  .history-card{background:var(--card);border-radius:14px;padding:14px;margin-bottom:10px;border:1px solid var(--border)}
  .history-top{display:flex;align-items:center;margin-bottom:10px}
  .history-opp{font-family:'Barlow Condensed',sans-serif;font-size:18px;font-weight:700;flex:1}
  .history-date{color:var(--muted);font-size:12px;margin-right:8px}
  .delete-btn{background:transparent;border:none;color:#475569;cursor:pointer;font-size:16px;padding:0 4px}
  .delete-btn:hover{color:var(--red)}
  .history-stats{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:8px}
  .hist-stat{display:flex;flex-direction:column;align-items:center;flex:1;min-width:38px}
  .hist-val{font-family:'Barlow Condensed',sans-serif;font-size:22px;font-weight:800;color:var(--orange);line-height:1}
  .hist-label{font-size:8px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;margin-top:2px}
  .history-ft{font-size:11px;color:#475569;border-top:1px solid var(--border);padding-top:8px}

  /* SHARE MODAL */
  .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.75);z-index:200;display:flex;align-items:center;justify-content:center;padding:20px}
  .modal-overlay.hidden{display:none}
  .modal{background:var(--card);border-radius:18px;padding:24px;width:100%;max-width:420px;border:1px solid var(--border)}
  .modal-title{font-family:'Barlow Condensed',sans-serif;font-size:20px;font-weight:900;margin-bottom:6px}
  .modal-sub{font-size:13px;color:var(--text2);margin-bottom:16px;line-height:1.5}
  .url-box{background:#1e293b;border:1px solid #2a3a5e;border-radius:10px;padding:10px 13px;font-size:12px;color:var(--text2);word-break:break-all;margin-bottom:14px;line-height:1.5;max-height:80px;overflow:auto}
  .modal-btns{display:flex;gap:10px}
  .btn-copy{background:var(--orange);color:#fff;padding:12px 18px;font-size:16px;border-radius:10px;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;flex:1;border:none;transition:filter .15s}
  .btn-copy:hover{filter:brightness(1.1)}
  .btn-close-modal{background:#1e293b;color:var(--text2);padding:12px 18px;font-size:16px;border-radius:10px;cursor:pointer;font-family:'Barlow Condensed',sans-serif;font-weight:800;border:1px solid #2a3a5e}
  .copied-badge{background:rgba(76,175,80,.2);color:var(--green);border:1px solid var(--green);padding:8px 16px;border-radius:8px;font-size:13px;font-weight:600;text-align:center;margin-bottom:12px;display:none}
  .copied-badge.show{display:block}

  /* READ-ONLY VIEW */
  #view-readonly .ro-header{background:linear-gradient(135deg,#1a2540,#0d1525);border-radius:16px;padding:20px;margin-bottom:14px;text-align:center;border:1px solid var(--border)}
  #view-readonly .ro-player{font-family:'Barlow Condensed',sans-serif;font-size:15px;color:var(--text2);text-transform:uppercase;letter-spacing:2px;margin-bottom:4px}
  #view-readonly .ro-opp{font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;margin-bottom:2px}
  #view-readonly .ro-date{font-size:12px;color:var(--muted);margin-bottom:16px}
  #view-readonly .ro-pts{font-family:'Barlow Condensed',sans-serif;font-size:80px;font-weight:900;color:var(--orange);line-height:1}
  #view-readonly .ro-pts-label{font-size:13px;color:var(--muted);letter-spacing:2px;text-transform:uppercase;margin-top:4px}
  #view-readonly .ro-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:14px}
  #view-readonly .ro-stat{background:var(--card);border-radius:12px;padding:14px 6px;text-align:center;border:1px solid var(--border)}
  #view-readonly .ro-val{font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;line-height:1}
  #view-readonly .ro-lbl{font-size:9px;color:var(--muted);letter-spacing:1px;text-transform:uppercase;margin-top:4px}
  #view-readonly .ro-footer{text-align:center;font-size:12px;color:var(--muted);margin-top:8px}
  #view-readonly .live-badge{display:inline-flex;align-items:center;gap:6px;background:rgba(255,107,53,.15);color:var(--orange);border:1px solid var(--orange);border-radius:8px;padding:5px 12px;font-size:12px;font-weight:700;margin-bottom:12px;font-family:'Barlow Condensed',sans-serif;letter-spacing:.5px}
  #view-readonly .live-dot{width:8px;height:8px;border-radius:50%;background:var(--orange);animation:blink 1s infinite}
  @keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
  #view-readonly .ro-timer{background:var(--card);border-radius:12px;padding:14px;border:1px solid var(--border);margin-bottom:14px;display:flex;align-items:center;justify-content:space-between}
  #view-readonly .ro-timer-val{font-family:'Barlow Condensed',sans-serif;font-size:36px;font-weight:900;color:var(--green)}
  #view-readonly .ro-timer-lbl{font-size:10px;color:var(--muted);letter-spacing:1.5px;text-transform:uppercase;margin-top:2px}

  #flash{position:fixed;top:0;left:0;right:0;bottom:0;display:flex;align-items:center;justify-content:center;pointer-events:none;z-index:999;opacity:0;transition:opacity .1s}
  #flash.show{opacity:1}
  #flash span{font-family:'Barlow Condensed',sans-serif;font-size:72px;font-weight:900;color:var(--orange);text-shadow:0 0 60px rgba(255,107,53,.9)}

  .live-pts{font-family:'Barlow Condensed',sans-serif;font-size:52px;font-weight:900;color:var(--orange);margin:6px 0}
  .vs-text{color:var(--text2);margin-bottom:8px;font-size:14px}

  @media(max-width:360px){.stat-btn{font-size:13px;padding:12px 4px}.avg-val{font-size:22px}.timer-display{font-size:30px}}
</style>
</head>
<body>

<div id="flash"><span id="flashText"></span></div>

<!-- SHARE MODAL -->
<div class="modal-overlay hidden" id="shareModal">
  <div class="modal">
    <div class="modal-title">📤 Share Live Stats</div>
    <p class="modal-sub">Anyone with this link can view a live read-only scoreboard — no account needed.</p>
    <div class="copied-badge" id="copiedBadge">✅ Copied to clipboard!</div>
    <div class="url-box" id="shareUrlBox"></div>
    <div class="modal-btns">
      <button class="btn-copy" onclick="copyShareUrl()">Copy Link</button>
      <button class="btn-close-modal" onclick="closeShareModal()">Close</button>
    </div>
  </div>
</div>

<!-- HEADER -->
<div class="header" id="mainHeader">
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
</div>

<!-- GAME VIEW -->
<div class="view" id="view-game">
  <div class="game-header">
    <div>
      <div class="game-vs" id="gameVs">vs. —</div>
      <div class="game-date" id="gameDate"></div>
    </div>
    <div style="display:flex;flex-direction:column;align-items:flex-end;gap:6px">
      <div class="game-score" id="gamePts">0</div>
      <button class="btn-share" onclick="openShareModal()">📤 Share</button>
    </div>
  </div>

  <div class="timer-bar">
    <div>
      <div class="timer-display stopped" id="timerDisplay">00:00</div>
      <div class="timer-label">Playing Time</div>
      <div class="timer-status on-bench" id="timerStatus">On Bench</div>
    </div>
    <div class="timer-controls">
      <button class="timer-btn start" id="timerStartBtn" onclick="timerSubIn()">▶ Sub In</button>
      <button class="timer-btn sub-out" id="timerSubOutBtn" onclick="timerSubOut()" style="display:none">⏸ Sub Out</button>
      <button class="timer-btn reset" onclick="timerReset()">↺ Reset</button>
    </div>
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

<!-- READ-ONLY VIEW (shared link) -->
<div class="view" id="view-readonly">
  <div class="ro-header">
    <div class="ro-player" id="roPlayer"></div>
    <div class="ro-opp" id="roOpp"></div>
    <div class="ro-date" id="roDate"></div>
    <div class="live-badge" id="roBadge"><span class="live-dot"></span> LIVE GAME</div>
    <div class="ro-pts" id="roPts">0</div>
    <div class="ro-pts-label">Points</div>
  </div>
  <div class="ro-timer" id="roTimerRow">
    <div>
      <div class="ro-timer-val" id="roTimerVal">00:00</div>
      <div class="ro-timer-lbl">Playing Time</div>
    </div>
    <div id="roTimerStatus" class="timer-status on-bench" style="margin:0">On Bench</div>
  </div>
  <div class="ro-grid" id="roGrid"></div>
  <div class="card">
    <div class="card-title">Shooting</div>
    <div style="display:flex;gap:20px;justify-content:center;text-align:center">
      <div><div style="font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;color:var(--orange)" id="roFG">0/0</div><div style="font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:1px">Field Goals</div></div>
      <div><div style="font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;color:var(--gold)" id="roFGpct">—%</div><div style="font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:1px">FG%</div></div>
      <div><div style="font-family:'Barlow Condensed',sans-serif;font-size:28px;font-weight:900;color:var(--cyan)" id="roFT">0/0</div><div style="font-size:9px;color:var(--muted);text-transform:uppercase;letter-spacing:1px">Free Throws</div></div>
    </div>
  </div>
  <div class="ro-footer" id="roFooter"></div>
</div>

<script>
const STAT_BUTTONS = [
  {key:"points2",label:"2 PTS",color:"#FF6B35"},
  {key:"points3",label:"3 PTS",color:"#F7931E"},
  {key:"ft",label:"FT",color:"#FFD700"},
  {key:"ftMiss",label:"FT MISS",color:"#9E9E9E"},
  {key:"offRebounds",label:"OFF REB",color:"#4CAF50"},
  {key:"defRebounds",label:"DEF REB",color:"#81C784"},
  {key:"assists",label:"AST",color:"#2196F3"},
  {key:"steals",label:"STL",color:"#9C27B0"},
  {key:"blocks",label:"BLK",color:"#00BCD4"},
  {key:"turnovers",label:"TO",color:"#F44336"},
  {key:"fouls",label:"FOUL",color:"#FF5722"},
];

let state = {playerName:"My Daughter",games:[],currentGame:null};
let timerInterval = null;
let roPollingInterval = null;

// ─── SHARE / URL STATE ──────────────────────────────────
function buildShareUrl() {
  const g = state.currentGame;
  if (!g) return null;
  const payload = {
    n: state.playerName,
    o: g.opponent,
    d: g.date,
    p: g.points,
    or: g.offRebounds||0,
    dr: g.defRebounds||0,
    a: g.assists||0,
    s: g.steals||0,
    b: g.blocks||0,
    t: g.turnovers||0,
    f: g.fouls||0,
    fm: g.ftMade||0,
    fa: g.ftAttempts||0,
    gm: g.fgMade||0,
    ga: g.fgAttempts||0,
    ts: getLiveSeconds(),
    tr: g.timerRunning ? 1 : 0,
  };
  const encoded = btoa(unescape(encodeURIComponent(JSON.stringify(payload))));
  // Use location.href base, strip existing hash
  const base = location.href.split('#')[0];
  return base + '#view=' + encoded;
}

function openShareModal() {
  const url = buildShareUrl();
  if (!url) return;
  document.getElementById("shareUrlBox").textContent = url;
  document.getElementById("shareModal").classList.remove("hidden");
  document.getElementById("copiedBadge").classList.remove("show");
}
function closeShareModal() {
  document.getElementById("shareModal").classList.add("hidden");
}
function copyShareUrl() {
  const url = document.getElementById("shareUrlBox").textContent;
  navigator.clipboard.writeText(url).then(() => {
    document.getElementById("copiedBadge").classList.add("show");
  }).catch(() => {
    // fallback
    const ta = document.createElement("textarea");
    ta.value = url; document.body.appendChild(ta);
    ta.select(); document.execCommand("copy");
    document.body.removeChild(ta);
    document.getElementById("copiedBadge").classList.add("show");
  });
}

// ─── READ-ONLY VIEW ─────────────────────────────────────
function fmtTime(sec) {
  const m = Math.floor(sec/60), s = sec%60;
  return String(m).padStart(2,'0')+':'+String(s).padStart(2,'0');
}

function parseHashData() {
  const hash = location.hash;
  if (!hash.startsWith('#view=')) return null;
  try {
    const encoded = hash.slice(6);
    return JSON.parse(decodeURIComponent(escape(atob(encoded))));
  } catch(e) { return null; }
}

function renderReadOnly(data) {
  document.getElementById("mainHeader").style.display = "none";
  showView("readonly");

  document.getElementById("roPlayer").textContent = data.n || "Player";
  document.getElementById("roOpp").textContent = "vs. " + (data.o || "Opponent");
  document.getElementById("roDate").textContent = data.d || "";
  document.getElementById("roPts").textContent = data.p || 0;

  const roBadge = document.getElementById("roBadge");
  roBadge.style.display = data.tr ? "inline-flex" : "none";

  // Timer
  const sec = data.ts || 0;
  document.getElementById("roTimerVal").textContent = fmtTime(sec);
  const roTimerStatus = document.getElementById("roTimerStatus");
  if (data.tr) {
    roTimerStatus.textContent = "On Court"; roTimerStatus.className = "timer-status on-court";
  } else {
    roTimerStatus.textContent = sec > 0 ? "Subbed Out" : "On Bench";
    roTimerStatus.className = "timer-status on-bench";
  }
  if (sec === 0 && !data.tr) document.getElementById("roTimerRow").style.display = "none";

  const stats = [
    {l:"OFF REB",v:data.or,c:"#4CAF50"},{l:"DEF REB",v:data.dr,c:"#81C784"},
    {l:"ASSISTS",v:data.a,c:"#2196F3"},{l:"STEALS",v:data.s,c:"#9C27B0"},
    {l:"BLOCKS",v:data.b,c:"#00BCD4"},{l:"TURNOVERS",v:data.t,c:"#F44336"},
    {l:"FOULS",v:data.f,c:"#FF5722"},
  ];
  document.getElementById("roGrid").innerHTML = stats.map(s=>
    `<div class="ro-stat"><div class="ro-val" style="color:${s.c}">${s.v}</div><div class="ro-lbl">${s.l}</div></div>`
  ).join("");

  const fgPct = data.ga > 0 ? Math.round(data.gm/data.ga*100)+"%" : "—%";
  document.getElementById("roFG").textContent = `${data.gm}/${data.ga}`;
  document.getElementById("roFGpct").textContent = fgPct;
  document.getElementById("roFT").textContent = `${data.fm}/${data.fa}`;

  document.getElementById("roFooter").textContent = "📱 Snapshot shared from Basketball Stat Tracker";
}

// ─── TIMER ───────────────────────────────────────────────
function timerSubIn() {
  const g = state.currentGame; if(!g||g.timerRunning) return;
  g.timerRunning=true; g.timerEpoch=Date.now();
  saveState(); startTimerTick();
}
function timerSubOut() {
  const g = state.currentGame; if(!g||!g.timerRunning) return;
  g.timerSeconds=(g.timerSeconds||0)+Math.floor((Date.now()-g.timerEpoch)/1000);
  g.timerRunning=false; g.timerEpoch=null;
  saveState(); stopTimerTick(); renderTimerUI();
}
function timerReset() {
  const g = state.currentGame; if(!g) return;
  if(!confirm("Reset playing time to 0:00?")) return;
  g.timerSeconds=0; g.timerRunning=false; g.timerEpoch=null;
  saveState(); stopTimerTick(); renderTimerUI();
}
function startTimerTick(){stopTimerTick();timerInterval=setInterval(renderTimerUI,1000);renderTimerUI()}
function stopTimerTick(){if(timerInterval){clearInterval(timerInterval);timerInterval=null}}
function getLiveSeconds(){
  const g=state.currentGame; if(!g) return 0;
  let s=g.timerSeconds||0;
  if(g.timerRunning&&g.timerEpoch) s+=Math.floor((Date.now()-g.timerEpoch)/1000);
  return s;
}
function renderTimerUI(){
  const g=state.currentGame;
  const disp=document.getElementById("timerDisplay");
  const status=document.getElementById("timerStatus");
  const startBtn=document.getElementById("timerStartBtn");
  const subOutBtn=document.getElementById("timerSubOutBtn");
  if(!disp) return;
  const sec=getLiveSeconds();
  disp.textContent=fmtTime(sec);
  if(g&&g.timerRunning){
    disp.className="timer-display running";status.textContent="On Court";
    status.className="timer-status on-court";startBtn.style.display="none";subOutBtn.style.display="inline-block";
  } else {
    disp.className=sec>0?"timer-display paused":"timer-display stopped";
    status.textContent="On Bench";status.className="timer-status on-bench";
    startBtn.style.display="inline-block";subOutBtn.style.display="none";
  }
}

// ─── STORAGE ─────────────────────────────────────────────
async function loadState(){
  const raw=localStorage.getItem("bball_state");
  if(raw){try{state=JSON.parse(raw)}catch(e){}}
  if(state.currentGame&&state.currentGame.timerRunning){
    const elapsed=Math.floor((Date.now()-(state.currentGame.timerEpoch||Date.now()))/1000);
    state.currentGame.timerSeconds=(state.currentGame.timerSeconds||0)+elapsed;
    state.currentGame.timerRunning=false;state.currentGame.timerEpoch=null;
  }
  renderAll();
}
function saveState(){localStorage.setItem("bball_state",JSON.stringify(state));renderAll()}

// ─── GAME LOGIC ──────────────────────────────────────────
function emptyGame(opp){
  return{id:Date.now(),date:new Date().toLocaleDateString(),opponent:opp||"Opponent",
    points:0,offRebounds:0,defRebounds:0,assists:0,steals:0,blocks:0,turnovers:0,fouls:0,
    ftMade:0,ftAttempts:0,fgMade:0,fgAttempts:0,timerSeconds:0,timerRunning:false,timerEpoch:null,log:[]};
}
function startGame(){
  const opp=document.getElementById("opponentInput").value.trim();
  state.currentGame=emptyGame(opp);
  document.getElementById("opponentInput").value="";
  saveState(); showView("game");
}
function endGame(){
  if(!state.currentGame) return;
  if(state.currentGame.timerRunning){
    state.currentGame.timerSeconds=(state.currentGame.timerSeconds||0)+Math.floor((Date.now()-state.currentGame.timerEpoch)/1000);
    state.currentGame.timerRunning=false;state.currentGame.timerEpoch=null;
  }
  stopTimerTick();
  state.games=[state.currentGame,...(state.games||[])];
  state.currentGame=null;
  saveState(); showView("history");
}
function recordStat(key){
  const g=state.currentGame; if(!g) return;
  const entry={key,time:new Date().toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'})};
  if(key==="points2"){g.points+=2;g.fgMade++;g.fgAttempts++}
  else if(key==="points3"){g.points+=3;g.fgMade++;g.fgAttempts++}
  else if(key==="ft"){g.points++;g.ftMade++;g.ftAttempts++}
  else if(key==="ftMiss"){g.ftAttempts++}
  else{g[key]=(g[key]||0)+1}
  g.log=[entry,...(g.log||[])];
  saveState();
  const btn=STAT_BUTTONS.find(b=>b.key===key);
  showFlash(btn?.label||key);
}
function undoLast(){
  const g=state.currentGame; if(!g||!g.log||!g.log.length) return;
  const[last,...rest]=g.log;const key=last.key;
  if(key==="points2"){g.points=Math.max(0,g.points-2);g.fgMade=Math.max(0,g.fgMade-1);g.fgAttempts=Math.max(0,g.fgAttempts-1)}
  else if(key==="points3"){g.points=Math.max(0,g.points-3);g.fgMade=Math.max(0,g.fgMade-1);g.fgAttempts=Math.max(0,g.fgAttempts-1)}
  else if(key==="ft"){g.points=Math.max(0,g.points-1);g.ftMade=Math.max(0,g.ftMade-1);g.ftAttempts=Math.max(0,g.ftAttempts-1)}
  else if(key==="ftMiss"){g.ftAttempts=Math.max(0,g.ftAttempts-1)}
  else{g[key]=Math.max(0,(g[key]||0)-1)}
  g.log=rest; saveState();
}
function deleteGame(id){
  if(!confirm("Delete this game?")) return;
  state.games=state.games.filter(g=>g.id!==id); saveState();
}

// ─── NAME ────────────────────────────────────────────────
function editName(){
  document.getElementById("playerNameDisplay").style.display="none";
  const inp=document.getElementById("nameInput");
  inp.style.display="inline-block";inp.value=state.playerName;inp.focus();
}
function saveName(){
  const inp=document.getElementById("nameInput");
  const val=inp.value.trim();if(val)state.playerName=val;
  inp.style.display="none";
  document.getElementById("playerNameDisplay").style.display="inline";
  saveState();
}

// ─── FLASH ───────────────────────────────────────────────
let flashTimer=null;
function showFlash(text){
  const el=document.getElementById("flash");
  document.getElementById("flashText").textContent=text;
  el.classList.add("show");
  if(flashTimer)clearTimeout(flashTimer);
  flashTimer=setTimeout(()=>el.classList.remove("show"),500);
}

// ─── VIEW NAV ────────────────────────────────────────────
function showView(name){
  document.querySelectorAll(".view").forEach(v=>v.classList.remove("active"));
  document.querySelectorAll(".nav-btn").forEach(b=>b.classList.remove("active"));
  document.getElementById("view-"+name).classList.add("active");
  const nb=document.getElementById("nav"+name.charAt(0).toUpperCase()+name.slice(1));
  if(nb)nb.classList.add("active");
  if(name==="game"&&state.currentGame&&state.currentGame.timerRunning)startTimerTick();
  else if(name!=="game")stopTimerTick();
}

// ─── RENDER ──────────────────────────────────────────────
function renderAll(){
  const g=state.currentGame,games=state.games||[];
  document.getElementById("playerNameDisplay").textContent=(state.playerName||"Player")+" ✏️";
  document.getElementById("navLive").style.display=g?"inline-flex":"none";

  // HOME
  const gp=games.length;
  if(gp===0){
    document.getElementById("avgContent").innerHTML='<p class="empty">No games yet — start tracking!</p>';
    document.getElementById("gamesPlayed").textContent="";
  } else {
    const tot=games.reduce((a,gm)=>({
      pts:a.pts+(gm.points||0),oreb:a.oreb+(gm.offRebounds||0),
      dreb:a.dreb+(gm.defRebounds||0),ast:a.ast+(gm.assists||0),
      stl:a.stl+(gm.steals||0),blk:a.blk+(gm.blocks||0),
      mins:a.mins+Math.floor((gm.timerSeconds||0)/60),
    }),{pts:0,oreb:0,dreb:0,ast:0,stl:0,blk:0,mins:0});
    const avg=k=>(tot[k]/gp).toFixed(1);
    const reb=(tot.oreb+tot.dreb)/gp;
    const items=[
      {l:"PPG",v:avg("pts")},{l:"REB",v:reb.toFixed(1)},
      {l:"APG",v:avg("ast")},{l:"SPG",v:avg("stl")},
      {l:"BPG",v:avg("blk")},{l:"MPG",v:(tot.mins/gp).toFixed(1)},
    ];
    document.getElementById("avgContent").innerHTML=
      '<div class="avg-grid">'+items.map(i=>`<div class="avg-item"><span class="avg-val">${i.v}</span><span class="avg-label">${i.l}</span></div>`).join("")+"</div>";
    document.getElementById("gamesPlayed").textContent=`${gp} game${gp!==1?"s":""} recorded`;
  }
  if(g){
    document.getElementById("liveCard").style.display="block";
    document.getElementById("newGameCard").style.display="none";
    document.getElementById("liveCardVs").textContent=`vs. ${g.opponent} — ${g.date}`;
    document.getElementById("liveCardPts").textContent=g.points+" PTS";
  } else {
    document.getElementById("liveCard").style.display="none";
    document.getElementById("newGameCard").style.display="block";
  }

  // GAME VIEW
  if(g){
    document.getElementById("gameVs").textContent="vs. "+g.opponent;
    document.getElementById("gameDate").textContent=g.date;
    document.getElementById("gamePts").textContent=g.points;
    const qs=[
      {l:"OREB",v:g.offRebounds||0},{l:"DREB",v:g.defRebounds||0},
      {l:"AST",v:g.assists||0},{l:"STL",v:g.steals||0},
      {l:"BLK",v:g.blocks||0},{l:"TO",v:g.turnovers||0},{l:"FOULS",v:g.fouls||0},
    ];
    document.getElementById("quickStats").innerHTML=qs.map(s=>
      `<div class="quick-stat"><span class="quick-val">${s.v}</span><span class="quick-label">${s.l}</span></div>`
    ).join("");
    const fgPct=g.fgAttempts>0?` (${Math.round(g.fgMade/g.fgAttempts*100)}%)`:"";
    document.getElementById("ftLine").textContent=`FT: ${g.ftMade}/${g.ftAttempts}  ·  FG: ${g.fgMade}/${g.fgAttempts}${fgPct}`;
    const grid=document.getElementById("statGrid");
    if(!grid.dataset.built){
      grid.innerHTML=STAT_BUTTONS.map(btn=>
        `<button class="stat-btn" style="border-color:${btn.color};color:${btn.color}" onclick="recordStat('${btn.key}')">${btn.label}</button>`
      ).join("");grid.dataset.built="1";
    }
    document.getElementById("undoBtn").disabled=!g.log||!g.log.length;
    const log=g.log||[];
    if(log.length>0){
      document.getElementById("logBox").style.display="block";
      document.getElementById("logEntries").innerHTML=log.slice(0,8).map(e=>{
        const btn=STAT_BUTTONS.find(b=>b.key===e.key);
        return `<div class="log-entry"><span style="color:${btn?.color||'#fff'}">${btn?.label||e.key}</span><span class="log-time">${e.time}</span></div>`;
      }).join("");
    } else document.getElementById("logBox").style.display="none";
    renderTimerUI();
  }

  // HISTORY
  const hl=document.getElementById("historyList");
  if(!games.length){
    hl.innerHTML='<div class="card"><p class="empty">No completed games yet.</p></div>';
  } else {
    hl.innerHTML=games.map(gm=>{
      const reb=(gm.offRebounds||0)+(gm.defRebounds||0);
      const fgPct=gm.fgAttempts>0?` (${Math.round(gm.fgMade/gm.fgAttempts*100)}%)`:"";
      const mins=Math.floor((gm.timerSeconds||0)/60),secs=(gm.timerSeconds||0)%60;
      const timeStr=(gm.timerSeconds||0)>0?`${mins}:${String(secs).padStart(2,'0')}`:"—";
      const stats=[
        {l:"PTS",v:gm.points},{l:"REB",v:reb},{l:"AST",v:gm.assists||0},
        {l:"STL",v:gm.steals||0},{l:"BLK",v:gm.blocks||0},{l:"MIN",v:timeStr},
      ];
      return `<div class="history-card">
        <div class="history-top">
          <span class="history-opp">vs. ${gm.opponent}</span>
          <span class="history-date">${gm.date}</span>
          <button class="delete-btn" onclick="deleteGame(${gm.id})">✕</button>
        </div>
        <div class="history-stats">
          ${stats.map(s=>`<div class="hist-stat"><span class="hist-val" style="font-size:${s.l==='MIN'?'18px':'22px'}">${s.v}</span><span class="hist-label">${s.l}</span></div>`).join("")}
        </div>
        <div class="history-ft">FT: ${gm.ftMade||0}/${gm.ftAttempts||0} · FG: ${gm.fgMade||0}/${gm.fgAttempts||0}${fgPct} · Fouls: ${gm.fouls||0}</div>
      </div>`;
    }).join("");
  }
}

// ─── DOWNLOAD AS HTML ────────────────────────────────────
function downloadHTML() {
  const html = document.documentElement.outerHTML;
  const blob = new Blob([html], {type: 'text/html'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'basketball-tracker.html';
  a.click();
  URL.revokeObjectURL(a.href);
}

// ─── INIT ────────────────────────────────────────────────
const hashData = parseHashData();
if (hashData) {
  // Viewer mode — don't load local state
  document.addEventListener("DOMContentLoaded", () => renderReadOnly(hashData));
  renderReadOnly(hashData);
} else {
  loadState();
}
</script>
</body>
</html>
