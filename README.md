<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>German to English Vocabulary Trainer</title>
  <link href="https://api.fontshare.com/v2/css?f[]=satoshi@400,500,700&f[]=cabinet-grotesk@700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg:#f7f6f2; --surface:#ffffff; --surface-2:#f0ede7; --text:#1f2937; --muted:#6b7280;
      --primary:#01696f; --primary-2:#0c4e54; --accent:#d19900; --good:#2e7d32; --bad:#b42318;
      --border:#d9d4cb; --shadow:0 16px 40px rgba(0,0,0,.08); --radius:20px;
      --font-body:'Satoshi', sans-serif; --font-display:'Cabinet Grotesk', sans-serif;
    }
    * { box-sizing:border-box; }
    body { margin:0; font-family:var(--font-body); background:linear-gradient(180deg,#f7f6f2 0%,#eeebe4 100%); color:var(--text); }
    .wrap { max-width:1100px; margin:0 auto; padding:24px; }
    .hero { background:var(--surface); border:1px solid var(--border); border-radius:28px; padding:28px; box-shadow:var(--shadow); }
    h1,h2,h3 { margin:0; line-height:1.08; }
    h1 { font-family:var(--font-display); font-size:clamp(2rem,4vw,3.8rem); margin-bottom:10px; }
    .sub { color:var(--muted); max-width:70ch; font-size:1.05rem; }
    .stats { display:grid; grid-template-columns:repeat(auto-fit,minmax(160px,1fr)); gap:12px; margin-top:20px; }
    .stat { background:var(--surface-2); border-radius:16px; padding:14px 16px; border:1px solid var(--border); }
    .stat strong { display:block; font-size:1.35rem; color:var(--primary); }
    .tabs { display:flex; flex-wrap:wrap; gap:10px; margin:22px 0 18px; }
    .tab { padding:12px 16px; border-radius:999px; border:1px solid var(--border); background:#fff; cursor:pointer; font-weight:700; min-height:44px; }
    .tab.active { background:var(--primary); color:#fff; border-color:var(--primary); }
    .panel { display:none; background:rgba(255,255,255,.88); backdrop-filter:blur(8px); border:1px solid var(--border); border-radius:24px; padding:22px; box-shadow:var(--shadow); margin-bottom:18px; }
    .panel.active { display:block; }
    .controls { display:flex; flex-wrap:wrap; gap:10px; margin:14px 0 18px; }
    button, input, select { font:inherit; }
    .btn { background:var(--primary); color:#fff; border:none; border-radius:14px; padding:12px 16px; cursor:pointer; font-weight:700; min-height:44px; }
    .btn.secondary { background:#fff; color:var(--text); border:1px solid var(--border); }
    .btn.warn { background:var(--accent); color:#2b2100; }
    .card { background:#fff; border:1px solid var(--border); border-radius:20px; padding:18px; }
    .prompt { font-size:clamp(1.5rem,3vw,2.3rem); font-weight:700; margin:10px 0 16px; }
    .answer-row { display:flex; flex-wrap:wrap; gap:10px; align-items:center; }
    input[type='text'] { flex:1 1 240px; min-height:48px; border-radius:14px; border:1px solid var(--border); padding:12px 14px; background:#fff; }
    .feedback { margin-top:12px; font-weight:700; min-height:1.5em; }
    .good { color:var(--good); } .bad { color:var(--bad); }
    .small { color:var(--muted); font-size:.95rem; }
    .grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(240px,1fr)); gap:14px; }
    .option { width:100%; text-align:left; background:#fff; color:var(--text); border:1px solid var(--border); border-radius:16px; padding:14px; cursor:pointer; min-height:52px; font-weight:700; }
    .option.correct { background:#e7f6ea; border-color:#7ac38a; }
    .option.wrong { background:#fdecec; border-color:#e59a9a; }
    .pair-list { display:grid; grid-template-columns:repeat(auto-fit,minmax(280px,1fr)); gap:12px; margin-top:14px; }
    .pair { display:flex; justify-content:space-between; gap:16px; background:#fff; border:1px solid var(--border); border-radius:16px; padding:12px 14px; }
    .pill { display:inline-block; padding:6px 10px; border-radius:999px; background:var(--surface-2); border:1px solid var(--border); font-size:.86rem; font-weight:700; }
    .progress { height:12px; background:#e8e3db; border-radius:999px; overflow:hidden; margin:14px 0; }
    .bar { height:100%; width:0%; background:linear-gradient(90deg,var(--primary),#39a2a7); transition:width .25s ease; }
    .footer-note { color:var(--muted); text-align:center; padding:10px 0 30px; }
    @media (max-width:640px) { .wrap{padding:14px;} .hero,.panel{padding:18px;} }
  </style>
</head>
<body>
  <div class="wrap">
    <section class="hero">
      <h1>German → English vocabulary trainer</h1>
      <p class="sub">This standalone practice page helps your student memorize the English versions of the words from the worksheet with flashcards, multiple choice, typing practice, and a mixed challenge.</p>
      <div class="stats">
        <div class="stat"><strong id="totalWords">0</strong><span>Total words</span></div>
        <div class="stat"><strong id="nounCount">0</strong><span>Nouns</span></div>
        <div class="stat"><strong id="adjCount">0</strong><span>Adjectives</span></div>
        <div class="stat"><strong id="verbCount">0</strong><span>Verbs</span></div>
      </div>
    </section>

    <div class="tabs">
      <button class="tab active" data-tab="flashcards">Flashcards</button>
      <button class="tab" data-tab="multiple">Multiple choice</button>
      <button class="tab" data-tab="typing">Typing</button>
      <button class="tab" data-tab="challenge">Mixed challenge</button>
      <button class="tab" data-tab="wordbank">Word bank</button>
    </div>

    <section class="panel active" id="flashcards">
      <h2>Flashcards</h2>
      <p class="small">Read the German word, guess the English word, then flip the card.</p>
      <div class="controls">
        <select id="flashCategory"></select>
        <button class="btn secondary" id="prevFlash">Previous</button>
        <button class="btn" id="flipFlash">Flip card</button>
        <button class="btn secondary" id="nextFlash">Next</button>
        <button class="btn warn" id="shuffleFlash">Shuffle</button>
      </div>
      <div class="card">
        <span class="pill" id="flashMeta"></span>
        <div class="prompt" id="flashPrompt"></div>
        <div class="feedback" id="flashAnswer"></div>
      </div>
    </section>

    <section class="panel" id="multiple">
      <h2>Multiple choice</h2>
      <p class="small">Choose the correct English translation.</p>
      <div class="progress"><div class="bar" id="mcBar"></div></div>
      <div class="card">
        <span class="pill" id="mcMeta"></span>
        <div class="prompt" id="mcPrompt"></div>
        <div class="grid" id="mcOptions"></div>
        <div class="feedback" id="mcFeedback"></div>
      </div>
      <div class="controls"><button class="btn secondary" id="nextMc">Next question</button></div>
    </section>

    <section class="panel" id="typing">
      <h2>Typing practice</h2>
      <p class="small">Type the English translation exactly. Minor capitalization differences are ignored.</p>
      <div class="progress"><div class="bar" id="typeBar"></div></div>
      <div class="card">
        <span class="pill" id="typeMeta"></span>
        <div class="prompt" id="typePrompt"></div>
        <div class="answer-row">
          <input id="typeInput" type="text" autocomplete="off" placeholder="Type the English word here" />
          <button class="btn" id="checkType">Check</button>
          <button class="btn secondary" id="skipType">Skip</button>
        </div>
        <div class="feedback" id="typeFeedback"></div>
      </div>
    </section>

    <section class="panel" id="challenge">
      <h2>Mixed challenge</h2>
      <p class="small">Ten random words with instant scoring.</p>
      <div class="controls">
        <button class="btn" id="startChallenge">Start new round</button>
      </div>
      <div class="card">
        <div class="progress"><div class="bar" id="challengeBar"></div></div>
        <span class="pill" id="challengeMeta"></span>
        <div class="prompt" id="challengePrompt">Press “Start new round”.</div>
        <div class="answer-row">
          <input id="challengeInput" type="text" autocomplete="off" placeholder="Type the English translation" />
          <button class="btn" id="submitChallenge">Submit</button>
        </div>
        <div class="feedback" id="challengeFeedback"></div>
        <p class="small" id="challengeScore">Score: 0/0</p>
      </div>
    </section>

    <section class="panel" id="wordbank">
      <h2>Word bank</h2>
      <p class="small">A full list for review.</p>
      <div id="wordbankContent"></div>
    </section>

    <p class="footer-note">Built as a standalone HTML file: open it in any browser and it works offline.</p>
  </div>

  <script>
    const vocab = [{"category": "Nouns", "de": "Armband", "en": "bracelet"}, {"category": "Nouns", "de": "Ohrringe", "en": "earrings"}, {"category": "Nouns", "de": "Ring", "en": "ring"}, {"category": "Nouns", "de": "Schnalle", "en": "buckle"}, {"category": "Nouns", "de": "Münze", "en": "coin"}, {"category": "Nouns", "de": "Gold", "en": "gold"}, {"category": "Nouns", "de": "Silber", "en": "silver"}, {"category": "Nouns", "de": "Helm", "en": "helmet"}, {"category": "Nouns", "de": "Statue", "en": "statue"}, {"category": "Nouns", "de": "Schild", "en": "shield"}, {"category": "Nouns", "de": "Säge", "en": "saw"}, {"category": "Nouns", "de": "Armee", "en": "army"}, {"category": "Nouns", "de": "Kleider", "en": "clothes"}, {"category": "Nouns", "de": "Bart", "en": "beard"}, {"category": "Nouns", "de": "Ehemann", "en": "husband"}, {"category": "Nouns", "de": "Macht / Kraft", "en": "power"}, {"category": "Nouns", "de": "König", "en": "king"}, {"category": "Nouns", "de": "Königin", "en": "queen"}, {"category": "Nouns", "de": "Händler", "en": "trader"}, {"category": "Nouns", "de": "Bauer / Landwirt", "en": "farmer"}, {"category": "Nouns", "de": "Soldat", "en": "soldier"}, {"category": "Nouns", "de": "Dieb", "en": "thief"}, {"category": "Nouns", "de": "Diener", "en": "servant"}, {"category": "Nouns", "de": "Ägypten", "en": "Egypt"}, {"category": "Nouns", "de": "Ägypter", "en": "Egyptians"}, {"category": "Nouns", "de": "Vergangenheit", "en": "past"}, {"category": "Nouns", "de": "Geschichte / Vergangenheit", "en": "history"}, {"category": "Nouns", "de": "Sprache", "en": "language"}, {"category": "Nouns", "de": "Handel", "en": "trade"}, {"category": "Nouns", "de": "Archäologie", "en": "archaeology"}, {"category": "Adjectives", "de": "interessant", "en": "interesting"}, {"category": "Adjectives", "de": "müde", "en": "tired"}, {"category": "Adjectives", "de": "aufregend", "en": "exciting"}, {"category": "Adjectives", "de": "wütend", "en": "angry"}, {"category": "Adjectives", "de": "überrascht", "en": "surprised"}, {"category": "Adjectives", "de": "besorgt", "en": "worried"}, {"category": "Adjectives", "de": "verängstigt / erschrocken", "en": "scared"}, {"category": "Adjectives", "de": "traurig", "en": "sad"}, {"category": "Adjectives", "de": "glücklich", "en": "happy"}, {"category": "Adjectives", "de": "arm", "en": "poor"}, {"category": "Adjectives", "de": "reich", "en": "rich"}, {"category": "Adjectives", "de": "heiss", "en": "hot"}, {"category": "Verbs", "de": "sich anziehen", "en": "get dressed"}, {"category": "Verbs", "de": "tragen / Kleidung", "en": "wear"}, {"category": "Verbs", "de": "bauen", "en": "build"}, {"category": "Verbs", "de": "unterstützen", "en": "support"}, {"category": "Verbs", "de": "antworten", "en": "answer"}, {"category": "Verbs", "de": "wollen", "en": "want"}, {"category": "Verbs", "de": "geniessen", "en": "enjoy"}, {"category": "Verbs", "de": "kaufen", "en": "buy"}, {"category": "Verbs", "de": "verkaufen", "en": "sell"}, {"category": "Verbs", "de": "warten", "en": "wait"}, {"category": "Verbs", "de": "stehlen", "en": "steal"}, {"category": "Verbs", "de": "entdecken", "en": "discover"}, {"category": "Verbs", "de": "handeln", "en": "trade"}, {"category": "Verbs", "de": "verschwinden", "en": "disappear"}, {"category": "Verbs", "de": "ankommen", "en": "arrive"}, {"category": "Verbs", "de": "Warten", "en": "wait"}];
    const categories = ['All', 'Nouns', 'Adjectives', 'Verbs'];

    document.getElementById('totalWords').textContent = vocab.length;
    document.getElementById('nounCount').textContent = vocab.filter(v => v.category === 'Nouns').length;
    document.getElementById('adjCount').textContent = vocab.filter(v => v.category === 'Adjectives').length;
    document.getElementById('verbCount').textContent = vocab.filter(v => v.category === 'Verbs').length;

    const norm = s => s.toLowerCase().trim().replace(/\s+/g,' ');
    const byCat = cat => cat === 'All' ? [...vocab] : vocab.filter(v => v.category === cat);
    const shuffle = arr => [...arr].sort(() => Math.random() - 0.5);

    document.querySelectorAll('.tab').forEach(btn => btn.addEventListener('click', () => {
      document.querySelectorAll('.tab').forEach(b => b.classList.remove('active'));
      document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
      btn.classList.add('active');
      document.getElementById(btn.dataset.tab).classList.add('active');
    }));

    const flashCategory = document.getElementById('flashCategory');
    categories.forEach(c => { const o = document.createElement('option'); o.value=c; o.textContent=c; flashCategory.appendChild(o); });
    let flashDeck = byCat('All'), flashIndex = 0, flashFlipped = false;
    function renderFlash() {
      if (!flashDeck.length) return;
      const item = flashDeck[flashIndex];
      document.getElementById('flashMeta').textContent = `${item.category} • ${flashIndex+1} / ${flashDeck.length}`;
      document.getElementById('flashPrompt').textContent = item.de;
      document.getElementById('flashAnswer').textContent = flashFlipped ? item.en : 'Think first, then press “Flip card”.';
      document.getElementById('flashAnswer').className = 'feedback';
    }
    flashCategory.addEventListener('change', () => { flashDeck = byCat(flashCategory.value); flashIndex = 0; flashFlipped = false; renderFlash(); });
    document.getElementById('flipFlash').addEventListener('click', () => { flashFlipped = !flashFlipped; renderFlash(); });
    document.getElementById('nextFlash').addEventListener('click', () => { flashIndex = (flashIndex + 1) % flashDeck.length; flashFlipped = false; renderFlash(); });
    document.getElementById('prevFlash').addEventListener('click', () => { flashIndex = (flashIndex - 1 + flashDeck.length) % flashDeck.length; flashFlipped = false; renderFlash(); });
    document.getElementById('shuffleFlash').addEventListener('click', () => { flashDeck = shuffle(flashDeck); flashIndex = 0; flashFlipped = false; renderFlash(); });
    renderFlash();

    let mcPool = shuffle(vocab), mcIndex = 0, mcAnswered = false;
    function renderMc() {
      const item = mcPool[mcIndex % mcPool.length];
      const distractors = shuffle(vocab.filter(v => v.en !== item.en)).slice(0,3).map(v => v.en);
      const opts = shuffle([item.en, ...distractors]);
      document.getElementById('mcMeta').textContent = `${item.category} • Question ${mcIndex+1}`;
      document.getElementById('mcPrompt').textContent = item.de;
      document.getElementById('mcFeedback').textContent = '';
      const box = document.getElementById('mcOptions'); box.innerHTML = '';
      mcAnswered = false;
      opts.forEach(opt => {
        const b = document.createElement('button');
        b.className = 'option'; b.textContent = opt;
        b.addEventListener('click', () => {
          if (mcAnswered) return; mcAnswered = true;
          if (opt === item.en) { b.classList.add('correct'); document.getElementById('mcFeedback').textContent = 'Correct!'; document.getElementById('mcFeedback').className='feedback good'; }
          else { b.classList.add('wrong'); [...box.children].forEach(x => { if (x.textContent === item.en) x.classList.add('correct'); }); document.getElementById('mcFeedback').textContent = `Not quite. Correct answer: ${item.en}`; document.getElementById('mcFeedback').className='feedback bad'; }
          document.getElementById('mcBar').style.width = `${((mcIndex+1)/mcPool.length)*100}%`;
        });
        box.appendChild(b);
      });
    }
    document.getElementById('nextMc').addEventListener('click', () => { mcIndex = (mcIndex + 1) % mcPool.length; renderMc(); });
    renderMc();

    let typePool = shuffle(vocab), typeIndex = 0;
    function renderType() {
      const item = typePool[typeIndex % typePool.length];
      document.getElementById('typeMeta').textContent = `${item.category} • Word ${typeIndex+1} / ${typePool.length}`;
      document.getElementById('typePrompt').textContent = item.de;
      document.getElementById('typeInput').value = '';
      document.getElementById('typeFeedback').textContent = '';
      document.getElementById('typeFeedback').className = 'feedback';
      document.getElementById('typeBar').style.width = `${(typeIndex/typePool.length)*100}%`;
      document.getElementById('typeInput').focus();
    }
    function checkType() {
      const item = typePool[typeIndex % typePool.length];
      const guess = norm(document.getElementById('typeInput').value);
      if (!guess) return;
      if (guess === norm(item.en)) { document.getElementById('typeFeedback').textContent = 'Correct!'; document.getElementById('typeFeedback').className='feedback good'; }
      else { document.getElementById('typeFeedback').textContent = `Try again. Correct answer: ${item.en}`; document.getElementById('typeFeedback').className='feedback bad'; }
      typeIndex = (typeIndex + 1) % typePool.length;
      setTimeout(renderType, 900);
    }
    document.getElementById('checkType').addEventListener('click', checkType);
    document.getElementById('typeInput').addEventListener('keydown', e => { if (e.key === 'Enter') checkType(); });
    document.getElementById('skipType').addEventListener('click', () => {
      const item = typePool[typeIndex % typePool.length];
      document.getElementById('typeFeedback').textContent = `Skipped. Correct answer: ${item.en}`;
      document.getElementById('typeFeedback').className='feedback bad';
      typeIndex = (typeIndex + 1) % typePool.length;
      setTimeout(renderType, 900);
    });
    renderType();

    let challengeSet = [], challengeIndex = 0, challengeScore = 0;
    function startChallenge() {
      challengeSet = shuffle(vocab).slice(0,10);
      challengeIndex = 0; challengeScore = 0;
      document.getElementById('challengeScore').textContent = 'Score: 0/10';
      document.getElementById('challengeFeedback').textContent = '';
      renderChallenge();
    }
    function renderChallenge() {
      if (!challengeSet.length) return;
      if (challengeIndex >= challengeSet.length) {
        document.getElementById('challengePrompt').textContent = `Round finished! Final score: ${challengeScore} / ${challengeSet.length}`;
        document.getElementById('challengeMeta').textContent = 'Completed';
        document.getElementById('challengeBar').style.width = '100%';
        return;
      }
      const item = challengeSet[challengeIndex];
      document.getElementById('challengeMeta').textContent = `${item.category} • ${challengeIndex+1} / ${challengeSet.length}`;
      document.getElementById('challengePrompt').textContent = item.de;
      document.getElementById('challengeInput').value = '';
      document.getElementById('challengeFeedback').textContent = '';
      document.getElementById('challengeBar').style.width = `${(challengeIndex/challengeSet.length)*100}%`;
      document.getElementById('challengeInput').focus();
    }
    function submitChallenge() {
      if (!challengeSet.length || challengeIndex >= challengeSet.length) return;
      const item = challengeSet[challengeIndex];
      const guess = norm(document.getElementById('challengeInput').value);
      if (!guess) return;
      if (guess === norm(item.en)) { challengeScore++; document.getElementById('challengeFeedback').textContent = 'Correct!'; document.getElementById('challengeFeedback').className='feedback good'; }
      else { document.getElementById('challengeFeedback').textContent = `Correct answer: ${item.en}`; document.getElementById('challengeFeedback').className='feedback bad'; }
      challengeIndex++;
      document.getElementById('challengeScore').textContent = `Score: ${challengeScore} / 10`;
      setTimeout(renderChallenge, 700);
    }
    document.getElementById('startChallenge').addEventListener('click', startChallenge);
    document.getElementById('submitChallenge').addEventListener('click', submitChallenge);
    document.getElementById('challengeInput').addEventListener('keydown', e => { if (e.key === 'Enter') submitChallenge(); });

    const wb = document.getElementById('wordbankContent');
    ['Nouns','Adjectives','Verbs'].forEach(cat => {
      const section = document.createElement('div');
      section.style.marginBottom = '18px';
      section.innerHTML = `<h3 style="margin-bottom:10px;">${cat}</h3>`;
      const list = document.createElement('div');
      list.className = 'pair-list';
      vocab.filter(v => v.category === cat).forEach(item => {
        const row = document.createElement('div');
        row.className = 'pair';
        row.innerHTML = `<strong>${item.de}</strong><span>${item.en}</span>`;
        list.appendChild(row);
      });
      section.appendChild(list);
      wb.appendChild(section);
    });
  </script>
</body>
</html>
