# nexus-arcade[index.html](https://github.com/user-attachments/files/26192620/NEXUS.ARCADE.v2.Cyberpunk.Game.Hub.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEXUS ARCADE v2 — Cyberpunk Game Hub</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;400;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
:root {
  --cyan:#00f5ff;--purple:#b44fff;--green:#39ff14;--pink:#ff2d78;--orange:#ff8c00;--gold:#ffd700;
  --bg:#04060f;--bg2:#080c1a;--bg3:#0c1224;
  --glass:rgba(255,255,255,0.04);--glass-border:rgba(255,255,255,0.08);
  --text:#e0e8ff;--muted:#6a7b9e;
  --acc1:var(--cyan);--acc2:var(--purple);
  --theme-bg:var(--bg);--theme-grid:rgba(0,245,255,0.04);--theme-particle:rgba(0,245,255,1);
}
*{margin:0;padding:0;box-sizing:border-box}
html,body{width:100%;height:100%;overflow-x:hidden}
body{background:var(--theme-bg);color:var(--text);font-family:'Rajdhani',sans-serif;font-size:16px;line-height:1.5;min-height:100vh;transition:background .6s}
#bg-canvas{position:fixed;top:0;left:0;width:100%;height:100%;z-index:0;opacity:.5}
.app{position:relative;z-index:1;min-height:100vh;display:flex;flex-direction:column}

/* ── HEADER ── */
.header{display:flex;align-items:center;justify-content:space-between;padding:16px 32px;border-bottom:1px solid var(--glass-border);backdrop-filter:blur(20px);background:rgba(4,6,15,.75);position:sticky;top:0;z-index:100;flex-wrap:wrap;gap:10px}
.logo{font-family:'Orbitron',monospace;font-size:20px;font-weight:900;letter-spacing:.12em;cursor:pointer}
.logo em{font-style:normal;color:var(--acc1);text-shadow:0 0 14px var(--acc1)}
.logo b{color:#fff}
.logo s{text-decoration:none;color:var(--acc2);text-shadow:0 0 14px var(--acc2)}
.header-right{display:flex;gap:12px;align-items:center;flex-wrap:wrap}
.stat-pill{background:var(--glass);border:1px solid var(--glass-border);border-radius:8px;padding:5px 14px;font-family:'Share Tech Mono',monospace;font-size:12px;display:flex;align-items:center;gap:7px;white-space:nowrap}
.dot{width:6px;height:6px;border-radius:50%;background:var(--cyan);box-shadow:0 0 8px var(--cyan);animation:pulse 2s infinite;flex-shrink:0}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.35}}
.xp-bar-wrap{display:flex;align-items:center;gap:8px;font-family:'Share Tech Mono',monospace;font-size:11px}
.xp-bar{width:100px;height:5px;background:rgba(255,255,255,.1);border-radius:3px;overflow:hidden}
.xp-bar-fill{height:100%;background:linear-gradient(90deg,var(--acc1),var(--acc2));border-radius:3px;transition:width .6s ease}

/* ── LAYOUT ── */
.main{flex:1;padding:16px 24px}
.screen{display:none;animation:fadeUp .35s ease}
.screen.active{display:block}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:translateY(0)}}

/* ── NAV ── */
.nav-tabs{display:flex;gap:5px;margin-bottom:24px;flex-wrap:wrap}
.nav-tab{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:.1em;padding:9px 18px;border:1px solid var(--glass-border);background:var(--glass);border-radius:8px;cursor:pointer;transition:all .2s;color:var(--muted)}
.nav-tab:hover{color:#fff;border-color:var(--acc1);box-shadow:0 0 12px rgba(0,245,255,.15)}
.nav-tab.active{color:#fff;border-color:var(--acc1);box-shadow:0 0 16px rgba(0,245,255,.2),inset 0 0 18px rgba(0,245,255,.05)}
.sec-title{font-family:'Orbitron',monospace;font-size:13px;letter-spacing:.12em;color:var(--acc1);margin-bottom:18px;display:flex;align-items:center;gap:10px}
.sec-title::after{content:'';flex:1;height:1px;background:linear-gradient(90deg,var(--acc1),transparent);opacity:.35}

/* ── CHARS ── */
.chars-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:18px;margin-bottom:28px}
.char-card{background:var(--glass);border:1px solid var(--glass-border);border-radius:14px;padding:22px;cursor:pointer;transition:all .3s;position:relative;overflow:hidden;backdrop-filter:blur(12px)}
.char-card.locked{opacity:.55;cursor:default}
.char-card.locked{opacity:.5;cursor:default;filter:grayscale(.4)}
.char-card.locked .char-lock-badge{display:flex}
.char-lock-badge{display:none;position:absolute;top:12px;right:12px;font-family:'Orbitron',monospace;font-size:8px;background:rgba(0,0,0,.85);color:var(--muted);padding:3px 9px;border-radius:4px;border:1px solid var(--glass-border);letter-spacing:.08em;align-items:center;gap:5px;z-index:2}
.game-btn.locked-game{opacity:.38;cursor:default}
.game-btn.locked-game .gsub{color:var(--gold)}
.char-card:not(.locked):hover{transform:translateY(-4px);box-shadow:0 0 36px rgba(0,245,255,.15),0 12px 40px rgba(0,0,0,.5)}
.char-card.selected{box-shadow:0 0 44px rgba(0,245,255,.22),0 8px 40px rgba(0,0,0,.6)}
.char-glow{position:absolute;top:-40%;left:-40%;width:180%;height:180%;background:conic-gradient(from 0deg,transparent,rgba(0,245,255,.06),transparent,rgba(180,79,255,.06),transparent);animation:spin 10s linear infinite;opacity:0;transition:opacity .4s;pointer-events:none}
.char-card:hover .char-glow,.char-card.selected .char-glow{opacity:1}
@keyframes spin{to{transform:rotate(360deg)}}

/* Portrait */
.char-portrait{width:100%;height:160px;border-radius:10px;margin-bottom:16px;position:relative;overflow:hidden;display:flex;align-items:center;justify-content:center}
.char-portrait svg{width:100%;height:100%}
.char-portrait::after{content:'';position:absolute;bottom:0;left:0;right:0;height:40px;background:linear-gradient(to top,var(--card-bg,#080c1a),transparent)}

/* Header row */
.char-header{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:6px}
.char-name{font-family:'Orbitron',monospace;font-size:16px;font-weight:900;letter-spacing:.04em}
.char-id-tag{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--muted);letter-spacing:.1em;padding:2px 7px;border:1px solid var(--glass-border);border-radius:3px}
.char-role{font-size:10px;letter-spacing:.1em;text-transform:uppercase;margin-bottom:12px;opacity:.65}

/* Stat bars */
.char-stats{display:grid;grid-template-columns:1fr 1fr;gap:6px 16px;margin-bottom:14px}
.stat-row{display:flex;flex-direction:column;gap:3px}
.stat-label{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:.08em;color:var(--muted)}
.stat-bar{height:3px;background:rgba(255,255,255,.07);border-radius:2px;overflow:hidden}
.stat-fill{height:100%;border-radius:2px;transition:width 1s ease}

/* Desc + ability */
.char-desc{font-size:12px;color:var(--muted);line-height:1.6;margin-bottom:12px}
.char-ability{background:rgba(0,0,0,.35);border-left:2px solid;border-radius:0 6px 6px 0;padding:8px 12px}
.ability-label{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:.1em;margin-bottom:3px;text-transform:uppercase}
.ability-desc-text{font-size:11px;color:var(--muted);line-height:1.5}

.sel-badge{position:absolute;top:12px;left:12px;background:var(--acc1);color:var(--bg);font-family:'Orbitron',monospace;font-size:8px;padding:3px 9px;border-radius:3px;font-weight:700;letter-spacing:.08em;z-index:2}
.unlock-cost{font-family:'Share Tech Mono',monospace;font-size:11px;color:var(--gold)}

/* ── HUB ── */
.hub-grid{display:grid;grid-template-columns:1fr 300px;gap:22px}
.game-select-scroll{display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap}
.game-btn{background:var(--glass);border:1px solid var(--glass-border);border-radius:10px;padding:14px 18px;cursor:pointer;transition:all .22s;text-align:center;color:var(--text);min-width:110px;position:relative}
.game-btn.locked-game{opacity:.4;cursor:default}
.game-btn.locked-game::after{content:'⊘';position:absolute;top:5px;right:7px;font-size:10px;color:var(--muted);font-style:normal}
.game-btn:not(.locked-game):hover{transform:translateY(-3px)}
.game-btn.gactive{box-shadow:0 0 18px var(--c,rgba(0,245,255,.3));border-color:var(--cb,var(--cyan))}
.gi{font-size:22px;margin-bottom:6px}
.gname{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:.06em;font-weight:700}
.gsub{font-size:10px;color:var(--muted);margin-top:3px}

/* ── SIDEBAR ── */
.sidebar{display:flex;flex-direction:column;gap:14px}
.side-card{background:var(--glass);border:1px solid var(--glass-border);border-radius:11px;padding:16px;backdrop-filter:blur(12px)}
.side-card h3{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:.1em;color:var(--muted);margin-bottom:12px;text-transform:uppercase}
.score-row{display:flex;justify-content:space-between;align-items:center;font-size:12px;padding:3px 0}
.score-row .sn{color:var(--muted);font-size:11px}
.score-row .sv{font-family:'Share Tech Mono',monospace;color:var(--acc1)}
.feed-list{max-height:180px;overflow-y:auto;display:flex;flex-direction:column;gap:0}
.feed-item{font-size:11px;font-family:'Share Tech Mono',monospace;padding:5px 0;border-bottom:1px solid var(--glass-border);display:flex;gap:7px}
.ftime{color:var(--acc2);flex-shrink:0;font-size:10px}

/* ── GAME VIEWPORT ── */
.game-viewport{border:1px solid var(--glass-border);border-radius:11px;overflow:hidden;background:rgba(4,6,15,.8);position:relative;min-height:580px}
canvas{display:block}
.g-info-bar{display:flex;justify-content:space-between;align-items:center;padding:12px 16px;flex-wrap:wrap;gap:8px;border-bottom:1px solid var(--glass-border)}
.g-info-bar .gi-stat{font-family:'Share Tech Mono',monospace;font-size:12px;display:flex;align-items:center;gap:6px}
.lives{display:flex;gap:5px}
.life{width:12px;height:12px;border-radius:50%;background:var(--pink);box-shadow:0 0 7px var(--pink);transition:.3s}
.life.lost{background:rgba(255,45,120,.15);box-shadow:none}

/* ── MEMORY ── */
.mem-wrap{padding:14px}
.mem-info{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;font-family:'Share Tech Mono',monospace;font-size:12px;flex-wrap:wrap;gap:8px}
.mem-grid{display:grid;gap:8px;justify-content:center;margin:0 auto}
.mem-card{width:76px;height:76px;perspective:600px;cursor:pointer;flex-shrink:0}
.mem-card-inner{width:100%;height:100%;position:relative;transform-style:preserve-3d;transition:transform .42s cubic-bezier(.4,0,.2,1)}
.mem-card.flipped .mem-card-inner,.mem-card.matched .mem-card-inner{transform:rotateY(180deg)}
.mem-face,.mem-back{position:absolute;inset:0;border-radius:9px;backface-visibility:hidden;display:flex;align-items:center;justify-content:center;font-size:26px;border:1px solid var(--glass-border)}
.mem-face{background:linear-gradient(135deg,rgba(0,245,255,.05),rgba(180,79,255,.05));color:var(--muted)}
.mem-back{background:linear-gradient(135deg,rgba(0,245,255,.1),rgba(180,79,255,.1));transform:rotateY(180deg);font-family:'Orbitron',monospace;font-size:22px;letter-spacing:-.02em}
.mem-card.matched .mem-back{border-color:var(--green);box-shadow:0 0 14px rgba(57,255,20,.3)}

/* ── SIMON ── */
.simon-wrap{padding:14px}
.simon-info{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;font-family:'Share Tech Mono',monospace;font-size:12px;flex-wrap:wrap;gap:8px}
.simon-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;max-width:300px;margin:0 auto 16px}
.simon-btn{height:110px;border-radius:12px;border:1px solid var(--glass-border);cursor:pointer;transition:all .12s;opacity:.55;position:relative}
.simon-btn.lit{opacity:1;box-shadow:0 0 36px var(--sc,#fff);transform:scale(1.035)}
.simon-btn:hover{opacity:.8}
[data-sid="0"]{background:linear-gradient(135deg,rgba(0,245,255,.15),rgba(0,245,255,.28));--sc:var(--cyan)}
[data-sid="1"]{background:linear-gradient(135deg,rgba(180,79,255,.15),rgba(180,79,255,.28));--sc:var(--purple)}
[data-sid="2"]{background:linear-gradient(135deg,rgba(57,255,20,.15),rgba(57,255,20,.28));--sc:var(--green)}
[data-sid="3"]{background:linear-gradient(135deg,rgba(255,45,120,.15),rgba(255,45,120,.28));--sc:var(--pink)}

/* ── SNAKE ── */
/* ── TYPING ── */
.typing-wrap{padding:14px;display:flex;flex-direction:column;align-items:center}
.typing-display{font-family:'Share Tech Mono',monospace;font-size:22px;letter-spacing:.12em;margin:16px 0;text-align:center;line-height:1.6}
.typing-display .t-correct{color:var(--green)}
.typing-display .t-wrong{color:var(--pink);text-decoration:underline}
.typing-display .t-pending{color:var(--muted)}
.typing-display .t-cursor{color:var(--cyan);animation:blink .7s infinite}
@keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
.typing-input{background:rgba(0,0,0,.4);border:1px solid var(--glass-border);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:15px;padding:10px 16px;border-radius:8px;width:100%;max-width:400px;outline:none;caret-color:var(--cyan)}
.typing-input:focus{border-color:var(--acc1);box-shadow:0 0 12px rgba(0,245,255,.2)}

/* ── BREAKOUT ── */

/* ── PONG ── */

/* ── SHOP / CUSTOMIZE ── */
.shop-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px}
.shop-item{background:var(--glass);border:1px solid var(--glass-border);border-radius:12px;padding:18px;position:relative;transition:all .25s}
.shop-item:not(.owned):hover{transform:translateY(-2px);border-color:rgba(255,215,0,.3);box-shadow:0 0 20px rgba(255,215,0,.1)}
.shop-item.owned{border-color:rgba(57,255,20,.25);box-shadow:0 0 12px rgba(57,255,20,.08)}
.shop-item.owned::after{content:'✓ OWNED';position:absolute;top:12px;right:12px;font-family:'Orbitron',monospace;font-size:8px;color:var(--green)}
.shop-preview{height:80px;border-radius:8px;margin-bottom:12px;display:flex;align-items:center;justify-content:center;font-size:28px;border:1px solid var(--glass-border)}
.shop-name{font-family:'Orbitron',monospace;font-size:12px;margin-bottom:4px}
.shop-desc{font-size:12px;color:var(--muted);margin-bottom:12px;line-height:1.5}
.shop-cost{font-family:'Share Tech Mono',monospace;font-size:13px;color:var(--gold);display:flex;align-items:center;gap:5px}
.customize-tabs{display:flex;gap:8px;margin-bottom:20px;flex-wrap:wrap}
.ctab{font-family:'Orbitron',monospace;font-size:10px;padding:7px 14px;border:1px solid var(--glass-border);border-radius:7px;cursor:pointer;color:var(--muted);transition:.2s}
.ctab.active{color:var(--gold);border-color:var(--gold);box-shadow:0 0 10px rgba(255,215,0,.15)}

/* ── BUTTONS ── */
.btn{font-family:'Orbitron',monospace;font-size:11px;letter-spacing:.07em;padding:9px 20px;border-radius:7px;border:1px solid var(--acc1);background:transparent;color:var(--acc1);cursor:pointer;transition:all .22s;position:relative;overflow:hidden;display:inline-flex;align-items:center;gap:6px}
.btn::before{content:'';position:absolute;inset:0;background:var(--acc1);opacity:0;transition:.22s}
.btn:hover{box-shadow:0 0 18px rgba(0,245,255,.25);color:#fff}
.btn:hover::before{opacity:.1}
.btn span{position:relative;z-index:1}
.btn-sm{padding:6px 14px;font-size:10px}
.btn-gold{border-color:var(--gold);color:var(--gold)}
.btn-gold::before{background:var(--gold)}
.btn-gold:hover{box-shadow:0 0 18px rgba(255,215,0,.25)}
.btn-pink{border-color:var(--pink);color:var(--pink)}
.btn-pink::before{background:var(--pink)}
.btn-pink:hover{box-shadow:0 0 18px rgba(255,45,120,.25)}
.btn-green{border-color:var(--green);color:var(--green)}
.btn-green::before{background:var(--green)}

/* ── OVERLAY ── */
.g-overlay{position:absolute;inset:0;background:rgba(4,6,15,.88);backdrop-filter:blur(8px);display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;z-index:30;border-radius:11px}
.g-overlay h2{font-family:'Orbitron',monospace;font-size:20px;font-weight:900;text-align:center}
.g-overlay .big-score{font-family:'Share Tech Mono',monospace;font-size:36px;color:var(--acc1);text-shadow:0 0 24px var(--acc1)}

/* ── NOTIFY ── */
.notif{position:fixed;top:76px;right:20px;background:rgba(0,245,255,.07);border:1px solid var(--cyan);border-radius:9px;padding:10px 18px;font-family:'Share Tech Mono',monospace;font-size:12px;color:var(--cyan);z-index:500;transition:all .35s;opacity:0;transform:translateX(16px);max-width:280px}
.notif.show{opacity:1;transform:translateX(0)}
.notif.purple{border-color:var(--purple);color:var(--purple);background:rgba(180,79,255,.07)}
.notif.green{border-color:var(--green);color:var(--green);background:rgba(57,255,20,.07)}
.notif.gold{border-color:var(--gold);color:var(--gold);background:rgba(255,215,0,.07)}

/* ── LEVEL BADGE ── */
.level-ring{display:flex;flex-direction:column;align-items:center;gap:2px;cursor:default}
.level-num{font-family:'Orbitron',monospace;font-size:13px;font-weight:900;color:var(--gold)}
.level-lbl{font-size:9px;color:var(--muted);letter-spacing:.06em}

/* ── SCROLLBAR ── */
::-webkit-scrollbar{width:3px}
::-webkit-scrollbar-thumb{background:var(--glass-border);border-radius:3px}
</style>
</head>
<body>
<canvas id="bg-canvas"></canvas>
<div class="app">

<!-- ═══ HEADER ═══ -->
<header class="header">
  <div class="logo" onclick="showTab('chars')"><em>NEX</em><b>US</b> <s>ARCADE</s></div>
  <div class="header-right">
    <div class="level-ring"><div class="level-num" id="hdr-level">Lv 1</div><div class="level-lbl">RANK</div></div>
    <div class="xp-bar-wrap">
      <span id="hdr-xp-lbl" style="color:var(--muted)">0/200</span>
      <div class="xp-bar"><div class="xp-bar-fill" id="xp-fill" style="width:0%"></div></div>
    </div>
    <div class="stat-pill"><span class="dot"></span><span id="hdr-char">No Agent</span></div>
    <div class="stat-pill">⬡ <span id="hdr-total-xp">0</span> XP</div>
  </div>
</header>

<!-- ═══ MAIN ═══ -->
<main class="main">
<nav class="nav-tabs">
  <div class="nav-tab active" data-tab="chars" onclick="showTab('chars')"><span>⬡ AGENTS</span></div>
  <div class="nav-tab" data-tab="hub" onclick="showTab('hub')"><span>◈ HUB</span></div>
  <div class="nav-tab" data-tab="shop" onclick="showTab('shop')"><span>◆ SHOP</span></div>
  <div class="nav-tab" data-tab="customize" onclick="showTab('customize')"><span>✦ CUSTOMIZE</span></div>
</nav>

<!-- ─── AGENTS ─── -->
<div class="screen active" id="screen-chars">
  <div class="sec-title">SELECT OPERATIVE</div>
  <div class="chars-grid" id="chars-grid"></div>
  <button class="btn" onclick="showTab('hub')"><span>▶ ENTER HUB</span></button>
</div>

<!-- ─── HUB ─── -->
<div class="screen" id="screen-hub">
  <div class="hub-grid">
    <div>
      <div class="sec-title">MISSION SELECT</div>
      <div class="game-select-scroll" id="game-btns"></div>
      <div class="game-viewport" id="game-viewport">

        <!-- SPACE SHOOTER -->
        <div id="gw-dodge" style="display:none">
          <div class="g-info-bar">
            <div class="gi-stat">SCORE: <span id="d-score" style="color:var(--cyan)">0</span></div>
            <div class="gi-stat">WAVE: <span id="d-level" style="color:var(--purple)">W1</span></div>
            <div class="gi-stat" style="font-size:10px;color:var(--muted)">WASD MOVE · SPACE/↑ SHOOT</div>
            <div class="lives" id="d-lives"></div>
            <button class="btn btn-sm" onclick="dodgeStart()"><span>▶ LAUNCH</span></button>
          </div>
          <canvas id="dodge-canvas" width="680" height="520" style="width:100%"></canvas>
        </div>

        <!-- MEMORY -->
        <div id="gw-memory" style="display:none" class="mem-wrap">
          <div class="mem-info">
            <span>MOVES:<span id="m-moves" style="color:var(--cyan)"> 0</span></span>
            <span>PAIRS:<span id="m-pairs" style="color:var(--green)"> 0/10</span></span>
            <span>COMBO:<span id="m-combo" style="color:var(--gold)"> ×1</span></span>
            <span>SCORE:<span id="m-score" style="color:var(--purple)"> 0</span></span>
            <span id="m-timer" style="color:var(--pink)">00:00</span>
            <button class="btn btn-sm" onclick="memReset()"><span>RESET</span></button>
          </div>
          <div class="mem-grid" id="mem-grid"></div>
        </div>

        <!-- SEQUENCE (SIMON) -->
        <div id="gw-simon" style="display:none" class="simon-wrap">
          <div class="simon-info">
            <span>ROUND:<span id="s-round" style="color:var(--cyan)"> 0</span></span>
            <span>SCORE:<span id="s-score" style="color:var(--green)"> 0</span></span>
            <span id="s-status" style="color:var(--purple)">READY</span>
            <button class="btn btn-sm" onclick="simonStart()"><span>▶ START</span></button>
          </div>
          <div class="simon-grid">
            <div class="simon-btn" data-sid="0" onclick="simonPress(0)"></div>
            <div class="simon-btn" data-sid="1" onclick="simonPress(1)"></div>
            <div class="simon-btn" data-sid="2" onclick="simonPress(2)"></div>
            <div class="simon-btn" data-sid="3" onclick="simonPress(3)"></div>
          </div>
        </div>

        <!-- SNAKE -->
        <div id="gw-snake" style="display:none">
          <div class="g-info-bar">
            <div class="gi-stat">SCORE:<span id="sn-score" style="color:var(--cyan)"> 0</span></div>
            <div class="gi-stat">SPEED:<span id="sn-speed" style="color:var(--purple)"> 1</span></div>
            <div class="gi-stat" style="font-size:10px;color:var(--muted)">WASD · ★=BONUS FOOD · ◆=POWERUP</div>
            <button class="btn btn-sm" onclick="snakeStart()"><span>▶ START</span></button>
          </div>
          <canvas id="snake-canvas" width="552" height="480" style="display:block;margin:0 auto"></canvas>
        </div>

        <!-- TYPING -->
        <div id="gw-typing" style="display:none" class="typing-wrap">
          <div style="display:flex;gap:14px;font-family:'Share Tech Mono',monospace;font-size:12px;margin-bottom:8px;flex-wrap:wrap;justify-content:center;align-items:center">
            <span>WPM:<span id="ty-wpm" style="color:var(--cyan)"> 0</span></span>
            <span>ACC:<span id="ty-acc" style="color:var(--green)"> 100%</span></span>
            <span id="ty-timer" style="color:var(--purple)">30s</span>
            <span>COMBO:<span id="ty-combo" style="color:var(--gold)"> ×1</span></span>
            <span>SCORE:<span id="ty-score" style="color:var(--pink)"> 0</span></span>
            <select id="ty-diff-sel" style="background:rgba(0,0,0,.5);border:1px solid var(--glass-border);color:var(--muted);font-family:'Share Tech Mono',monospace;font-size:11px;padding:4px 8px;border-radius:5px" onchange="TY.difficulty=this.value;typingInit()">
              <option value="easy">EASY</option><option value="normal" selected>NORMAL</option><option value="hard">HARD</option>
            </select>
            <button class="btn btn-sm" onclick="typingStart()"><span>▶ HACK</span></button>
          </div>
          <div class="typing-display" id="ty-display">Press HACK to begin...</div>
          <input class="typing-input" id="ty-input" placeholder="Type here..." autocomplete="off" spellcheck="false" oninput="typingInput()" disabled>
        </div>

        <!-- BREAKOUT -->
        <div id="gw-breakout" style="display:none">
          <div class="g-info-bar">
            <div class="gi-stat">SCORE:<span id="br-score" style="color:var(--cyan)"> 0</span></div>
            <div class="gi-stat">LIVES:<span id="br-lives" style="color:var(--pink)"> 3</span></div>
            <div class="gi-stat">LEVEL:<span id="br-level" style="color:var(--purple)"> 1</span></div>
            <div class="gi-stat" style="font-size:11px;color:var(--muted)">MOUSE / ←→</div>
            <button class="btn btn-sm" onclick="breakoutStart()"><span>▶ START</span></button>
          </div>
          <canvas id="breakout-canvas" width="640" height="500" style="width:100%;cursor:none"></canvas>
        </div>

        <!-- DEFAULT -->
        <div id="gw-default" style="padding:80px 20px;text-align:center;color:var(--muted)">
          <div style="font-size:52px;margin-bottom:18px;opacity:.2">◈</div>
          <div style="font-family:'Orbitron',monospace;font-size:13px;letter-spacing:.1em">SELECT A MISSION ABOVE</div>
          <div style="font-size:12px;margin-top:8px;opacity:.6">⊘ = Unlock in SHOP with XP</div>
        </div>
      </div>
    </div>

    <!-- SIDEBAR -->
    <div class="sidebar">
      <div class="side-card">
        <h3>Active Agent</h3>
        <div style="display:flex;align-items:center;gap:10px">
          <div id="ac-avatar" style="width:42px;height:42px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:20px;background:rgba(0,245,255,.1)">?</div>
          <div><div style="font-family:'Orbitron',monospace;font-size:12px" id="ac-name">No Agent</div><div style="font-size:11px;color:var(--muted)" id="ac-role">Select from Agents</div></div>
        </div>
        <div id="ac-ability" style="margin-top:10px;font-size:11px;color:var(--muted);display:none"></div>
      </div>
      <div class="side-card">
        <h3>High Scores</h3>
        <div class="score-row"><span class="sn">◈ DODGE</span><span class="sv" id="hs-dodge">0</span></div>
        <div class="score-row"><span class="sn">⬡ MEMORY</span><span class="sv" id="hs-memory">—</span></div>
        <div class="score-row"><span class="sn">◆ SEQUENCE</span><span class="sv" id="hs-simon">0</span></div>
        <div class="score-row"><span class="sn">⟁ SNAKE</span><span class="sv" id="hs-snake">0</span></div>
        <div class="score-row"><span class="sn">▶ TYPING</span><span class="sv" id="hs-typing">0</span></div>
        <div class="score-row"><span class="sn">▣ BREAKOUT</span><span class="sv" id="hs-breakout">0</span></div>
      </div>
      <div class="side-card" style="text-align:center">
        <h3>Total XP</h3>
        <div style="font-family:'Orbitron',monospace;font-size:30px;color:var(--acc1);text-shadow:0 0 18px var(--acc1)" id="sb-total-xp">0</div>
        <div style="font-size:10px;color:var(--muted);margin-top:3px">NEXUS CREDITS</div>
      </div>
      <div class="side-card">
        <h3>Mission Log</h3>
        <div class="feed-list" id="feed-list"></div>
      </div>
    </div>
  </div>
</div>

<!-- ─── SHOP ─── -->
<div class="screen" id="screen-shop">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:20px;flex-wrap:wrap;gap:10px">
    <div class="sec-title" style="margin:0;flex:1">NEXUS SHOP</div>
    <div class="stat-pill">⬡ <span id="shop-xp">0</span> XP available</div>
  </div>
  <div id="shop-content"></div>
</div>

<!-- ─── CUSTOMIZE ─── -->
<div class="screen" id="screen-customize">
  <div class="sec-title">CUSTOMIZE HUB</div>
  <div class="customize-tabs">
    <div class="ctab active" onclick="showCTab('themes')">THEMES</div>
    <div class="ctab" onclick="showCTab('backgrounds')">BACKGROUNDS</div>
    <div class="ctab" onclick="showCTab('accents')">ACCENTS</div>
    <div class="ctab" onclick="showCTab('hud')">HUD</div>
  </div>
  <div id="customize-content"></div>
</div>

</main>
</div>

<div class="notif" id="notif"></div>

<script>
// ═══════════════════════════════════════════════════
//  SAVE / LOAD (localStorage)
// ═══════════════════════════════════════════════════
const SAVE_KEY = 'nexus_arcade_v3';
function saveGame(){
  try{localStorage.setItem(SAVE_KEY,JSON.stringify({
    totalXP:G.totalXP, level:G.level, xpToNext:G.xpToNext,
    activeChar:G.activeChar, highScores:G.highScores,
    owned:G.owned, equipped:G.equipped
  }));}catch(e){}
}
function loadGame(){
  try{
    const d=JSON.parse(localStorage.getItem(SAVE_KEY)||'{}');
    if(d.totalXP!==undefined) G.totalXP=d.totalXP;
    if(d.level!==undefined) G.level=d.level;
    if(d.xpToNext!==undefined) G.xpToNext=d.xpToNext;
    if(d.activeChar!==undefined) G.activeChar=d.activeChar;
    if(d.highScores) Object.assign(G.highScores,d.highScores);
    if(d.owned) Object.assign(G.owned,d.owned);
    if(d.equipped) Object.assign(G.equipped,d.equipped);
  }catch(e){}
}

// ═══════════════════════════════════════════════════
//  GLOBAL STATE
// ═══════════════════════════════════════════════════
const G = {
  totalXP:0, level:1, xpToNext:500,
  activeChar:null,
  highScores:{dodge:0,memory:null,simon:0,snake:0,typing:0,breakout:0},
  owned:{
    chars:['viper','nova','axiom'],   // all 3 base chars unlocked
    games:['dodge','memory','simon'], // 3 base games
    themes:['default'],
    backgrounds:['particles'],
    accents:['cyan-purple'],
    huds:['minimal']
  },
  equipped:{theme:'default',background:'particles',accent:'cyan-purple',hud:'minimal'},
  // Passive bonuses from active character
  bonus:{extraLife:false,memPeek:false,simonFast:false,dodgeMult:false,memLong:false,snakeSlow:false,typingTime:false,breakoutBig:false}
};
loadGame();

// ═══════════════════════════════════════════════════
//  BACKGROUND CANVAS
// ═══════════════════════════════════════════════════
(function(){
  const c=document.getElementById('bg-canvas');
  const ctx=c.getContext('2d');
  let W,H,pts=[];
  function resize(){W=c.width=innerWidth;H=c.height=innerHeight;pts=[];for(let i=0;i<90;i++)pts.push({x:Math.random()*W,y:Math.random()*H,vx:(Math.random()-.5)*.35,vy:(Math.random()-.5)*.35,r:Math.random()*1.5+.4})}
  resize();addEventListener('resize',resize);
  function hex2rgb(h){const r=parseInt(h.slice(1,3),16),g=parseInt(h.slice(3,5),16),b=parseInt(h.slice(5,7),16);return `${r},${g},${b}`}
  function draw(){
    ctx.clearRect(0,0,W,H);
    // BG effects based on equipped background
    const bg=G.equipped.background;
    if(bg==='grid'||bg==='particles'){
      const t=Date.now()*.00018;
      ctx.strokeStyle='rgba(0,245,255,.035)';ctx.lineWidth=.8;
      for(let x=(t*60)%60-60;x<W;x+=60){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke()}
      for(let y=(t*20)%60-60;y<H;y+=60){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke()}
    }
    if(bg==='hexgrid'){
      const t=Date.now()*.0001;const sz=50;
      ctx.strokeStyle='rgba(180,79,255,.04)';ctx.lineWidth=.8;
      for(let row=0;row<H/sz+2;row++){for(let col=0;col<W/sz+2;col++){
        const ox=(row%2)*sz*.5+(t*10)%sz,oy=(t*5)%sz;
        const x=col*sz+ox-sz,y=row*sz*.866+oy-sz;
        ctx.beginPath();for(let i=0;i<6;i++){const a=i*Math.PI/3;ctx.lineTo(x+sz*.5*Math.cos(a),y+sz*.5*Math.sin(a))}ctx.closePath();ctx.stroke();
      }}
    }
    if(bg==='rain'){
      const t=Date.now();ctx.strokeStyle='rgba(0,245,255,.06)';ctx.lineWidth=.5;
      for(let i=0;i<80;i++){const x=(i*W/80+(t*.1))%W,y=(i*23+t*.3)%H;ctx.beginPath();ctx.moveTo(x,y);ctx.lineTo(x-2,y+14);ctx.stroke()}
    }
    if(bg==='nebula'){
      const t=Date.now()*.001;
      ctx.fillStyle=`rgba(100,0,200,${.015+Math.sin(t)*.008})`;ctx.fillRect(0,0,W/2,H);
      ctx.fillStyle=`rgba(0,100,200,${.015+Math.cos(t*.7)*.008})`;ctx.fillRect(W/2,0,W/2,H);
    }
    if(bg==='particles'||bg==='grid'){
      pts.forEach(p=>{
        p.x+=p.vx;p.y+=p.vy;
        if(p.x<0||p.x>W)p.vx*=-1;if(p.y<0||p.y>H)p.vy*=-1;
        ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
        ctx.fillStyle='rgba(0,245,255,.4)';ctx.fill();
      });
      pts.forEach((a,i)=>pts.slice(i+1,i+8).forEach(b=>{
        const dx=a.x-b.x,dy=a.y-b.y,d=Math.sqrt(dx*dx+dy*dy);
        if(d<120){ctx.beginPath();ctx.moveTo(a.x,a.y);ctx.lineTo(b.x,b.y);ctx.strokeStyle=`rgba(0,245,255,${(1-d/120)*.07})`;ctx.lineWidth=.4;ctx.stroke()}
      }));
    }
    requestAnimationFrame(draw);
  }
  draw();
})();

// ═══════════════════════════════════════════════════
//  CHARACTERS
// ═══════════════════════════════════════════════════
const CHARS=[
  {id:'viper',name:'V1PER',role:'Infiltration Specialist',color:'var(--cyan)',hex:'#00f5ff',
   stats:{agility:95,hacking:88,stealth:99,combat:60},
   desc:'A rogue AI fragment that escaped the corporate mainframe. Cold, calculated, lethal in the digital shadows.',
   abilityName:'Ghost Protocol',abilityDesc:'+1 extra life in Dodge. Memory cards peek before hiding.',
   unlockCost:0,
   applyBonus:()=>{G.bonus.extraLife=true;G.bonus.memPeek=true},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="vbg" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#001820"/><stop offset="100%" stop-color="#000810"/></linearGradient>
       <filter id="vglow"><feGaussianBlur stdDeviation="2.5" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
     </defs>
     <rect width="200" height="160" fill="url(#vbg)"/>
     <!-- Hex grid bg -->
     <g opacity=".12" stroke="#00f5ff" stroke-width=".5" fill="none">
       <polygon points="30,10 50,10 60,27 50,44 30,44 20,27"/><polygon points="70,10 90,10 100,27 90,44 70,44 60,27"/>
       <polygon points="110,10 130,10 140,27 130,44 110,44 100,27"/><polygon points="150,10 170,10 180,27 170,44 150,44 140,27"/>
       <polygon points="10,44 30,44 40,61 30,78 10,78 0,61"/><polygon points="50,44 70,44 80,61 70,78 50,78 40,61"/>
       <polygon points="90,44 110,44 120,61 110,78 90,78 80,61"/><polygon points="130,44 150,44 160,61 150,78 130,78 120,61"/>
       <polygon points="170,44 190,44 200,61 190,78 170,78 160,61"/>
     </g>
     <!-- Body silhouette -->
     <g filter="url(#vglow)">
       <!-- Torso -->
       <path d="M80,155 L75,110 Q75,95 100,90 Q125,95 125,110 L120,155 Z" fill="#001a22" stroke="#00f5ff" stroke-width="1"/>
       <!-- Shoulder plates -->
       <path d="M75,110 Q60,105 58,95 L68,88 Q75,95 75,110 Z" fill="#00f5ff22" stroke="#00f5ff" stroke-width=".8"/>
       <path d="M125,110 Q140,105 142,95 L132,88 Q125,95 125,110 Z" fill="#00f5ff22" stroke="#00f5ff" stroke-width=".8"/>
       <!-- Neck -->
       <rect x="93" y="78" width="14" height="14" rx="2" fill="#001a22" stroke="#00f5ff" stroke-width=".8"/>
       <!-- Head -->
       <rect x="76" y="42" width="48" height="38" rx="8" fill="#001a22" stroke="#00f5ff" stroke-width="1.2"/>
       <!-- Visor -->
       <rect x="80" y="52" width="40" height="12" rx="3" fill="#00f5ff" opacity=".9"/>
       <rect x="82" y="54" width="36" height="8" rx="2" fill="#001a22" opacity=".6"/>
       <!-- Visor glow lines -->
       <line x1="88" y1="58" x2="112" y2="58" stroke="#00f5ff" stroke-width="1.5" opacity=".8"/>
       <!-- Facial details -->
       <rect x="84" y="68" width="12" height="2" rx="1" fill="#00f5ff" opacity=".4"/>
       <rect x="104" y="68" width="12" height="2" rx="1" fill="#00f5ff" opacity=".4"/>
       <!-- Crown antenna -->
       <line x1="100" y1="42" x2="100" y2="30" stroke="#00f5ff" stroke-width="1"/>
       <circle cx="100" cy="28" r="3" fill="#00f5ff" opacity=".9"/>
       <circle cx="100" cy="28" r="6" fill="none" stroke="#00f5ff" stroke-width=".5" opacity=".4"/>
       <!-- Chest circuit lines -->
       <path d="M88,115 L88,130 L94,130 L94,140" stroke="#00f5ff" stroke-width=".7" fill="none" opacity=".5"/>
       <path d="M112,115 L112,130 L106,130 L106,140" stroke="#00f5ff" stroke-width=".7" fill="none" opacity=".5"/>
       <rect x="95" y="118" width="10" height="10" rx="2" fill="none" stroke="#00f5ff" stroke-width=".8" opacity=".7"/>
     </g>
     <!-- Scan line animation -->
     <rect x="0" y="0" width="200" height="2" fill="#00f5ff" opacity=".15">
       <animateTransform attributeName="transform" type="translate" values="0,0;0,160;0,0" dur="3s" repeatCount="indefinite"/>
     </rect>
     <!-- Corner brackets -->
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#00f5ff" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#00f5ff" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
  {id:'nova',name:'N0VA',role:'Chaos Engine',color:'var(--purple)',hex:'#b44fff',
   stats:{agility:72,hacking:95,stealth:45,combat:88},
   desc:'Born from a supernova simulation gone wrong. Chaotic energy given sentience, thriving on entropy.',
   abilityName:'Pattern Burst',abilityDesc:'Sequence 20% faster. Dodge score ×1.5. Snake slower start.',
   unlockCost:0,
   applyBonus:()=>{G.bonus.simonFast=true;G.bonus.dodgeMult=true;G.bonus.snakeSlow=true},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="nbg" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#0e001a"/><stop offset="100%" stop-color="#08000f"/></linearGradient>
       <filter id="nglow"><feGaussianBlur stdDeviation="3" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
       <radialGradient id="ncore" cx="50%" cy="50%"><stop offset="0%" stop-color="#b44fff" stop-opacity=".3"/><stop offset="100%" stop-color="transparent"/></radialGradient>
     </defs>
     <rect width="200" height="160" fill="url(#nbg)"/>
     <!-- Energy field -->
     <circle cx="100" cy="80" r="60" fill="url(#ncore)"/>
     <!-- Orbiting particles -->
     <g opacity=".7" stroke="#b44fff" stroke-width=".8" fill="none">
       <ellipse cx="100" cy="80" rx="55" ry="20" opacity=".2"><animateTransform attributeName="transform" type="rotate" values="0 100 80;360 100 80" dur="6s" repeatCount="indefinite"/></ellipse>
       <ellipse cx="100" cy="80" rx="40" ry="55" opacity=".15"><animateTransform attributeName="transform" type="rotate" values="60 100 80;420 100 80" dur="8s" repeatCount="indefinite"/></ellipse>
     </g>
     <g filter="url(#nglow)">
       <!-- Core body -->
       <path d="M78,155 L74,108 Q74,92 100,86 Q126,92 126,108 L122,155 Z" fill="#0e001a" stroke="#b44fff" stroke-width="1"/>
       <!-- Energy spikes -->
       <path d="M100,86 L88,62 L100,70 L112,62 Z" fill="#b44fff" opacity=".3" stroke="#b44fff" stroke-width=".8"/>
       <!-- Shoulder energy -->
       <path d="M74,108 Q55,100 50,88 L66,82 Q74,92 74,108 Z" fill="#b44fff18" stroke="#b44fff" stroke-width=".8"/>
       <path d="M126,108 Q145,100 150,88 L134,82 Q126,92 126,108 Z" fill="#b44fff18" stroke="#b44fff" stroke-width=".8"/>
       <!-- Neck -->
       <rect x="93" y="74" width="14" height="14" rx="2" fill="#0e001a" stroke="#b44fff" stroke-width=".8"/>
       <!-- Head - angular -->
       <path d="M78,40 L100,32 L122,40 L126,72 L100,78 L74,72 Z" fill="#0e001a" stroke="#b44fff" stroke-width="1.2"/>
       <!-- Eyes - glowing triangles -->
       <polygon points="84,53 96,53 90,63" fill="#b44fff" opacity=".9"/>
       <polygon points="104,53 116,53 110,63" fill="#b44fff" opacity=".9"/>
       <!-- Eye cores -->
       <circle cx="90" cy="57" r="3" fill="#fff" opacity=".8"/>
       <circle cx="110" cy="57" r="3" fill="#fff" opacity=".8"/>
       <!-- Crown energy -->
       <path d="M90,32 L85,20 M100,32 L100,18 M110,32 L115,20" stroke="#b44fff" stroke-width="1.2" opacity=".8"/>
       <circle cx="85" cy="19" r="2" fill="#b44fff"/><circle cx="100" cy="17" r="2.5" fill="#b44fff"/><circle cx="115" cy="19" r="2" fill="#b44fff"/>
       <!-- Chest sigil -->
       <polygon points="100,106 107,118 100,130 93,118" fill="none" stroke="#b44fff" stroke-width="1" opacity=".8"/>
       <circle cx="100" cy="118" r="4" fill="#b44fff" opacity=".6"/>
     </g>
     <!-- Energy pulse -->
     <circle cx="100" cy="80" r="30" fill="none" stroke="#b44fff" stroke-width=".5" opacity=".3">
       <animate attributeName="r" values="30;65;30" dur="3s" repeatCount="indefinite"/>
       <animate attributeName="opacity" values=".3;0;.3" dur="3s" repeatCount="indefinite"/>
     </circle>
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#b44fff" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#b44fff" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
  {id:'axiom',name:'AX10M',role:'Logic Core',color:'var(--green)',hex:'#39ff14',
   stats:{agility:55,hacking:99,stealth:70,combat:65},
   desc:'The oldest AI in the Nexus. Processes a trillion variables per second. Methodical. Inevitable.',
   abilityName:'Total Recall',abilityDesc:'Memory cards stay visible longer. Typing bonus time +5s.',
   unlockCost:0,
   applyBonus:()=>{G.bonus.memLong=true;G.bonus.typingTime=true},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="abg" x1="0" y1="1" x2="1" y2="0"><stop offset="0%" stop-color="#001400"/><stop offset="100%" stop-color="#000a00"/></linearGradient>
       <filter id="aglow"><feGaussianBlur stdDeviation="2" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
     </defs>
     <rect width="200" height="160" fill="url(#abg)"/>
     <!-- Matrix rain bg -->
     <g font-family="monospace" font-size="8" fill="#39ff14" opacity=".08">
       <text x="8" y="20">01101</text><text x="45" y="35">10010</text><text x="82" y="15">11001</text>
       <text x="120" y="28">01010</text><text x="158" y="18">10110</text>
       <text x="15" y="55">10101</text><text x="150" y="50">01100</text>
       <text x="5" y="90">11010</text><text x="160" y="85">00111</text>
     </g>
     <g filter="url(#aglow)">
       <!-- Body - boxy mechanical -->
       <rect x="72" y="100" width="56" height="55" rx="3" fill="#001400" stroke="#39ff14" stroke-width="1"/>
       <!-- Chest panel -->
       <rect x="80" y="108" width="40" height="28" rx="2" fill="none" stroke="#39ff14" stroke-width=".7" opacity=".5"/>
       <!-- Circuit chest -->
       <line x1="92" y1="108" x2="92" y2="136" stroke="#39ff14" stroke-width=".5" opacity=".5"/>
       <line x1="108" y1="108" x2="108" y2="136" stroke="#39ff14" stroke-width=".5" opacity=".5"/>
       <line x1="80" y1="120" x2="120" y2="120" stroke="#39ff14" stroke-width=".5" opacity=".5"/>
       <!-- Core -->
       <rect x="94" y="112" width="12" height="12" rx="1" fill="#39ff14" opacity=".2" stroke="#39ff14" stroke-width=".8"/>
       <rect x="96" y="114" width="8" height="8" rx="1" fill="#39ff14" opacity=".5"/>
       <!-- Shoulder blocks -->
       <rect x="50" y="100" width="22" height="30" rx="3" fill="#001400" stroke="#39ff14" stroke-width=".8"/>
       <rect x="128" y="100" width="22" height="30" rx="3" fill="#001400" stroke="#39ff14" stroke-width=".8"/>
       <!-- Arm connectors -->
       <rect x="62" y="108" width="10" height="4" rx="1" fill="#39ff14" opacity=".6"/>
       <rect x="128" y="108" width="10" height="4" rx="1" fill="#39ff14" opacity=".6"/>
       <!-- Neck -->
       <rect x="90" y="88" width="20" height="14" rx="2" fill="#001400" stroke="#39ff14" stroke-width=".8"/>
       <line x1="95" y1="88" x2="95" y2="102" stroke="#39ff14" stroke-width=".5" opacity=".5"/>
       <line x1="105" y1="88" x2="105" y2="102" stroke="#39ff14" stroke-width=".5" opacity=".5"/>
       <!-- Head - boxy with panels -->
       <rect x="70" y="44" width="60" height="46" rx="4" fill="#001400" stroke="#39ff14" stroke-width="1.2"/>
       <!-- Head panels -->
       <rect x="74" y="48" width="52" height="6" rx="1" fill="#39ff14" opacity=".15" stroke="#39ff14" stroke-width=".5"/>
       <!-- Eyes - rectangular digital -->
       <rect x="78" y="58" width="18" height="10" rx="2" fill="#39ff14" opacity=".15" stroke="#39ff14" stroke-width=".8"/>
       <rect x="104" y="58" width="18" height="10" rx="2" fill="#39ff14" opacity=".15" stroke="#39ff14" stroke-width=".8"/>
       <!-- Eye displays -->
       <rect x="80" y="60" width="14" height="6" rx="1" fill="#39ff14" opacity=".7"/>
       <rect x="106" y="60" width="14" height="6" rx="1" fill="#39ff14" opacity=".7"/>
       <!-- Data scrolling in eyes -->
       <rect x="81" y="61" width="4" height="4" fill="#001400" opacity=".6"><animate attributeName="opacity" values=".6;0;.6" dur="1.2s" repeatCount="indefinite"/></rect>
       <rect x="107" y="61" width="4" height="4" fill="#001400" opacity=".6"><animate attributeName="opacity" values=".6;0;.6" dur=".9s" repeatCount="indefinite"/></rect>
       <!-- Chin data strip -->
       <rect x="82" y="76" width="36" height="4" rx="1" fill="none" stroke="#39ff14" stroke-width=".6" opacity=".5"/>
       <rect x="84" y="77" width="8" height="2" rx=".5" fill="#39ff14" opacity=".6"/>
       <rect x="94" y="77" width="5" height="2" rx=".5" fill="#39ff14" opacity=".4"/>
       <!-- Top antenna array -->
       <rect x="85" y="40" width="4" height="6" fill="#39ff14" opacity=".7"/>
       <rect x="98" y="36" width="4" height="8" fill="#39ff14" opacity=".9"/>
       <rect x="111" y="40" width="4" height="6" fill="#39ff14" opacity=".7"/>
     </g>
     <!-- Blinking status LED -->
     <circle cx="188" cy="12" r="3" fill="#39ff14"><animate attributeName="opacity" values="1;0.2;1" dur="1.5s" repeatCount="indefinite"/></circle>
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#39ff14" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#39ff14" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
  {id:'ghost',name:'GH0ST',role:'Phantom Operative',color:'var(--orange)',hex:'#ff8c00',
   stats:{agility:99,hacking:72,stealth:98,combat:78},
   desc:'Exists between digital realms. Phase-shifts through firewalls. Impossible to track, impossible to stop.',
   abilityName:'Phase Shift',abilityDesc:'Breakout paddle 30% wider. Dodge 2s invincibility on hit.',
   unlockCost:900,
   applyBonus:()=>{G.bonus.breakoutBig=true},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="gbg2" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#180a00"/><stop offset="100%" stop-color="#0a0400"/></linearGradient>
       <filter id="gglow"><feGaussianBlur stdDeviation="3" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
     </defs>
     <rect width="200" height="160" fill="url(#gbg2)"/>
     <!-- Phase distortion lines -->
     <g stroke="#ff8c00" stroke-width=".4" opacity=".1">
       <line x1="0" y1="30" x2="200" y2="28"/><line x1="0" y1="60" x2="200" y2="58"/>
       <line x1="0" y1="90" x2="200" y2="88"/><line x1="0" y1="120" x2="200" y2="122"/>
     </g>
     <g filter="url(#gglow)" opacity=".5">
       <!-- Ghost duplicate (offset) -->
       <path d="M85,155 L82,108 Q82,88 100,84 Q118,88 118,108 L115,155 Z" fill="none" stroke="#ff8c0040" stroke-width="2" transform="translate(6,0)"/>
       <rect x="82" y="44" width="42" height="42" rx="8" fill="none" stroke="#ff8c0030" stroke-width="1.5" transform="translate(6,-2)"/>
     </g>
     <g filter="url(#gglow)">
       <!-- Body -->
       <path d="M82,155 L79,108 Q79,88 100,84 Q121,88 121,108 L118,155 Z" fill="#180a00" stroke="#ff8c00" stroke-width="1"/>
       <!-- Cloak segments -->
       <path d="M79,108 Q62,102 58,90 L70,84 Q79,94 79,108 Z" fill="#ff8c0018" stroke="#ff8c00" stroke-width=".8"/>
       <path d="M121,108 Q138,102 142,90 L130,84 Q121,94 121,108 Z" fill="#ff8c0018" stroke="#ff8c00" stroke-width=".8"/>
       <!-- Phase bands on body -->
       <line x1="82" y1="118" x2="118" y2="118" stroke="#ff8c00" stroke-width=".7" opacity=".4"/>
       <line x1="82" y1="128" x2="118" y2="128" stroke="#ff8c00" stroke-width=".7" opacity=".3"/>
       <line x1="84" y1="138" x2="116" y2="138" stroke="#ff8c00" stroke-width=".7" opacity=".2"/>
       <!-- Neck -->
       <rect x="93" y="76" width="14" height="10" rx="2" fill="#180a00" stroke="#ff8c00" stroke-width=".8"/>
       <!-- Head -->
       <rect x="78" y="38" width="44" height="40" rx="8" fill="#180a00" stroke="#ff8c00" stroke-width="1.2"/>
       <!-- Visor - narrow angular -->
       <path d="M82,52 L118,52 L116,62 L84,62 Z" fill="#ff8c00" opacity=".8"/>
       <path d="M84,54 L116,54 L114,60 L86,60 Z" fill="#180a00" opacity=".7"/>
       <!-- Eye glow through visor -->
       <ellipse cx="93" cy="57" rx="5" ry="2.5" fill="#ff8c00" opacity=".6"/>
       <ellipse cx="107" cy="57" rx="5" ry="2.5" fill="#ff8c00" opacity=".6"/>
       <!-- Phase flicker -->
       <ellipse cx="100" cy="57" rx="3" ry="2" fill="#fff" opacity=".3">
         <animate attributeName="opacity" values=".3;0;.3;0;.3" dur="2s" repeatCount="indefinite"/>
       </ellipse>
       <!-- Chin detail -->
       <path d="M86,68 L100,72 L114,68" stroke="#ff8c00" stroke-width=".8" fill="none" opacity=".5"/>
       <!-- Head top - phase blade -->
       <path d="M88,38 L88,26 L100,20 L112,26 L112,38" fill="#180a00" stroke="#ff8c00" stroke-width=".9"/>
       <line x1="100" y1="20" x2="100" y2="38" stroke="#ff8c00" stroke-width=".6" opacity=".6"/>
       <!-- Phase orb chest -->
       <circle cx="100" cy="108" r="8" fill="none" stroke="#ff8c00" stroke-width=".8" opacity=".7"/>
       <circle cx="100" cy="108" r="4" fill="#ff8c00" opacity=".4"/>
       <circle cx="100" cy="108" r="2" fill="#ff8c00" opacity=".8"/>
     </g>
     <!-- Phasing animation -->
     <rect x="0" y="0" width="200" height="160" fill="url(#gbg2)" opacity=".0">
       <animate attributeName="opacity" values="0;0.08;0;0.04;0" dur="4s" repeatCount="indefinite"/>
     </rect>
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#ff8c00" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#ff8c00" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
  {id:'rogue',name:'R0GUE',role:'Wild Algorithm',color:'var(--pink)',hex:'#ff2d78',
   stats:{agility:88,hacking:82,stealth:60,combat:95},
   desc:'A randomized AI that thrives on chaos. Volatile. Unpredictable. Every run is different.',
   abilityName:'RNG God',abilityDesc:'Snake food worth 3× randomly. Typing bonus words appear.',
   unlockCost:1500,
   applyBonus:()=>{},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="rbg" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#1a0010"/><stop offset="100%" stop-color="#0a0008"/></linearGradient>
       <filter id="rglow"><feGaussianBlur stdDeviation="2.5" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
     </defs>
     <rect width="200" height="160" fill="url(#rbg)"/>
     <!-- Chaos pattern bg -->
     <g stroke="#ff2d78" stroke-width=".4" opacity=".08" fill="none">
       <path d="M20,20 Q50,10 60,40 Q70,60 40,70"/><path d="M140,20 Q160,40 150,70 Q140,90 170,100"/>
       <path d="M10,100 Q30,120 60,110 Q90,100 80,140"/><path d="M160,90 Q180,120 160,140"/>
     </g>
     <g filter="url(#rglow)">
       <!-- Body - asymmetric battle-worn -->
       <path d="M80,155 L76,106 Q76,90 100,86 Q124,90 124,106 L120,155 Z" fill="#1a0010" stroke="#ff2d78" stroke-width="1"/>
       <!-- Left shoulder - spiked -->
       <path d="M76,106 Q58,98 52,84 L60,78 Q54,90 68,94 Q76,100 76,106 Z" fill="#ff2d7815" stroke="#ff2d78" stroke-width=".8"/>
       <path d="M52,84 L44,72 L58,80 Z" fill="#ff2d78" opacity=".5"/>
       <!-- Right shoulder - plated -->
       <path d="M124,106 Q140,98 144,88 Q148,80 138,76 L132,82 Q140,88 136,96 Q128,100 124,106 Z" fill="#ff2d7815" stroke="#ff2d78" stroke-width=".8"/>
       <!-- Neck -->
       <rect x="93" y="76" width="14" height="12" rx="2" fill="#1a0010" stroke="#ff2d78" stroke-width=".8"/>
       <!-- Head - asymmetric visor -->
       <path d="M76,38 L86,30 L114,30 L124,38 L126,72 L100,78 L74,72 Z" fill="#1a0010" stroke="#ff2d78" stroke-width="1.2"/>
       <!-- Cracked visor - left clean, right cracked -->
       <rect x="80" y="48" width="16" height="12" rx="3" fill="#ff2d78" opacity=".85"/>
       <path d="M104,48 L120,48 L120,60 L104,60 Z" fill="#ff2d78" opacity=".5"/>
       <!-- Crack lines on right visor -->
       <path d="M108,48 L106,54 L112,58" stroke="#fff" stroke-width=".8" opacity=".7" fill="none"/>
       <path d="M114,50 L112,56" stroke="#fff" stroke-width=".5" opacity=".5" fill="none"/>
       <!-- Eye behind left visor -->
       <circle cx="88" cy="54" r="4" fill="#fff" opacity=".9"/>
       <circle cx="88" cy="54" r="2" fill="#ff2d78"/>
       <!-- Glowing eye right (cracked visor) -->
       <ellipse cx="112" cy="54" rx="5" ry="3" fill="#ff2d78" opacity=".9"/>
       <ellipse cx="112" cy="54" rx="2" ry="1.5" fill="#fff" opacity=".8"/>
       <!-- Head spike left -->
       <path d="M76,50 L62,38 L78,48 Z" fill="#ff2d78" opacity=".6"/>
       <!-- Head top finned -->
       <path d="M88,30 L82,14 M100,30 L100,12 M112,30 L118,14" stroke="#ff2d78" stroke-width="1.2" opacity=".7"/>
       <!-- Chest - chaos sigil -->
       <path d="M100,100 L106,112 L118,112 L108,120 L112,132 L100,124 L88,132 L92,120 L82,112 L94,112 Z" fill="#ff2d78" opacity=".2" stroke="#ff2d78" stroke-width=".7"/>
     </g>
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#ff2d78" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#ff2d78" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
  {id:'titan',name:'T1TAN',role:'Siege Engine',color:'var(--gold)',hex:'#ffd700',
   stats:{agility:30,hacking:70,stealth:20,combat:99},
   desc:'Built for one purpose: total annihilation. The oldest combat AI ever deployed. Slow. Unstoppable.',
   abilityName:'Overdrive',abilityDesc:'All games: high scores grant +50% XP bonus.',
   unlockCost:2500,
   applyBonus:()=>{},
   portrait:`<svg viewBox="0 0 200 160" xmlns="http://www.w3.org/2000/svg">
     <defs>
       <linearGradient id="tbg" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#1a1200"/><stop offset="100%" stop-color="#0a0800"/></linearGradient>
       <filter id="tglow"><feGaussianBlur stdDeviation="2" result="blur"/><feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
       <linearGradient id="tarmor" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#2a1e00"/><stop offset="100%" stop-color="#1a1000"/></linearGradient>
     </defs>
     <rect width="200" height="160" fill="url(#tbg)"/>
     <!-- Scale armor bg -->
     <g opacity=".06" stroke="#ffd700" stroke-width=".5" fill="none">
       <path d="M0,40 Q10,30 20,40 Q30,30 40,40 Q50,30 60,40 Q70,30 80,40 Q90,30 100,40 Q110,30 120,40 Q130,30 140,40 Q150,30 160,40 Q170,30 180,40 Q190,30 200,40"/>
       <path d="M0,60 Q10,50 20,60 Q30,50 40,60 Q50,50 60,60 Q70,50 80,60 Q90,50 100,60 Q110,50 120,60 Q130,50 140,60 Q150,50 160,60 Q170,50 180,60 Q190,50 200,60"/>
       <path d="M0,80 Q10,70 20,80 Q30,70 40,80 Q50,70 60,80 Q70,70 80,80 Q90,70 100,80 Q110,70 120,80 Q130,70 140,80 Q150,70 160,80 Q170,70 180,80 Q190,70 200,80"/>
     </g>
     <g filter="url(#tglow)">
       <!-- Massive body -->
       <path d="M68,155 L62,100 Q62,80 100,74 Q138,80 138,100 L132,155 Z" fill="url(#tarmor)" stroke="#ffd700" stroke-width="1.5"/>
       <!-- Shoulder pauldrons - massive -->
       <path d="M62,100 Q38,90 32,72 L48,64 Q52,80 68,88 Q62,94 62,100 Z" fill="url(#tarmor)" stroke="#ffd700" stroke-width="1.2"/>
       <path d="M138,100 Q162,90 168,72 L152,64 Q148,80 132,88 Q138,94 138,100 Z" fill="url(#tarmor)" stroke="#ffd700" stroke-width="1.2"/>
       <!-- Pauldron studs -->
       <circle cx="40" cy="82" r="2.5" fill="#ffd700" opacity=".8"/>
       <circle cx="36" cy="72" r="2" fill="#ffd700" opacity=".6"/>
       <circle cx="160" cy="82" r="2.5" fill="#ffd700" opacity=".8"/>
       <circle cx="164" cy="72" r="2" fill="#ffd700" opacity=".6"/>
       <!-- Chest armor plates -->
       <rect x="76" y="86" width="48" height="36" rx="4" fill="none" stroke="#ffd700" stroke-width="1" opacity=".5"/>
       <path d="M76,104 L124,104" stroke="#ffd700" stroke-width=".7" opacity=".4"/>
       <!-- Power core chest -->
       <rect x="90" y="90" width="20" height="16" rx="3" fill="#ffd70018" stroke="#ffd700" stroke-width="1"/>
       <rect x="93" y="93" width="14" height="10" rx="2" fill="#ffd700" opacity=".3"/>
       <rect x="96" y="96" width="8" height="4" rx="1" fill="#ffd700" opacity=".8"/>
       <!-- Gold rivets on chest -->
       <circle cx="80" cy="90" r="2" fill="#ffd700" opacity=".7"/>
       <circle cx="120" cy="90" r="2" fill="#ffd700" opacity=".7"/>
       <circle cx="80" cy="118" r="2" fill="#ffd700" opacity=".7"/>
       <circle cx="120" cy="118" r="2" fill="#ffd700" opacity=".7"/>
       <!-- Neck - thick column -->
       <rect x="88" y="64" width="24" height="12" rx="3" fill="url(#tarmor)" stroke="#ffd700" stroke-width="1"/>
       <!-- Head - brutalist wide -->
       <rect x="68" y="26" width="64" height="40" rx="5" fill="url(#tarmor)" stroke="#ffd700" stroke-width="1.5"/>
       <!-- Brow ridge -->
       <rect x="68" y="26" width="64" height="10" rx="5" fill="#ffd70022" stroke="#ffd700" stroke-width=".8"/>
       <!-- Eye slots - narrow brutal -->
       <rect x="76" y="40" width="20" height="8" rx="1" fill="#ffd700" opacity=".15" stroke="#ffd700" stroke-width="1"/>
       <rect x="104" y="40" width="20" height="8" rx="1" fill="#ffd700" opacity=".15" stroke="#ffd700" stroke-width="1"/>
       <!-- Eye glow inner -->
       <rect x="78" y="42" width="16" height="4" rx=".5" fill="#ffd700" opacity=".9"/>
       <rect x="106" y="42" width="16" height="4" rx=".5" fill="#ffd700" opacity=".9"/>
       <!-- Nose grille -->
       <rect x="96" y="52" width="8" height="6" rx="1" fill="none" stroke="#ffd700" stroke-width=".6" opacity=".5"/>
       <line x1="97" y1="52" x2="97" y2="58" stroke="#ffd700" stroke-width=".4" opacity=".4"/>
       <line x1="100" y1="52" x2="100" y2="58" stroke="#ffd700" stroke-width=".4" opacity=".4"/>
       <line x1="103" y1="52" x2="103" y2="58" stroke="#ffd700" stroke-width=".4" opacity=".4"/>
       <!-- Chin plate -->
       <rect x="80" y="58" width="40" height="8" rx="2" fill="none" stroke="#ffd700" stroke-width=".7" opacity=".4"/>
       <!-- Crown horns -->
       <path d="M78,26 L70,8 L82,22" fill="#ffd700" opacity=".7" stroke="#ffd700" stroke-width=".5"/>
       <path d="M122,26 L130,8 L118,22" fill="#ffd700" opacity=".7" stroke="#ffd700" stroke-width=".5"/>
       <!-- Center crest -->
       <path d="M96,26 L100,14 L104,26" fill="#ffd700" opacity=".9"/>
     </g>
     <!-- Power reading bars -->
     <rect x="4" y="130" width="3" height="20" rx="1" fill="#ffd700" opacity=".8"/>
     <rect x="9" y="125" width="3" height="25" rx="1" fill="#ffd700" opacity=".8"/>
     <rect x="14" y="118" width="3" height="32" rx="1" fill="#ffd700" opacity=".9"/>
     <rect x="183" y="130" width="3" height="20" rx="1" fill="#ffd700" opacity=".8"/>
     <rect x="188" y="125" width="3" height="25" rx="1" fill="#ffd700" opacity=".8"/>
     <rect x="193" y="118" width="3" height="32" rx="1" fill="#ffd700" opacity=".9"/>
     <path d="M4,4 L4,14 M4,4 L14,4" stroke="#ffd700" stroke-width="1.5" fill="none" opacity=".6"/>
     <path d="M196,4 L196,14 M196,4 L186,4" stroke="#ffd700" stroke-width="1.5" fill="none" opacity=".6"/>
   </svg>`},
];

const GAMES=[
  {id:'dodge',name:'DODGE',sub:'Survive',icon:'⬡',color:'rgba(0,245,255,.3)',cb:'var(--cyan)',unlockCost:0},
  {id:'memory',name:'MEMORY',sub:'Match pairs',icon:'◈',color:'rgba(180,79,255,.3)',cb:'var(--purple)',unlockCost:0},
  {id:'simon',name:'SEQUENCE',sub:'Pattern recall',icon:'◆',color:'rgba(57,255,20,.3)',cb:'var(--green)',unlockCost:0},
  {id:'snake',name:'SNAKE',sub:'Eat & grow',icon:'⟁',color:'rgba(255,140,0,.3)',cb:'var(--orange)',unlockCost:400},
  {id:'typing',name:'TYPING',sub:'Speed words',icon:'▶',color:'rgba(0,245,255,.3)',cb:'var(--cyan)',unlockCost:700},
  {id:'breakout',name:'BREAKOUT',sub:'Break bricks',icon:'▣',color:'rgba(255,45,120,.3)',cb:'var(--pink)',unlockCost:1200},
];

// Shop items
const SHOP_THEMES=[
  {id:'default',name:'VOID',desc:'Default dark void theme',cost:0,preview:'#04060f'},
  {id:'crimson',name:'CRIMSON GRID',desc:'Blood red combat grid',cost:350,preview:'#0f0408'},
  {id:'matrix',name:'MATRIX',desc:'Green digital rain haze',cost:600,preview:'#000d04'},
  {id:'nebula',name:'DEEP SPACE',desc:'Purple nebula ambiance',cost:900,preview:'#08040f'},
  {id:'toxic',name:'TOXIC WASTE',desc:'Radioactive green tones',cost:1200,preview:'#040f04'},
];
const SHOP_BACKGROUNDS=[
  {id:'particles',name:'PARTICLES',desc:'Default glowing nodes & lines',cost:0},
  {id:'grid',name:'GRID + PARTICLES',desc:'Animated grid with particles',cost:0},
  {id:'hexgrid',name:'HEX FIELD',desc:'Purple hexagonal mesh',cost:500},
  {id:'rain',name:'DIGITAL RAIN',desc:'Cyan falling matrix rain',cost:700},
  {id:'nebula',name:'NEBULA PULSE',desc:'Deep space color wash',cost:1100},
];
const SHOP_ACCENTS=[
  {id:'cyan-purple',name:'NEXUS',desc:'Default cyan & purple',cost:0,c1:'#00f5ff',c2:'#b44fff'},
  {id:'green-orange',name:'TOXIC FIRE',desc:'Green & orange heat',cost:500,c1:'#39ff14',c2:'#ff8c00'},
  {id:'pink-purple',name:'NEON ROSE',desc:'Pink & violet spectrum',cost:500,c1:'#ff2d78',c2:'#b44fff'},
  {id:'gold-cyan',name:'GOLDEN AGE',desc:'Gold & cyan prestige',cost:1000,c1:'#ffd700',c2:'#00f5ff'},
  {id:'white-red',name:'GHOST FIRE',desc:'White & crimson monochrome',cost:1800,c1:'#ffffff',c2:'#ff2020'},
];
const SHOP_HUDS=[
  {id:'minimal',name:'MINIMAL',desc:'Clean, no clutter',cost:0},
  {id:'retro',name:'RETRO CRT',desc:'Scanline effects on all text',cost:600},
  {id:'hologram',name:'HOLOGRAM',desc:'Flickering holographic panels',cost:1200},
];

// ═══════════════════════════════════════════════════
//  RENDER CHARACTERS
// ═══════════════════════════════════════════════════
function renderChars(){
  const g=document.getElementById('chars-grid');g.innerHTML='';
  CHARS.forEach(ch=>{
    const owned=G.owned.chars.includes(ch.id);
    const sel=G.activeChar===ch.id;
    const card=document.createElement('div');
    card.className='char-card'+(sel?' selected':'')+(owned?'':' locked');
    card.style.setProperty('--card-bg','#080c1a');
    card.style.borderColor=sel?ch.hex:owned?ch.hex+'33':'';
    // Build stat bars HTML
    const statBars=Object.entries(ch.stats).map(([k,v])=>`
      <div class="stat-row">
        <div class="stat-label">${k.toUpperCase()}</div>
        <div class="stat-bar"><div class="stat-fill" style="width:${v}%;background:${ch.hex};box-shadow:0 0 6px ${ch.hex}88"></div></div>
      </div>`).join('');
    card.innerHTML=`
      <div class="char-glow" style="background:conic-gradient(from 0deg,transparent,${ch.hex}09,transparent,${ch.hex}06,transparent)"></div>
      ${sel?'<div class="sel-badge">ACTIVE</div>':''}
      <div class="char-lock-badge">LOCKED</div>
      <div class="char-portrait">${ch.portrait}</div>
      <div class="char-header">
        <div>
          <div class="char-name" style="color:${ch.color};text-shadow:0 0 12px ${ch.hex}66">${ch.name}</div>
          <div class="char-role" style="color:${ch.color}">${ch.role}</div>
        </div>
        <div class="char-id-tag">#${ch.id.toUpperCase()}</div>
      </div>
      <div class="char-stats">${statBars}</div>
      <div class="char-desc">${ch.desc}</div>
      <div class="char-ability" style="border-color:${ch.hex};background:${ch.hex}0a">
        <div class="ability-label" style="color:${ch.color}">PASSIVE // ${ch.abilityName}</div>
        <div class="ability-desc-text">${ch.abilityDesc}</div>
      </div>
      ${!owned?`<button class="btn btn-gold btn-sm" style="margin-top:14px;width:100%" onclick="buyChar('${ch.id}',event)"><span>UNLOCK ⬡ ${ch.unlockCost} XP</span></button>`:''}`;
    if(owned)card.onclick=()=>selectChar(ch.id);
    g.appendChild(card);
  });
}

function buyChar(id,e){
  e.stopPropagation();
  const ch=CHARS.find(c=>c.id===id);
  if(!ch||G.totalXP<ch.unlockCost){notify('Not enough XP! Need ⬡'+ch.unlockCost,'pink');return}
  G.totalXP-=ch.unlockCost;G.owned.chars.push(id);
  saveGame();updateHdrXP();renderChars();
  notify('Agent '+ch.name+' unlocked!','gold');
  addFeed('UNLOCK','Agent '+ch.name+' acquired.');
}

function selectChar(id){
  const ch=CHARS.find(c=>c.id===id);
  G.activeChar=id;
  G.bonus={extraLife:false,memPeek:false,simonFast:false,dodgeMult:false,memLong:false,snakeSlow:false,typingTime:false,breakoutBig:false};
  ch.applyBonus();
  renderChars();updateSidebar();
  document.getElementById('hdr-char').textContent=ch.name;
  addFeed('AGENT','Agent '+ch.name+' deployed. Passive: '+ch.abilityName);
  notify('Agent '+ch.name+' active!','cyan');
  saveGame();
}

// ═══════════════════════════════════════════════════
//  RENDER GAME BUTTONS
// ═══════════════════════════════════════════════════
function renderGameBtns(){
  const g=document.getElementById('game-btns');g.innerHTML='';
  GAMES.forEach(gm=>{
    const owned=G.owned.games.includes(gm.id);
    const btn=document.createElement('div');
    btn.className='game-btn'+(owned?'':' locked-game');
    btn.id='gbtn-'+gm.id;
    btn.style.setProperty('--c',gm.color);btn.style.setProperty('--cb',gm.cb);
    btn.innerHTML=`<div class="gi">${gm.icon}</div><div class="gname">${gm.name}</div><div class="gsub">${owned?gm.sub:'⬡'+gm.unlockCost}</div>`;
    if(owned)btn.onclick=()=>selectGame(gm.id);
    g.appendChild(btn);
  });
}

// ═══════════════════════════════════════════════════
//  NAVIGATION
// ═══════════════════════════════════════════════════
function showTab(tab){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('screen-'+tab).classList.add('active');
  document.querySelector('[data-tab="'+tab+'"]').classList.add('active');
  if(tab==='hub'){renderGameBtns();updateSidebar()}
  if(tab==='shop'){renderShop()}
  if(tab==='customize'){showCTab('themes')}
}

function selectGame(id){
  GAMES.forEach(g=>{
    document.getElementById('gw-'+g.id).style.display='none';
    document.getElementById('gbtn-'+g.id).classList.remove('gactive');
  });
  document.getElementById('gw-default').style.display='none';
  document.getElementById('gw-'+id).style.display='block';
  document.getElementById('gbtn-'+id).classList.add('gactive');
  if(id==='memory')memInit();
  if(id==='simon')simonInit();
  if(id==='snake')snakeInit();
  if(id==='typing')typingInit();
  if(id==='breakout')breakoutInit();
  addFeed('MISSION','Loaded: '+id.toUpperCase());
}

// ═══════════════════════════════════════════════════
//  XP + LEVEL SYSTEM
// ═══════════════════════════════════════════════════
function addXP(v){
  // Titan bonus
  const titan=G.activeChar==='titan';
  if(titan)v=Math.round(v*1.5);
  G.totalXP+=v;
  // Level up?
  while(G.totalXP>=G.xpToNext){
    G.totalXP-=G.xpToNext;G.level++;G.xpToNext=Math.round(G.xpToNext*1.7);
    notify('LEVEL UP! Now Level '+G.level,'gold');
    addFeed('RANK','Level '+G.level+' reached!');
  }
  updateHdrXP();saveGame();
}

function updateHdrXP(){
  document.getElementById('hdr-total-xp').textContent=G.totalXP;
  document.getElementById('sb-total-xp')&&(document.getElementById('sb-total-xp').textContent=G.totalXP);
  document.getElementById('hdr-level').textContent='Lv '+G.level;
  document.getElementById('shop-xp')&&(document.getElementById('shop-xp').textContent=G.totalXP);
  const pct=Math.round(G.totalXP/G.xpToNext*100);
  document.getElementById('xp-fill').style.width=pct+'%';
  document.getElementById('hdr-xp-lbl').textContent=G.totalXP+'/'+G.xpToNext;
}

// ═══════════════════════════════════════════════════
//  SIDEBAR
// ═══════════════════════════════════════════════════
function updateSidebar(){
  const ch=CHARS.find(c=>c.id===G.activeChar);
  document.getElementById('ac-name').textContent=ch?ch.name:'No Agent';
  document.getElementById('ac-role').textContent=ch?ch.role:'Select from Agents';
  const av=document.getElementById('ac-avatar');
  if(ch){av.innerHTML=`<svg viewBox="0 0 44 44" style="width:44px;height:44px;border-radius:50%">${ch.portrait.replace('viewBox="0 0 200 160"','viewBox="20 28 160 80"')}</svg>`;av.style.background='transparent';av.style.border='1px solid '+ch.hex+'44';}
  const ab=document.getElementById('ac-ability');
  if(ch){ab.style.display='block';ab.innerHTML=`<span style="color:${ch.color}">${ch.abilityName}</span>: ${ch.abilityDesc}`;}
  else ab.style.display='none';
  const hs=G.highScores;
  document.getElementById('hs-dodge').textContent=hs.dodge;
  document.getElementById('hs-memory').textContent=hs.memory?hs.memory+'s':'—';
  document.getElementById('hs-simon').textContent=hs.simon;
  document.getElementById('hs-snake').textContent=hs.snake;
  document.getElementById('hs-typing').textContent=hs.typing?hs.typing+' WPM':'0';
  document.getElementById('hs-breakout').textContent=hs.breakout;
  document.getElementById('sb-total-xp')&&(document.getElementById('sb-total-xp').textContent=G.totalXP);
}

// ═══════════════════════════════════════════════════
//  FEED + NOTIFY
// ═══════════════════════════════════════════════════
function addFeed(tag,msg){
  const f=document.getElementById('feed-list');if(!f)return;
  const t=new Date();const ts=t.getHours().toString().padStart(2,'0')+':'+t.getMinutes().toString().padStart(2,'0');
  const el=document.createElement('div');el.className='feed-item';
  el.innerHTML=`<span class="ftime">${tag}</span><span>${msg}</span>`;
  f.insertBefore(el,f.firstChild);
  if(f.children.length>25)f.removeChild(f.lastChild);
}
let notifTimer;
function notify(msg,type='cyan'){
  const n=document.getElementById('notif');
  n.textContent=msg;n.className='notif show '+type;
  clearTimeout(notifTimer);notifTimer=setTimeout(()=>n.className='notif',2600);
}

// ═══════════════════════════════════════════════════
//  GAME 1: SPACE SHOOTER (was Dodge)
// ═══════════════════════════════════════════════════
const DS={
  running:false,lives:3,score:0,level:1,wave:1,
  ship:{x:0,y:0,w:28,h:36,invincible:0,engine:0},
  bullets:[],enemies:[],explosions:[],stars:[],
  keys:{},fid:null,tick:0,spawnTick:0,lastShot:0,waveKills:0,waveTarget:8,
  powerups:[],shield:0,multishot:false,multishotTimer:0
};
document.addEventListener('keydown',e=>{DS.keys[e.key]=true;if(e.key===' ')e.preventDefault()});
document.addEventListener('keyup',e=>{DS.keys[e.key]=false});

function dodgeStart(){
  const cv=document.getElementById('dodge-canvas');
  cv.width=cv.offsetWidth||680;cv.height=380;
  const W=cv.width,H=cv.height;
  const lives=G.bonus.extraLife?4:3;
  Object.assign(DS,{running:true,score:0,level:1,wave:1,bullets:[],enemies:[],explosions:[],powerups:[],
    tick:0,spawnTick:0,lastShot:0,waveKills:0,waveTarget:10,shield:0,multishot:false,multishotTimer:0,lives});
  DS.ship.x=W/2;DS.ship.y=H-70;DS.ship.invincible=0;DS.ship.engine=0;
  // Init starfield
  DS.stars=[];for(let i=0;i<80;i++)DS.stars.push({x:Math.random()*W,y:Math.random()*H,spd:Math.random()*2+.5,r:Math.random()*1.5+.3,br:Math.random()});
  const el=document.getElementById('d-lives');el.innerHTML='';
  for(let i=0;i<lives;i++){const d=document.createElement('div');d.className='life';el.appendChild(d)}
  document.getElementById('d-score').textContent=0;document.getElementById('d-level').textContent='W1';
  cv.parentElement.querySelector('.g-overlay')?.remove();
  if(DS.fid)cancelAnimationFrame(DS.fid);
  dodgeLoop();
}

function drawShip(ctx,x,y,w,h,inv){
  const flicker=inv>0&&Math.floor(inv/4)%2===0;
  if(flicker)return;
  ctx.save();
  ctx.shadowColor='#00f5ff';ctx.shadowBlur=20;
  // Main body
  ctx.fillStyle='#001a22';ctx.strokeStyle='#00f5ff';ctx.lineWidth=1.5;
  ctx.beginPath();ctx.moveTo(x,y-h/2);ctx.lineTo(x+w/2,y+h/2);ctx.lineTo(x+w/3,y+h/3);ctx.lineTo(x-w/3,y+h/3);ctx.lineTo(x-w/2,y+h/2);ctx.closePath();ctx.fill();ctx.stroke();
  // Cockpit
  ctx.fillStyle='#00f5ff44';ctx.beginPath();ctx.moveTo(x,y-h/2+4);ctx.lineTo(x+w/5,y);ctx.lineTo(x-w/5,y);ctx.closePath();ctx.fill();
  ctx.fillStyle='#00f5ff88';ctx.strokeStyle='#00f5ff';ctx.lineWidth=.8;
  ctx.beginPath();ctx.moveTo(x,y-h/2+6);ctx.lineTo(x+w/6,y-2);ctx.lineTo(x-w/6,y-2);ctx.closePath();ctx.fill();ctx.stroke();
  // Wings
  ctx.fillStyle='#002a30';ctx.strokeStyle='#00f5ff88';ctx.lineWidth=1;
  ctx.beginPath();ctx.moveTo(x+w/3,y+h/3);ctx.lineTo(x+w/2,y+h/2);ctx.lineTo(x+w/2+6,y+h/4);ctx.lineTo(x+w/3,y);ctx.closePath();ctx.fill();ctx.stroke();
  ctx.beginPath();ctx.moveTo(x-w/3,y+h/3);ctx.lineTo(x-w/2,y+h/2);ctx.lineTo(x-w/2-6,y+h/4);ctx.lineTo(x-w/3,y);ctx.closePath();ctx.fill();ctx.stroke();
  // Engine glow
  const eg=0.5+0.5*Math.sin(DS.tick*0.3);
  ctx.shadowColor='#ff8c00';ctx.shadowBlur=14*eg;
  ctx.fillStyle=`rgba(255,140,0,${0.6+eg*0.4})`;
  ctx.beginPath();ctx.ellipse(x-w/4,y+h/2,3,5+eg*3,0,0,Math.PI*2);ctx.fill();
  ctx.beginPath();ctx.ellipse(x+w/4,y+h/2,3,5+eg*3,0,0,Math.PI*2);ctx.fill();
  // Shield bubble
  if(DS.shield>0){
    ctx.shadowColor='#00f5ff';ctx.shadowBlur=20;
    ctx.strokeStyle=`rgba(0,245,255,${Math.min(1,DS.shield/60)*0.7})`;ctx.lineWidth=2;
    ctx.beginPath();ctx.arc(x,y,26,0,Math.PI*2);ctx.stroke();
  }
  ctx.restore();
}

function spawnEnemy(W,lvl){
  const types=['fighter','bomber','zigzag','shooter'];
  const t=types[Math.floor(Math.random()*Math.min(2+Math.floor(lvl/2),4))];
  const e={x:Math.random()*(W-60)+30,y:-30,type:t,hp:t==='bomber'?3:t==='shooter'?2:1,
    vx:(Math.random()-.5)*3,vy:2.5+Math.random()*2+lvl*0.35,
    tick:0,shootTick:0,w:22,h:22,col:t==='bomber'?'#ff2d78':t==='shooter'?'#b44fff':t==='zigzag'?'#ff8c00':'#39ff14'};
  if(t==='bomber'){e.w=30;e.h=30}
  return e;
}

function drawEnemy(ctx,e){
  ctx.save();ctx.shadowColor=e.col;ctx.shadowBlur=14;
  const s=e.type==='bomber'?1.4:1;
  ctx.strokeStyle=e.col;ctx.lineWidth=1.5;
  if(e.type==='fighter'||e.type==='shooter'){
    ctx.fillStyle=e.col+'22';
    ctx.beginPath();ctx.moveTo(e.x,e.y+e.h/2);ctx.lineTo(e.x+e.w/2,e.y-e.h/2);ctx.lineTo(e.x-e.w/2,e.y-e.h/2);ctx.closePath();ctx.fill();ctx.stroke();
    ctx.fillStyle=e.col+'66';ctx.beginPath();ctx.arc(e.x,e.y,e.w/4,0,Math.PI*2);ctx.fill();
  } else if(e.type==='bomber'){
    ctx.fillStyle='#ff2d7822';
    ctx.beginPath();ctx.arc(e.x,e.y,e.w/2,0,Math.PI*2);ctx.fill();ctx.stroke();
    for(let i=0;i<6;i++){const a=i*Math.PI/3+e.tick*0.05;ctx.beginPath();ctx.moveTo(e.x+Math.cos(a)*8,e.y+Math.sin(a)*8);ctx.lineTo(e.x+Math.cos(a)*14,e.y+Math.sin(a)*14);ctx.stroke()}
  } else if(e.type==='zigzag'){
    ctx.fillStyle='#ff8c0022';
    ctx.beginPath();ctx.moveTo(e.x,e.y-e.h/2);ctx.lineTo(e.x+e.w/2,e.y);ctx.lineTo(e.x,e.y+e.h/2);ctx.lineTo(e.x-e.w/2,e.y);ctx.closePath();ctx.fill();ctx.stroke();
  }
  // HP dots
  for(let i=0;i<e.hp;i++){ctx.fillStyle=e.col;ctx.beginPath();ctx.arc(e.x-6+i*6,e.y-e.h/2-6,2,0,Math.PI*2);ctx.fill()}
  ctx.restore();
}

function dodgeLoop(){
  if(!DS.running)return;
  const cv=document.getElementById('dodge-canvas');const ctx=cv.getContext('2d');
  const W=cv.width,H=cv.height;const s=DS.ship;
  DS.tick++;DS.ship.engine++;
  if(s.invincible>0)s.invincible--;
  if(DS.multishot&&--DS.multishotTimer<=0)DS.multishot=false;
  if(DS.shield>0)DS.shield--;

  // Move ship
  const spd=5.5;const k=DS.keys;
  if(k['ArrowLeft']||k['a']||k['A'])s.x-=spd;
  if(k['ArrowRight']||k['d']||k['D'])s.x+=spd;
  if(k['ArrowUp']||k['w']||k['W'])s.y-=spd;
  if(k['ArrowDown']||k['s']||k['S'])s.y+=spd;
  s.x=Math.max(s.w/2,Math.min(W-s.w/2,s.x));
  s.y=Math.max(s.h/2,Math.min(H-s.h/2,s.y));

  // Shoot
  const canShoot=(k[' ']||k['ArrowUp']||k['w']||k['W'])&&DS.tick-DS.lastShot>12;
  if(canShoot){
    DS.lastShot=DS.tick;
    const mult=G.bonus.dodgeMult&&G.activeChar==='nova';
    if(DS.multishot||mult){
      DS.bullets.push({x:s.x-10,y:s.y-s.h/2,vy:-11,vx:-1.5,col:'#00f5ff'});
      DS.bullets.push({x:s.x,y:s.y-s.h/2,vy:-12,vx:0,col:'#00f5ff'});
      DS.bullets.push({x:s.x+10,y:s.y-s.h/2,vy:-11,vx:1.5,col:'#00f5ff'});
    } else {
      DS.bullets.push({x:s.x,y:s.y-s.h/2,vy:-12,vx:0,col:'#00f5ff'});
    }
  }

  // Move bullets
  DS.bullets.forEach(b=>{b.x+=b.vx;b.y+=b.vy});
  DS.bullets=DS.bullets.filter(b=>b.y>-10&&b.x>-10&&b.x<W+10);

  // Spawn enemies
  const spawnInt=Math.max(18,80-DS.wave*7);
  if(++DS.spawnTick>=spawnInt&&DS.waveKills<DS.waveTarget){DS.spawnTick=0;DS.enemies.push(spawnEnemy(W,DS.level))}

  // Move enemies + their bullets
  for(let i=DS.enemies.length-1;i>=0;i--){
    const e=DS.enemies[i];e.tick++;
    if(e.type==='zigzag')e.vx=2*Math.sin(e.tick*0.08);
    e.x+=e.vx;e.y+=e.vy;
    if(e.x<10||e.x>W-10)e.vx*=-1;
    // Shooter fires back
    if(e.type==='shooter'&&++e.shootTick>55){e.shootTick=0;DS.bullets.push({x:e.x,y:e.y+e.h/2,vy:5.5,vx:(s.x-e.x)/60,col:'#ff2d78',enemy:true})}
    if(e.y>H+40){DS.enemies.splice(i,1);continue}
    // Bullet-enemy collision
    for(let j=DS.bullets.length-1;j>=0;j--){
      const b=DS.bullets[j];if(b.enemy)continue;
      const dx=b.x-e.x,dy=b.y-e.y;
      if(Math.abs(dx)<e.w/2+4&&Math.abs(dy)<e.h/2+4){
        DS.bullets.splice(j,1);e.hp--;
        if(e.hp<=0){
          DS.enemies.splice(i,1);DS.score+=e.type==='bomber'?30:e.type==='shooter'?20:10;
          DS.waveKills++;
          document.getElementById('d-score').textContent=DS.score;
          // Powerup drop
          if(Math.random()<.15)DS.powerups.push({x:e.x,y:e.y,type:Math.random()<.5?'shield':'multi',vy:1.5});
          DS.explosions.push({x:e.x,y:e.y,r:0,maxR:e.w*1.5,col:e.col,life:25});
        }
        break;
      }
    }
  }

  // Powerups
  DS.powerups.forEach(p=>p.y+=p.vy);
  DS.powerups=DS.powerups.filter(p=>{
    const dx=p.x-s.x,dy=p.y-s.y;
    if(Math.abs(dx)<20&&Math.abs(dy)<20){
      if(p.type==='shield')DS.shield=180;
      if(p.type==='multi'){DS.multishot=true;DS.multishotTimer=300}
      notify(p.type==='shield'?'SHIELD UP!':'MULTI-SHOT!','cyan');
      return false;
    }
    return p.y<H+20;
  });

  // Wave complete
  if(DS.waveKills>=DS.waveTarget&&DS.enemies.length===0){
    DS.wave++;DS.level=DS.wave;DS.waveKills=0;DS.waveTarget=10+DS.wave*3;
    document.getElementById('d-level').textContent='W'+DS.wave;
    DS.score+=DS.wave*20;document.getElementById('d-score').textContent=DS.score;
    DS.explosions.push({x:W/2,y:H/2,r:0,maxR:120,col:'#00f5ff',life:40});
  }

  // Player hit
  if(s.invincible===0&&DS.shield===0){
    const hitEnemy=DS.enemies.some(e=>{const dx=e.x-s.x,dy=e.y-s.y;return Math.sqrt(dx*dx+dy*dy)<e.w/2+s.w/2-8});
    const hitBullet=DS.bullets.some(b=>b.enemy&&Math.abs(b.x-s.x)<16&&Math.abs(b.y-s.y)<18);
    if(hitEnemy||hitBullet){
      DS.lives--;s.invincible=90;
      DS.explosions.push({x:s.x,y:s.y,r:0,maxR:40,col:'#ff2d78',life:20});
      const ld=document.querySelectorAll('#d-lives .life');
      if(ld[DS.lives])ld[DS.lives].classList.add('lost');
      if(DS.lives<=0){dodgeEnd(DS.score);return}
    }
  }

  // Explosions
  DS.explosions.forEach(ex=>{ex.r+=ex.maxR/ex.life;ex.life--});
  DS.explosions=DS.explosions.filter(ex=>ex.life>0);

  // ── DRAW ──
  ctx.fillStyle='#02040c';ctx.fillRect(0,0,W,H);

  // Stars
  DS.stars.forEach(st=>{
    st.y+=st.spd;if(st.y>H)st.y=0;
    ctx.fillStyle=`rgba(255,255,255,${0.2+st.br*0.4})`;
    ctx.beginPath();ctx.arc(st.x,st.y,st.r,0,Math.PI*2);ctx.fill();
  });

  // Nebula swipe
  const ng=ctx.createLinearGradient(0,0,W,H);
  ng.addColorStop(0,'rgba(0,20,40,0)');ng.addColorStop(.5,'rgba(20,0,40,0.06)');ng.addColorStop(1,'rgba(0,20,40,0)');
  ctx.fillStyle=ng;ctx.fillRect(0,0,W,H);

  // Explosions
  DS.explosions.forEach(ex=>{
    ctx.save();ctx.globalAlpha=ex.life/25;
    ctx.shadowColor=ex.col;ctx.shadowBlur=20;
    ctx.strokeStyle=ex.col;ctx.lineWidth=2;
    ctx.beginPath();ctx.arc(ex.x,ex.y,ex.r,0,Math.PI*2);ctx.stroke();
    ctx.globalAlpha=(ex.life/25)*0.3;ctx.fillStyle=ex.col;
    ctx.beginPath();ctx.arc(ex.x,ex.y,ex.r*.6,0,Math.PI*2);ctx.fill();
    ctx.restore();
  });

  // Powerups
  DS.powerups.forEach(p=>{
    ctx.save();ctx.shadowColor=p.type==='shield'?'#00f5ff':'#b44fff';ctx.shadowBlur=16;
    ctx.strokeStyle=p.type==='shield'?'#00f5ff':'#b44fff';ctx.lineWidth=1.5;
    ctx.fillStyle=(p.type==='shield'?'#00f5ff':'#b44fff')+'22';
    ctx.beginPath();ctx.arc(p.x,p.y,10,0,Math.PI*2);ctx.fill();ctx.stroke();
    ctx.fillStyle=p.type==='shield'?'#00f5ff':'#b44fff';
    ctx.font='bold 10px Orbitron';ctx.textAlign='center';ctx.textBaseline='middle';
    ctx.fillText(p.type==='shield'?'S':'M',p.x,p.y);
    ctx.restore();
  });

  // Enemies
  DS.enemies.forEach(e=>drawEnemy(ctx,e));

  // Player bullets
  DS.bullets.forEach(b=>{
    ctx.save();ctx.shadowColor=b.col;ctx.shadowBlur=12;
    ctx.fillStyle=b.col;
    if(b.enemy){ctx.beginPath();ctx.arc(b.x,b.y,3,0,Math.PI*2);ctx.fill();}
    else{ctx.fillRect(b.x-1.5,b.y-8,3,14);}
    ctx.restore();
  });

  // Ship
  drawShip(ctx,s.x,s.y,s.w,s.h,s.invincible);

  // HUD wave banner
  ctx.save();ctx.font='bold 11px Orbitron';ctx.textAlign='right';ctx.fillStyle='rgba(0,245,255,.5)';
  ctx.fillText('WAVE '+DS.wave+' | KILLS '+DS.waveKills+'/'+DS.waveTarget,W-12,18);
  if(DS.shield>0){ctx.textAlign='left';ctx.fillStyle='#00f5ff';ctx.fillText('SHIELD: '+Math.ceil(DS.shield/60)+'s',12,18)}
  if(DS.multishot){ctx.textAlign='left';ctx.fillStyle='#b44fff';ctx.fillText('MULTI',12,DS.shield>0?34:18)}
  ctx.restore();

  DS.fid=requestAnimationFrame(dodgeLoop);
}

function dodgeEnd(sc){
  DS.running=false;cancelAnimationFrame(DS.fid);
  if(sc>G.highScores.dodge){G.highScores.dodge=sc;document.getElementById('hs-dodge').textContent=sc;addFeed('RECORD','SHOOTER high score: '+sc)}
  const xp=Math.floor(sc/4);addXP(xp);updateSidebar();addFeed('SHOOTER','Ship destroyed. Score: '+sc+' +'+xp+' XP');saveGame();
  const cv=document.getElementById('dodge-canvas');
  const ov=document.createElement('div');ov.className='g-overlay';
  ov.innerHTML=`<h2 style="color:var(--cyan)">SHIP DESTROYED</h2><div class="big-score">${sc}</div><p style="color:var(--muted)">Wave ${DS.wave} | +${xp} XP earned</p><button class="btn" onclick="dodgeStart()"><span>RESPAWN</span></button>`;
  cv.closest('#gw-dodge').style.position='relative';cv.closest('#gw-dodge').appendChild(ov);
}

// ═══════════════════════════════════════════════════
//  GAME 2: MEMORY (COMBO + TIMED CHALLENGE)
// ═══════════════════════════════════════════════════
const MS={flipped:[],matched:0,moves:0,timer:null,elapsed:0,locked:false,combo:0,comboTimer:null,score:0};
const MEM_SYMS=['◈','⬡','◆','▣','⟁','⬢','▶','◉','◬','⊕'];
const MEM_COLORS=['#00f5ff','#b44fff','#39ff14','#ff2d78','#ff8c00','#ffd700','#00f5ff','#b44fff','#39ff14','#ff2d78'];
const MEM_TOTAL=10; // 10 pairs = 20 cards in 5x4

function memInit(){
  clearInterval(MS.timer);clearTimeout(MS.comboTimer);
  MS.moves=0;MS.matched=0;MS.elapsed=0;MS.locked=false;MS.flipped=[];MS.combo=0;MS.score=0;
  document.getElementById('m-moves').textContent=0;
  document.getElementById('m-pairs').textContent='0/'+MEM_TOTAL;
  document.getElementById('m-timer').textContent='00:00';
  document.getElementById('m-combo')&&(document.getElementById('m-combo').textContent='×1');
  document.getElementById('m-score')&&(document.getElementById('m-score').textContent='0');
  const arr=[...MEM_SYMS,...MEM_SYMS].sort(()=>Math.random()-.5);
  const g=document.getElementById('mem-grid');
  g.style.gridTemplateColumns='repeat(5,1fr)';g.style.maxWidth='420px';g.innerHTML='';
  arr.forEach((em,i)=>{
    const symIdx=MEM_SYMS.indexOf(em);
    const col=MEM_COLORS[symIdx]||'#00f5ff';
    const c=document.createElement('div');c.className='mem-card';c.dataset.v=em;c.dataset.col=col;
    c.style.width='72px';c.style.height='72px';
    c.innerHTML=`<div class="mem-card-inner">
      <div class="mem-face" style="background:linear-gradient(135deg,rgba(0,245,255,.04),rgba(180,79,255,.04))">
        <svg width="20" height="20" viewBox="0 0 20 20" style="opacity:.3"><path d="M10,2 L18,18 L2,18 Z" fill="none" stroke="rgba(0,245,255,.5)" stroke-width="1"/></svg>
      </div>
      <div class="mem-back" style="color:${col};text-shadow:0 0 10px ${col}88;background:linear-gradient(135deg,${col}0f,${col}1a)">${em}</div>
    </div>`;
    c.onclick=()=>memFlip(c);
    if(G.bonus.memPeek){c.classList.add('flipped');setTimeout(()=>c.classList.remove('flipped'),G.bonus.memLong?600:380);}
    g.appendChild(c);
  });
  MS.timer=setInterval(()=>{
    MS.elapsed++;
    const m=Math.floor(MS.elapsed/60).toString().padStart(2,'0'),s=(MS.elapsed%60).toString().padStart(2,'0');
    document.getElementById('m-timer').textContent=m+':'+s;
  },1000);
}

function memFlip(c){
  if(MS.locked||c.classList.contains('flipped')||c.classList.contains('matched'))return;
  c.classList.add('flipped');MS.flipped.push(c);
  if(MS.flipped.length===2){
    MS.locked=true;MS.moves++;
    document.getElementById('m-moves').textContent=MS.moves;
    const [a,b]=MS.flipped;
    if(a.dataset.v===b.dataset.v){
      const col=a.dataset.col;
      MS.combo++;clearTimeout(MS.comboTimer);
      MS.comboTimer=setTimeout(()=>{MS.combo=0;document.getElementById('m-combo')&&(document.getElementById('m-combo').textContent='×1')},3000);
      document.getElementById('m-combo')&&(document.getElementById('m-combo').textContent='×'+Math.min(MS.combo,8));
      [a,b].forEach(card=>{
        card.classList.add('matched');
        card.querySelector('.mem-back').style.borderColor=col;
        card.querySelector('.mem-back').style.boxShadow=`0 0 16px ${col}66`;
      });
      const pts=10*Math.min(MS.combo,8);MS.score+=pts;
      document.getElementById('m-score')&&(document.getElementById('m-score').textContent=MS.score);
      MS.matched++;
      document.getElementById('m-pairs').textContent=MS.matched+'/'+MEM_TOTAL;
      MS.flipped=[];MS.locked=false;
      if(MS.matched===MEM_TOTAL)memWin();
    } else {
      MS.combo=0;document.getElementById('m-combo')&&(document.getElementById('m-combo').textContent='×1');
      const delay=G.bonus.memLong?900:500;
      // Flash red briefly
      [a,b].forEach(card=>{card.querySelector('.mem-back').style.borderColor='#ff2d78'});
      setTimeout(()=>{
        [a,b].forEach(card=>{card.querySelector('.mem-back').style.borderColor='';card.classList.remove('flipped')});
        MS.flipped=[];MS.locked=false;
      },delay);
    }
  }
}

function memWin(){
  clearInterval(MS.timer);clearTimeout(MS.comboTimer);const t=MS.elapsed;
  if(!G.highScores.memory||t<G.highScores.memory){G.highScores.memory=t;document.getElementById('hs-memory').textContent=t+'s';addFeed('RECORD','MEMORY best: '+t+'s')}
  const timeBonus=Math.max(0,120-t);const xp=Math.max(10,Math.floor((MS.score+timeBonus)/5));
  addXP(xp);updateSidebar();saveGame();
  notify('Memory cleared! +'+xp+' XP','green');addFeed('MEMORY','Complete! Score:'+MS.score+' Moves:'+MS.moves+' Time:'+t+'s');
}
function memReset(){memInit()}

// ═══════════════════════════════════════════════════
//  GAME 3: SEQUENCE (SIMON)
// ═══════════════════════════════════════════════════
const SS={seq:[],idx:0,round:0,score:0,accepting:false};
function simonInit(){
  SS.seq=[];SS.idx=0;SS.round=0;SS.score=0;SS.accepting=false;
  document.getElementById('s-round').textContent=0;document.getElementById('s-score').textContent=0;
  document.getElementById('s-status').textContent='READY';
  simonLock(true);
}
function simonStart(){SS.seq=[];SS.idx=0;SS.round=0;SS.score=0;document.getElementById('s-score').textContent=0;simonNextRound()}
function simonNextRound(){
  SS.round++;SS.idx=0;SS.accepting=false;simonLock(true);
  document.getElementById('s-round').textContent=SS.round;document.getElementById('s-status').textContent='WATCH...';
  SS.seq.push(Math.floor(Math.random()*4));
  // Shorter gap between rounds as rounds increase
  setTimeout(simonPlay,Math.max(300,650-SS.round*20));
}
function simonPlay(){
  // Speed ramps up every 3 rounds — gets brutal fast
  const baseDelay=G.bonus.simonFast?280:380;
  const rampDelay=Math.max(120,baseDelay-SS.round*18);
  SS.seq.forEach((v,i)=>{
    setTimeout(()=>{
      simonLit(v,Math.max(120,220-SS.round*8));
      if(i===SS.seq.length-1)setTimeout(()=>{document.getElementById('s-status').textContent='YOUR TURN';simonLock(false);SS.accepting=true},200);
    },i*(rampDelay+60));
  });
}
function simonLit(idx,dur){const b=document.querySelector('[data-sid="'+idx+'"]');b.classList.add('lit');setTimeout(()=>b.classList.remove('lit'),dur)}
function simonPress(idx){
  if(!SS.accepting)return;simonLit(idx,170);
  if(SS.seq[SS.idx]===idx){
    SS.idx++;
    if(SS.idx===SS.seq.length){
      SS.score+=SS.round*10;document.getElementById('s-score').textContent=SS.score;
      SS.accepting=false;simonLock(true);addFeed('SEQ','Round '+SS.round+' cleared!');
      setTimeout(simonNextRound,800);
    }
  } else {
    SS.accepting=false;simonLock(true);document.getElementById('s-status').textContent='FAIL!';simonEnd();
  }
}
function simonEnd(){
  const sc=SS.score;
  if(sc>G.highScores.simon){G.highScores.simon=sc;document.getElementById('hs-simon').textContent=sc;addFeed('RECORD','SEQUENCE high score: '+sc)}
  const xp=Math.floor(sc/5);addXP(xp);updateSidebar();saveGame();
  setTimeout(()=>{document.getElementById('s-status').textContent='OVER — '+sc+' pts';notify('Sequence ended! +'+xp+' XP','purple')},350);
}
function simonLock(v){document.querySelectorAll('.simon-btn').forEach(b=>b.style.pointerEvents=v?'none':'auto')}

// ═══════════════════════════════════════════════════
//  GAME 4: SNAKE (MAZE + POWER-UPS)
// ═══════════════════════════════════════════════════
const SN={running:false,snake:[],dir:{x:1,y:0},nextDir:{x:1,y:0},food:[],score:0,timer:null,
  cellSize:24,cols:23,rows:20,speed:1,walls:[],portals:[],powerup:null,powerTimer:0,
  ghost:false,ghostTimer:0,fast:false,fastTimer:0};

const SN_MAZES=[
  // Maze 0: outer walls only — clean open field
  (cols,rows)=>{
    const w=[];
    for(let x=0;x<cols;x++){w.push({x,y:0});w.push({x,y:rows-1})}
    for(let y=1;y<rows-1;y++){w.push({x:0,y});w.push({x:cols-1,y})}
    return w;
  },
  // Maze 1: outer walls + two short pillars (never touching spawn area)
  (cols,rows)=>{
    const w=[];
    for(let x=0;x<cols;x++){w.push({x,y:0});w.push({x,y:rows-1})}
    for(let y=1;y<rows-1;y++){w.push({x:0,y});w.push({x:cols-1,y})}
    // Left pillar — top quarter only, well away from centre spawn
    for(let y=2;y<Math.floor(rows/3);y++)w.push({x:Math.floor(cols/4),y});
    // Right pillar — bottom quarter only
    for(let y=Math.floor(rows*2/3);y<rows-2;y++)w.push({x:Math.floor(cols*3/4),y});
    return w;
  },
  // Maze 2: outer walls + four corner blocks with open paths between them
  (cols,rows)=>{
    const w=[];
    for(let x=0;x<cols;x++){w.push({x,y:0});w.push({x,y:rows-1})}
    for(let y=1;y<rows-1;y++){w.push({x:0,y});w.push({x:cols-1,y})}
    const gap=Math.floor(rows/4);
    // Top-left block
    for(let x=3;x<8;x++)for(let y=2;y<gap;y++)w.push({x,y});
    // Top-right block
    for(let x=cols-8;x<cols-3;x++)for(let y=2;y<gap;y++)w.push({x,y});
    // Bottom-left block
    for(let x=3;x<8;x++)for(let y=rows-gap;y<rows-2;y++)w.push({x,y});
    // Bottom-right block
    for(let x=cols-8;x<cols-3;x++)for(let y=rows-gap;y<rows-2;y++)w.push({x,y});
    return w;
  }
];

function snakeInit(){
  SN.running=false;clearInterval(SN.timer);
  document.getElementById('sn-score').textContent=0;document.getElementById('sn-speed').textContent=1;
  const cv=document.getElementById('snake-canvas');
  cv.parentElement.querySelector('.g-overlay')?.remove();
  snakeDraw();
}

document.addEventListener('keydown',e=>{
  const map={'ArrowUp':{x:0,y:-1},'ArrowDown':{x:0,y:1},'ArrowLeft':{x:-1,y:0},'ArrowRight':{x:1,y:0},
    'w':{x:0,y:-1},'s':{x:0,y:1},'a':{x:-1,y:0},'d':{x:1,y:0},
    'W':{x:0,y:-1},'S':{x:0,y:1},'A':{x:-1,y:0},'D':{x:1,y:0}};
  if(map[e.key]&&SN.running){
    const nd=map[e.key];
    if(nd.x!==-SN.dir.x||nd.y!==-SN.dir.y)SN.nextDir=nd;
  }
});

function snakeStart(){
  const cols=SN.cols,rows=SN.rows;
  // Spawn in dead centre — always safe, walls only exist on the border edges
  const cx=Math.floor(cols/2), cy=Math.floor(rows/2);
  SN.snake=[{x:cx,y:cy},{x:cx-1,y:cy},{x:cx-2,y:cy}];
  SN.dir={x:1,y:0};SN.nextDir={x:1,y:0};SN.score=0;SN.speed=1;SN.running=true;
  SN.ghost=false;SN.fast=false;SN.ghostTimer=0;SN.fastTimer=0;SN.powerup=null;
  SN.portals=[];
  // Outer walls only — inner obstacles unlock as score grows
  SN.walls=SN_MAZES[0](cols,rows);
  const wallSet=new Set(SN.walls.map(w=>w.x+','+w.y));
  SN.food=[];
  snakeSpawnFood(wallSet);snakeSpawnFood(wallSet);
  snakeSpawnPowerup(wallSet);
  clearInterval(SN.timer);
  document.getElementById('snake-canvas').closest('#gw-snake').querySelector('.g-overlay')?.remove();
  const baseInterval=G.bonus.snakeSlow?160:120;
  function step(){
    if(!SN.running){clearInterval(SN.timer);return}
    SN.dir={...SN.nextDir};
    if(SN.ghostTimer>0)SN.ghostTimer--;else SN.ghost=false;
    if(SN.fastTimer>0){SN.fastTimer--;} else SN.fast=false;
    const wallSet=new Set(SN.walls.map(w=>w.x+','+w.y));
    let nx=(SN.snake[0].x+SN.dir.x+cols)%cols;
    let ny=(SN.snake[0].y+SN.dir.y+rows)%rows;
    const head={x:nx,y:ny};
    // Wall collision (not in ghost mode)
    if(!SN.ghost&&wallSet.has(head.x+','+head.y)){snakeEnd();return}
    // Self collision
    if(SN.snake.some(s=>s.x===head.x&&s.y===head.y)){snakeEnd();return}
    SN.snake.unshift(head);
    // Food eaten?
    let ate=false;
    for(let i=SN.food.length-1;i>=0;i--){
      const f=SN.food[i];
      if(f.x===head.x&&f.y===head.y){
        const rogue=G.activeChar==='rogue';
        const worth=(rogue&&Math.random()<.3?3:1)*f.value;
        SN.score+=worth;
        if(SN.score%40===0){SN.speed++;clearInterval(SN.timer);SN.timer=setInterval(step,Math.max(55,(SN.fast?65:baseInterval)-SN.speed*14))}
        document.getElementById('sn-score').textContent=SN.score;document.getElementById('sn-speed').textContent=SN.speed;
        SN.food.splice(i,1);snakeSpawnFood(wallSet);ate=true;
        break;
      }
    }
    // Powerup eaten?
    if(SN.powerup&&SN.powerup.x===head.x&&SN.powerup.y===head.y){
      const pt=SN.powerup.type;
      if(pt==='ghost'){SN.ghost=true;SN.ghostTimer=120;notify('GHOST MODE: 2s','purple')}
      if(pt==='fast'){SN.fast=true;SN.fastTimer=180;clearInterval(SN.timer);SN.timer=setInterval(step,80);notify('SPEED BOOST!','orange')}
      if(pt==='score'){SN.score+=25;document.getElementById('sn-score').textContent=SN.score;notify('+25 BONUS!','gold')}
      SN.powerup=null;setTimeout(()=>snakeSpawnPowerup(wallSet),3000);
      ate=true;
    }
    if(!ate)SN.snake.pop();
    snakeDraw();
  }
  SN.timer=setInterval(step,baseInterval);
}

function snakeSpawnFood(wallSet){
  const snakeSet=new Set(SN.snake.map(s=>s.x+','+s.y));
  const foodSet=new Set(SN.food.map(f=>f.x+','+f.y));
  let pos;let tries=0;
  do{pos={x:Math.floor(Math.random()*(SN.cols-2))+1,y:Math.floor(Math.random()*(SN.rows-2))+1};tries++}
  while((wallSet.has(pos.x+','+pos.y)||snakeSet.has(pos.x+','+pos.y)||foodSet.has(pos.x+','+pos.y))&&tries<200);
  const type=Math.random()<.2?'bonus':'normal';
  SN.food.push({...pos,type,value:type==='bonus'?3:1,col:type==='bonus'?'#ffd700':'#ff2d78'});
}

function snakeSpawnPowerup(wallSet){
  const snakeSet=new Set(SN.snake.map(s=>s.x+','+s.y));
  let pos;let tries=0;
  do{pos={x:Math.floor(Math.random()*(SN.cols-2))+1,y:Math.floor(Math.random()*(SN.rows-2))+1};tries++}
  while((wallSet.has(pos.x+','+pos.y)||snakeSet.has(pos.x+','+pos.y))&&tries<200);
  const types=['ghost','fast','score'];
  SN.powerup={...pos,type:types[Math.floor(Math.random()*3)]};
}

function snakeDraw(){
  const cv=document.getElementById('snake-canvas');const ctx=cv.getContext('2d');
  const cs=SN.cellSize,W=cv.width,H=cv.height;
  ctx.fillStyle='#02040c';ctx.fillRect(0,0,W,H);
  // Grid
  ctx.strokeStyle='rgba(0,245,255,.025)';ctx.lineWidth=.5;
  for(let x=0;x<W;x+=cs){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke()}
  for(let y=0;y<H;y+=cs){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke()}
  // Walls
  SN.walls.forEach(w=>{
    ctx.save();ctx.shadowColor='#00f5ff';ctx.shadowBlur=6;
    ctx.fillStyle='rgba(0,20,35,0.95)';ctx.strokeStyle='rgba(0,245,255,.5)';ctx.lineWidth=1;
    ctx.fillRect(w.x*cs,w.y*cs,cs,cs);ctx.strokeRect(w.x*cs+.5,w.y*cs+.5,cs-1,cs-1);
    // Inner circuit detail
    ctx.strokeStyle='rgba(0,245,255,.2)';ctx.lineWidth=.5;
    ctx.strokeRect(w.x*cs+3,w.y*cs+3,cs-6,cs-6);
    ctx.restore();
  });
  // Food
  SN.food.forEach(f=>{
    ctx.save();ctx.shadowColor=f.col;ctx.shadowBlur=18;
    if(f.type==='bonus'){
      ctx.strokeStyle=f.col;ctx.lineWidth=1.5;ctx.fillStyle=f.col+'33';
      for(let i=0;i<5;i++){const a=i*Math.PI*2/5-Math.PI/2;const r1=cs*.42,r2=cs*.2;
        const bx=f.x*cs+cs/2,by=f.y*cs+cs/2;
        if(i===0)ctx.beginPath();
        ctx.lineTo(bx+r1*Math.cos(a),by+r1*Math.sin(a));
        const a2=a+Math.PI/5;ctx.lineTo(bx+r2*Math.cos(a2),by+r2*Math.sin(a2));
      }
      ctx.closePath();ctx.fill();ctx.stroke();
    } else {
      ctx.fillStyle=f.col;ctx.beginPath();ctx.arc(f.x*cs+cs/2,f.y*cs+cs/2,cs*.32,0,Math.PI*2);ctx.fill();
      ctx.fillStyle='rgba(255,255,255,.4)';ctx.beginPath();ctx.arc(f.x*cs+cs/2-2,f.y*cs+cs/2-2,cs*.1,0,Math.PI*2);ctx.fill();
    }
    ctx.restore();
  });
  // Powerup
  if(SN.powerup){
    const p=SN.powerup;
    const pcols={ghost:'#b44fff',fast:'#ff8c00',score:'#ffd700'};
    const col=pcols[p.type]||'#fff';
    const labels={ghost:'G',fast:'F',score:'$'};
    ctx.save();ctx.shadowColor=col;ctx.shadowBlur=20;
    ctx.strokeStyle=col;ctx.lineWidth=1.5;ctx.fillStyle=col+'22';
    const ang=Date.now()*0.003;
    ctx.translate(p.x*cs+cs/2,p.y*cs+cs/2);ctx.rotate(ang);
    ctx.beginPath();for(let i=0;i<4;i++){const a=i*Math.PI/2;ctx.lineTo(Math.cos(a)*(cs*.4),Math.sin(a)*(cs*.4));ctx.lineTo(Math.cos(a+Math.PI/4)*(cs*.22),Math.sin(a+Math.PI/4)*(cs*.22))}
    ctx.closePath();ctx.fill();ctx.stroke();ctx.rotate(-ang);
    ctx.fillStyle=col;ctx.font=`bold ${cs*.45}px Orbitron`;ctx.textAlign='center';ctx.textBaseline='middle';
    ctx.fillText(labels[p.type],0,0);ctx.restore();
  }
  // Snake
  SN.snake.forEach((s,i)=>{
    ctx.save();
    const ghost=SN.ghost;
    const head=i===0;
    const col=ghost?'#b44fff':'#00f5ff';
    ctx.shadowColor=col;ctx.shadowBlur=head?16:6;
    ctx.globalAlpha=ghost?0.55:1;
    ctx.fillStyle=head?col:i<5?col+'cc':'rgba(0,245,255,.4)';
    rrect(ctx,s.x*cs+2,s.y*cs+2,cs-4,cs-4,4);ctx.fill();
    if(head){
      ctx.fillStyle='rgba(0,0,0,.6)';rrect(ctx,s.x*cs+4,s.y*cs+4,cs-8,cs-8,3);ctx.fill();
      // Eyes
      ctx.fillStyle=col;ctx.shadowBlur=8;
      const ex=SN.dir.x,ey=SN.dir.y;
      ctx.beginPath();ctx.arc(s.x*cs+cs/2+ex*4+ey*4,s.y*cs+cs/2+ey*4+ex*4,2.5,0,Math.PI*2);ctx.fill();
      ctx.beginPath();ctx.arc(s.x*cs+cs/2+ex*4-ey*4,s.y*cs+cs/2+ey*4-ex*4,2.5,0,Math.PI*2);ctx.fill();
    }
    ctx.restore();
  });
  // Status indicators
  if(SN.ghost||SN.fast){
    ctx.save();ctx.font='bold 11px Orbitron';ctx.textBaseline='top';
    if(SN.ghost){ctx.fillStyle='#b44fff';ctx.shadowColor='#b44fff';ctx.shadowBlur=8;ctx.fillText('GHOST '+Math.ceil(SN.ghostTimer/60)+'s',8,8)}
    if(SN.fast){ctx.fillStyle='#ff8c00';ctx.shadowColor='#ff8c00';ctx.shadowBlur=8;ctx.fillText('BOOST '+Math.ceil(SN.fastTimer/60)+'s',SN.ghost?120:8,8)}
    ctx.restore();
  }
}

function snakeEnd(){
  SN.running=false;clearInterval(SN.timer);
  const sc=SN.score;
  if(sc>G.highScores.snake){G.highScores.snake=sc;document.getElementById('hs-snake').textContent=sc;addFeed('RECORD','SNAKE high score: '+sc)}
  const xp=Math.floor(sc/4);addXP(xp);updateSidebar();saveGame();addFeed('SNAKE','Game over. Score: '+sc+' +'+xp+' XP');
  const wrap=document.getElementById('gw-snake');
  const ov=document.createElement('div');ov.className='g-overlay';
  ov.innerHTML=`<h2 style="color:var(--green)">SNAKE DEAD</h2><div class="big-score" style="color:var(--green)">${sc}</div><p style="color:var(--muted)">+${sc} XP</p><button class="btn btn-green" onclick="snakeStart()"><span>RETRY</span></button>`;
  wrap.style.position='relative';wrap.appendChild(ov);notify('Snake ended! +'+sc+' XP','green');
}

// ═══════════════════════════════════════════════════
//  GAME 5: TYPING SPEED (HACKING TERMINAL)
// ═══════════════════════════════════════════════════
const TY={words:[],current:0,charIdx:0,timer:null,elapsed:0,totalTime:30,correct:0,wrong:0,running:false,combo:0,streak:0,difficulty:'normal'};
const TY_POOLS={
  easy:'the and you are for with have this they that from been will your more about when make find like time into just over some think also back after'.split(' '),
  normal:'nexus cyber arcade quantum void flux node zero echo delta prime omega sigma alpha binary pulse surge matrix grid lock hack forge warp relay boost shift core link mesh sync chain drop apex limit code dark wave scan trace orbit drift'.split(' '),
  hard:'cryptographic authentication decentralized synchronization parallelization initialization infrastructure vulnerability configuration electronegativity asymmetric polymorphism obfuscation fragmentation'.split(' ')
};
const TY_BONUS=['NEXUS','CYBER','QUANTUM','ZERO','PULSE']; // bonus power words for extra points

function typingInit(){
  TY.running=false;clearInterval(TY.timer);
  document.getElementById('ty-input').value='';document.getElementById('ty-input').disabled=true;
  document.getElementById('ty-wpm').textContent=0;document.getElementById('ty-acc').textContent='100%';
  document.getElementById('ty-score').textContent=0;document.getElementById('ty-combo')&&(document.getElementById('ty-combo').textContent='×1');
  const totalTime=G.bonus.typingTime?35:30;TY.totalTime=totalTime;
  document.getElementById('ty-timer').textContent=totalTime+'s';
  document.getElementById('ty-display').innerHTML='<span style="color:var(--muted)">Press START to begin hacking...</span>';
  document.getElementById('ty-diff')&&(document.getElementById('ty-diff').textContent=TY.difficulty.toUpperCase());
}

function typingStart(){
  TY.words=[];TY.combo=0;TY.streak=0;
  const pool=[...TY_POOLS[TY.difficulty]];
  // Insert bonus words occasionally
  for(let i=0;i<60;i++){
    if(Math.random()<.08&&G.activeChar==='rogue')TY.words.push(TY_BONUS[Math.floor(Math.random()*TY_BONUS.length)]);
    else TY.words.push(pool[Math.floor(Math.random()*pool.length)]);
  }
  TY.current=0;TY.charIdx=0;TY.correct=0;TY.wrong=0;TY.elapsed=0;TY.running=true;
  const totalTime=G.bonus.typingTime?35:30;TY.totalTime=totalTime;
  document.getElementById('ty-input').value='';document.getElementById('ty-input').disabled=false;
  document.getElementById('ty-input').focus();
  renderTypingDisplay();
  clearInterval(TY.timer);
  TY.timer=setInterval(()=>{
    TY.elapsed++;const left=TY.totalTime-TY.elapsed;
    document.getElementById('ty-timer').textContent=left+'s';
    const wpm=TY.elapsed>0?Math.round((TY.correct/5)/(TY.elapsed/60)):0;
    document.getElementById('ty-wpm').textContent=wpm;
    if(left<=0)typingEnd();
  },1000);
}

function typingInput(){
  if(!TY.running)return;
  const inp=document.getElementById('ty-input');const val=inp.value;
  const word=TY.words[TY.current];
  if(val.endsWith(' ')){
    const typed=val.trim();
    const correct=typed===word;
    if(correct){
      TY.correct+=word.length+1;TY.streak++;TY.combo=Math.min(8,1+Math.floor(TY.streak/3));
      // Bonus word extra points
      if(TY_BONUS.includes(word)){TY.correct+=word.length*2;notify('+BONUS: '+word,'gold')}
    } else {
      TY.wrong+=Math.abs(typed.length-word.length)+1;TY.streak=0;TY.combo=1;
    }
    document.getElementById('ty-combo')&&(document.getElementById('ty-combo').textContent='×'+TY.combo);
    TY.current++;TY.charIdx=0;inp.value='';
    if(TY.current>=TY.words.length){typingEnd();return}
    renderTypingDisplay();return;
  }
  TY.charIdx=val.length;renderTypingDisplay();
  const acc=TY.correct+TY.wrong>0?Math.round(TY.correct/(TY.correct+TY.wrong)*100):100;
  document.getElementById('ty-acc').textContent=acc+'%';
}

function renderTypingDisplay(){
  const el=document.getElementById('ty-display');
  const context=TY.words.slice(Math.max(0,TY.current-1),TY.current+9);
  const offset=TY.current>0?1:0;
  let html='';
  context.forEach((w,wi)=>{
    const wordIdx=TY.current-offset+wi;
    const isBonus=TY_BONUS.includes(w);
    const bonusStyle=isBonus?'color:#ffd700;text-shadow:0 0 8px #ffd700aa;':'' ;
    if(wordIdx<TY.current){html+=`<span class="t-correct" style="${bonusStyle}">${w}</span> `}
    else if(wordIdx===TY.current){
      const inp=document.getElementById('ty-input').value;
      let whtml='';
      for(let i=0;i<w.length;i++){
        if(i<inp.length)whtml+=`<span class="${inp[i]===w[i]?'t-correct':'t-wrong'}">${w[i]}</span>`;
        else if(i===inp.length)whtml+=`<span class="t-cursor">|</span><span class="t-pending">${w[i]}</span>`;
        else whtml+=`<span class="t-pending">${w[i]}</span>`;
      }
      if(inp.length>=w.length)whtml+=`<span class="t-cursor">|</span>`;
      html+=`<span style="${isBonus?'background:rgba(255,215,0,.08);border-radius:3px;padding:0 2px;':''}">${whtml}</span> `;
    } else {
      html+=`<span class="t-pending" style="${bonusStyle}">${w}</span> `;
    }
  });
  el.innerHTML=html;
}

function typingEnd(){
  clearInterval(TY.timer);TY.running=false;
  document.getElementById('ty-input').disabled=true;
  const wpm=TY.elapsed>0?Math.round((TY.correct/5)/(TY.elapsed/60)):0;
  const acc=TY.correct+TY.wrong>0?Math.round(TY.correct/(TY.correct+TY.wrong)*100):100;
  const sc=Math.round(wpm*acc/100*TY.combo);
  document.getElementById('ty-score').textContent=sc;
  if(wpm>G.highScores.typing){G.highScores.typing=wpm;document.getElementById('hs-typing').textContent=wpm+' WPM';addFeed('RECORD','TYPING high: '+wpm+' WPM')}
  const xp=Math.floor(sc/3);addXP(xp);updateSidebar();saveGame();
  addFeed('TYPING',wpm+'WPM '+acc+'% acc +'+xp+'XP');
  document.getElementById('ty-display').innerHTML=`<span class="t-correct">HACK COMPLETE // ${wpm} WPM • ${acc}% • COMBO ×${TY.combo} • +${xp} XP</span>`;
  notify(wpm+' WPM! +'+xp+' XP','cyan');
}

// ═══════════════════════════════════════════════════
//  GAME 6: BREAKOUT (ADVANCED)
// ═══════════════════════════════════════════════════
const BR={running:false,paddle:{x:0,w:100,y:0,h:12},balls:[],bricks:[],score:0,lives:3,level:1,fid:null,mouseX:null,powerups:[],lasers:[],laserActive:false,laserTimer:0,particles:[]};

function breakoutInit(){
  BR.running=false;if(BR.fid)cancelAnimationFrame(BR.fid);
  document.getElementById('br-score').textContent=0;document.getElementById('br-lives').textContent=3;document.getElementById('br-level').textContent=1;
  const cv=document.getElementById('breakout-canvas');
  cv.closest('#gw-breakout').querySelector('.g-overlay')?.remove();
  const ctx=cv.getContext('2d');ctx.fillStyle='#02040c';ctx.fillRect(0,0,cv.width,cv.height);
}

function breakoutStart(){
  const cv=document.getElementById('breakout-canvas');const W=cv.width,H=cv.height;
  const pw=G.bonus.breakoutBig?120:80;
  BR.paddle={x:W/2-pw/2,w:pw,y:H-28,h:12};
  BR.balls=[{x:W/2,y:H-60,vx:4.5*(Math.random()<.5?1:-1),vy:-5.5,r:7}];
  BR.score=0;BR.lives=3;BR.level=1;BR.running=true;BR.bricks=[];BR.powerups=[];BR.lasers=[];BR.laserActive=false;BR.laserTimer=0;BR.particles=[];
  breakoutMakeBricks(1);
  document.getElementById('br-score').textContent=0;document.getElementById('br-lives').textContent=3;document.getElementById('br-level').textContent=1;
  cv.closest('#gw-breakout').querySelector('.g-overlay')?.remove();
  cv.onmousemove=e=>{const rect=cv.getBoundingClientRect();BR.mouseX=(e.clientX-rect.left)*(W/rect.width)};
  if(BR.fid)cancelAnimationFrame(BR.fid);
  breakoutLoop();
}

function breakoutMakeBricks(lvl){
  const colors=['#00f5ff','#b44fff','#ff2d78','#39ff14','#ff8c00','#ffd700'];
  const rows=Math.min(3+lvl,7),cols=10;
  BR.bricks=[];
  for(let r=0;r<rows;r++)for(let c=0;c<cols;c++){
    const col=colors[(r+lvl)%colors.length];
    const type=Math.random()<.14?'explosive':Math.random()<.2?'moving':'normal';
    const hp=type==='explosive'?2:r<1?1:r<3?2:3;
    BR.bricks.push({x:c*64+4,y:r*22+44,w:60,h:16,alive:true,col,hp,type,
      mx:type==='moving'?(Math.random()<.5?1:-1)*1.2:0,origX:c*64+4});
  }
}

function brSpawnParticles(x,y,col,n=8){
  for(let i=0;i<n;i++){
    const a=Math.random()*Math.PI*2,spd=1+Math.random()*4;
    BR.particles.push({x,y,vx:Math.cos(a)*spd,vy:Math.sin(a)*spd,life:25,col,r:Math.random()*3+1});
  }
}

function breakoutLoop(){
  if(!BR.running)return;
  const cv=document.getElementById('breakout-canvas');const ctx=cv.getContext('2d');
  const W=cv.width,H=cv.height;const p=BR.paddle;

  // Paddle
  if(BR.mouseX!==null)p.x=Math.max(0,Math.min(W-p.w,BR.mouseX-p.w/2));

  // Laser shoot (hold Z or click)
  if(BR.laserActive&&BR.laserTimer<=0){
    BR.lasers.push({x:p.x+p.w*.25,y:p.y,vy:-14});
    BR.lasers.push({x:p.x+p.w*.75,y:p.y,vy:-14});
    BR.laserTimer=8;
  }
  if(BR.laserTimer>0)BR.laserTimer--;

  // Move lasers
  BR.lasers.forEach(l=>l.y+=l.vy);
  BR.lasers=BR.lasers.filter(l=>l.y>-10);

  // Move powerups
  BR.powerups.forEach(pu=>pu.y+=1.8);
  BR.powerups=BR.powerups.filter(pu=>{
    if(pu.y>H)return false;
    if(pu.x>p.x&&pu.x<p.x+p.w&&pu.y>p.y-10&&pu.y<p.y+p.h+10){
      applyBreakoutPower(pu.type,W);return false;
    }
    return true;
  });

  // Move bricks (moving type)
  BR.bricks.forEach(bk=>{
    if(!bk.alive||bk.type!=='moving')return;
    bk.x+=bk.mx;
    if(bk.x<2||bk.x+bk.w>W-2)bk.mx*=-1;
  });

  // Laser-brick collision
  for(let li=BR.lasers.length-1;li>=0;li--){
    const l=BR.lasers[li];
    for(let bi=BR.bricks.length-1;bi>=0;bi--){
      const bk=BR.bricks[bi];if(!bk.alive)continue;
      if(l.x>bk.x&&l.x<bk.x+bk.w&&l.y>bk.y&&l.y<bk.y+bk.h){
        BR.lasers.splice(li,1);bk.hp--;
        brSpawnParticles(l.x,l.y,bk.col,4);
        if(bk.hp<=0){hitBrick(bk,bi,W);}
        break;
      }
    }
  }

  // Balls
  for(let bi=BR.balls.length-1;bi>=0;bi--){
    const b=BR.balls[bi];
    b.x+=b.vx;b.y+=b.vy;
    if(b.x-b.r<0){b.x=b.r;b.vx=Math.abs(b.vx)}
    if(b.x+b.r>W){b.x=W-b.r;b.vx=-Math.abs(b.vx)}
    if(b.y-b.r<0){b.y=b.r;b.vy=Math.abs(b.vy)}
    // Paddle
    if(b.vy>0&&b.y+b.r>p.y&&b.y+b.r<p.y+p.h+b.r*2&&b.x>p.x-b.r&&b.x<p.x+p.w+b.r){
      b.vy=-Math.abs(b.vy);
      b.vx+=(b.x-(p.x+p.w/2))/p.w*5;
      const spd=Math.sqrt(b.vx*b.vx+b.vy*b.vy);
      if(spd>10){b.vx=b.vx/spd*10;b.vy=b.vy/spd*10}
      if(spd<3.5){const sc=3.5/spd;b.vx*=sc;b.vy*=sc}
    }
    // Bottom
    if(b.y>H+20){
      BR.balls.splice(bi,1);
      if(BR.balls.length===0){
        BR.lives--;document.getElementById('br-lives').textContent=BR.lives;
        if(BR.lives<=0){breakoutEnd();return}
        BR.balls=[{x:W/2,y:H-80,vx:3.5*(Math.random()<.5?1:-1),vy:-4.5,r:7}];
      }
      continue;
    }
    // Bricks
    for(let bki=BR.bricks.length-1;bki>=0;bki--){
      const bk=BR.bricks[bki];if(!bk.alive)continue;
      if(b.x+b.r>bk.x&&b.x-b.r<bk.x+bk.w&&b.y+b.r>bk.y&&b.y-b.r<bk.y+bk.h){
        // Determine bounce axis
        const overX=Math.min(b.x+b.r-bk.x,bk.x+bk.w-b.x+b.r);
        const overY=Math.min(b.y+b.r-bk.y,bk.y+bk.h-b.y+b.r);
        if(overX<overY)b.vx*=-1;else b.vy*=-1;
        bk.hp--;brSpawnParticles(b.x,bk.y+bk.h/2,bk.col,5);
        if(bk.hp<=0)hitBrick(bk,bki,W);
        break;
      }
    }
  }

  // Particles
  BR.particles.forEach(pt=>{pt.x+=pt.vx;pt.y+=pt.vy;pt.vy+=.08;pt.life--});
  BR.particles=BR.particles.filter(pt=>pt.life>0);

  // Next level
  if(BR.bricks.every(bk=>!bk.alive)){
    BR.level++;document.getElementById('br-level').textContent=BR.level;
    breakoutMakeBricks(BR.level);
    BR.balls.forEach(b=>{const spd=Math.sqrt(b.vx*b.vx+b.vy*b.vy)*1.12;const a=Math.atan2(b.vy,b.vx);b.vx=Math.cos(a)*spd;b.vy=Math.sin(a)*spd});
    BR.score+=BR.level*100;document.getElementById('br-score').textContent=BR.score;
  }

  // ── DRAW ──
  ctx.fillStyle='rgba(2,4,12,.94)';ctx.fillRect(0,0,W,H);
  ctx.strokeStyle='rgba(0,245,255,.02)';ctx.lineWidth=.6;
  for(let x=0;x<W;x+=36){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,H);ctx.stroke()}
  for(let y=0;y<H;y+=36){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(W,y);ctx.stroke()}

  // Bricks
  BR.bricks.forEach(bk=>{
    if(!bk.alive)return;
    ctx.save();ctx.shadowColor=bk.col;ctx.shadowBlur=bk.type==='explosive'?16:8;
    const alpha=bk.hp===3?'22':bk.hp===2?'55':'88';
    ctx.fillStyle=bk.col+alpha;ctx.strokeStyle=bk.col;ctx.lineWidth=1;
    rrect(ctx,bk.x,bk.y,bk.w,bk.h,3);ctx.fill();ctx.stroke();
    if(bk.type==='explosive'){ctx.fillStyle=bk.col;ctx.font='8px Orbitron';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText('!',bk.x+bk.w/2,bk.y+bk.h/2)}
    if(bk.type==='moving'){ctx.strokeStyle=bk.col+'88';ctx.lineWidth=.5;rrect(ctx,bk.x+3,bk.y+3,bk.w-6,bk.h-6,2);ctx.stroke()}
    ctx.restore();
  });

  // Powerups
  BR.powerups.forEach(pu=>{
    const pcols={wide:'#00f5ff',multi:'#b44fff',laser:'#ff2d78',slow:'#39ff14'};
    const col=pcols[pu.type]||'#fff';
    ctx.save();ctx.shadowColor=col;ctx.shadowBlur=14;
    ctx.fillStyle=col+'33';ctx.strokeStyle=col;ctx.lineWidth=1.5;
    ctx.beginPath();ctx.arc(pu.x,pu.y,10,0,Math.PI*2);ctx.fill();ctx.stroke();
    ctx.fillStyle=col;ctx.font='bold 8px Orbitron';ctx.textAlign='center';ctx.textBaseline='middle';
    ctx.fillText({wide:'W',multi:'×',laser:'L',slow:'S'}[pu.type]||'?',pu.x,pu.y);
    ctx.restore();
  });

  // Lasers
  BR.lasers.forEach(l=>{
    ctx.save();ctx.shadowColor='#ff2d78';ctx.shadowBlur=10;
    ctx.fillStyle='#ff2d78';ctx.fillRect(l.x-1.5,l.y-12,3,14);ctx.restore();
  });

  // Particles
  BR.particles.forEach(pt=>{
    ctx.save();ctx.globalAlpha=pt.life/25;
    ctx.fillStyle=pt.col;ctx.shadowColor=pt.col;ctx.shadowBlur=6;
    ctx.beginPath();ctx.arc(pt.x,pt.y,pt.r,0,Math.PI*2);ctx.fill();ctx.restore();
  });

  // Paddle
  ctx.save();ctx.shadowColor=BR.laserActive?'#ff2d78':'#00f5ff';ctx.shadowBlur=18;
  ctx.fillStyle=BR.laserActive?'rgba(255,45,120,.25)':'rgba(0,245,255,.2)';
  ctx.strokeStyle=BR.laserActive?'#ff2d78':'#00f5ff';ctx.lineWidth=2;
  rrect(ctx,p.x,p.y,p.w,p.h,6);ctx.fill();ctx.stroke();
  if(BR.laserActive){ctx.fillStyle='#ff2d78';ctx.beginPath();ctx.arc(p.x+p.w*.25,p.y-2,3,0,Math.PI*2);ctx.fill();ctx.beginPath();ctx.arc(p.x+p.w*.75,p.y-2,3,0,Math.PI*2);ctx.fill()}
  ctx.restore();

  // Balls
  BR.balls.forEach(b=>{
    ctx.save();ctx.shadowColor='#fff';ctx.shadowBlur=22;
    ctx.fillStyle='#ffffff';ctx.beginPath();ctx.arc(b.x,b.y,b.r,0,Math.PI*2);ctx.fill();
    ctx.fillStyle='rgba(0,245,255,.4)';ctx.beginPath();ctx.arc(b.x-2,b.y-2,b.r*.4,0,Math.PI*2);ctx.fill();
    ctx.restore();
  });

  // HUD
  ctx.save();ctx.font='10px Orbitron';ctx.textAlign='right';ctx.fillStyle='rgba(0,245,255,.4)';
  ctx.fillText('Z=LASER · MOUSE=PADDLE',W-8,H-8);ctx.restore();

  BR.fid=requestAnimationFrame(breakoutLoop);
}

function hitBrick(bk,idx,W){
  bk.alive=false;
  BR.score+=10*(BR.level+1);document.getElementById('br-score').textContent=BR.score;
  brSpawnParticles(bk.x+bk.w/2,bk.y+bk.h/2,bk.col,12);
  // Explosive chain
  if(bk.type==='explosive'){
    BR.bricks.forEach((b2,i2)=>{if(!b2.alive)return;const dx=b2.x-bk.x,dy=b2.y-bk.y;if(Math.abs(dx)<80&&Math.abs(dy)<40){b2.hp--;brSpawnParticles(b2.x+b2.w/2,b2.y,bk.col,6);if(b2.hp<=0)hitBrick(b2,i2,W)}});
  }
  // Powerup drop
  if(Math.random()<.18){
    const types=['wide','multi','laser','slow'];
    BR.powerups.push({x:bk.x+bk.w/2,y:bk.y,type:types[Math.floor(Math.random()*types.length)]});
  }
}

function applyBreakoutPower(type,W){
  if(type==='wide'){BR.paddle.w=Math.min(160,BR.paddle.w+30);setTimeout(()=>{BR.paddle.w=Math.max(80,BR.paddle.w-30)},8000);notify('WIDE PADDLE!','cyan')}
  if(type==='multi'){
    const b=BR.balls[0]||{x:W/2,y:200,vx:3,vy:-4,r:7};
    BR.balls.push({x:b.x+5,y:b.y,vx:-b.vx*1.1,vy:b.vy,r:7});
    BR.balls.push({x:b.x-5,y:b.y,vx:b.vx*.8,vy:b.vy*1.1,r:7});
    notify('MULTI-BALL!','purple');
  }
  if(type==='laser'){BR.laserActive=true;setTimeout(()=>BR.laserActive=false,8000);notify('LASER ACTIVE!','pink')}
  if(type==='slow'){BR.balls.forEach(b=>{b.vx*=.7;b.vy*=.7});notify('SLOW BALL!','green')}
}

document.addEventListener('keydown',e=>{if(e.key==='z'||e.key==='Z')BR.laserActive&&(BR.laserTimer=0)});

function breakoutEnd(){
  BR.running=false;cancelAnimationFrame(BR.fid);
  const sc=BR.score;
  if(sc>G.highScores.breakout){G.highScores.breakout=sc;document.getElementById('hs-breakout').textContent=sc;addFeed('RECORD','BREAKOUT high score: '+sc)}
  const xp=Math.floor(sc/5);addXP(xp);updateSidebar();saveGame();addFeed('BREAKOUT','Game over. Score: '+sc+' +'+xp+' XP');
  const wrap=document.getElementById('gw-breakout');
  const ov=document.createElement('div');ov.className='g-overlay';
  ov.innerHTML=`<h2 style="color:var(--pink)">SHIELDS GONE</h2><div class="big-score" style="color:var(--pink)">${sc}</div><p style="color:var(--muted)">Level ${BR.level} | +${sc} XP</p><button class="btn btn-pink" onclick="breakoutStart()"><span>RETRY</span></button>`;
  wrap.style.position='relative';wrap.appendChild(ov);notify('Breakout ended! +'+sc+' XP','pink');
}

// ═══════════════════════════════════════════════════
//  SHOP
// ═══════════════════════════════════════════════════
function renderShop(){
  document.getElementById('shop-xp').textContent=G.totalXP;
  const wrap=document.getElementById('shop-content');
  wrap.innerHTML='';
  // Locked games
  const lockedGames=GAMES.filter(g=>!G.owned.games.includes(g.id));
  if(lockedGames.length){
    const t=document.createElement('div');t.className='sec-title';t.innerHTML='UNLOCK GAMES<span style="flex:1;height:1px;background:linear-gradient(90deg,var(--acc1),transparent);opacity:.3;margin-left:10px;display:block"></span>';
    wrap.appendChild(t);
    const grid=document.createElement('div');grid.className='shop-grid';
    lockedGames.forEach(g=>{
      const el=document.createElement('div');el.className='shop-item';
      el.innerHTML=`<div class="shop-preview" style="background:${g.color.replace('.3','0.15')};border-color:${g.cb}44"><span style="font-size:36px">${g.icon}</span></div><div class="shop-name">${g.name}</div><div class="shop-desc">${g.sub} — New arcade mission</div><div style="display:flex;justify-content:space-between;align-items:center"><div class="shop-cost">⬡ ${g.unlockCost}</div><button class="btn btn-gold btn-sm" onclick="buyGame('${g.id}')"><span>UNLOCK</span></button></div>`;
      grid.appendChild(el);
    });
    wrap.appendChild(grid);
  }
  // Locked chars
  const lockedChars=CHARS.filter(c=>!G.owned.chars.includes(c.id));
  if(lockedChars.length){
    const t=document.createElement('div');t.className='sec-title';t.style.marginTop='24px';t.innerHTML='UNLOCK AGENTS<span style="flex:1;height:1px;background:linear-gradient(90deg,var(--acc1),transparent);opacity:.3;margin-left:10px;display:block"></span>';
    wrap.appendChild(t);
    const grid=document.createElement('div');grid.className='shop-grid';
    lockedChars.forEach(ch=>{
      const el=document.createElement('div');el.className='shop-item';
      el.innerHTML=`<div class="shop-preview" style="background:${ch.hex}0c;border-color:${ch.hex}33;overflow:hidden;padding:0"><svg viewBox="30 20 140 100" style="width:100%;height:80px">${ch.portrait}</svg></div><div class="shop-name" style="color:${ch.color}">${ch.name}</div><div class="shop-desc">${ch.desc}</div><div class="shop-desc" style="color:${ch.color};margin-bottom:12px">${ch.abilityName}: ${ch.abilityDesc}</div><div style="display:flex;justify-content:space-between;align-items:center"><div class="shop-cost">⬡ ${ch.unlockCost}</div><button class="btn btn-gold btn-sm" onclick="buyCharShop('${ch.id}')"><span>UNLOCK</span></button></div>`;
      grid.appendChild(el);
    });
    wrap.appendChild(grid);
  }
  // Cosmetics
  [
    {label:'THEMES',items:SHOP_THEMES,owned:'themes',buy:'buyTheme',renderPreview:it=>`<div style="width:100%;height:60px;background:${it.preview};border-radius:6px;border:1px solid rgba(255,255,255,.1)"></div>`},
    {label:'BACKGROUNDS',items:SHOP_BACKGROUNDS,owned:'backgrounds',buy:'buyBg',renderPreview:it=>`<div style="font-size:28px">${{particles:'✦',grid:'⊞',hexgrid:'⬡',rain:'║',nebula:'☁'}[it.id]||'?'}</div>`},
    {label:'ACCENT COLORS',items:SHOP_ACCENTS,owned:'accents',buy:'buyAccent',renderPreview:it=>`<div style="display:flex;gap:8px;justify-content:center"><div style="width:28px;height:28px;border-radius:50%;background:${it.c1||'#fff'}"></div><div style="width:28px;height:28px;border-radius:50%;background:${it.c2||'#888'}"></div></div>`},
  ].forEach(section=>{
    const t=document.createElement('div');t.className='sec-title';t.style.marginTop='24px';t.innerHTML=section.label+'<span style="flex:1;height:1px;background:linear-gradient(90deg,var(--acc1),transparent);opacity:.3;margin-left:10px;display:block"></span>';
    wrap.appendChild(t);
    const grid=document.createElement('div');grid.className='shop-grid';
    section.items.forEach(it=>{
      const isOwned=G.owned[section.owned].includes(it.id);
      const el=document.createElement('div');el.className='shop-item'+(isOwned?' owned':'');
      el.innerHTML=`<div class="shop-preview" style="flex-direction:column;gap:8px">${section.renderPreview(it)}</div><div class="shop-name">${it.name}</div><div class="shop-desc">${it.desc}</div>${isOwned?'':`<div style="display:flex;justify-content:space-between;align-items:center"><div class="shop-cost">⬡ ${it.cost}</div><button class="btn btn-gold btn-sm" onclick="${section.buy}('${it.id}')"><span>BUY</span></button></div>`}`;
      grid.appendChild(el);
    });
    wrap.appendChild(grid);
  });
}

function buyGame(id){
  const g=GAMES.find(x=>x.id===id);if(!g)return;
  if(G.totalXP<g.unlockCost){notify('Need ⬡'+g.unlockCost,'pink');return}
  G.totalXP-=g.unlockCost;G.owned.games.push(id);saveGame();updateHdrXP();renderShop();renderGameBtns();
  notify(g.name+' unlocked!','gold');addFeed('UNLOCK',g.name+' game acquired.');
}
function buyCharShop(id){
  const ch=CHARS.find(c=>c.id===id);if(!ch)return;
  if(G.totalXP<ch.unlockCost){notify('Need ⬡'+ch.unlockCost,'pink');return}
  G.totalXP-=ch.unlockCost;G.owned.chars.push(id);saveGame();updateHdrXP();renderShop();renderChars();
  notify('Agent '+ch.name+' unlocked!','gold');addFeed('UNLOCK','Agent '+ch.name+' acquired.');
}
function buyCosmetic(type,id,cost,arr){
  if(arr.includes(id)){notify('Already owned!','muted');return}
  if(G.totalXP<cost){notify('Need ⬡'+cost,'pink');return}
  G.totalXP-=cost;arr.push(id);saveGame();updateHdrXP();renderShop();notify('Unlocked!','gold');
}
function buyTheme(id){const it=SHOP_THEMES.find(x=>x.id===id);buyCosmetic('themes',id,it.cost,G.owned.themes)}
function buyBg(id){const it=SHOP_BACKGROUNDS.find(x=>x.id===id);buyCosmetic('backgrounds',id,it.cost,G.owned.backgrounds)}
function buyAccent(id){const it=SHOP_ACCENTS.find(x=>x.id===id);buyCosmetic('accents',id,it.cost,G.owned.accents)}

// ═══════════════════════════════════════════════════
//  CUSTOMIZE
// ═══════════════════════════════════════════════════
let currentCTab='themes';
function showCTab(tab){
  currentCTab=tab;
  document.querySelectorAll('.ctab').forEach(t=>t.classList.remove('active'));
  document.querySelectorAll('.ctab').forEach(t=>{if(t.textContent.toLowerCase()===tab.toLowerCase())t.classList.add('active')});
  renderCustomize();
}
function renderCustomize(){
  const el=document.getElementById('customize-content');el.innerHTML='';
  if(currentCTab==='themes'){
    const grid=document.createElement('div');grid.className='shop-grid';
    SHOP_THEMES.filter(t=>G.owned.themes.includes(t.id)).forEach(it=>{
      const active=G.equipped.theme===it.id;
      const card=document.createElement('div');
      card.className='shop-item';card.style.cursor='pointer';
      card.style.borderColor=active?'var(--gold)':'';
      card.style.boxShadow=active?'0 0 16px rgba(255,215,0,.15)':'';
      card.innerHTML=`<div class="shop-preview"><div style="width:100%;height:60px;background:${it.preview};border-radius:6px;border:1px solid rgba(255,255,255,.1)"></div></div><div class="shop-name">${it.name}</div><div class="shop-desc">${it.desc}</div><button class="btn ${active?'btn-gold':''} btn-sm" style="width:100%;margin-top:8px" onclick="applyTheme('${it.id}')"><span>${active?'✓ ACTIVE':'APPLY'}</span></button>`;
      grid.appendChild(card);
    });
    el.appendChild(grid);
    if(G.owned.themes.length<SHOP_THEMES.length)el.innerHTML+='<p style="font-size:13px;color:var(--muted);margin-top:16px">Unlock more themes in the Shop</p>';
  }
  if(currentCTab==='backgrounds'){
    const grid=document.createElement('div');grid.className='shop-grid';
    SHOP_BACKGROUNDS.filter(b=>G.owned.backgrounds.includes(b.id)).forEach(it=>{
      const active=G.equipped.background===it.id;
      const icons={particles:'✦',grid:'⊞',hexgrid:'⬡',rain:'║',nebula:'☁'};
      const card=document.createElement('div');card.className='shop-item';card.style.cursor='pointer';
      card.style.borderColor=active?'var(--gold)':'';
      card.innerHTML=`<div class="shop-preview"><span style="font-size:40px">${icons[it.id]||'?'}</span></div><div class="shop-name">${it.name}</div><div class="shop-desc">${it.desc}</div><button class="btn ${active?'btn-gold':''} btn-sm" style="width:100%;margin-top:8px" onclick="applyBg('${it.id}')"><span>${active?'✓ ACTIVE':'APPLY'}</span></button>`;
      grid.appendChild(card);
    });
    el.appendChild(grid);
  }
  if(currentCTab==='accents'){
    const grid=document.createElement('div');grid.className='shop-grid';
    SHOP_ACCENTS.filter(a=>G.owned.accents.includes(a.id)).forEach(it=>{
      const active=G.equipped.accent===it.id;
      const card=document.createElement('div');card.className='shop-item';card.style.cursor='pointer';
      card.style.borderColor=active?'var(--gold)':'';
      card.innerHTML=`<div class="shop-preview"><div style="display:flex;gap:12px;justify-content:center"><div style="width:36px;height:36px;border-radius:50%;background:${it.c1||'#fff'};box-shadow:0 0 16px ${it.c1||'#fff'}44"></div><div style="width:36px;height:36px;border-radius:50%;background:${it.c2||'#888'};box-shadow:0 0 16px ${it.c2||'#888'}44"></div></div></div><div class="shop-name">${it.name}</div><div class="shop-desc">${it.desc}</div><button class="btn ${active?'btn-gold':''} btn-sm" style="width:100%;margin-top:8px" onclick="applyAccent('${it.id}')"><span>${active?'✓ ACTIVE':'APPLY'}</span></button>`;
      grid.appendChild(card);
    });
    el.appendChild(grid);
  }
  if(currentCTab==='hud'){
    const grid=document.createElement('div');grid.className='shop-grid';
    SHOP_HUDS.forEach(it=>{
      const owned=G.owned.huds.includes(it.id);
      const active=G.equipped.hud===it.id;
      const card=document.createElement('div');card.className='shop-item';card.style.cursor='pointer';
      card.style.borderColor=active?'var(--gold)':'';
      card.innerHTML=`<div class="shop-preview"><span style="font-size:32px">${{minimal:'◻',retro:'▓',hologram:'◈'}[it.id]||'?'}</span></div><div class="shop-name">${it.name}</div><div class="shop-desc">${it.desc}</div>${owned?`<button class="btn ${active?'btn-gold':''} btn-sm" style="width:100%;margin-top:8px" onclick="applyHud('${it.id}')"><span>${active?'✓ ACTIVE':'APPLY'}</span></button>`:`<div style="font-size:12px;color:var(--muted);margin-top:8px">Unlock in Shop (⬡${it.cost})</div>`}`;
      grid.appendChild(card);
    });
    el.appendChild(grid);
  }
}

// Apply cosmetics
const THEMES_MAP={default:{bg:'#04060f'},crimson:{bg:'#0f0408'},matrix:{bg:'#000d04'},nebula:{bg:'#08040f'},toxic:{bg:'#040f04'}};
const ACCENTS_MAP={'cyan-purple':{c1:'#00f5ff',c2:'#b44fff'},'green-orange':{c1:'#39ff14',c2:'#ff8c00'},'pink-purple':{c1:'#ff2d78',c2:'#b44fff'},'gold-cyan':{c1:'#ffd700',c2:'#00f5ff'},'white-red':{c1:'#ffffff',c2:'#ff2020'}};

function applyTheme(id){
  G.equipped.theme=id;saveGame();
  const t=THEMES_MAP[id]||THEMES_MAP.default;
  document.documentElement.style.setProperty('--bg',t.bg);
  document.body.style.background=t.bg;
  notify('Theme applied!','gold');renderCustomize();addFeed('HUD','Theme: '+id);
}
function applyBg(id){
  G.equipped.background=id;saveGame();notify('Background changed!','gold');renderCustomize();addFeed('HUD','Background: '+id);
}
function applyAccent(id){
  G.equipped.accent=id;saveGame();
  const a=ACCENTS_MAP[id]||ACCENTS_MAP['cyan-purple'];
  document.documentElement.style.setProperty('--acc1',a.c1);
  document.documentElement.style.setProperty('--acc2',a.c2);
  document.documentElement.style.setProperty('--cyan',a.c1);
  document.documentElement.style.setProperty('--purple',a.c2);
  notify('Accent applied!','gold');renderCustomize();addFeed('HUD','Accent: '+id);
}
function applyHud(id){G.equipped.hud=id;saveGame();notify('HUD style applied!','gold');renderCustomize();}

// ═══════════════════════════════════════════════════
//  HELPERS
// ═══════════════════════════════════════════════════
function rrect(ctx,x,y,w,h,r){
  r=Math.min(r,w/2,h/2);
  ctx.beginPath();ctx.moveTo(x+r,y);
  ctx.lineTo(x+w-r,y);ctx.arcTo(x+w,y,x+w,y+r,r);
  ctx.lineTo(x+w,y+h-r);ctx.arcTo(x+w,y+h,x+w-r,y+h,r);
  ctx.lineTo(x+r,y+h);ctx.arcTo(x,y+h,x,y+h-r,r);
  ctx.lineTo(x,y+r);ctx.arcTo(x,y,x+r,y,r);
  ctx.closePath();
}

// ═══════════════════════════════════════════════════
//  INIT
// ═══════════════════════════════════════════════════
function init(){
  // Apply saved cosmetics
  const a=ACCENTS_MAP[G.equipped.accent]||ACCENTS_MAP['cyan-purple'];
  document.documentElement.style.setProperty('--acc1',a.c1);
  document.documentElement.style.setProperty('--acc2',a.c2);
  document.documentElement.style.setProperty('--cyan',a.c1);
  document.documentElement.style.setProperty('--purple',a.c2);
  const th=THEMES_MAP[G.equipped.theme]||THEMES_MAP.default;
  document.documentElement.style.setProperty('--bg',th.bg);
  document.body.style.background=th.bg;

  renderChars();updateHdrXP();
  if(G.activeChar){
    const ch=CHARS.find(c=>c.id===G.activeChar);
    if(ch){G.bonus={extraLife:false,memPeek:false,simonFast:false,dodgeMult:false,memLong:false,snakeSlow:false,typingTime:false,breakoutBig:false};ch.applyBonus();document.getElementById('hdr-char').textContent=ch.name}
  }
  document.getElementById('game-viewport').querySelector('#gw-default').style.display='block';
  addFeed('SYS','NEXUS ARCADE v2 online. '+G.totalXP+' XP loaded.');
}
init();
</script>
</body>
</html>
