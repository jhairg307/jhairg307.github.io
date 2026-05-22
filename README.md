<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Malanguita 🌸</title>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;1,300;1,400&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    :root {
      --rose:  #e8a4b0;
      --mauve: #c47a8a;
      --deep:  #7a3d4e;
      --cream: #fdf6f0;
      --gold:  #d4a96a;
      --text:  #4a2530;
    }
    body {
      font-family: 'DM Sans', sans-serif;
      background: var(--cream);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 2rem;
      overflow: hidden;
      position: relative;
    }
    #petals-canvas {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 0;
    }
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background: radial-gradient(ellipse 80% 60% at 50% 40%, rgba(232,164,176,0.3) 0%, transparent 70%);
      z-index: 0;
      pointer-events: none;
    }
    .wrap {
      position: relative;
      z-index: 1;
      text-align: center;
      max-width: 560px;
      width: 100%;
    }
    .sorry-text {
      font-family: 'Cormorant Garamond', serif;
      font-size: clamp(1.6rem, 5vw, 2.8rem);
      font-weight: 300;
      color: var(--deep);
      line-height: 1.5;
      margin-bottom: 0.6rem;
      animation: fadeUp 1s 0.3s both;
    }
    .sorry-text em { font-style: italic; color: var(--mauve); }
    .sorry-sub {
      font-family: 'Cormorant Garamond', serif;
      font-style: italic;
      font-size: clamp(1rem, 2.5vw, 1.25rem);
      color: var(--mauve);
      margin-bottom: 3rem;
      animation: fadeUp 1s 0.6s both;
    }
    .question-box {
      background: rgba(255,255,255,0.75);
      backdrop-filter: blur(14px);
      border: 1px solid rgba(232,164,176,0.35);
      border-radius: 20px;
      padding: 2.5rem 2rem 2rem;
      box-shadow: 0 8px 40px rgba(122,61,78,0.1);
      animation: fadeUp 1s 0.9s both;
      position: relative;
      min-height: 190px;
    }
    .question {
      font-family: 'Cormorant Garamond', serif;
      font-size: clamp(1.5rem, 4vw, 2.2rem);
      font-weight: 400;
      color: var(--deep);
      margin-bottom: 2.2rem;
      transition: opacity 0.3s;
    }
    .btn-area {
      position: relative;
      height: 90px;
    }
    .btn {
      border: none;
      border-radius: 50px;
      font-family: 'DM Sans', sans-serif;
      font-size: 1rem;
      font-weight: 500;
      cursor: pointer;
      padding: 0.75rem 2rem;
      position: absolute;
      white-space: nowrap;
      transition: box-shadow 0.2s, transform 0.2s;
    }
    .btn-yes {
      background: linear-gradient(135deg, var(--mauve), var(--deep));
      color: #fff;
      box-shadow: 0 4px 20px rgba(196,122,138,0.45);
      left: 30%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
    .btn-yes:hover { box-shadow: 0 6px 28px rgba(196,122,138,0.6); transform: translate(-50%,-50%) scale(1.06); }
    .btn-no {
      background: rgba(232,164,176,0.18);
      color: var(--mauve);
      border: 1px solid rgba(196,122,138,0.3);
      left: 70%;
      top: 50%;
      transform: translate(-50%, -50%);
      transition: left 0.15s, top 0.15s;
    }

    /* HEART OVERLAY */
    #heart-overlay {
      display: none;
      position: fixed;
      inset: 0;
      z-index: 100;
      align-items: center;
      justify-content: center;
      pointer-events: none;
    }
    #heart-overlay.show { display: flex; }
    .heart-svg {
      position: absolute;
      animation: heartGrow 3.2s cubic-bezier(0.22,1,0.36,1) forwards;
    }
    .tqm-text {
      position: absolute;
      font-family: 'Cormorant Garamond', serif;
      font-size: clamp(3rem, 12vw, 8rem);
      font-weight: 400;
      color: #fff;
      text-shadow: 0 0 60px rgba(122,61,78,0.5);
      opacity: 0;
      animation: tqmAppear 0.8s 2.4s forwards;
      letter-spacing: 0.08em;
      z-index: 2;
    }
    @keyframes heartGrow {
      0%   { transform: scale(0); opacity: 0; }
      15%  { opacity: 1; }
      100% { transform: scale(20); opacity: 1; }
    }
    @keyframes tqmAppear {
      from { opacity: 0; transform: scale(0.7); }
      to   { opacity: 1; transform: scale(1); }
    }
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(22px); }
      to   { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>

<canvas id="petals-canvas"></canvas>

<div class="wrap">
  <p class="sorry-text">Perdóname por <em>joderte</em> tanto,<br/>Malanguita 🌸</p>
  <p class="sorry-sub">Te lo juro que no lo vuelvo a hacer… bueno, lo intentaré.</p>

  <div class="question-box" id="qbox">
    <p class="question" id="qtext">¿Me perdonas?</p>
    <div class="btn-area" id="btnArea">
      <button class="btn btn-yes" id="btnYes">Sí</button>
      <button class="btn btn-no"  id="btnNo">No</button>
    </div>
  </div>
</div>

<div id="heart-overlay">
  <svg class="heart-svg" viewBox="0 0 200 185" width="150" height="150" xmlns="http://www.w3.org/2000/svg">
    <path d="M100 165 C100 165 8 102 8 50 C8 22 28 5 54 5 C71 5 87 14 100 30 C113 14 129 5 146 5 C172 5 192 22 192 50 C192 102 100 165 100 165Z" fill="#e8a4b0"/>
  </svg>
  <span class="tqm-text">TQM 🌸</span>
</div>

<script>
  /* PETALS */
  const canvas = document.getElementById('petals-canvas');
  const ctx = canvas.getContext('2d');
  function resize(){ canvas.width=innerWidth; canvas.height=innerHeight; }
  resize(); window.addEventListener('resize', resize);
  const COLORS=['#e8a4b0','#f5d6dd','#c47a8a','#d4a96a'];
  const petals=Array.from({length:22},()=>({
    x:Math.random()*innerWidth, y:Math.random()*innerHeight-innerHeight,
    r:4+Math.random()*6, color:COLORS[Math.floor(Math.random()*COLORS.length)],
    sy:.5+Math.random()*.8, sx:(Math.random()-.5)*.5,
    angle:Math.random()*Math.PI*2, spin:(Math.random()-.5)*.03,
    op:.3+Math.random()*.5, wb:Math.random()*Math.PI*2, ws:.01+Math.random()*.015
  }));
  (function loop(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    petals.forEach(p=>{
      p.wb+=p.ws; p.x+=p.sx+Math.sin(p.wb)*.4; p.y+=p.sy; p.angle+=p.spin;
      if(p.y>canvas.height+20){p.y=-20; p.x=Math.random()*canvas.width;}
      ctx.save(); ctx.globalAlpha=p.op; ctx.translate(p.x,p.y); ctx.rotate(p.angle);
      ctx.fillStyle=p.color; ctx.beginPath();
      ctx.ellipse(0,0,p.r*1.6,p.r,0,0,Math.PI*2); ctx.fill(); ctx.restore();
    });
    requestAnimationFrame(loop);
  })();

  /* LOGIC */
  const btnYes  = document.getElementById('btnYes');
  const btnNo   = document.getElementById('btnNo');
  const qtext   = document.getElementById('qtext');
  const btnArea = document.getElementById('btnArea');
  const overlay = document.getElementById('heart-overlay');

  const questions = [
    '¿Me perdonas?',
    '¿Segura que me perdonas?',
    '¿Pero segurísima?',
    '¿100% segura segura segura?'
  ];
  const yesLabels = ['Sí','¿Segura?','¿Segurísima?','¿Estás segura segura?','¡Acepto! 🌸'];

  let step = 0;
  let done = false;

  /* Escape "No" button */
  function escapeTo(el, areaEl) {
    const aw = areaEl.offsetWidth;
    const ah = areaEl.offsetHeight;
    const bw = el.offsetWidth  || 72;
    const bh = el.offsetHeight || 42;

    /* keep away from yes button center (~30% left) */
    let lp, tp, tries=0;
    do {
      lp = 10 + Math.random() * 75;
      tp = 10 + Math.random() * 72;
      tries++;
    } while(Math.abs(lp - 30) < 28 && tries < 40);

    el.style.left      = lp + '%';
    el.style.top       = tp + '%';
    el.style.transform = 'translate(-50%, -50%)';
  }

  btnNo.addEventListener('mouseenter', () => escapeTo(btnNo, btnArea));
  btnNo.addEventListener('touchstart', (e) => { e.preventDefault(); escapeTo(btnNo, btnArea); });

  btnYes.addEventListener('click', () => {
    if (done) return;
    step++;
    if (step < yesLabels.length - 1) {
      btnYes.textContent = yesLabels[step];
      qtext.style.opacity = '0';
      setTimeout(() => {
        qtext.textContent  = questions[step] || questions[questions.length-1];
        qtext.style.opacity = '1';
      }, 250);
    } else {
      done = true;
      btnYes.textContent = yesLabels[yesLabels.length - 1];
      btnNo.style.display = 'none';
      setTimeout(() => overlay.classList.add('show'), 700);
    }
  });
</script>
</body>
</html>
