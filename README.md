<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Sameh Tools — آلة حاسبة & لعبة XO</title>

<style>
  :root{
    --bg-light:#f6f8fb; --card-light:#ffffff; --text-light:#0b1220; --muted-light:#43506a;
    --bg-dark:#0b1220; --card-dark:#0f1724; --text-dark:#e6eef6; --muted-dark:#9aa7bf;
    --accent:#4a6cf7; --glass: rgba(255,255,255,0.03);
    --radius:12px;
  }

  /* auto theme by OS */
  html{font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
  body{margin:0;min-height:100vh;display:flex;flex-direction:column;}
  header{
    padding:20px 16px;
    display:flex;align-items:center;justify-content:space-between;
    gap:12px;
  }

  /* container */
  .wrap{max-width:1000px;margin:12px auto;padding:0 14px;width:100%;flex:1 0 auto;}

  .brand {display:flex;gap:12px;align-items:center;}
  .logo{
    width:56px;height:56px;border-radius:14px;display:flex;align-items:center;justify-content:center;
    font-weight:700;font-size:20px;color:white;background:linear-gradient(135deg,var(--accent),#2b5bf0);
    box-shadow:0 6px 18px rgba(74,108,247,0.18);
  }
  h1{font-size:18px;margin:0}
  .subtitle{font-size:13px;opacity:0.9;margin-top:4px}

  .theme-toggle{display:flex;gap:8px;align-items:center}
  .toggle-btn{background:transparent;border:1px solid; padding:8px 10px;border-radius:10px;cursor:pointer;font-size:14px}

  .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:18px;margin-top:16px}

  .card{
    padding:16px;border-radius:var(--radius);box-shadow:0 8px 28px rgba(2,6,23,0.45);
    border:1px solid rgba(255,255,255,0.03);
  }
  h2{margin:0 0 10px;font-size:16px;color:var(--accent)}

  /* calculator */
  .display{
    border-radius:10px;padding:12px;font-size:20px;min-height:56px;text-align:left;direction:ltr;word-break:break-all;
  }
  .keys{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin-top:12px}
  .btn{padding:12px;border-radius:10px;border:none;font-size:16px;cursor:pointer}
  .btn.op{background:rgba(74,108,247,0.12)}
  .btn.equal{background:var(--accent);color:#fff}

  .small{font-size:13px;margin-top:8px}

  /* XO */
  .xo-board{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;max-width:360px;margin:auto}
  .cell{height:92px;border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:44px;cursor:pointer}
  .meta{text-align:center;margin-top:12px}
  .controls{display:flex;gap:8px;justify-content:center;margin-top:12px}
  .secondary{padding:8px 12px;border-radius:10px;border:1px solid;cursor:pointer}

  footer{padding:18px 14px;text-align:center;font-size:13px}

  /* light & dark definitions */
  @media (prefers-color-scheme:dark){
    :root{
      --bg:var(--bg-dark);--card:var(--card-dark);--text:var(--text-dark);--muted:var(--muted-dark);
    }
  }
  @media (prefers-color-scheme:light){
    :root{
      --bg:var(--bg-light);--card:var(--card-light);--text:var(--text-light);--muted:var(--muted-light);
    }
  }

  /* apply vars */
  body{background:var(--bg);color:var(--text);font-family:Arial, sans-serif}
  .card{background:var(--card);color:var(--text)}
  .display{background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.03)}
  .cell{background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.03)}

  /* responsive tweaks */
  @media (max-width:420px){
    .logo{width:48px;height:48px;font-size:18px}
    .cell{height:72px;font-size:36px}
    .display{font-size:18px}
  }
</style>
</head>
<body>

<header>
  <div class="brand">
    <div class="logo">ST</div>
    <div>
      <h1>Sameh Tools</h1>
      <div class="subtitle">آلة حاسبة آمنة &amp; لعبة XO — تصميم متجاوب</div>
    </div>
  </div>

  <div class="theme-toggle" aria-hidden="false">
    <button id="themeBtn" class="toggle-btn">تبديل الوضع</button>
  </div>
</header>

<main class="wrap">
  <div class="grid">
    <!-- Calculator -->
    <section class="card" aria-labelledby="calcTitle">
      <h2 id="calcTitle">آلة حاسبة</h2>
      <div id="calcDisplay" class="display" role="textbox" aria-live="polite">0</div>

      <div class="keys" id="calcKeys" role="group" aria-label="مفاتيح الآلة الحاسبة">
        <button class="btn" data-action="clear" aria-label="مسح">C</button>
        <button class="btn" data-action="back" aria-label="حذف">←</button>
        <button class="btn" data-value="(" aria-label="قوس فتح">(</button>
        <button class="btn" data-value=")" aria-label="قوس إغلاق">)</button>

        <button class="btn" data-value="7">7</button>
        <button class="btn" data-value="8">8</button>
        <button class="btn" data-value="9">9</button>
        <button class="btn op" data-value="/">÷</button>

        <button class="btn" data-value="4">4</button>
        <button class="btn" data-value="5">5</button>
        <button class="btn" data-value="6">6</button>
        <button class="btn op" data-value="*">×</button>

        <button class="btn" data-value="1">1</button>
        <button class="btn" data-value="2">2</button>
        <button class="btn" data-value="3">3</button>
        <button class="btn op" data-value="-">−</button>

        <button class="btn" data-value="0">0</button>
        <button class="btn" data-value=".">.</button>
        <button class="btn equal" data-action="eval" aria-label="يساوي">=</button>
        <button class="btn op" data-value="+">+</button>
      </div>

      <div class="small">مسموح: أرقام، + - * / . ( ) . الإدخالات يتم التحقق منها قبل الحساب.</div>
    </section>

    <!-- Tic Tac Toe -->
    <section class="card" aria-labelledby="xoTitle">
      <h2 id="xoTitle">لعبة XO</h2>

      <div id="xo">
        <div class="xo-board" id="board" role="grid" aria-label="لوحة لعبة XO">
          <div class="cell" data-i="0" role="button" tabindex="0"></div>
          <div class="cell" data-i="1" role="button" tabindex="0"></div>
          <div class="cell" data-i="2" role="button" tabindex="0"></div>
          <div class="cell" data-i="3" role="button" tabindex="0"></div>
          <div class="cell" data-i="4" role="button" tabindex="0"></div>
          <div class="cell" data-i="5" role="button" tabindex="0"></div>
          <div class="cell" data-i="6" role="button" tabindex="0"></div>
          <div class="cell" data-i="7" role="button" tabindex="0"></div>
          <div class="cell" data-i="8" role="button" tabindex="0"></div>
        </div>

        <div class="meta" id="xoStatus">دور اللاعب: X</div>
        <div class="controls">
          <button id="resetXO" class="secondary">إعادة</button>
          <button id="swapStart" class="secondary">ابدأ بـ O</button>
        </div>
      </div>
    </section>
  </div>

  <footer>© 2025 Sameh — تواصل: w3173854@gmail.com</footer>
</main>

<script>
/* ---------------- Theme ---------------- */
(function(){
  const btn = document.getElementById('themeBtn');
  const root = document.documentElement;

  // store theme in localStorage: 'dark' or 'light' or 'auto'
  function setTheme(mode){
    if(mode === 'dark'){
      root.style.setProperty('--bg', getComputedStyle(root).getPropertyValue('--bg-dark'));
      root.style.setProperty('--card', getComputedStyle(root).getPropertyValue('--card-dark'));
      root.style.setProperty('--text', getComputedStyle(root).getPropertyValue('--text-dark'));
      root.style.setProperty('--muted', getComputedStyle(root).getPropertyValue('--muted-dark'));
      localStorage.setItem('site-theme','dark');
    } else if(mode === 'light'){
      root.style.setProperty('--bg', getComputedStyle(root).getPropertyValue('--bg-light'));
      root.style.setProperty('--card', getComputedStyle(root).getPropertyValue('--card-light'));
      root.style.setProperty('--text', getComputedStyle(root).getPropertyValue('--text-light'));
      root.style.setProperty('--muted', getComputedStyle(root).getPropertyValue('--muted-light'));
      localStorage.setItem('site-theme','light');
    } else { // auto
      localStorage.removeItem('site-theme');
      applyAuto();
    }
  }

  function applyAuto(){
    const dark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
    setTheme(dark ? 'dark' : 'light');
  }

  // init: if user has a choice, apply it; otherwise auto
  const saved = localStorage.getItem('site-theme');
  if(saved) setTheme(saved);
  else applyAuto();

  btn.addEventListener('click', ()=>{
    const current = localStorage.getItem('site-theme') || 'auto';
    const next = current === 'dark' ? 'light' : (current === 'light' ? 'auto' : 'dark');
    setTheme(next);
    btn.textContent = next === 'auto' ? 'الوضع التلقائي' : (next === 'dark' ? 'وضع داكن' : 'وضع فاتح');
    setTimeout(()=> location.reload(), 250); // refresh to apply some CSS vars consistently
  });
})();

/* ---------------- Calculator ---------------- */
(function(){
  const display = document.getElementById('calcDisplay');
  const keys = document.getElementById('calcKeys');
  let expr = '';

  function updateDisplay(){
    display.textContent = expr === '' ? '0' : expr;
  }

  // keyboard support (numbers and operators)
  window.addEventListener('keydown', (e)=>{
    if(e.key === 'Enter'){ evaluate(); e.preventDefault(); return; }
    if(e.key === 'Backspace'){ expr = expr.slice(0,-1); updateDisplay(); return; }
    if(/^[0-9+\-*/().]$/.test(e.key)){ expr += e.key; updateDisplay(); }
  });

  function evaluate(){
    const safe = expr.replace(/÷/g,'/').replace(/×/g,'*').trim();
    if(!/^[0-9+\-*/().\s]+$/.test(safe) || safe === ''){
      display.textContent = 'خطأ';
      return;
    }
    try{
      const res = Function('"use strict";return ('+safe+')')();
      expr = String(res);
      updateDisplay();
    }catch(err){
      display.textContent = 'خطأ';
    }
  }

  keys.addEventListener('click', (e)=>{
    const btn = e.target.closest('button');
    if(!btn) return;
    const val = btn.getAttribute('data-value');
    const action = btn.getAttribute('data-action');

    if(action === 'clear'){ expr = ''; updateDisplay(); return; }
    if(action === 'back'){ expr = expr.slice(0,-1); updateDisplay(); return; }
    if(action === 'eval'){ evaluate(); return; }
    if(val){ expr += val; updateDisplay(); }
  });

  updateDisplay();
})();

/* ---------------- Tic Tac Toe (XO) ---------------- */
(function(){
  const cells = Array.from(document.querySelectorAll('.cell'));
  const status = document.getElementById('xoStatus');
  const resetBtn = document.getElementById('resetXO');
  const swapBtn = document.getElementById('swapStart');

  let board = Array(9).fill('');
  let current = 'X';
  let running = true;
  let startWithO = false;

  function render(){
    cells.forEach((c,i)=> c.textContent = board[i]);
  }

  function checkWinner(){
    const wins = [
      [0,1,2],[3,4,5],[6,7,8],
      [0,3,6],[1,4,7],[2,5,8],
      [0,4,8],[2,4,6]
    ];
    for(const w of wins){
      const [a,b,c] = w;
      if(board[a] && board[a] === board[b] && board[a] === board[c]) return board[a];
    }
    if(board.every(Boolean)) return 'tie';
    return null;
  }

  function updateStatus(){
    const res = checkWinner();
    if(res === 'tie'){ status.textContent = 'تعادل!'; running = false; return; }
    if(res){ status.textContent = 'الفائز: ' + res; running = false; return; }
    status.textContent = 'دور اللاعب: ' + current;
  }

  function clickCell(e){
    if(!running) return;
    const i = Number(e.target.getAttribute('data-i'));
    if(board[i]) return;
    board[i] = current;
    current = current === 'X' ? 'O' : 'X';
    render(); updateStatus();
  }

  // keyboard accessibility: arrow + enter to play (optional simpler)
  cells.forEach(c=>{
    c.addEventListener('click', clickCell);
    c.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter') clickCell({target:c});
    });
  });

  resetBtn.addEventListener('click', ()=>{
    board = Array(9).fill(''); current = startWithO ? 'O' : 'X'; running = true; render(); updateStatus();
  });

  swapBtn.addEventListener('click', ()=>{
    startWithO = !startWithO;
    current = startWithO ? 'O' : 'X';
    swapBtn.textContent = startWithO ? 'ابدأ بـ X' : 'ابدأ بـ O';
    resetBtn.click();
  });

  // init
  current = startWithO ? 'O' : 'X';
  updateStatus();
})();
</script>

</body>
</html>
