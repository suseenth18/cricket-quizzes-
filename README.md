# cricket-quizzes-
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Cricket Fan Quiz</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600&family=Big+Shoulders+Display:wght@700;800;900&display=swap">
<style>[hidden]{display:none!important}img{max-width:100%}</style>
<style>
/* Layout: one screen at a time over a full-bleed cricket scene. Login, menu, solo, friends, room, quiz, result and scoreboard each get their own scene. Dark floodlit look only; the scenes are night and dusk pictures. Player accents come from IPL jersey colours. */
:root {
  --bg: #070d1a;
  --surface: rgba(12, 21, 40, 0.9);
  --surface-hover: rgba(21, 35, 64, 0.95);
  --line: #2b3f69;
  --fg: #eef3fb;
  --muted: #a9b6cf;
  --accent: #f0c14b;
  --accent-ink: #1d1600;
  --good: #3ccd7e;
  --good-bg: #0f3321;
  --bad: #f2605d;
  --bad-bg: #3b1618;
  --focus: #8dbcff;
  --font-display: "Big Shoulders Display", "Arial Narrow", Impact, sans-serif;
  --font-body: "Barlow", system-ui, -apple-system, "Segoe UI", sans-serif;
  color-scheme: dark;
}

* { box-sizing: border-box; }
html, body { min-height: 100%; }
body {
  margin: 0;
  background-color: var(--bg);
  color: var(--fg);
  font-family: var(--font-body);
  font-size: 17px;
  line-height: 1.5;
  padding-block: 28px 20px;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.wrap { width: 100%; max-width: 980px; margin: 0 auto; flex: 1; }
h1, h2, h3 { font-family: var(--font-display); text-wrap: balance; margin: 0; line-height: 1; }
button { font: inherit; color: inherit; }
:focus-visible { outline: 3px solid var(--focus); outline-offset: 3px; }

/* ---------- Scenes ---------- */
.bg { position: fixed; inset: 0; z-index: -1; background: var(--bg); overflow: hidden; }
.scene { position: absolute; inset: 0; opacity: 0; transition: opacity 0.5s ease; }
.scene.on { opacity: 1; }
.scene svg { width: 100%; height: 100%; display: block; }
.bg::after { content: ""; position: absolute; inset: 0; background: linear-gradient(180deg, rgba(5, 10, 22, 0.3) 0%, rgba(5, 10, 22, 0.72) 100%); }

.eyebrow { font-family: var(--font-display); font-weight: 700; letter-spacing: 0.18em; text-transform: uppercase; font-size: 0.95rem; color: var(--muted); margin: 0 0 12px; }
.home-head { text-align: center; margin-block: 8px 30px; }
.home-head h1 { font-weight: 900; font-size: clamp(2.6rem, 9vw, 5rem); text-transform: uppercase; letter-spacing: 0.01em; text-shadow: 0 2px 28px rgba(0, 0, 0, 0.65); }
.home-head p { color: var(--muted); margin: 14px auto 0; max-width: 52ch; }
.home-head p.eyebrow { margin: 0 0 12px; }

.btn {
  font-family: var(--font-display); font-weight: 800; font-size: 1.15rem;
  letter-spacing: 0.1em; text-transform: uppercase;
  padding: 13px 26px; border-radius: 10px; border: 0; cursor: pointer;
  background: var(--pc, var(--accent)); color: var(--ink, var(--accent-ink));
  transition: transform 0.15s, filter 0.15s;
}
.btn:hover:not(:disabled) { transform: translateY(-2px); filter: brightness(1.08); }
.btn:disabled { opacity: 0.55; cursor: default; }
.btn.ghost { background: rgba(12, 21, 40, 0.7); color: var(--fg); border: 1.5px solid var(--line); }
.btn.ghost:hover:not(:disabled) { border-color: var(--pc, var(--accent)); }
.res-actions { display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }
.res-actions.back { margin-top: 30px; }
.link { background: rgba(12, 21, 40, 0.7); border: 1.5px solid var(--line); border-radius: 999px; padding: 6px 14px; cursor: pointer; font-family: var(--font-display); font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; font-size: 0.95rem; transition: border-color 0.15s; }
.link:hover { border-color: var(--focus); }
.status { min-height: 1.4em; margin: 10px 0 0; color: var(--muted); font-size: 0.95rem; text-align: center; }
.status.err { color: var(--bad); }

/* ---------- Login ---------- */
#login { max-width: 460px; margin: 0 auto; }
.login-card, .panel { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 24px 20px; display: grid; gap: 18px; }
.field { display: grid; gap: 6px; }
.field label, .panel label { font-family: var(--font-display); font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; font-size: 0.95rem; color: var(--muted); }
.field input, .panel input { font: inherit; color: var(--fg); background: #060c1a; border: 1.5px solid var(--line); border-radius: 10px; padding: 12px 14px; min-height: 50px; width: 100%; min-width: 0; }
.field input:focus-visible, .panel input:focus-visible { border-color: var(--focus); }
.field.err input { border-color: var(--bad); }
.field .msg { font-size: 0.9rem; color: var(--bad); min-height: 1.2em; }
.mobile-row, .join-row { display: flex; gap: 8px; }
.mobile-row .cc { flex: none; display: grid; place-items: center; padding: 0 14px; border: 1.5px solid var(--line); border-radius: 10px; color: var(--muted); background: #060c1a; }
.join-row input { text-transform: uppercase; letter-spacing: 0.3em; font-family: var(--font-display); font-weight: 800; font-size: 1.4rem; text-align: center; }

/* ---------- Menu ---------- */
.userbar { display: flex; align-items: center; justify-content: space-between; gap: 10px 16px; flex-wrap: wrap; margin-bottom: 18px; color: var(--muted); font-size: 0.95rem; text-shadow: 0 1px 8px rgba(0, 0, 0, 0.6); }
.userbar b { color: var(--fg); overflow-wrap: anywhere; }
.ub-actions { display: flex; gap: 8px; flex-wrap: wrap; }
.modes { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 20px; max-width: 780px; margin: 0 auto; }
.mode { display: flex; flex-direction: column; align-items: flex-start; gap: 10px; text-align: left; padding: 26px 22px 28px; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; cursor: pointer; transition: transform 0.2s, box-shadow 0.2s, border-color 0.2s, background 0.2s; }
.mode:hover, .mode:focus-visible { transform: translateY(-6px); background: var(--surface-hover); border-color: var(--accent); box-shadow: 0 18px 44px -14px rgba(240, 193, 75, 0.45); }
.mode svg { width: 68px; height: 68px; }
.mode h2 { font-weight: 800; font-size: 2.2rem; text-transform: uppercase; }
.mode p { margin: 0; color: var(--muted); }

/* ---------- Player cards ---------- */
.cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 20px; }
.card { --pc: #888; --ink: #fff; display: flex; flex-direction: column; align-items: center; gap: 6px; padding: 22px 18px 24px; background: var(--surface); border: 1px solid var(--line); border-radius: 14px; cursor: pointer; text-align: center; transition: transform 0.2s ease, box-shadow 0.2s ease, background 0.2s ease, border-color 0.2s ease; }
.card:hover, .card:focus-visible { transform: translateY(-8px); background: var(--surface-hover); border-color: var(--pc); box-shadow: 0 0 0 1px var(--pc), 0 18px 44px -12px color-mix(in srgb, var(--pc) 60%, transparent); }
.card svg { width: min(190px, 70%); height: auto; display: block; }
.card .num { font-family: var(--font-display); font-weight: 800; font-size: 1rem; letter-spacing: 0.16em; text-transform: uppercase; color: var(--pc); margin-top: 6px; }
.card h2 { font-weight: 800; font-size: 2.1rem; text-transform: uppercase; }
.card .role { color: var(--muted); font-size: 0.95rem; }
.card .go { margin-top: 10px; font-family: var(--font-display); font-weight: 700; letter-spacing: 0.12em; text-transform: uppercase; font-size: 0.95rem; padding: 7px 16px; border-radius: 999px; border: 1px solid var(--line); transition: background 0.2s, color 0.2s, border-color 0.2s; }
.card:hover .go, .card:focus-visible .go { background: var(--pc); color: var(--ink); border-color: var(--pc); }

/* ---------- Friends and room ---------- */
#friends { max-width: 980px; margin: 0 auto; }
#friends .panel { max-width: 460px; margin: 0 auto; }
.sep { display: flex; align-items: center; gap: 14px; color: var(--muted); font-family: var(--font-display); letter-spacing: 0.16em; text-transform: uppercase; font-weight: 700; margin: 30px 0 22px; }
.sep::before, .sep::after { content: ""; flex: 1; height: 1px; background: var(--line); }
#room { max-width: 640px; margin: 0 auto; --pc: #888; --ink: #fff; }
.code { font-family: var(--font-display); font-weight: 900; font-size: clamp(3.4rem, 17vw, 6rem); letter-spacing: 0.16em; color: var(--accent); line-height: 1; margin: 4px 0 10px; text-shadow: 0 2px 28px rgba(0, 0, 0, 0.6); user-select: all; }
#room .home-head p { margin-top: 8px; }
.sub-h { font-weight: 800; font-size: 1.6rem; text-transform: uppercase; letter-spacing: 0.04em; margin: 30px 0 12px; text-align: center; text-shadow: 0 2px 16px rgba(0, 0, 0, 0.6); }

/* ---------- Quiz ---------- */
#quiz { --pc: #888; --ink: #fff; max-width: 720px; margin: 0 auto; }
.q-top { display: flex; align-items: center; justify-content: space-between; gap: 12px; flex-wrap: wrap; margin-bottom: 14px; }
.chip { display: inline-flex; align-items: center; gap: 10px; font-family: var(--font-display); font-weight: 800; font-size: 1.3rem; text-transform: uppercase; letter-spacing: 0.04em; }
.chip b { display: inline-grid; place-items: center; min-width: 2.2em; height: 2.2em; padding: 0 6px; background: var(--pc); color: var(--ink); border-radius: 8px; font-size: 0.95rem; letter-spacing: 0; }
.q-right { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; }
.tag { font-family: var(--font-display); font-weight: 800; letter-spacing: 0.14em; text-transform: uppercase; font-size: 0.95rem; padding: 4px 12px; border-radius: 999px; border: 1.5px solid var(--accent); color: var(--accent); }
.score { font-family: var(--font-display); font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; color: var(--muted); font-size: 1rem; font-variant-numeric: tabular-nums; }
.score strong { color: var(--fg); font-size: 1.3rem; }
.bar { height: 6px; background: var(--line); border-radius: 99px; overflow: hidden; margin-bottom: 26px; }
.bar i { display: block; height: 100%; width: 0; background: var(--pc); border-radius: 99px; transition: width 0.35s ease; }
.q-count { font-family: var(--font-display); font-weight: 700; letter-spacing: 0.16em; text-transform: uppercase; color: var(--pc); font-size: 1rem; margin: 0 0 8px; }
.question { font-family: var(--font-display); font-weight: 800; font-size: clamp(1.8rem, 6vw, 2.6rem); line-height: 1.08; margin-bottom: 24px; text-shadow: 0 2px 20px rgba(0, 0, 0, 0.6); }
.options { display: grid; gap: 12px; }
.opt { display: flex; align-items: center; gap: 14px; width: 100%; min-height: 56px; padding: 12px 16px; text-align: left; cursor: pointer; background: var(--surface); border: 1.5px solid var(--line); border-radius: 12px; transition: background 0.15s, border-color 0.15s, transform 0.15s; }
.opt .k { flex: none; display: grid; place-items: center; width: 32px; height: 32px; border-radius: 8px; border: 1px solid var(--line); font-family: var(--font-display); font-weight: 800; font-size: 1.05rem; color: var(--muted); }
.opt .t { min-width: 0; overflow-wrap: anywhere; }
.opt:not(:disabled):hover { border-color: var(--pc); background: var(--surface-hover); transform: translateX(3px); }
.opt:disabled { cursor: default; }
.opt.correct { background: var(--good-bg); border-color: var(--good); }
.opt.correct .k { background: var(--good); border-color: var(--good); color: #fff; }
.opt.wrong { background: var(--bad-bg); border-color: var(--bad); }
.opt.wrong .k { background: var(--bad); border-color: var(--bad); color: #fff; }
.opt.dim { opacity: 0.55; }
.feedback { min-height: 1.6em; margin: 16px 0 0; font-weight: 600; text-shadow: 0 1px 10px rgba(0, 0, 0, 0.7); }
.feedback.good { color: var(--good); }
.feedback.bad { color: #ff8582; }
.actions { margin-top: 12px; min-height: 52px; }

/* ---------- Result ---------- */
#result { --pc: #888; --ink: #fff; max-width: 640px; margin: 0 auto; text-align: center; padding-top: 10px; }
.big-score { font-family: var(--font-display); font-weight: 900; font-size: clamp(5.5rem, 26vw, 9rem); line-height: 0.9; color: var(--pc); font-variant-numeric: tabular-nums; text-shadow: 0 2px 30px rgba(0, 0, 0, 0.6); }
.big-score small { font-size: 0.4em; color: var(--muted); font-weight: 700; }
#result h2 { font-weight: 900; font-size: clamp(2.2rem, 8vw, 3.4rem); text-transform: uppercase; margin: 14px 0 10px; text-shadow: 0 2px 20px rgba(0, 0, 0, 0.6); }
#result p.sub { color: var(--muted); margin: 0 auto 18px; max-width: 42ch; }
#result p.note { color: var(--muted); font-size: 0.95rem; margin: 0 auto 22px; min-height: 1.4em; max-width: 46ch; }

/* ---------- Scoreboard lists ---------- */
#board { max-width: 640px; margin: 0 auto; }
.board { list-style: none; margin: 0 0 26px; padding: 0; display: grid; gap: 8px; }
.board .row { display: grid; grid-template-columns: 2.4rem minmax(0, 1fr) auto; align-items: center; gap: 14px; padding: 12px 16px; background: var(--surface); border: 1px solid var(--line); border-radius: 12px; }
.board .row.you { border-color: var(--accent); }
.board .rk { font-family: var(--font-display); font-weight: 900; font-size: 1.8rem; color: var(--muted); font-variant-numeric: tabular-nums; text-align: center; }
.board .top .rk { color: var(--accent); }
.board .nm { min-width: 0; display: grid; }
.board .nm b { font-weight: 600; overflow-wrap: anywhere; }
.board .nm small { color: var(--muted); }
.board .pts { font-family: var(--font-display); font-weight: 900; font-size: 2rem; font-variant-numeric: tabular-nums; line-height: 1; }
.board .pts small { font-size: 0.5em; color: var(--muted); font-weight: 700; letter-spacing: 0.1em; margin-left: 4px; text-transform: uppercase; }
.board .empty { padding: 28px 16px; text-align: center; color: var(--muted); background: var(--surface); border: 1px dashed var(--line); border-radius: 12px; }

footer { text-align: center; color: var(--muted); font-size: 0.9rem; padding-top: 30px; letter-spacing: 0.04em; text-shadow: 0 1px 8px rgba(0, 0, 0, 0.8); }

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { transition: none !important; animation: none !important; }
  .card:hover, .card:focus-visible, .btn:hover, .opt:not(:disabled):hover, .mode:hover, .mode:focus-visible { transform: none; }
}
</style>
</head>
<body>

<div class="bg" id="bg" aria-hidden="true"></div>

<div class="wrap">

  <!-- 1. Login -->
  <section id="login" aria-labelledby="login-title">
    <div class="home-head">
      <p class="eyebrow">Cricket Fan Quiz</p>
      <h1 id="login-title">Log in to play</h1>
      <p>Your name goes on the scoreboard. Your mobile number is never shown there.</p>
    </div>
    <form class="login-card" id="login-form" novalidate>
      <div class="field" id="f-name">
        <label for="l-name">Your name</label>
        <input id="l-name" type="text" maxlength="24" autocomplete="name" placeholder="e.g. Suresh Kumar">
        <span class="msg" id="m-name" role="alert"></span>
      </div>
      <div class="field" id="f-mobile">
        <label for="l-mobile">Mobile number</label>
        <div class="mobile-row">
          <span class="cc" aria-hidden="true">+91</span>
          <input id="l-mobile" type="tel" inputmode="numeric" maxlength="14" autocomplete="tel-national" placeholder="10-digit number">
        </div>
        <span class="msg" id="m-mobile" role="alert"></span>
      </div>
      <button class="btn" type="submit">Log In</button>
    </form>
  </section>

  <!-- 2. Menu -->
  <section id="menu" hidden aria-labelledby="menu-title">
    <div class="userbar">
      <span>Playing as <b id="u-name"></b></span>
      <span class="ub-actions">
        <button class="link" id="u-board" type="button">Scoreboard</button>
        <button class="link" id="u-out" type="button">Log out</button>
      </span>
    </div>
    <div class="home-head">
      <p class="eyebrow">Main Menu</p>
      <h1 id="menu-title">How do you want to play?</h1>
    </div>
    <div class="modes">
      <button class="mode" id="m-solo" type="button">
        <svg viewBox="0 0 64 64" aria-hidden="true"><rect x="27" y="4" width="12" height="38" rx="5" transform="rotate(28 33 23)" fill="#e6c88e"/><rect x="38" y="36" width="6" height="22" rx="3" transform="rotate(28 41 47)" fill="#6b4a2b"/><circle cx="14" cy="48" r="9" fill="#c8242b"/><path d="M8 44 Q14 48 8 52 M20 44 Q14 48 20 52" stroke="#f3e6c0" stroke-width="1.5" fill="none"/></svg>
        <h2>Solo Play</h2>
        <p>Pick a player and take a 10-question quiz on your own.</p>
      </button>
      <button class="mode" id="m-friends" type="button">
        <svg viewBox="0 0 64 64" aria-hidden="true"><circle cx="20" cy="22" r="9" fill="#2e7be8"/><path d="M4 54 Q4 36 20 36 Q36 36 36 54Z" fill="#2e7be8"/><circle cx="44" cy="22" r="9" fill="#f9c80e"/><path d="M28 54 Q28 36 44 36 Q60 36 60 54Z" fill="#f9c80e" opacity=".92"/></svg>
        <h2>Play with Friends</h2>
        <p>Open a room, share the code, and race your friends on the same 10 questions.</p>
      </button>
    </div>
  </section>

  <!-- 3. Solo: pick a player -->
  <section id="solo" hidden aria-labelledby="solo-title">
    <div class="home-head">
      <p class="eyebrow">Solo Play</p>
      <h1 id="solo-title">Who is your favorite player?</h1>
      <p>Pick one. You get 10 questions about him, and they change every time you play.</p>
    </div>
    <div class="cards" id="cards-solo"></div>
    <div class="res-actions back"><button class="btn ghost" id="solo-back" type="button">Back to Menu</button></div>
  </section>

  <!-- 4. Friends hub -->
  <section id="friends" hidden aria-labelledby="fr-title">
    <div class="home-head">
      <p class="eyebrow">Play with Friends</p>
      <h1 id="fr-title">Race your friends</h1>
      <p>Everyone in a room answers the same 10 questions, once. The room leaderboard updates live.</p>
    </div>
    <p class="status" id="fr-note" role="status"></p>
    <form class="panel" id="join-form" novalidate>
      <label for="j-code">Have a room code?</label>
      <div class="join-row">
        <input id="j-code" type="text" maxlength="5" autocapitalize="characters" autocomplete="off" spellcheck="false" placeholder="K7M2Q">
        <button class="btn" id="j-btn" type="submit">Join</button>
      </div>
      <span class="status err" id="j-msg" role="alert"></span>
    </form>
    <div class="sep"><span>or open a new room</span></div>
    <div class="cards" id="cards-room"></div>
    <p class="status" id="fr-msg" role="status"></p>
    <div class="res-actions back"><button class="btn ghost" id="fr-back" type="button">Back to Menu</button></div>
  </section>

  <!-- 5. Room lobby -->
  <section id="room" hidden aria-labelledby="rm-title">
    <div class="home-head">
      <p class="eyebrow" id="rm-title">Room code</p>
      <div class="code" id="rm-code"></div>
      <p id="rm-quiz"></p>
      <p>Share this code. Friends enter it under Play with Friends.</p>
    </div>
    <div class="res-actions">
      <button class="btn ghost" id="rm-copy" type="button">Copy Code</button>
      <button class="btn" id="rm-play" type="button">Play Now</button>
    </div>
    <h2 class="sub-h">Room leaderboard</h2>
    <ol class="board" id="rm-list"></ol>
    <div class="res-actions back"><button class="btn ghost" id="rm-leave" type="button">Leave Room</button></div>
  </section>

  <!-- 6. Quiz -->
  <section id="quiz" hidden aria-live="polite">
    <div class="q-top">
      <div class="chip"><b id="q-num"></b><span id="q-name"></span></div>
      <div class="q-right">
        <span class="tag" id="q-room" hidden></span>
        <div class="score">Score <strong id="q-score">0</strong></div>
        <button class="link" id="q-quit" type="button">Quit</button>
      </div>
    </div>
    <div class="bar" aria-hidden="true"><i id="q-bar"></i></div>
    <p class="q-count" id="q-count"></p>
    <h2 class="question" id="q-text"></h2>
    <div class="options" id="q-options"></div>
    <p class="feedback" id="q-feedback" role="status"></p>
    <div class="actions"><button class="btn" id="q-next" type="button" hidden>Next Question</button></div>
  </section>

  <!-- 7. Result -->
  <section id="result" hidden>
    <p class="eyebrow" id="r-who"></p>
    <div class="big-score"><span id="r-score"></span><small id="r-total"></small></div>
    <h2 id="r-title"></h2>
    <p class="sub" id="r-sub"></p>
    <p class="note" id="r-save" role="status"></p>
    <div class="res-actions">
      <button class="btn" id="r-choose" type="button">Choose Another Player</button>
      <button class="btn" id="r-room" type="button" hidden>Room Leaderboard</button>
      <button class="btn ghost" id="r-again" type="button">Play Again</button>
      <button class="btn ghost" id="r-board" type="button">Scoreboard</button>
      <button class="btn ghost" id="r-menu" type="button">Main Menu</button>
    </div>
  </section>

  <!-- 8. Scoreboard -->
  <section id="board" hidden aria-labelledby="board-title">
    <div class="home-head">
      <p class="eyebrow">Scoreboard</p>
      <h1 id="board-title">Top Fans</h1>
      <p>Best score per player. 10 points for every right answer.</p>
    </div>
    <ol class="board" id="b-list"></ol>
    <div class="res-actions"><button class="btn" id="b-back" type="button">Back</button></div>
  </section>

</div>
<footer>Made by Suseenth</footer>

<script>
(function () {
  var QUIZ_LEN = 10;
  var POINTS_EACH = 10;

  // Each question lists the correct answer FIRST; options are shuffled when shown.
  // Every player has a pool of 16. A round draws 10 of them.
  var PLAYERS = [
    {
      id: "kohli", name: "Virat Kohli", surname: "KOHLI", num: 18,
      role: "Right-hand batter", color: "#E23B3B", ink: "#ffffff",
      questions: [
        { q: "Which IPL franchise has Virat Kohli played for throughout his IPL career?", a: ["Royal Challengers Bengaluru", "Mumbai Indians", "Delhi Capitals", "Kolkata Knight Riders"] },
        { q: "Kohli captained India to which Under-19 World Cup title?", a: ["2008", "2006", "2010", "2012"] },
        { q: "Which city is Virat Kohli from?", a: ["Delhi", "Mumbai", "Chennai", "Bengaluru"] },
        { q: "In the 2023 World Cup, Kohli broke whose record for most ODI centuries?", a: ["Sachin Tendulkar", "Ricky Ponting", "Sourav Ganguly", "Kumar Sangakkara"] },
        { q: "How many runs did Kohli score in IPL 2016, a record for a single season?", a: ["973", "865", "917", "1,024"] },
        { q: "Which jersey number does Kohli wear for India?", a: ["18", "7", "45", "10"] },
        { q: "Against which team did Kohli make his Test debut in 2011?", a: ["West Indies", "Australia", "England", "Sri Lanka"] },
        { q: "Which stadium is the home ground of Royal Challengers Bengaluru?", a: ["M. Chinnaswamy Stadium", "Wankhede Stadium", "Eden Gardens", "Narendra Modi Stadium"] },
        { q: "Kohli's 50th ODI century came in the 2023 World Cup semi-final against which team?", a: ["New Zealand", "Australia", "South Africa", "Pakistan"] },
        { q: "Kohli's unbeaten 82 at the MCG in the 2022 T20 World Cup came against which team?", a: ["Pakistan", "Australia", "South Africa", "Bangladesh"] },
        { q: "How many double centuries has Kohli scored in Test cricket?", a: ["7", "5", "6", "9"] },
        { q: "In which year did Royal Challengers Bengaluru win their first IPL title?", a: ["2025", "2016", "2009", "2024"] },
        { q: "Against which team did Kohli make his ODI debut in 2008?", a: ["Sri Lanka", "Pakistan", "Australia", "New Zealand"] },
        { q: "Kohli was Player of the Match in the 2024 T20 World Cup final against which team?", a: ["South Africa", "Australia", "England", "Afghanistan"] },
        { q: "How many runs did Kohli score at the 2023 World Cup, the most in a single edition?", a: ["765", "673", "711", "824"] },
        { q: "Kohli's highest Test score is an unbeaten 254. Which team was India playing in Pune in 2019?", a: ["South Africa", "England", "Bangladesh", "New Zealand"] }
      ]
    },
    {
      id: "dhoni", name: "MS Dhoni", surname: "DHONI", num: 7,
      role: "Wicketkeeper-batter", color: "#F9C80E", ink: "#1a1500",
      questions: [
        { q: "What is MS Dhoni's famous nickname?", a: ["Captain Cool", "Hitman", "Chase Master", "The Wall"] },
        { q: "At which ground did India win the 2011 World Cup final under Dhoni?", a: ["Wankhede Stadium, Mumbai", "Eden Gardens, Kolkata", "M. A. Chidambaram Stadium, Chennai", "Arun Jaitley Stadium, Delhi"] },
        { q: "Which ICC trophy did India win under Dhoni in 2013?", a: ["Champions Trophy", "T20 World Cup", "World Test Championship", "ODI World Cup"] },
        { q: "Which IPL team does Dhoni play for?", a: ["Chennai Super Kings", "Rajasthan Royals", "Punjab Kings", "Sunrisers Hyderabad"] },
        { q: "How many IPL titles has Dhoni won as Chennai Super Kings captain?", a: ["5", "3", "4", "6"] },
        { q: "Which Indian city is Dhoni from?", a: ["Ranchi", "Pune", "Jaipur", "Kanpur"] },
        { q: "Before cricket, Dhoni worked as a ticket collector at which railway station?", a: ["Kharagpur", "Howrah", "Nagpur", "Ranchi"] },
        { q: "In which year did Dhoni retire from international cricket?", a: ["2020", "2017", "2019", "2022"] },
        { q: "Which tournament did Dhoni captain India to win in 2007?", a: ["The first T20 World Cup", "The ODI World Cup", "The Champions Trophy", "The Asia Cup"] },
        { q: "Dhoni scored an unbeaten 183 in 2005. Which team was India playing?", a: ["Sri Lanka", "Pakistan", "Australia", "England"] },
        { q: "India first became the No. 1 ranked Test team in which year under Dhoni?", a: ["2009", "2007", "2011", "2013"] },
        { q: "Which sporting honour did Dhoni receive in 2007?", a: ["Rajiv Gandhi Khel Ratna", "Arjuna Award", "Dronacharya Award", "Dhyan Chand Award"] },
        { q: "Dhoni holds an honorary rank in which part of the Indian Army?", a: ["Territorial Army", "Gorkha Rifles", "Madras Regiment", "Sikh Regiment"] },
        { q: "Against which team did Dhoni make his ODI debut in 2004?", a: ["Bangladesh", "Sri Lanka", "Pakistan", "Zimbabwe"] },
        { q: "India beat which team in the 2011 World Cup final?", a: ["Sri Lanka", "Pakistan", "Australia", "South Africa"] },
        { q: "Which jersey number does Dhoni wear?", a: ["7", "18", "45", "10"] }
      ]
    },
    {
      id: "rohit", name: "Rohit Sharma", surname: "SHARMA", num: 45,
      role: "Right-hand opening batter", color: "#2E7BE8", ink: "#ffffff",
      questions: [
        { q: "What is Rohit Sharma's famous nickname?", a: ["Hitman", "Captain Cool", "King", "Master Blaster"] },
        { q: "What is Rohit's highest ODI score, made against Sri Lanka at Eden Gardens in 2014?", a: ["264", "208", "237", "219"] },
        { q: "How many double centuries has Rohit scored in ODIs?", a: ["3", "1", "2", "4"] },
        { q: "Rohit captained which IPL team to five titles?", a: ["Mumbai Indians", "Chennai Super Kings", "Deccan Chargers", "Gujarat Titans"] },
        { q: "How many centuries did Rohit score at the 2019 World Cup, a record for one tournament?", a: ["5", "3", "4", "6"] },
        { q: "In which year did Rohit captain India to the T20 World Cup title?", a: ["2024", "2022", "2021", "2023"] },
        { q: "Which jersey number does Rohit wear for India?", a: ["45", "18", "7", "264"] },
        { q: "Which city did Rohit grow up in?", a: ["Mumbai", "Chennai", "Hyderabad", "Kolkata"] },
        { q: "Rohit's first ODI double century, 209 in 2013, came against which team?", a: ["Australia", "Sri Lanka", "England", "New Zealand"] },
        { q: "Rohit scored a century on his Test debut in 2013 against which team?", a: ["West Indies", "Australia", "England", "South Africa"] },
        { q: "Rohit made 212 in his first Test as an opener at Ranchi in 2019. Which team was India playing?", a: ["South Africa", "Australia", "England", "Bangladesh"] },
        { q: "India won the 2025 Champions Trophy final under Rohit by beating which team?", a: ["New Zealand", "Australia", "South Africa", "Pakistan"] },
        { q: "In which country was the 2024 T20 World Cup final played?", a: ["Barbados", "India", "Australia", "England"] },
        { q: "Rohit won his first IPL title in 2009 with which team?", a: ["Deccan Chargers", "Mumbai Indians", "Chennai Super Kings", "Kolkata Knight Riders"] },
        { q: "India lost the 2023 World Cup final under Rohit to which team?", a: ["Australia", "New Zealand", "England", "South Africa"] },
        { q: "Against which team did Rohit make his ODI debut in 2007?", a: ["Ireland", "Bangladesh", "Pakistan", "Sri Lanka"] }
      ]
    }
  ];

  var $ = function (id) { return document.getElementById(id); };
  var SECTIONS = ["login", "menu", "solo", "friends", "room", "quiz", "result", "board"];
  var screens = {};
  SECTIONS.forEach(function (n) { screens[n] = $(n); });

  /* ---------- Cricket scenes (one per page) ---------- */
  function R(seed) { var a = seed; return function () { a = (a * 16807) % 2147483647; return (a - 1) / 2147483646; }; }

  function sceneLogin() { // floodlit stadium at night
    var r = R(11), s = "", i, x, y, row;
    s += '<defs><linearGradient id="lg-sky" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#040815"/><stop offset=".6" stop-color="#0d2144"/><stop offset="1" stop-color="#1a4a78"/></linearGradient>' +
      '<linearGradient id="lg-beam" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#fff3c4" stop-opacity=".55"/><stop offset="1" stop-color="#fff3c4" stop-opacity="0"/></linearGradient>' +
      '<radialGradient id="lg-glow"><stop offset="0" stop-color="#fff7d6" stop-opacity=".9"/><stop offset="1" stop-color="#fff7d6" stop-opacity="0"/></radialGradient></defs>';
    s += '<rect width="1200" height="800" fill="url(#lg-sky)"/>';
    for (i = 0; i < 70; i++) s += '<circle cx="' + (r() * 1200 | 0) + '" cy="' + (r() * 330 | 0) + '" r="' + (0.6 + r() * 1.2).toFixed(1) + '" fill="#cfe0ff" opacity="' + (0.3 + r() * 0.6).toFixed(2) + '"/>';
    [130, 400, 800, 1070].forEach(function (tx) {
      var ex = 600 + (tx - 600) * 0.35, a, b;
      s += '<path d="M' + (tx - 40) + ' 236 L' + (tx + 40) + ' 236 L' + (ex + 260) + ' 740 L' + (ex - 260) + ' 740Z" fill="url(#lg-beam)"/>';
      s += '<rect x="' + (tx - 5) + '" y="240" width="10" height="290" fill="#0a1224"/>';
      s += '<rect x="' + (tx - 48) + '" y="196" width="96" height="48" rx="4" fill="#0e1830"/>';
      for (a = 0; a < 3; a++) for (b = 0; b < 2; b++) s += '<circle cx="' + (tx - 30 + a * 30) + '" cy="' + (210 + b * 22) + '" r="8" fill="#fffbe6"/>';
      s += '<circle cx="' + tx + '" cy="220" r="120" fill="url(#lg-glow)" opacity=".55"/>';
    });
    s += '<path d="M0 480 Q600 440 1200 480 L1200 800 L0 800Z" fill="#0a1428"/>';
    for (row = 0; row < 6; row++) {
      y = 510 + row * 36;
      for (x = -8; x < 1220; x += 15 + r() * 6) {
        s += '<circle cx="' + x.toFixed(0) + '" cy="' + (y + r() * 8).toFixed(0) + '" r="5.5" fill="' + (row % 2 ? "#182a4a" : "#12213c") + '"/>';
        if (r() < 0.05) s += '<circle cx="' + x.toFixed(0) + '" cy="' + (y - 6).toFixed(0) + '" r="1.8" fill="#ffe9a8"/>';
      }
    }
    s += '<path d="M0 745 L1200 720 L1200 800 L0 800Z" fill="#15582f"/>';
    return s;
  }

  function sceneMenu() { // the ground from above: mown stripes, pitch, creases, stumps, ball
    var s = "", i;
    for (i = 0; i < 20; i++) s += '<rect x="' + (i * 60) + '" y="0" width="60" height="800" fill="' + (i % 2 ? "#1f7c40" : "#196d36") + '"/>';
    s += '<ellipse cx="600" cy="400" rx="580" ry="380" fill="none" stroke="#fff" stroke-opacity=".55" stroke-width="5"/>';
    s += '<ellipse cx="600" cy="400" rx="340" ry="225" fill="none" stroke="#fff" stroke-opacity=".35" stroke-width="3" stroke-dasharray="14 12"/>';
    s += '<rect x="548" y="150" width="104" height="500" fill="#c9a874"/>';
    [[225, 150, 175], [575, 625, 650]].forEach(function (c) {
      s += '<line x1="500" y1="' + c[0] + '" x2="700" y2="' + c[0] + '" stroke="#fff" stroke-width="3" opacity=".9"/>';
      s += '<line x1="540" y1="' + c[1] + '" x2="660" y2="' + c[1] + '" stroke="#fff" stroke-width="3" opacity=".9"/>';
      s += '<line x1="565" y1="' + c[1] + '" x2="565" y2="' + c[0] + '" stroke="#fff" stroke-width="2.5" opacity=".8"/><line x1="635" y1="' + c[1] + '" x2="635" y2="' + c[0] + '" stroke="#fff" stroke-width="2.5" opacity=".8"/>';
    });
    [172, 628].forEach(function (y) { [588, 600, 612].forEach(function (x) { s += '<circle cx="' + x + '" cy="' + y + '" r="4.5" fill="#f3e6c0"/>'; }); });
    s += '<circle cx="930" cy="580" r="26" fill="#b5202b"/><path d="M906 566 Q930 580 954 566 M906 594 Q930 580 954 594" stroke="#f3e6c0" stroke-width="2" fill="none" stroke-dasharray="3 3"/>';
    return s;
  }

  function sceneSolo() { // stands at dusk
    var r = R(23), s = "", i, row, x, t, y, yy;
    s += '<defs><linearGradient id="ls-sky" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#1b0f3d"/><stop offset=".5" stop-color="#7d3668"/><stop offset=".85" stop-color="#e8885a"/><stop offset="1" stop-color="#f6c27c"/></linearGradient>' +
      '<radialGradient id="ls-sun"><stop offset="0" stop-color="#ffd9a0" stop-opacity=".9"/><stop offset="1" stop-color="#ffd9a0" stop-opacity="0"/></radialGradient></defs>';
    s += '<rect width="1200" height="800" fill="url(#ls-sky)"/>';
    s += '<circle cx="880" cy="430" r="230" fill="url(#ls-sun)"/><circle cx="880" cy="430" r="64" fill="#ffe3b0"/>';
    [[300, 150, 240], [760, 230, 300], [500, 330, 200], [1000, 300, 180]].forEach(function (c) { s += '<ellipse cx="' + c[0] + '" cy="' + c[1] + '" rx="' + c[2] + '" ry="12" fill="#f3a074" opacity=".28"/>'; });
    [150, 1060].forEach(function (tx) {
      s += '<rect x="' + (tx - 5) + '" y="260" width="10" height="280" fill="#150a26"/><rect x="' + (tx - 46) + '" y="226" width="92" height="44" rx="4" fill="#150a26"/>';
      for (i = 0; i < 3; i++) s += '<circle cx="' + (tx - 28 + i * 28) + '" cy="248" r="7" fill="#ffd9a0" opacity=".8"/>';
    });
    var tiers = ["#2b1745", "#241339", "#1d0f2f", "#170c27", "#110820"];
    var dots = ["#46296b", "#3a2259", "#2f1b49", "#261539", "#1d102d"];
    for (row = 0; row < 5; row++) {
      y = 470 + row * 60;
      s += '<path d="M0 ' + (y + 10) + ' Q600 ' + (y - 30) + ' 1200 ' + (y + 10) + ' L1200 800 L0 800Z" fill="' + tiers[row] + '"/>';
      for (x = -6; x < 1210; x += 13 + r() * 5) {
        t = x / 1200; yy = y + 10 - 80 * t * (1 - t) + 16 + r() * 6;
        s += '<circle cx="' + x.toFixed(0) + '" cy="' + yy.toFixed(0) + '" r="5" fill="' + dots[row] + '"/>';
        if (r() < 0.04) s += '<circle cx="' + x.toFixed(0) + '" cy="' + (yy - 7).toFixed(0) + '" r="1.7" fill="#ffd9a0"/>';
      }
    }
    return s;
  }

  function sceneFriends() { // match day: bunting, ad boards, outfield
    var r = R(31), s = "", i, x;
    function cloud(cx, cy, k) { return '<g opacity=".9" fill="#fff"><circle cx="' + cx + '" cy="' + cy + '" r="' + 34 * k + '"/><circle cx="' + (cx + 38 * k) + '" cy="' + (cy + 6 * k) + '" r="' + 28 * k + '"/><circle cx="' + (cx - 38 * k) + '" cy="' + (cy + 8 * k) + '" r="' + 26 * k + '"/><rect x="' + (cx - 64 * k) + '" y="' + cy + '" width="' + 128 * k + '" height="' + 34 * k + '" rx="' + 17 * k + '"/></g>'; }
    s += '<defs><linearGradient id="lf-sky" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#2f88d6"/><stop offset="1" stop-color="#bfe8ff"/></linearGradient>' +
      '<linearGradient id="lf-grass" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#2f9a4c"/><stop offset="1" stop-color="#175f2e"/></linearGradient></defs>';
    s += '<rect width="1200" height="800" fill="url(#lf-sky)"/>';
    s += '<circle cx="1010" cy="170" r="110" fill="#fff6c8" opacity=".35"/><circle cx="1010" cy="170" r="56" fill="#fff6c8" opacity=".95"/>';
    s += cloud(220, 210, 1.3) + cloud(640, 150, 1) + cloud(860, 300, 1.1);
    s += '<rect x="0" y="400" width="1200" height="80" fill="#27446c"/>';
    var tc = ["#e23b3b", "#f9c80e", "#2e7be8", "#ffffff", "#2fb26b"];
    for (x = 4; x < 1200; x += 10) s += '<circle cx="' + x + '" cy="' + (415 + (r() * 52 | 0)) + '" r="4" fill="' + tc[(r() * 5) | 0] + '" opacity=".85"/>';
    for (i = 0; i < 12; i++) s += '<rect x="' + (i * 100) + '" y="470" width="100" height="38" fill="' + ["#e23b3b", "#f9c80e", "#2e7be8", "#fff"][i % 4] + '"/>';
    s += '<rect x="0" y="508" width="1200" height="292" fill="url(#lf-grass)"/>';
    for (i = 0; i < 6; i++) s += '<rect x="0" y="' + (520 + i * 48) + '" width="1200" height="24" fill="#fff" opacity=".05"/>';
    function f(px) { return 44 + 46 * Math.sin(Math.PI * (px % 600) / 600); }
    var pts = "";
    for (x = 0; x <= 1200; x += 12) pts += x + "," + f(x).toFixed(1) + " ";
    s += '<polyline points="' + pts + '" fill="none" stroke="#f4f1e6" stroke-width="3" opacity=".9"/>';
    for (x = 24; x < 1200; x += 48) s += '<polygon points="' + (x - 15) + ',' + f(x - 15).toFixed(1) + ' ' + (x + 15) + ',' + f(x + 15).toFixed(1) + ' ' + x + ',' + (f(x) + 38).toFixed(1) + '" fill="' + tc[((x - 24) / 48) % 5] + '"/>';
    [160, 182, 204].forEach(function (sx) { s += '<rect x="' + sx + '" y="640" width="7" height="120" rx="2" fill="#f3e6c0"/>'; });
    s += '<rect x="163" y="636" width="22" height="5" rx="2" fill="#d9c48e"/><rect x="185" y="636" width="22" height="5" rx="2" fill="#d9c48e"/>';
    s += '<circle cx="330" cy="760" r="20" fill="#b5202b"/><path d="M314 750 Q330 760 346 750" stroke="#f3e6c0" stroke-width="2" fill="none"/>';
    return s;
  }

  function sceneRoom() { // practice nets
    var s = "", x;
    s += '<defs><linearGradient id="lr-bg" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#0b2024"/><stop offset="1" stop-color="#123d31"/></linearGradient>' +
      '<radialGradient id="lr-lamp" cx=".5" cy="0" r=".8"><stop offset="0" stop-color="#fff3c4" stop-opacity=".4"/><stop offset="1" stop-color="#fff3c4" stop-opacity="0"/></radialGradient></defs>';
    s += '<rect width="1200" height="800" fill="url(#lr-bg)"/><rect width="1200" height="800" fill="url(#lr-lamp)"/>';
    for (x = -800; x < 1200; x += 44) s += '<line x1="' + x + '" y1="60" x2="' + (x + 700) + '" y2="760" stroke="#cfe6dc" stroke-opacity=".14" stroke-width="1.5"/><line x1="' + (x + 700) + '" y1="60" x2="' + x + '" y2="760" stroke="#cfe6dc" stroke-opacity=".14" stroke-width="1.5"/>';
    s += '<line x1="40" y1="60" x2="1160" y2="60" stroke="#e8e2c8" stroke-width="7" opacity=".6"/>';
    s += '<rect x="64" y="56" width="14" height="680" fill="#d9d3b8" opacity=".85"/><rect x="1122" y="56" width="14" height="680" fill="#d9d3b8" opacity=".85"/>';
    s += '<rect x="0" y="700" width="1200" height="100" fill="#1d7a3d"/>';
    s += '<polygon points="320,700 880,700 1000,800 200,800" fill="#34905a"/><line x1="260" y1="760" x2="940" y2="760" stroke="#fff" stroke-width="3" opacity=".8"/>';
    [583, 600, 617].forEach(function (sx) { s += '<rect x="' + (sx - 4) + '" y="640" width="8" height="104" rx="2" fill="#f3e6c0"/>'; });
    s += '<rect x="580" y="636" width="22" height="5" rx="2" fill="#d9c48e"/><rect x="598" y="636" width="22" height="5" rx="2" fill="#d9c48e"/>';
    s += '<circle cx="840" cy="755" r="20" fill="#b5202b"/><path d="M824 745 Q840 755 856 745" stroke="#f3e6c0" stroke-width="2" fill="none"/>';
    return s;
  }

  function sceneQuiz() { // the view from the crease: down the pitch to the far stumps
    var r = R(59), s = "", i, x, y, h;
    function xl(py) { return 528 - (py - 300) * 0.676; }
    function xr(py) { return 672 + (py - 300) * 0.676; }
    s += '<defs><linearGradient id="lq-sky" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#0b1630"/><stop offset="1" stop-color="#2a5283"/></linearGradient>' +
      '<linearGradient id="lq-grass" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#1d6b39"/><stop offset="1" stop-color="#0e4424"/></linearGradient>' +
      '<linearGradient id="lq-pitch" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#a98d5f"/><stop offset="1" stop-color="#d8bb89"/></linearGradient></defs>';
    s += '<rect width="1200" height="300" fill="url(#lq-sky)"/>';
    s += '<rect x="0" y="246" width="1200" height="60" fill="#0d1a33"/>';
    for (x = 0; x < 1200; x += 11) s += '<circle cx="' + x + '" cy="' + (254 + (r() * 44 | 0)) + '" r="3" fill="' + ["#243a63", "#31507f", "#243a63", "#ffe9a8"][(r() * 4) | 0] + '"/>';
    s += '<rect x="0" y="300" width="1200" height="500" fill="url(#lq-grass)"/>';
    y = 300;
    for (i = 0; i < 9; i++) { h = 8 + i * 7; if (i % 2) s += '<rect x="0" y="' + y + '" width="1200" height="' + h + '" fill="#fff" opacity=".05"/>'; y += h; }
    s += '<polygon points="528,300 672,300 1010,800 190,800" fill="url(#lq-pitch)"/>';
    s += '<line x1="' + xl(340).toFixed(0) + '" y1="340" x2="' + xr(340).toFixed(0) + '" y2="340" stroke="#fff" stroke-width="3" opacity=".85"/>';
    s += '<line x1="' + xl(650).toFixed(0) + '" y1="650" x2="' + xr(650).toFixed(0) + '" y2="650" stroke="#fff" stroke-width="7" opacity=".85"/>';
    [590, 600, 610].forEach(function (sx) { s += '<rect x="' + (sx - 2) + '" y="292" width="4" height="46" fill="#f3e6c0"/>'; });
    s += '<rect x="589" y="290" width="11" height="3" fill="#d9c48e"/><rect x="600" y="290" width="11" height="3" fill="#d9c48e"/>';
    return s;
  }

  function sceneResult() { // fireworks, confetti and a trophy
    var r = R(47), s = "", i, j, ang, cx, cy;
    var cols = ["#f9c80e", "#e23b3b", "#2e7be8", "#3ccd7e", "#ffffff", "#ff8fb1"];
    s += '<defs><linearGradient id="lz-sky" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#060a1c"/><stop offset="1" stop-color="#1e1548"/></linearGradient>' +
      '<linearGradient id="lz-gold" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stop-color="#ffe08a"/><stop offset="1" stop-color="#c08a1e"/></linearGradient></defs>';
    s += '<rect width="1200" height="800" fill="url(#lz-sky)"/>';
    s += '<g opacity=".65">';
    [[150, 210, "#f9c80e"], [1050, 200, "#e23b3b"], [260, 440, "#2e7be8"], [950, 470, "#3ccd7e"], [450, 600, "#ff8fb1"], [760, 620, "#f9c80e"]].forEach(function (b) {
      for (j = 0; j < 22; j++) {
        ang = j / 22 * Math.PI * 2; var l2 = 62 + r() * 40;
        cx = b[0] + Math.cos(ang) * l2; cy = b[1] + Math.sin(ang) * l2;
        s += '<line x1="' + (b[0] + Math.cos(ang) * 22).toFixed(0) + '" y1="' + (b[1] + Math.sin(ang) * 22).toFixed(0) + '" x2="' + cx.toFixed(0) + '" y2="' + cy.toFixed(0) + '" stroke="' + b[2] + '" stroke-width="3" stroke-linecap="round" opacity=".85"/>';
        s += '<circle cx="' + cx.toFixed(0) + '" cy="' + cy.toFixed(0) + '" r="3" fill="' + b[2] + '"/>';
      }
    });
    s += '</g>';
    for (i = 0; i < 80; i++) s += '<rect x="' + (r() * 1200 | 0) + '" y="' + (r() * 800 | 0) + '" width="8" height="14" fill="' + cols[(r() * 6) | 0] + '" opacity=".7" transform="rotate(' + (r() * 180 | 0) + ' ' + (r() * 1200 | 0) + ' ' + (r() * 800 | 0) + ')"/>';
    s += '<g transform="translate(600 640)" opacity=".55"><path d="M-90 -140 H90 C90 -40 50 10 0 20 C-50 10 -90 -40 -90 -140Z" fill="url(#lz-gold)"/>' +
      '<path d="M-90 -120 C-150 -120 -150 -40 -80 -30" fill="none" stroke="#e5b53b" stroke-width="14"/><path d="M90 -120 C150 -120 150 -40 80 -30" fill="none" stroke="#e5b53b" stroke-width="14"/>' +
      '<rect x="-14" y="20" width="28" height="70" fill="url(#lz-gold)"/><rect x="-70" y="90" width="140" height="26" rx="6" fill="url(#lz-gold)"/></g>';
    return s;
  }

  function sceneBoard() { // a stadium scoreboard of amber LEDs
    var s = "";
    s += '<defs><pattern id="lb-led" width="16" height="16" patternUnits="userSpaceOnUse"><circle cx="8" cy="8" r="2.4" fill="#ffb23a" opacity=".2"/></pattern></defs>';
    s += '<rect width="1200" height="800" fill="#050a07"/><rect width="1200" height="800" fill="url(#lb-led)"/>';
    s += '<text x="70" y="360" font-size="300" font-weight="900" fill="#ffb23a" opacity=".09" font-family="Big Shoulders Display, Impact, sans-serif">186/4</text>';
    s += '<text x="460" y="700" font-size="300" font-weight="900" fill="#ffb23a" opacity=".09" font-family="Big Shoulders Display, Impact, sans-serif">19.2</text>';
    s += '<rect x="40" y="40" width="1120" height="720" rx="18" fill="none" stroke="#3a4a3e" stroke-width="6"/>';
    s += '<rect x="40" y="40" width="1120" height="56" fill="#0d1a12" opacity=".8"/>';
    return s;
  }

  var scenes = { login: sceneLogin, menu: sceneMenu, solo: sceneSolo, friends: sceneFriends, room: sceneRoom, quiz: sceneQuiz, result: sceneResult, board: sceneBoard };
  var sceneEls = {};
  function buildScenes() {
    var bg = $("bg");
    Object.keys(scenes).forEach(function (k) {
      var d = document.createElement("div");
      d.className = "scene";
      d.innerHTML = '<svg viewBox="0 0 1200 800" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">' + scenes[k]() + '</svg>';
      bg.appendChild(d);
      sceneEls[k] = d;
    });
  }
  function show(name) {
    SECTIONS.forEach(function (n) { screens[n].hidden = (n !== name); });
    Object.keys(sceneEls).forEach(function (k) { sceneEls[k].classList.toggle("on", k === name); });
    window.scrollTo(0, 0);
  }

  /* ---------- Helpers ---------- */
  function shuffle(arr) {
    var a = arr.slice();
    for (var n = a.length - 1; n > 0; n--) {
      var j = Math.floor(Math.random() * (n + 1));
      var t = a[n]; a[n] = a[j]; a[j] = t;
    }
    return a;
  }
  function findPlayer(id) { for (var i = 0; i < PLAYERS.length; i++) if (PLAYERS[i].id === id) return PLAYERS[i]; return null; }
  function setAccent(el, p) { el.style.setProperty("--pc", p.color); el.style.setProperty("--ink", p.ink); }

  // Draw QUIZ_LEN questions, using ones the fan has not seen before repeating any.
  function seenGet(id) { try { return JSON.parse(localStorage.getItem("cq-seen-" + id) || "[]"); } catch (e) { return []; } }
  function seenSet(id, arr) { try { localStorage.setItem("cq-seen-" + id, JSON.stringify(arr)); } catch (e) {} }
  function pickIndexes(p) {
    var seen = seenGet(p.id);
    var all = p.questions.map(function (_, i) { return i; });
    var fresh = all.filter(function (i) { return seen.indexOf(p.questions[i].q) === -1; });
    if (fresh.length < QUIZ_LEN) {
      var old = shuffle(all.filter(function (i) { return fresh.indexOf(i) === -1; }));
      fresh = fresh.concat(old.slice(0, QUIZ_LEN - fresh.length));
      seen = [];
    }
    var chosen = shuffle(fresh).slice(0, QUIZ_LEN);
    seenSet(p.id, seen.concat(chosen.map(function (i) { return p.questions[i].q; })));
    return chosen;
  }

  function jerseySVG(p) {
    return '<svg viewBox="0 0 200 220" role="img" aria-label="' + p.name + ' jersey, number ' + p.num + '">' +
      '<path d="M70 18 L38 32 L6 82 L36 104 L52 84 L52 208 L148 208 L148 84 L164 104 L194 82 L162 32 L130 18 Q100 46 70 18 Z" fill="' + p.color + '"/>' +
      '<path d="M70 18 Q100 46 130 18" fill="none" stroke="' + p.ink + '" stroke-opacity="0.45" stroke-width="4"/>' +
      '<path d="M6 82 L36 104 M194 82 L164 104" stroke="' + p.ink + '" stroke-opacity="0.35" stroke-width="5"/>' +
      '<text x="100" y="82" text-anchor="middle" fill="' + p.ink + '" font-family="Big Shoulders Display, Arial Narrow, sans-serif" font-weight="800" font-size="22" letter-spacing="3">' + p.surname + '</text>' +
      '<text x="100" y="168" text-anchor="middle" fill="' + p.ink + '" font-family="Big Shoulders Display, Arial Narrow, sans-serif" font-weight="900" font-size="' + (String(p.num).length > 1 ? 88 : 96) + '">' + p.num + '</text>' +
      '</svg>';
  }

  function buildCards(wrapId, mode) {
    var wrap = $(wrapId);
    PLAYERS.forEach(function (p) {
      var b = document.createElement("button");
      b.type = "button";
      b.className = "card";
      b.setAttribute("aria-label", (mode === "room" ? "Open a room for the " : "Take the ") + p.name + " quiz");
      b.style.setProperty("--pc", p.color);
      b.style.setProperty("--ink", p.ink);
      b.innerHTML = jerseySVG(p) +
        '<span class="num">Jersey No. ' + p.num + '</span>' +
        '<h2>' + p.name + '</h2>' +
        '<span class="role">' + p.role + '</span>' +
        '<span class="go">' + (mode === "room" ? "Open Room" : "Start Quiz") + '</span>';
      b.addEventListener("click", function () { if (mode === "room") createRoom(p); else startSolo(p); });
      wrap.appendChild(b);
    });
  }

  /* ---------- Quiz ---------- */
  var state = { player: null, list: [], i: 0, score: 0, locked: false, room: null };

  function startQuiz(p, list, roomCode) {
    state.player = p; state.list = list; state.i = 0; state.score = 0; state.room = roomCode || null;
    setAccent($("quiz"), p);
    $("q-num").textContent = "#" + p.num;
    $("q-name").textContent = p.name;
    var tag = $("q-room");
    tag.hidden = !roomCode;
    if (roomCode) tag.textContent = "Room " + roomCode;
    show("quiz");
    renderQuestion();
  }
  function startSolo(p) {
    startQuiz(p, pickIndexes(p).map(function (i) { return p.questions[i]; }), null);
  }

  function renderQuestion() {
    var total = state.list.length;
    var item = state.list[state.i];
    state.locked = false;
    $("q-score").textContent = state.score;
    $("q-count").textContent = "Question " + (state.i + 1) + " of " + total;
    $("q-bar").style.width = (state.i / total * 100) + "%";
    $("q-text").textContent = item.q;
    var fb = $("q-feedback");
    fb.textContent = "";
    fb.className = "feedback";
    $("q-next").hidden = true;
    var box = $("q-options");
    box.innerHTML = "";
    shuffle(item.a.map(function (t, idx) { return { t: t, ok: idx === 0 }; })).forEach(function (o, idx) {
      var b = document.createElement("button");
      b.type = "button";
      b.className = "opt";
      b.innerHTML = '<span class="k">' + "ABCD"[idx] + '</span><span class="t"></span>';
      b.querySelector(".t").textContent = o.t;
      b.dataset.ok = o.ok ? "1" : "0";
      b.addEventListener("click", function () { answer(b, o); });
      box.appendChild(b);
    });
  }

  function answer(btn, o) {
    if (state.locked) return;
    state.locked = true;
    var fb = $("q-feedback");
    $("q-options").querySelectorAll(".opt").forEach(function (b) {
      b.disabled = true;
      if (b.dataset.ok === "1") b.classList.add("correct");
      else if (b !== btn) b.classList.add("dim");
    });
    if (o.ok) {
      state.score++;
      fb.textContent = "Correct!";
      fb.classList.add("good");
    } else {
      btn.classList.add("wrong");
      fb.textContent = "Not quite. The answer is " + state.list[state.i].a[0] + ".";
      fb.classList.add("bad");
    }
    $("q-score").textContent = state.score;
    $("q-bar").style.width = ((state.i + 1) / state.list.length * 100) + "%";
    var next = $("q-next");
    next.textContent = state.i === state.list.length - 1 ? "See My Scorecard" : "Next Question";
    next.hidden = false;
    next.focus({ preventScroll: true });
  }

  $("q-next").addEventListener("click", function () {
    if (state.i < state.list.length - 1) { state.i++; renderQuestion(); } else showResult();
  });
  $("q-quit").addEventListener("click", function () { show(state.room ? "room" : "solo"); });

  /* ---------- Result ---------- */
  function fanTitle(score, total) {
    var pct = score / total;
    if (score === total) return ["Die-Hard Fan!", "A perfect innings. You know every detail of the game."];
    if (pct >= 0.75) return ["True Cricket Lover!", "You follow the game closely. Just a few misses."];
    if (pct >= 0.5) return ["Stadium Regular", "A solid knock. Watch a few more highlights and go again."];
    if (pct >= 0.25) return ["Weekend Viewer", "You know the basics. Time to catch up on the stats."];
    return ["Fresh on Debut", "Everyone starts somewhere. Try again and beat this score."];
  }

  function showResult() {
    var p = state.player, total = state.list.length;
    var t = fanTitle(state.score, total);
    var inRoom = !!state.room;
    setAccent($("result"), p);
    $("r-who").textContent = p.name + " Quiz" + (inRoom ? " · Room " + state.room : "");
    $("r-score").textContent = state.score;
    $("r-total").textContent = "/" + total;
    $("r-title").textContent = t[0];
    $("r-sub").textContent = t[1] + " You earned " + (state.score * POINTS_EACH) + " points.";
    $("r-room").hidden = !inRoom;
    $("r-choose").hidden = inRoom;
    $("r-again").hidden = inRoom;
    show("result");
    (inRoom ? $("r-room") : $("r-choose")).focus({ preventScroll: true });
    saveScore(p, state.score, total);
  }

  $("r-again").addEventListener("click", function () { startSolo(state.player); });
  $("r-choose").addEventListener("click", function () { show("solo"); });
  $("r-room").addEventListener("click", function () { show("room"); });
  $("r-menu").addEventListener("click", function () { show("menu"); });
  $("r-board").addEventListener("click", function () { openBoard("result"); });

  /* ---------- Identity and shared data ---------- */
  var me = null;       // { name, mobile }
  var uid = null;      // viewer id from Claude, null when signed out
  var DB = null;       // shared store, null when this view can't use it
  var canWrite = null; // true, false, or null when the platform didn't say

  function useCap(name) {
    try { return (window.claude && window.claude.use) ? window.claude.use(name) : Promise.resolve(null); }
    catch (e) { return Promise.resolve(null); }
  }
  function lsGet() { try { return JSON.parse(localStorage.getItem("cq-login") || "null"); } catch (e) { return null; } }
  function lsSet(v) { try { if (v) localStorage.setItem("cq-login", JSON.stringify(v)); else localStorage.removeItem("cq-login"); } catch (e) {} }
  function profileRef() { return DB.doc("data/users/" + uid + "/profile"); }

  function initData() {
    return Promise.all([useCap("db"), useCap("user")]).then(function (r) {
      DB = r[0];
      var user = r[1];
      if (!user) return;
      return Promise.all([user.id(), user.can("data.write")]).then(function (v) { uid = v[0]; canWrite = v[1]; });
    }).catch(function () {});
  }

  function cleanMobile(raw) {
    var d = String(raw || "").replace(/\D/g, "");
    if (d.length === 12 && d.indexOf("91") === 0) d = d.slice(2);
    if (d.length === 11 && d.charAt(0) === "0") d = d.slice(1);
    return /^[6-9]\d{9}$/.test(d) ? d : null;
  }
  function fieldError(id, msg) {
    $("f-" + id).classList.toggle("err", !!msg);
    $("m-" + id).textContent = msg || "";
  }

  var ready = initData();

  $("login-form").addEventListener("submit", function (e) {
    e.preventDefault();
    var name = $("l-name").value.replace(/\s+/g, " ").trim();
    var mobile = cleanMobile($("l-mobile").value);
    fieldError("name", name.length < 2 ? "Enter your name (at least 2 letters)." : "");
    fieldError("mobile", mobile ? "" : "Enter a valid 10-digit mobile number.");
    if (name.length < 2 || !mobile) return;
    ready.then(function () {
      me = { name: name, mobile: mobile };
      lsSet(me);
      if (DB && uid) profileRef().set({ name: name, mobile: mobile }).catch(function () {});
      $("u-name").textContent = name;
      show("menu");
    });
  });

  $("u-out").addEventListener("click", function () {
    if (DB && uid) profileRef().delete().catch(function () {});
    lsSet(null);
    me = null;
    $("l-name").value = "";
    $("l-mobile").value = "";
    leaveRoomSub();
    show("login");
  });

  // Fill the login form for returning players (they still see the login page first).
  ready.then(function () {
    var saved = lsGet();
    if (DB && uid) {
      return profileRef().get().then(function (s) {
        var d = s.exists ? s.data() : null;
        return d && d.name ? { name: String(d.name), mobile: String(d.mobile || "") } : saved;
      }).catch(function () { return saved; });
    }
    return saved;
  }).then(function (p) {
    if (p && p.name) {
      if (!$("l-name").value) $("l-name").value = p.name;
      if (!$("l-mobile").value && p.mobile) $("l-mobile").value = p.mobile;
    }
    startBoard();
  });

  /* ---------- Menu navigation ---------- */
  $("m-solo").addEventListener("click", function () { show("solo"); });
  $("m-friends").addEventListener("click", function () { updateFriendsNote(); show("friends"); });
  $("u-board").addEventListener("click", function () { openBoard("menu"); });
  $("solo-back").addEventListener("click", function () { show("menu"); });
  $("fr-back").addEventListener("click", function () { show("menu"); });

  /* ---------- Global scoreboard: one doc per player at scores/<id>, best score kept ---------- */
  var boardFrom = "menu";
  var boardUnsub = null;
  var boardRows = [];
  var boardState = "loading";

  function startBoard() {
    if (boardUnsub) return;
    if (!DB) { boardState = "none"; renderBoard(); return; }
    var timer = setTimeout(function () { if (boardState === "loading") { boardState = "slow"; renderBoard(); } }, 10000);
    try {
      boardUnsub = DB.collection("scores").orderBy("points", "desc").limit(100).onSnapshot(function (snap) {
        if (!snap.metadata.fromCache) clearTimeout(timer);
        boardState = "ok";
        boardRows = snap.docs.map(function (d) {
          var x = d.data() || {};
          return { id: d.id, name: String(x.name || "Player"), points: Number(x.points) || 0, correct: Number(x.correct) || 0, total: Number(x.total) || 0, player: String(x.player || ""), ts: Number(x.ts) || 0 };
        });
        renderBoard();
      }, function () { clearTimeout(timer); boardState = "error"; renderBoard(); });
    } catch (err) { clearTimeout(timer); boardState = "error"; renderBoard(); }
  }

  function playerLabel(id) { var p = findPlayer(id); return p ? p.name + " quiz" : "Quiz"; }

  function fillList(list, rows, opts) {
    list.innerHTML = "";
    var li;
    if (opts.message) {
      li = document.createElement("li");
      li.className = "empty";
      li.textContent = opts.message;
      list.appendChild(li);
      if (!rows.length) return;
    }
    rows.slice().sort(function (a, b) { return (b.points - a.points) || (a.ts - b.ts); }).forEach(function (r, i) {
      li = document.createElement("li");
      li.className = "row" + (i < 3 ? " top" : "") + (r.id === uid ? " you" : "");
      var rk = document.createElement("span"); rk.className = "rk"; rk.textContent = i + 1;
      var nm = document.createElement("span"); nm.className = "nm";
      var b = document.createElement("b"); b.textContent = r.name + (r.id === uid ? " (you)" : "");
      var s = document.createElement("small"); s.textContent = opts.sub(r);
      nm.appendChild(b); nm.appendChild(s);
      var pts = document.createElement("span"); pts.className = "pts"; pts.textContent = r.points;
      var u = document.createElement("small"); u.textContent = "pts"; pts.appendChild(u);
      li.appendChild(rk); li.appendChild(nm); li.appendChild(pts);
      list.appendChild(li);
    });
  }

  function renderBoard() {
    var msg = "";
    if (boardState === "none") msg = "The shared scoreboard needs a Claude sign-in. Sign in and reopen this page to see everyone's scores.";
    else if (boardState === "error") msg = "The scoreboard can't be loaded right now.";
    else if (boardState === "loading") msg = "Loading scores...";
    else if (boardState === "slow") msg = "Still trying to reach the scoreboard. Check your connection.";
    else if (!boardRows.length) msg = "No scores yet. Finish a quiz to be the first on the board.";
    fillList($("b-list"), (boardState === "ok" || boardState === "slow") ? boardRows : [], {
      message: msg,
      sub: function (r) { return playerLabel(r.player) + " · " + r.correct + "/" + r.total + " right"; }
    });
  }

  function openBoard(from) { boardFrom = from; renderBoard(); show("board"); }
  $("b-back").addEventListener("click", function () { show(boardFrom); });

  function saveScore(p, score, total) {
    var el = $("r-save");
    var roomCode = state.room;
    if (!DB || !uid) { el.textContent = "Sign in to Claude to put your score on the scoreboard."; return; }
    if (canWrite === false) { el.textContent = "You have view-only access, so your score can't be saved. Ask the owner for Contributor access."; return; }
    var pts = score * POINTS_EACH;
    var ref = DB.doc("scores/" + uid);
    el.textContent = "Saving your score...";
    var g = ref.get().then(function (s) {
      var old = s.exists ? s.data() : null;
      if (!old || pts > (Number(old.points) || 0)) {
        return ref.set({ name: me.name, points: pts, correct: score, total: total, player: p.id, ts: Date.now() })
          .then(function () { return old ? "New personal best! Your score is on the scoreboard." : "Your score is on the scoreboard."; });
      }
      var fix = old.name !== me.name ? ref.update({ name: me.name }) : Promise.resolve();
      return fix.then(function () { return "Your best of " + old.points + " points stays on the scoreboard."; });
    });
    var rm = Promise.resolve();
    if (roomCode) {
      var rref = DB.doc("rooms/" + roomCode + "/results/" + uid);
      rm = rref.get().then(function (s) {
        if (s.exists) return;
        return rref.set({ name: me.name, points: pts, correct: score, total: total, ts: Date.now() });
      });
    }
    Promise.all([g, rm]).then(function (r) {
      el.textContent = r[0] + (roomCode ? " It is on the room leaderboard too." : "");
    }).catch(function () { el.textContent = "Your score couldn't be saved. Try again in a moment."; });
  }

  /* ---------- Friends: rooms with a 5-character code ---------- */
  var ROOM_CHARS = "ABCDEFGHJKLMNPQRSTUVWXYZ23456789";
  var room = null;       // { code, player, qids, hostName }
  var roomUnsub = null;
  var roomRows = [];
  var roomState = "loading";

  function makeCode() { var s = ""; for (var i = 0; i < 5; i++) s += ROOM_CHARS.charAt(Math.floor(Math.random() * ROOM_CHARS.length)); return s; }
  function friendsOk() {
    var msg = "";
    if (!DB) msg = "Rooms need shared storage, which isn't available in this browser view. Open the page while signed in to claude.ai.";
    else if (!uid) msg = "You're not signed in to Claude in this browser, so rooms can't be created. Sign in at claude.ai, then reopen the page.";
    else if (canWrite === false) msg = "You have view-only access. Ask the owner for Contributor access to open or join rooms.";
    $("fr-note").textContent = msg;
    if (msg) $("fr-msg").textContent = msg;
    return !msg;
  }
  function updateFriendsNote() {
    $("fr-note").textContent = "";
    $("fr-msg").textContent = "";
    $("j-msg").textContent = "";
    var ok = friendsOk();
    $("j-code").disabled = !ok;
    $("j-btn").disabled = !ok;
  }
  function validQids(p, q) {
    if (!p || !Array.isArray(q) || q.length < 1 || q.length > QUIZ_LEN) return false;
    return q.every(function (i) { return Number.isInteger(i) && i >= 0 && i < p.questions.length; });
  }

  function createRoom(p) {
    if (!friendsOk()) return;
    $("fr-msg").textContent = "Opening your room...";
    var idx = pickIndexes(p), tries = 0;
    (function attempt() {
      var code = makeCode(), ref = DB.doc("rooms/" + code);
      ref.get().then(function (s) {
        if (s.exists) { if (++tries < 6) return attempt(); throw new Error("busy"); }
        return ref.set({ host: uid, hostName: me.name, player: p.id, qids: idx, ts: Date.now() })
          .then(function () { $("fr-msg").textContent = ""; enterRoom(code, { player: p.id, qids: idx, hostName: me.name }); });
      }).catch(function (e) {
        var code = e && e.code;
        $("fr-msg").textContent = "Couldn't open the room" + (code ? " (" + code + ")" : "") + ". " +
          (code === "invalid_argument" || code === "not_granted" ? "Your access may be view-only. Ask the owner for Contributor access." : "Try again in a moment.");
      });
    })();
  }

  $("join-form").addEventListener("submit", function (e) {
    e.preventDefault();
    var code = $("j-code").value.toUpperCase().replace(/[^A-Z0-9]/g, "");
    var msg = $("j-msg");
    msg.classList.add("err");
    if (!friendsOk()) { msg.textContent = $("fr-note").textContent; return; }
    if (code.length !== 5) { msg.textContent = "Enter the 5-character room code."; return; }
    msg.classList.remove("err");
    msg.textContent = "Looking for the room...";
    DB.doc("rooms/" + code).get().then(function (s) {
      var d = s.exists ? s.data() : null;
      var p = d ? findPlayer(d.player) : null;
      if (!p || !validQids(p, d.qids)) { msg.classList.add("err"); msg.textContent = "No room found with that code. Check it and try again."; return; }
      msg.textContent = "";
      enterRoom(code, d);
    }).catch(function () { msg.classList.add("err"); msg.textContent = "Couldn't reach the room. Try again in a moment."; });
  });

  function leaveRoomSub() { if (roomUnsub) { try { roomUnsub(); } catch (e) {} roomUnsub = null; } }

  function enterRoom(code, d) {
    leaveRoomSub();
    var p = findPlayer(d.player);
    room = { code: code, player: p, qids: d.qids.slice(), hostName: String(d.hostName || "") };
    setAccent($("room"), p);
    $("rm-code").textContent = code;
    $("rm-quiz").textContent = p.name + " quiz · " + room.qids.length + " questions" + (room.hostName ? " · opened by " + room.hostName : "");
    roomRows = [];
    roomState = "loading";
    renderRoom();
    try {
      roomUnsub = DB.collection("rooms/" + code + "/results").onSnapshot(function (snap) {
        roomState = "ok";
        roomRows = snap.docs.map(function (x) {
          var v = x.data() || {};
          return { id: x.id, name: String(v.name || "Player"), points: Number(v.points) || 0, correct: Number(v.correct) || 0, total: Number(v.total) || 0, ts: Number(v.ts) || 0 };
        });
        renderRoom();
      }, function () { roomState = "error"; renderRoom(); });
    } catch (err) { roomState = "error"; renderRoom(); }
    show("room");
  }

  function renderRoom() {
    var own = null;
    roomRows.forEach(function (r) { if (r.id === uid) own = r; });
    var msg = "";
    if (roomState === "error") msg = "The room leaderboard can't be loaded right now.";
    else if (roomState === "loading") msg = "Loading the room...";
    else if (!roomRows.length) msg = "Nobody has finished yet. Be the first on the board.";
    fillList($("rm-list"), roomState === "ok" ? roomRows : [], {
      message: msg,
      sub: function (r) { return r.correct + "/" + r.total + " right"; }
    });
    var play = $("rm-play");
    play.disabled = !!own;
    play.textContent = own ? "You scored " + own.points + " pts" : "Play Now";
  }

  $("rm-play").addEventListener("click", function () {
    if (!room) return;
    var p = room.player;
    startQuiz(p, room.qids.map(function (i) { return p.questions[i]; }), room.code);
  });
  $("rm-leave").addEventListener("click", function () { leaveRoomSub(); room = null; show("friends"); });
  $("rm-copy").addEventListener("click", function () {
    var b = this, code = room ? room.code : "";
    function selectCode() {
      var rg = document.createRange(); rg.selectNodeContents($("rm-code"));
      var sel = window.getSelection(); sel.removeAllRanges(); sel.addRange(rg);
    }
    try {
      navigator.clipboard.writeText(code).then(function () {
        b.textContent = "Copied";
        setTimeout(function () { b.textContent = "Copy Code"; }, 1600);
      }, selectCode);
    } catch (e) { selectCode(); }
  });

  /* ---------- Start ---------- */
  buildScenes();
  buildCards("cards-solo", "solo");
  buildCards("cards-room", "room");
  renderBoard();
  show("login");
})();
</script>

</body>
</html>
