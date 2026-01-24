<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Signals — CSOC × USC</title>

  <!-- keep your existing stylesheet -->
  <link rel="stylesheet" href="style.css" />

  <!-- page-local overrides: layout + logo size + star ring -->
  <style>
    :root{
      --bg0:#070a10;
      --bg1:#0b1020;
      --card:rgba(255,255,255,.06);
      --card2:rgba(255,255,255,.08);
      --line:rgba(255,255,255,.10);
      --txt:rgba(255,255,255,.92);
      --muted:rgba(255,255,255,.70);
      --muted2:rgba(255,255,255,.55);
      --blue:#79c6ff;
      --shadow: 0 18px 60px rgba(0,0,0,.55);
      --radius: 18px;
      --max: 980px;
    }

    /* background */
    body{
      margin:0;
      color:var(--txt);
      background:
        radial-gradient(1200px 700px at 50% -10%, rgba(121,198,255,.18), transparent 60%),
        radial-gradient(900px 600px at 10% 20%, rgba(255,255,255,.06), transparent 55%),
        radial-gradient(900px 600px at 90% 30%, rgba(255,255,255,.05), transparent 55%),
        linear-gradient(180deg, var(--bg0), var(--bg1));
      min-height:100vh;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji","Segoe UI Emoji";
      overflow-x:hidden;
    }

    /* top nav */
    header.top{
      position:relative;
      padding: 22px 16px 10px;
    }
    .top-inner{
      max-width: var(--max);
      margin: 0 auto;
      display:flex;
      align-items:center;
      justify-content:center;
      flex-direction:column;
      gap: 14px;
    }

    /* logo ring */
    .logo-wrap{
      position:relative;
      width: 128px;
      height: 128px;
      display:grid;
      place-items:center;
      margin-top: 6px;
      margin-bottom: 2px;
      filter: drop-shadow(0 18px 40px rgba(0,0,0,.55));
    }
    .logo-img{
      width: 92px;   /* bigger logo */
      height: 92px;
      border-radius: 999px;
      display:block;
      z-index: 3;
    }
    /* rotating halo */
    .logo-wrap::before{
      content:"";
      position:absolute;
      inset: 0;
      border-radius: 999px;
      background:
        radial-gradient(circle at 50% 50%, rgba(121,198,255,.18), rgba(121,198,255,.05) 55%, transparent 62%),
        conic-gradient(from 0deg,
          rgba(121,198,255,.0),
          rgba(121,198,255,.35),
          rgba(255,255,255,.10),
          rgba(121,198,255,.30),
          rgba(121,198,255,.0)
        );
      mask: radial-gradient(circle, transparent 46%, #000 50%, #000 72%, transparent 76%);
      opacity:.95;
      animation: spin 10s linear infinite;
      z-index: 1;
    }
    /* star dots ring */
    .logo-wrap::after{
      content:"";
      position:absolute;
      inset: -8px;
      border-radius: 999px;
      background:
        radial-gradient(circle at 20% 30%, rgba(255,255,255,.85) 0 1.2px, transparent 1.4px),
        radial-gradient(circle at 70% 24%, rgba(255,255,255,.75) 0 1px, transparent 1.2px),
        radial-gradient(circle at 82% 58%, rgba(255,255,255,.65) 0 1.1px, transparent 1.3px),
        radial-gradient(circle at 58% 84%, rgba(255,255,255,.75) 0 1.2px, transparent 1.4px),
        radial-gradient(circle at 28% 76%, rgba(255,255,255,.60) 0 1px, transparent 1.2px),
        radial-gradient(circle at 12% 54%, rgba(255,255,255,.70) 0 1.1px, transparent 1.3px),
        radial-gradient(circle at 44% 10%, rgba(255,255,255,.55) 0 1px, transparent 1.2px),
        radial-gradient(circle at 92% 40%, rgba(255,255,255,.55) 0 1px, transparent 1.2px);
      opacity:.95;
      animation: twinkle 2.6s ease-in-out infinite;
      z-index: 2;
      pointer-events:none;
    }
    @keyframes spin{ to{ transform: rotate(360deg);} }
    @keyframes twinkle{
      0%,100%{ opacity:.75; transform: scale(1); }
      50%{ opacity:1; transform: scale(1.01); }
    }

    /* nav links */
    nav{
      display:flex;
      gap: 18px;
      flex-wrap:wrap;
      justify-content:center;
      align-items:center;
      font-size: 14px;
    }
    nav a{
      color: rgba(121,198,255,.92);
      text-decoration:none;
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid transparent;
      transition: .18s ease;
      background: transparent;
    }
    nav a:hover{
      border-color: rgba(121,198,255,.25);
      background: rgba(121,198,255,.06);
    }
    nav a.active{
      color: rgba(255,255,255,.92);
      border-color: rgba(255,255,255,.16);
      background: rgba(255,255,255,.06);
    }

    /* page container */
    main.page{
      max-width: var(--max);
      margin: 0 auto;
      padding: 18px 16px 46px;
    }

    .hero{
      text-align:center;
      padding: 18px 10px 26px;
    }
    .hero h1{
      margin: 8px 0 10px;
      font-size: clamp(34px, 4.6vw, 56px);
      letter-spacing: .4px;
      line-height: 1.05;
    }
    .hero .lead{
      margin: 0 auto;
      max-width: 860px;
      color: var(--muted);
      font-size: 16px;
      line-height: 1.65;
    }
    .hero .meta{
      margin-top: 14px;
      color: var(--muted2);
      font-size: 13px;
    }

    /* cards */
    .grid{
      display:grid;
      grid-template-columns: 1fr;
      gap: 16px;
      margin-top: 18px;
    }
    .card{
      background: linear-gradient(180deg, var(--card2), var(--card));
      border: 1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 18px 18px 16px;
      backdrop-filter: blur(8px);
    }
    .card h2{
      margin: 0 0 10px;
      font-size: 18px;
      letter-spacing: .2px;
      color: rgba(121,198,255,.95);
    }
    .card p{
      margin: 0 0 10px;
      color: rgba(255,255,255,.86);
      line-height: 1.72;
      font-size: 15px;
    }
    .collapse{
      margin-top: 10px;
      padding-top: 10px;
      border-top: 1px dashed rgba(255,255,255,.16);
      color: rgba(255,255,255,.72);
      font-size: 14px;
      line-height: 1.7;
    }

    /* footer */
    footer.footer{
      max-width: var(--max);
      margin: 0 auto;
      padding: 18px 16px 34px;
      color: rgba(255,255,255,.55);
      font-size: 13px;
      text-align:center;
    }
    footer.footer a{
      color: rgba(121,198,255,.9);
      text-decoration:none;
      border-bottom: 1px dotted rgba(121,198,255,.35);
    }
  </style>
</head>

<body>

  <header class="top">
    <div class="top-inner">

      <!-- LOGO -->
      <div class="logo-wrap" aria-label="Ω-Structure Institute logo">
        <img class="logo-img" src="logo.svg" alt="Ω" />
      </div>

      <!-- NAV (IMPORTANT: Signals points to signals.html, not signals.md) -->
      <nav>
        <a href="index.html">Home</a>
        <a class="active" href="signals.html">Signals</a>
        <a href="source.html">Source</a>
        <a href="irreversibility.html">Irreversibility</a>
        <a href="fork.html">Fork</a>
        <a href="csoc.html">CSOC</a>
        <a href="systems.html">Structural Systems</a>
      </nav>

    </div>
  </header>

  <main class="page">

    <section class="hero">
      <h1>Public Signal Nodes</h1>

      <p class="lead">
        This page records a set of public structural statements.
        It does not introduce a project, theory, or proposal.
        No interpretation is provided. No discussion is expected.
      </p>

      <div class="meta">
        CSOC × USC — Source-level generative structure. No introduction provided.
      </div>
    </section>

    <section class="grid">

      <article class="card">
        <h2>Public Node #1 — Code-First Execution</h2>
        <p>Events are not primary causes. They are execution traces of deeper generative rules.</p>
        <div class="collapse">
          If no rule-level operators can explain cross-domain regularities, this structure collapses.
        </div>
      </article>

      <article class="card">
        <h2>Public Node #2 — Finite Alphabet Constraint</h2>
        <p>A finite primitive alphabet constrains all higher-level diversity. Complexity emerges through composition, not expansion of primitives.</p>
        <div class="collapse">
          If unlimited novelty requires unlimited primitives, this structure collapses.
        </div>
      </article>

      <article class="card">
        <h2>Public Node #3 — Irreversibility of Traces</h2>
        <p>Execution traces do not uniquely reconstruct their generators. Irreversibility is structural, not historical.</p>
        <div class="collapse">
          If full reconstruction from events is always possible, this structure collapses.
        </div>
      </article>

      <article class="card">
        <h2>Public Node #4 — Cross-Domain Structural Invariants</h2>
        <p>Physics, biology, and computation share invariants when downstream of the same operator family.</p>
        <div class="collapse">
          If no invariant survives cross-domain projection, this structure collapses.
        </div>
      </article>

      <article class="card">
        <h2>Public Node #5 — Civilizational Fork</h2>
        <p>Recognition of code-first generation forces a shift from event narratives to operator identification.</p>
        <div class="collapse">
          If civilization can progress without this shift, this structure collapses.
        </div>
      </article>

    </section>

  </main>

  <footer class="footer">
    Ω-Structure Institute — Independent Non-Commercial Research in Structural Science.
    <br />
    Primary contact (public): <a href="mailto:osi.research666@gmail.com">osi.research666@gmail.com</a>
  </footer>

</body>
</html>
