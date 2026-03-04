<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tobías Albirosa</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700;900&family=Roboto+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080810;
    --surface: #0f0f1a;
    --surface2: #141425;
    --border: #1c1c30;
    --accent: #00e5a0;
    --accent2: #ff4d7d;
    --accent3: #4d9fff;
    --accent4: #ffd54d;
    --text: #dde0f0;
    --muted: #44445a;
    --muted2: #666680;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  html { scroll-behavior: smooth; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Roboto', sans-serif;
    font-weight: 400;
    min-height: 100vh;
    overflow-x: hidden;
  }
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
    pointer-events: none;
    z-index: 9999;
  }
  .container { max-width: 860px; margin: 0 auto; padding: 0 24px 120px; }

  /* ── HERO ── */
  .hero {
    padding: 100px 0 60px;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
  }
  .hero-glow {
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%,-50%);
    width: 600px; height: 220px;
    background: radial-gradient(ellipse, rgba(0,229,160,0.06) 0%, transparent 70%);
    pointer-events: none;
    filter: blur(25px);
  }

  /* Name row — perspective for 3D rotateY */
  .name-row {
    display: flex;
    align-items: baseline;
    justify-content: center;
    perspective: 700px;
    perspective-origin: 50% 50%;
    gap: 0;
  }

  /* Every character */
  .char {
    display: inline-block;
    font-family: 'Roboto', sans-serif;
    font-weight: 900;
    font-size: clamp(2.8rem, 8.5vw, 5.8rem);
    line-height: 1;
    letter-spacing: -0.01em;
    transform-style: preserve-3d;
    will-change: transform, color, opacity;
  }

  /* PHASE 0: invisible, folded away */
  .char.phase-hidden {
    opacity: 0;
    transform: rotateY(90deg);
    color: var(--muted);
    transition: none;
  }

  /* PHASE 1: visible, shuffling — rotateY set by JS */
  .char.phase-shuffle {
    opacity: 1;
    color: var(--muted2);
    font-family: 'Roboto Mono', monospace;
  }

  /* PHASE 2: fold back to 90° before reveal */
  .char.phase-fold {
    opacity: 1;
    transition: transform 0.18s ease-in, color 0.15s ease;
    transform: rotateY(90deg);
    color: transparent;
  }

  /* PHASE 3: final letter, snap back */
  .char.phase-final {
    opacity: 1;
    color: var(--accent);
    font-family: 'Roboto', sans-serif;
    font-weight: 900;
    transform: rotateY(0deg);
    transition: transform 0.28s cubic-bezier(.22,.68,0,1.3), color 0.2s ease;
  }

  .char.space {
    width: 0.3em;
    opacity: 1 !important;
    transform: none !important;
    color: transparent !important;
    transition: none !important;
  }

  /* hero sub */
  .hero-sub {
    margin-top: 16px;
    font-size: 0.7rem;
    font-weight: 500;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--muted2);
    opacity: 0;
    animation: fadeIn 0.5s ease forwards;
  }
  .linkedin-pill {
    margin-top: 26px;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 10px 22px;
    border: 1px solid var(--border);
    border-radius: 100px;
    background: var(--surface);
    color: var(--accent3);
    font-size: 0.78rem;
    font-weight: 700;
    letter-spacing: 0.04em;
    text-decoration: none;
    opacity: 0;
    animation: fadeIn 0.5s ease forwards;
    transition: border-color 0.2s, background 0.2s;
  }
  .linkedin-pill:hover { border-color: var(--accent3); background: rgba(77,159,255,0.06); }
  @keyframes fadeIn { to { opacity: 1; } }

  /* ── DIVIDER ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border) 20%, var(--border) 80%, transparent);
    margin-bottom: 56px;
  }

  /* ── SECTION ── */
  .section {
    margin-bottom: 60px;
    opacity: 0;
    transform: translateY(20px);
    animation: fadeUp 0.55s ease forwards;
  }
  @keyframes fadeUp { to { opacity: 1; transform: translateY(0); } }
  .sd1{animation-delay:0.3s} .sd2{animation-delay:0.5s} .sd3{animation-delay:0.7s}
  .sd4{animation-delay:0.9s} .sd5{animation-delay:1.1s}

  .section-label {
    display: flex;
    align-items: center;
    gap: 12px;
    font-size: 0.62rem;
    font-weight: 700;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--muted2);
    margin-bottom: 22px;
  }
  .section-label .tag { color: var(--accent); font-family: 'Roboto Mono', monospace; font-size: 0.7rem; }
  .section-label::after { content: ''; flex: 1; height: 1px; background: var(--border); }

  /* ── PROJECT CARDS ── */
  .project-card {
    border: 1px solid var(--border);
    border-radius: 6px;
    overflow: hidden;
    margin-bottom: 20px;
    background: var(--surface);
    transition: border-color 0.25s;
  }
  .project-card:hover { border-color: var(--accent); }
  .card-bar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 18px;
    border-bottom: 1px solid var(--border);
    background: rgba(0,229,160,0.02);
  }
  .card-bar-left { display: flex; align-items: center; gap: 8px; }
  .card-dot { width: 7px; height: 7px; border-radius: 50%; background: var(--accent); box-shadow: 0 0 6px var(--accent); }
  .card-name { font-family: 'Roboto Mono', monospace; font-size: 0.75rem; font-weight: 700; color: var(--accent); letter-spacing: 0.04em; }
  .card-type { font-size: 0.62rem; font-weight: 500; color: var(--muted2); letter-spacing: 0.1em; text-transform: uppercase; }
  .iframe-wrap { width: 100%; height: 370px; background: #05050f; }
  .iframe-wrap iframe { width: 100%; height: 100%; border: none; display: block; }

  /* ── OSS ── */
  .oss-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  @media (max-width: 560px) { .oss-grid { grid-template-columns: 1fr; } }
  .oss-card {
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 22px 24px;
    background: var(--surface);
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s, background 0.2s;
  }
  .oss-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; width: 100%; height: 2px;
    transform: scaleX(0); transform-origin: left;
    transition: transform 0.25s ease;
  }
  .oss-card.node::before { background: var(--accent); }
  .oss-card.python::before { background: var(--accent4); }
  .oss-card:hover::before { transform: scaleX(1); }
  .oss-card:hover { border-color: var(--muted); background: var(--surface2); }
  .oss-lang { font-family: 'Roboto Mono', monospace; font-size: 0.6rem; font-weight: 700; letter-spacing: 0.2em; text-transform: uppercase; margin-bottom: 10px; }
  .oss-card.node .oss-lang { color: var(--accent); }
  .oss-card.python .oss-lang { color: var(--accent4); }
  .oss-title { font-size: 1.3rem; font-weight: 900; color: var(--text); letter-spacing: -0.01em; }

  /* ── GAMES ── */
  .games-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 14px; }
  @media (max-width: 650px) { .games-grid { grid-template-columns: 1fr; } }
  .game-card { border: 1px solid var(--border); border-radius: 6px; overflow: hidden; background: var(--surface); transition: border-color 0.2s; }
  .game-card:hover { border-color: var(--accent3); }
  .game-bar { display: flex; align-items: center; gap: 8px; padding: 10px 14px; border-bottom: 1px solid var(--border); }
  .game-led { width: 6px; height: 6px; border-radius: 50%; background: var(--accent3); box-shadow: 0 0 5px var(--accent3); }
  .game-name { font-family: 'Roboto Mono', monospace; font-size: 0.68rem; font-weight: 700; color: var(--accent3); text-transform: uppercase; letter-spacing: 0.1em; }
  .game-frame { width: 100%; height: 200px; background: #05050f; }
  .game-frame iframe { width: 100%; height: 100%; border: none; }

  /* ── CERTS ── */
  .cert-group { margin-bottom: 28px; }
  .cert-group-title {
    font-size: 0.62rem; font-weight: 700; letter-spacing: 0.2em; text-transform: uppercase;
    color: var(--muted2); margin-bottom: 10px; padding-left: 12px;
    border-left: 2px solid var(--accent2);
  }
  .cert-list { display: flex; flex-direction: column; gap: 8px; }
  .cert-item {
    display: flex; align-items: center; gap: 14px;
    padding: 14px 18px; background: var(--surface); border: 1px solid var(--border); border-radius: 5px;
    transition: border-color 0.2s, background 0.2s;
  }
  .cert-item:hover { border-color: var(--muted); background: var(--surface2); }
  .cert-issuer-badge {
    flex-shrink: 0; width: 36px; height: 36px; border-radius: 4px;
    display: flex; align-items: center; justify-content: center;
    font-size: 0.55rem; font-weight: 900; letter-spacing: 0.05em; text-transform: uppercase;
  }
  .badge-hr  { background: rgba(0,229,160,0.1); color: var(--accent); border: 1px solid rgba(0,229,160,0.2); }
  .badge-ibm { background: rgba(77,159,255,0.1); color: var(--accent3); border: 1px solid rgba(77,159,255,0.2); }
  .badge-fcc { background: rgba(255,77,125,0.1); color: var(--accent2); border: 1px solid rgba(255,77,125,0.2); }
  .badge-ft  { background: rgba(255,213,77,0.1); color: var(--accent4); border: 1px solid rgba(255,213,77,0.2); }
  .badge-ml  { background: rgba(0,229,160,0.08); color: var(--accent); border: 1px solid rgba(0,229,160,0.15); }
  .cert-info { flex: 1; min-width: 0; }
  .cert-title { font-size: 0.82rem; font-weight: 700; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; margin-bottom: 3px; }
  .cert-meta { font-size: 0.62rem; color: var(--muted2); font-weight: 400; font-family: 'Roboto Mono', monospace; }
  .cert-date { flex-shrink: 0; font-family: 'Roboto Mono', monospace; font-size: 0.6rem; color: var(--muted); font-weight: 500; }
</style>
</head>
<body>
<div class="container">

  <!-- HERO -->
  <div class="hero">
    <div class="hero-glow"></div>
    <div class="name-row" id="nameRow"></div>
    <div class="hero-sub" id="heroSub">Full Stack Developer &nbsp;·&nbsp; Open Source &nbsp;·&nbsp; AI</div>
    <a href="https://linkedin.com/in/tobiasalbirosa" target="_blank" class="linkedin-pill" id="linkedinPill">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
        <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
      </svg>
      in/tobiasalbirosa
    </a>
  </div>

  <div class="divider"></div>

  <!-- PRODUCTION 01 -->
  <div class="section sd1">
    <div class="section-label"><span class="tag">01</span> Production Projects · Online Multi-Service</div>
    <div class="project-card">
      <div class="card-bar">
        <div class="card-bar-left"><div class="card-dot"></div><span class="card-name">albibi.online</span></div>
        <span class="card-type">Multi-Service Platform</span>
      </div>
      <div class="iframe-wrap"><iframe src="https://albibi.online" loading="lazy" title="albibi.online"></iframe></div>
    </div>
  </div>

  <!-- PRODUCTION 02 -->
  <div class="section sd2">
    <div class="section-label"><span class="tag">02</span> Production Projects · Take Away &amp; Delivery</div>
    <div class="project-card">
      <div class="card-bar">
        <div class="card-bar-left">
          <div class="card-dot" style="background:var(--accent2);box-shadow:0 0 6px var(--accent2)"></div>
          <span class="card-name" style="color:var(--accent2)">delivip.net</span>
        </div>
        <span class="card-type">Take Away &amp; Delivery</span>
      </div>
      <div class="iframe-wrap"><iframe src="https://delivip.net" loading="lazy" title="delivip.net"></iframe></div>
    </div>
  </div>

  <!-- OPEN SOURCE -->
  <div class="section sd3">
    <div class="section-label"><span class="tag">03</span> Open Source Projects</div>
    <div class="oss-grid">
      <div class="oss-card node"><div class="oss-lang">NodeJS</div><div class="oss-title">Registrator</div></div>
      <div class="oss-card python"><div class="oss-lang">Python</div><div class="oss-title">Uroboros</div></div>
    </div>
  </div>

  <!-- VIDEOGAMES -->
  <div class="section sd4">
    <div class="section-label"><span class="tag">04</span> Videogames</div>
    <div class="games-grid">
      <div class="game-card">
        <div class="game-bar"><div class="game-led"></div><span class="game-name">Miner</span></div>
        <div class="game-frame"><iframe src="https://albibi.online/miner" loading="lazy" title="Miner"></iframe></div>
      </div>
      <div class="game-card">
        <div class="game-bar">
          <div class="game-led" style="background:var(--accent2);box-shadow:0 0 5px var(--accent2)"></div>
          <span class="game-name" style="color:var(--accent2)">Roulette</span>
        </div>
        <div class="game-frame"><iframe src="https://albibi.online/roulette" loading="lazy" title="Roulette"></iframe></div>
      </div>
      <div class="game-card">
        <div class="game-bar">
          <div class="game-led" style="background:var(--accent4);box-shadow:0 0 5px var(--accent4)"></div>
          <span class="game-name" style="color:var(--accent4)">Shooter</span>
        </div>
        <div class="game-frame"><iframe src="https://albibi.online/shooter" loading="lazy" title="Shooter"></iframe></div>
      </div>
    </div>
  </div>

  <!-- CERTIFICATIONS -->
  <div class="section sd5">
    <div class="section-label"><span class="tag">05</span> Licenses &amp; Certifications</div>

    <div class="cert-group">
      <div class="cert-group-title">Software Engineering</div>
      <div class="cert-list">
        <div class="cert-item">
          <div class="cert-issuer-badge badge-hr">HR</div>
          <div class="cert-info"><div class="cert-title">Software Engineer Certificate</div><div class="cert-meta">HackerRank · c54408f7a283</div></div>
          <div class="cert-date">mar 2026</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-fcc">fCC</div>
          <div class="cert-info"><div class="cert-title">JavaScript Algorithms and Data Structures</div><div class="cert-meta">freeCodeCamp · iamcronos-jaads</div></div>
          <div class="cert-date">jun 2025</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ml">ML</div>
          <div class="cert-info"><div class="cert-title">Checkout Pro</div><div class="cert-meta">Mercado Libre · cert_d0e8c4fc…</div></div>
          <div class="cert-date">ene 2025</div>
        </div>
      </div>
    </div>

    <div class="cert-group">
      <div class="cert-group-title" style="border-left-color:var(--accent3)">Artificial Intelligence</div>
      <div class="cert-list">
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ibm">IBM</div>
          <div class="cert-info"><div class="cert-title">Unleashing the Power of AI Agents</div><div class="cert-meta">IBM · ALM-COURSE_3825456</div></div>
          <div class="cert-date">oct 2025</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ibm">IBM</div>
          <div class="cert-info"><div class="cert-title">An Introduction to Generative AI and Content Creation</div><div class="cert-meta">IBM · ALM-COURSE_3826560</div></div>
          <div class="cert-date">ene 2026</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ibm">IBM</div>
          <div class="cert-info"><div class="cert-title">IBM and NASA: Powering AI for a Safer Planet</div><div class="cert-meta">IBM · ALM-COURSE_4061528</div></div>
          <div class="cert-date">ene 2026</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ibm">IBM</div>
          <div class="cert-info"><div class="cert-title">Vector Databases: An Introduction with Chroma DB</div><div class="cert-meta">IBM · 5AWBP1A4O3C7</div></div>
          <div class="cert-date">feb 2026</div>
        </div>
      </div>
    </div>

    <div class="cert-group">
      <div class="cert-group-title" style="border-left-color:var(--accent4)">Communication &amp; Security</div>
      <div class="cert-list">
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ibm">IBM</div>
          <div class="cert-info"><div class="cert-title">Course 1: Communication Skills</div><div class="cert-meta">IBM · ILB-EKRGXPZQVNYJ29BZ</div></div>
          <div class="cert-date">feb 2026</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-fcc">fCC</div>
          <div class="cert-info"><div class="cert-title">A2 English for Developers</div><div class="cert-meta">freeCodeCamp · iamcronos-a2efd</div></div>
          <div class="cert-date">feb 2026</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ft">FT</div>
          <div class="cert-info"><div class="cert-title">AR Introducción a Ciberseguridad — FTM Ed 7</div><div class="cert-meta">Fundación Telefónica · 387f7c16…</div></div>
          <div class="cert-date">abr 2025</div>
        </div>
        <div class="cert-item">
          <div class="cert-issuer-badge badge-ft">FT</div>
          <div class="cert-info"><div class="cert-title">Introducción a la Seguridad de la Información</div><div class="cert-meta">Fundación Telefónica · 69595112…</div></div>
          <div class="cert-date">—</div>
        </div>
      </div>
    </div>
  </div>

</div>

<script>
(function(){
  const NAME = "Tobías Albirosa";
  const POOL = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%&*?/<>|~^+=";
  const row = document.getElementById('nameRow');
  const spans = [];
  const ivals = {};

  // Build spans
  NAME.split('').forEach((ch, i) => {
    const s = document.createElement('span');
    if (ch === ' ') {
      s.className = 'char space';
      s.innerHTML = '&nbsp;';
    } else {
      s.className = 'char phase-hidden';
      s.dataset.final = ch;
    }
    row.appendChild(s);
    spans.push(s);
  });

  // After one frame, start shuffling all at once
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      spans.forEach((s, i) => {
        if (s.classList.contains('space')) return;
        // Switch to shuffle phase
        s.classList.remove('phase-hidden');
        s.classList.add('phase-shuffle');
        s.textContent = POOL[Math.floor(Math.random() * POOL.length)];

        // Shuffle loop
        ivals[i] = setInterval(() => {
          const deg = (Math.random() > 0.5 ? 1 : -1) * (10 + Math.floor(Math.random() * 60));
          s.style.transform = `rotateY(${deg}deg)`;
          s.textContent = POOL[Math.floor(Math.random() * POOL.length)];
        }, 85);
      });
    });
  });

  // Reveal chars one per second (skip spaces for counting)
  let revealIdx = 0;
  spans.forEach((s, i) => {
    if (s.classList.contains('space')) return;
    revealIdx++;
    const delay = revealIdx * 1000;

    setTimeout(() => {
      // Stop shuffle
      clearInterval(ivals[i]);

      // Fold away
      s.classList.remove('phase-shuffle');
      s.classList.add('phase-fold');
      s.style.transform = 'rotateY(90deg)';

      // After fold, show real char and unfold
      setTimeout(() => {
        s.textContent = s.dataset.final;
        s.style.transform = ''; // let CSS handle it
        s.classList.remove('phase-fold');
        s.classList.add('phase-final');
      }, 200);

    }, delay);
  });

  // Delay sub-elements after last reveal
  const totalDelay = (revealIdx * 1000 + 600) / 1000;
  document.getElementById('heroSub').style.animationDelay = totalDelay + 's';
  document.getElementById('linkedinPill').style.animationDelay = (totalDelay + 0.4) + 's';
})();
</script>
</body>
</html>
