# new
First repository
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Modern Calculator</title>
  <style>
    :root{
      --bg: #0f1724;
      --panel: rgba(255,255,255,0.06);
      --accent: #7c5cff;
      --muted: rgba(255,255,255,0.6);
      --glass: rgba(255,255,255,0.04);
      --danger: #ff6b6b;
      --radius: 16px;
      --shadow: 0 6px 18px rgba(2,6,23,0.6);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      background: radial-gradient(1200px 600px at 10% 10%, rgba(124,92,255,0.12), transparent),
                  radial-gradient(800px 400px at 90% 90%, rgba(0,200,255,0.04), transparent),
                  var(--bg);
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px;
      color: #e6eef8;
    }

    .calculator{
      width: 380px;
      max-width:100%;
      border-radius: calc(var(--radius) - 4px);
      background: linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
      box-shadow: var(--shadow);
      padding:18px;
      backdrop-filter: blur(8px) saturate(140%);
    }

    .top-row{
      display:flex;
      gap:12px;
      align-items:center;
      margin-bottom:12px;
    }

    .title{
      font-weight:600;
      letter-spacing:0.2px;
    }

    .controls{
      margin-left:auto;
      display:flex;
      gap:8px;
      align-items:center;
    }

    .btn-icon{
      background:var(--glass);
      border:1px solid rgba(255,255,255,0.03);
      padding:8px;
      border-radius:10px;
      cursor:pointer;
      display:inline-grid;
      place-items:center;
    }

    .screen{
      background: linear-gradient(180deg, rgba(0,0,0,0.2), rgba(255,255,255,0.02));
      padding:14px;
      border-radius:12px;
      margin-bottom:14px;
      min-height:78px;
      display:flex;
      flex-direction:column;
      gap:6px;
      align-items:flex-end;
      color:var(--muted);
    }

    .expression{
      font-size:14px;
      color:var(--muted);
      width:100%;
      overflow:hidden;
      white-space:nowrap;
      text-overflow:ellipsis;
    }

    .output{
      font-size:28px;
      font-weight:700;
      color:#fff;
      width:100%;
      overflow:hidden;
      white-space:nowrap;
      text-overflow:ellipsis;
    }

    .pad{
      display:grid;
      grid-template-columns: repeat(4, 1fr);
      gap:10px;
    }

    button.key{
      padding:16px 12px;
      font-size:18px;
      border-radius:12px;
      border:0;
      cursor:pointer;
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      box-shadow: 0 4px 10px rgba(2,6,23,0.45);
      color:var(--muted);
      transition: transform .06s ease, box-shadow .06s ease;
    }
    button.key:active{transform:translateY(2px);}

    .op{ color: var(--accent); font-weight:600; }
    .func{ background: rgba(255,255,255,0.03); }
    .equals{ background: linear-gradient(180deg,var(--accent), #5b49ff); color:white; grid-column:span 1;}
    .zero{ grid-column: span 2; }
    .danger{ color:var(--danger); }

    .history{
      margin-top:12px;
      max-height:120px;
      overflow:auto;
      padding:8px;
      border-radius:10px;
      background: rgba(255,255,255,0.02);
      font-size:13px;
    }

    .history-item{ display:flex; justify-content:space-between; gap:10px; padding:6px 4px; }
    .history-item span:first-child{ color:var(--muted); }

    .footer{
      margin-top:12px;
      display:flex;
      gap:10px;
      align-items:center;
      justify-content:space-between;
      font-size:13px;
      color:var(--muted);
    }

    /* responsive */
    @media (max-width:420px){
      .calculator{ padding:12px; }
      button.key{ padding:14px 10px; font-size:16px; }
    }

    /* light theme */
    .light{
      --bg: #f4f7fb;
      --panel: rgba(0,0,0,0.02);
      --accent: #6b46ff;
      --muted: rgba(0,0,0,0.6);
      color: #0b1220;
    }

  </style>
</head>
<body>
  <main class="calculator" id="app">
    <div class="top-row">
      <div>
        <div class="title">Calculator</div>
        <div style="font-size:12px; color:var(--muted);">Clean • Responsive • Keyboard-friendly</div>
      </div>

      <div class="controls">
        <button class="btn-icon" id="themeToggle" title="Toggle theme" aria-label="Toggle theme">🌓</button>
        <button class="btn-icon" id="clearHistory" title="Clear history" aria-label="Clear history">🗑️</button>
      </div>
    </div>

    <section class="screen" role="region" aria-label="Calculator display">
      <div class="expression" id="expression" aria-live="polite">&nbsp;</div>
      <div class="output" id="output" aria-live="polite">0</div>
    </section>

    <section class="pad" role="group" aria-label="Calculator keypad">
      <!-- row 1 -->
      <button class="key func" data-action="mc">MC</button>
      <button class="key func" data-action="mr">MR</button>
      <button class="key func" data-action="mplus">M+</button>
      <button class="key func" data-action="mminus">M-</button>

      <!-- row 2 -->
      <button class="key func" data-value="(">(</button>
      <button class="key func" data-value=")">)</button>
      <button class="key func" data-action="back">⌫</button>
      <button class="key op" data-action="clear">C</button>

      <!-- row 3 -->
      <button class="key" data-value="7">7</button>
      <button class="key" data-value="8">8</button>
      <button class="key" data-value="9">9</button>
      <button class="key op" data-value="/">÷</button>

      <!-- row 4 -->
      <button class="key" data-value="4">4</button>
      <button class="key" data-value="5">5</button>
      <button class="key" data-value="6">6</button>
      <button class="key op" data-value="*">×</button>

      <!-- row 5 -->
      <button class="key" data-value="1">1</button>
      <button class="key" data-value="2">2</button>
      <button class="key" data-value="3">3</button>
      <button class="key op" data-value="-">−</button>

      <!-- row 6 -->
      <button class="key zero" data-value="0">0</button>
      <button class="key" data-value=".">.</button>
      <button class="key equals" data-action="equals">=</button>
      <button class="key op" data-value="+">+</button>
    </section>

    <div class="history" id="history" aria-live="polite">
      <!-- history items inserted here -->
    </div>

    <div class="footer">
      <div>Memory: <span id="memVal">0</span></div>
      <div style="opacity:.8; font-size:12px">Tip: Use keyboard numbers, + - * /, Enter for =, Backspace to delete</div>
    </div>
  </main>

  <script>
    (function(){
      const expressionEl = document.getElementById('expression');
      const outputEl = document.getElementById('output');
      const historyEl = document.getElementById('history');
      const memValEl = document.getElementById('memVal');
      const themeToggle = document.getElementById('themeToggle');
      const clearHistoryBtn = document.getElementById('clearHistory');
      const app = document.getElementById('app');

      let expr = '';
      let memory = 0;
      let history = [];

      // Load saved theme if present
      if(localStorage.getItem('calcTheme') === 'light') document.documentElement.classList.add('light');

      function render(){
        expressionEl.textContent = expr || '\u00A0';
        try{
          const val = safeEval(expr || '0');
          outputEl.textContent = (String(val).length>16) ? Number(val).toPrecision(12) : String(val);
        }catch(e){
          outputEl.textContent = 'Error';
        }
        memValEl.textContent = String(memory);
        renderHistory();
      }

      function safeEval(input){
        // allow digits, parentheses, decimals, spaces and operators + - * / %
        if(!input.trim()) return 0;
        if(!/^[0-9+\-*/().%\s]+$/.test(input)) throw new Error('Invalid characters');
        // prevent sequences like 2**3 in some engines? still allow ** for exponent? we won't allow **
        if(/\*\*/.test(input)) throw new Error('Invalid operator');
        // Basic safety: evaluate using Function but only after validation
        /* eslint-disable no-new-func */
        return Function('return (' + input + ')')();
      }

      function append(v){
        // avoid leading zeros weirdness
        if(v === '.' && /\.[0-9]*$/.test(expr)) return;
        expr += v;
        render();
      }

      function back(){ expr = expr.slice(0,-1); render(); }
      function clearAll(){ expr=''; render(); }

      function doEquals(){
        try{
          const result = safeEval(expr);
          history.unshift({q:expr, a: result});
          expr = String(result);
        }catch(e){ expr = ''; }
        render();
      }

      function renderHistory(){
        historyEl.innerHTML = '';
        if(history.length === 0){ historyEl.textContent = 'No history yet'; return; }
        history.slice(0,20).forEach((h, idx)=>{
          const item = document.createElement('div');
          item.className = 'history-item';
          const q = document.createElement('span'); q.textContent = h.q;
          const a = document.createElement('strong'); a.textContent = h.a;
          item.appendChild(q); item.appendChild(a);
          item.title = 'Click to reuse result';
          item.style.cursor = 'pointer';
          item.addEventListener('click', ()=>{ expr = String(h.a); render(); });
          historyEl.appendChild(item);
        });
      }

      // Button clicks
      document.querySelectorAll('button.key').forEach(btn => {
        btn.addEventListener('click', ()=>{
          const v = btn.dataset.value;
          const action = btn.dataset.action;
          if(v) append(v);
          else if(action){
            if(action === 'equals') doEquals();
            else if(action === 'back') back();
            else if(action === 'clear') clearAll();
            else if(action === 'mc'){ memory = 0; render(); }
            else if(action === 'mr'){ expr = String(memory); render(); }
            else if(action === 'mplus'){
              try{ memory = Number(safeEval(expr || '0')) + memory; render(); }catch(e){}
            }
            else if(action === 'mminus'){
              try{ memory = memory - Number(safeEval(expr || '0')); render(); }catch(e){}
            }
          }
        });
      });

      // Keyboard support
      window.addEventListener('keydown', (e)=>{
        if(e.key === 'Enter') { e.preventDefault(); doEquals(); return; }
        if(e.key === 'Backspace'){ e.preventDefault(); back(); return; }
        if(e.key === 'Delete'){ e.preventDefault(); clearAll(); return; }
        const allowed = '0123456789+-*/().%';
        if(allowed.includes(e.key)){
          append(e.key);
          e.preventDefault();
        }
      });

      // Theme toggle
      themeToggle.addEventListener('click', ()=>{
        const isLight = document.documentElement.classList.toggle('light');
        localStorage.setItem('calcTheme', isLight? 'light' : 'dark');
      });

      clearHistoryBtn.addEventListener('click', ()=>{ history = []; render(); });

      // initial render
      render();
    })();
  </script>
</body>
</html>
