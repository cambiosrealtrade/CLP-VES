<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <title>Calculadora P2P · Chile</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:      #050810;
      --ink:     #e8edf8;
      --muted:   #4a5568;
      --cl:      #e63946;
      --ve:      #5b9cf6;
      --shadow:  0 20px 60px rgba(0,0,0,0.7);
      --glow-cl: 0 0 60px rgba(230,57,70,0.20), 0 0 120px rgba(230,57,70,0.08);
      --glow-ve: 0 0 60px rgba(91,156,246,0.18), 0 0 120px rgba(91,156,246,0.08);
    }

    html, body {
      min-height: 100dvh;
      background: var(--bg);
      font-family: 'Inter', sans-serif;
      color: var(--ink);
      -webkit-font-smoothing: antialiased;
      overflow-x: hidden;
    }

    body::before {
      content: '';
      position: fixed; inset: 0; pointer-events: none; z-index: 0;
      background:
        radial-gradient(ellipse 70% 55% at 15% 5%,  rgba(230,57,70,0.18) 0%, transparent 65%),
        radial-gradient(ellipse 60% 65% at 85% 90%, rgba(91,156,246,0.18) 0%, transparent 65%),
        radial-gradient(ellipse 45% 40% at 70% 25%, rgba(180,60,120,0.10) 0%, transparent 55%),
        radial-gradient(ellipse 35% 50% at 30% 75%, rgba(0,180,200,0.08) 0%, transparent 50%);
      animation: orbs 12s ease-in-out infinite alternate;
    }
    @keyframes orbs {
      0%   { opacity: 1;   transform: scale(1)    translateY(0);    }
      50%  { opacity: 0.8; transform: scale(1.04) translateY(-12px);}
      100% { opacity: 1;   transform: scale(1)    translateY(0);    }
    }

    body::after {
      content: '';
      position: fixed; inset: 0; pointer-events: none; z-index: 0;
      background-image:
        linear-gradient(rgba(255,255,255,0.018) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.018) 1px, transparent 1px);
      background-size: 44px 44px;
      mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 40%, transparent 100%);
    }

    .wrapper {
      position: relative; z-index: 1;
      max-width: 440px; margin: 0 auto;
      padding: 52px 18px 70px;
      display: flex; flex-direction: column; gap: 20px;
    }

    header { text-align: center; margin-bottom: 4px; }
    .pill {
      display: inline-flex; align-items: center; gap: 7px;
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.14);
      color: var(--muted);
      font-family: 'DM Mono', monospace; font-size: 10px;
      letter-spacing: 0.16em; text-transform: uppercase;
      padding: 5px 15px; border-radius: 100px; margin-bottom: 18px;
      backdrop-filter: blur(16px);
    }
    .pill span { opacity: 0.3; }
    h1 {
      font-size: clamp(30px, 9vw, 42px); font-weight: 800;
      line-height: 1.04; letter-spacing: -0.035em;
    }
    h1 em { font-style: normal; color: rgba(255,255,255,0.25); }

    .result-card {
      position: relative;
      background: rgba(255,255,255,0.04);
      border: 1px solid rgba(255,255,255,0.10);
      border-radius: 26px; overflow: hidden;
      backdrop-filter: blur(32px) saturate(160%);
      -webkit-backdrop-filter: blur(32px) saturate(160%);
      box-shadow: var(--shadow);
    }
    .result-card::before {
      content: '';
      position: absolute; top: 0; left: 10%; right: 10%; height: 1px;
      border-radius: 100%; pointer-events: none; z-index: 2;
    }
    .result-card.chile     { box-shadow: var(--shadow), var(--glow-cl); border-color: rgba(230,57,70,0.18); }
    .result-card.chile::before { background: linear-gradient(90deg, transparent, rgba(230,57,70,0.6), transparent); }
    .result-card.venezuela { box-shadow: var(--shadow), var(--glow-ve); border-color: rgba(91,156,246,0.18); }
    .result-card.venezuela::before { background: linear-gradient(90deg, transparent, rgba(91,156,246,0.6), transparent); }

    .card-inner { padding: 24px 22px 26px; display: flex; flex-direction: column; gap: 18px; }

    .card-head  { display: flex; align-items: center; justify-content: space-between; }
    .card-title { display: flex; align-items: center; gap: 10px; }
    .flag { font-size: 26px; line-height: 1; }
    .country { font-size: 19px; font-weight: 800; letter-spacing: -0.02em; }
    .country.cl { color: var(--cl); }
    .country.ve { color: var(--ve); }

    .badge-group { display: flex; align-items: center; gap: 6px; }

    .badge {
      font-family: 'DM Mono', monospace; font-size: 11px;
      padding: 5px 13px; border-radius: 100px;
      color: #050810; letter-spacing: 0.05em; font-weight: 700;
    }
    .badge.cl { background: var(--cl); box-shadow: 0 0 14px rgba(230,57,70,0.5); }
    .badge.ve { background: var(--ve); box-shadow: 0 0 14px rgba(91,156,246,0.5); }
    .badge.comm {
      background: rgba(255,255,255,0.10);
      border: 1px solid rgba(255,255,255,0.18);
      color: var(--ink);
      box-shadow: none;
      font-size: 10px;
    }

    .block  { display: flex; flex-direction: column; gap: 5px; }
    .blabel {
      font-size: 10px; font-weight: 600;
      letter-spacing: 0.15em; text-transform: uppercase; color: var(--muted);
    }
    .brow { display: flex; align-items: baseline; gap: 7px; }

    .input-row { display: flex; align-items: center; gap: 10px; }
    .curr-tag {
      font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 500;
      background: rgba(255,255,255,0.07); border: 1px solid rgba(255,255,255,0.13);
      border-radius: 10px; padding: 9px 13px;
      color: var(--muted); flex-shrink: 0; white-space: nowrap;
    }

    .local-val { font-family: 'DM Mono', monospace; font-size: clamp(22px, 6.5vw, 30px); font-weight: 500; letter-spacing: -0.025em; }
    .local-val.cl { color: var(--cl); }
    .local-val.ve { color: var(--ve); }
    .local-unit { font-family: 'DM Mono', monospace; font-size: 13px; font-weight: 600; opacity: 0.45; }
    .local-unit.cl { color: var(--cl); }
    .local-unit.ve { color: var(--ve); }

    .div { height: 1px; background: rgba(255,255,255,0.07); }

    .ves-val {
      font-family: 'DM Mono', monospace;
      font-size: clamp(36px, 11vw, 52px); font-weight: 500;
      letter-spacing: -0.03em; line-height: 1.02;
      color: var(--ink); word-break: break-all;
    }
    .ves-unit { font-family: 'DM Mono', monospace; font-size: 15px; font-weight: 600; color: var(--muted); }

    .bar-cl { height: 3px; background: linear-gradient(90deg, transparent, var(--cl), #ff8a92, var(--cl), transparent); }
    .bar-ve { height: 3px; background: linear-gradient(90deg, transparent, var(--ve), #a5c8ff, var(--ve), transparent); }

    .sep-row { display: flex; align-items: center; gap: 12px; margin: 4px 0; }
    .sep-row .line { flex: 1; height: 1px; background: rgba(255,255,255,0.07); }
    .sep-row .sep-label {
      font-family: 'DM Mono', monospace; font-size: 10px;
      letter-spacing: 0.13em; text-transform: uppercase; color: var(--muted);
    }

    @keyframes shimmer {
      0%  { background-position: -400px 0; }
      100%{ background-position:  400px 0; }
    }
    .shimmer {
      background: linear-gradient(90deg, rgba(255,255,255,0.04) 25%, rgba(255,255,255,0.11) 50%, rgba(255,255,255,0.04) 75%);
      background-size: 800px 100%; animation: shimmer 1.6s infinite;
      border-radius: 6px; color: transparent !important;
      min-width: 140px; display: inline-block; user-select: none;
    }

    .fade { opacity: 0; transform: translateY(18px); animation: up 0.55s ease forwards; }
    @keyframes up { to { opacity: 1; transform: translateY(0); } }
    header                      { animation-delay: 0.00s; }
    .result-card:nth-of-type(1) { animation-delay: 0.10s; }
    .sep-row                    { animation-delay: 0.16s; }
    .result-card:nth-of-type(2) { animation-delay: 0.22s; }

    .status { display: none; }
    input[type=number] { -moz-appearance: textfield; }
    input[type=number]::-webkit-outer-spin-button,
    input[type=number]::-webkit-inner-spin-button { -webkit-appearance: none; }
  </style>
</head>
<body>

<div class="wrapper">

  <header class="fade">
    <div class="pill">P2P <span>·</span> Chile</div>
    <h1>Calculadora<br /><em>de cambio</em></h1>
  </header>

  <!-- SECCIÓN 1: CLP → VES -->
  <div class="result-card chile fade">
    <div class="bar-cl"></div>
    <div class="card-inner">

      <div class="card-head">
        <div class="card-title">
          <span class="flag">🇨🇱</span>
          <span class="country cl">Chile</span>
        </div>
        <div class="badge-group">
          <span class="badge comm" id="badge-comm-clp" style="display:none;"></span>
          <span class="badge cl"   id="badge-clp"      style="display:none;">CLP</span>
        </div>
      </div>

      <div class="block">
        <span class="blabel">Pesos que envías</span>
        <div class="input-row">
          <span class="curr-tag">$ CLP</span>
          <input id="amount" type="number" inputmode="decimal"
                 placeholder="0" min="0" step="any"
                 oninput="calculate()" autofocus
                 style="width:100%; border:none; outline:none; background:transparent;
                        font-family:'DM Mono',monospace; font-size:clamp(28px,8vw,40px);
                        font-weight:500; color:var(--cl); letter-spacing:-0.025em;" />
        </div>
      </div>

      <div class="block">
        <span class="blabel">Equivalente en dólares</span>
        <div class="brow">
          <span class="local-val cl" id="local-clp-usd">——</span>
          <span class="local-unit cl">USD $</span>
        </div>
      </div>

      <div class="div"></div>

      <div class="block">
        <span class="blabel">Recibes en bolívares</span>
        <div class="brow">
          <span class="ves-val shimmer" id="result-ves">——</span>
          <span class="ves-unit"> Bs.</span>
        </div>
      </div>



    </div>
  </div>

  <!-- SEPARADOR -->
  <div class="sep-row fade">
    <div class="line"></div>
    <span class="sep-label">Enviar Bs. → CLP</span>
    <div class="line"></div>
  </div>

  <!-- SECCIÓN 2: VES → CLP -->
  <div class="result-card venezuela fade">
    <div class="bar-ve"></div>
    <div class="card-inner">

      <div class="card-head">
        <div class="card-title">
          <span class="flag">🇻🇪</span>
          <span class="country ve">Venezuela</span>
        </div>
        <div class="badge-group">
          <span class="badge comm" id="badge-comm-ves" style="display:none;"></span>
          <span class="badge ve"   id="badge-ves"      style="display:none;">Bs.</span>
        </div>
      </div>

      <div class="block">
        <span class="blabel">Bolívares que envías</span>
        <div class="input-row">
          <span class="curr-tag">Bs.</span>
          <input id="amount-ves" type="number" inputmode="decimal"
                 placeholder="0" min="0" step="any"
                 oninput="calculateVES()"
                 style="width:100%; border:none; outline:none; background:transparent;
                        font-family:'DM Mono',monospace; font-size:clamp(28px,8vw,40px);
                        font-weight:500; color:var(--ve); letter-spacing:-0.025em;" />
        </div>
      </div>

      <div class="block">
        <span class="blabel">Equivalente en dólares</span>
        <div class="brow">
          <span class="local-val ve" id="local-ves-usd">——</span>
          <span class="local-unit ve">USD $</span>
        </div>
      </div>

      <div class="div"></div>

      <div class="block">
        <span class="blabel">Pesos que recibes</span>
        <div class="brow">
          <span class="ves-val shimmer" id="result-clp">——</span>
          <span class="ves-unit"> $</span>
        </div>
      </div>



    </div>
  </div>

</div>

<script>
/* =========================================================
   STATE
========================================================= */
const S = {
  usdtVes : null,
  usdClp  : null,
  usdtClp : null,
  clpVes  : null,
  ready   : false,
};

/* =========================================================
   MARKUP Y COMISIONES
========================================================= */
const CLP_MARKUP   = 0.10;
const CLP_FALLBACK = 920;

function commCLP(clp) { return clp < 500000 ? 0.05 : 0.08; }

/* =========================================================
   FETCH USDT/VES
========================================================= */
async function fetchUSDTVES() {
  const r = await fetch('https://ve.dolarapi.com/v1/dolares/paralelo', { cache: 'no-cache' });
  if (!r.ok) throw new Error();
  const d = await r.json();
  const v = Number(d?.promedio ?? d?.precio ?? d?.venta ?? 0);
  if (v < 1) throw new Error();
  return v;
}

/* =========================================================
   FETCH USDT/CLP — Binance → CoinGecko → fallback
========================================================= */
async function fetchUSDTCLP() {
  try {
    const r = await fetch('https://api.binance.com/api/v3/ticker/price?symbol=USDTCLP', { cache: 'no-cache' });
    if (r.ok) { const d = await r.json(); const v = Number(d?.price ?? 0); if (v > 1) return v + CLP_MARKUP; }
  } catch (_) {}
  try {
    const r = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=tether&vs_currencies=clp', { cache: 'no-cache' });
    if (r.ok) { const d = await r.json(); const v = Number(d?.tether?.clp ?? 0); if (v > 1) return v + CLP_MARKUP; }
  } catch (_) {}
  return CLP_FALLBACK + CLP_MARKUP;
}

/* =========================================================
   LOAD
========================================================= */
async function loadRates() {
  const [vesRes, clpRes] = await Promise.allSettled([fetchUSDTVES(), fetchUSDTCLP()]);

  if (vesRes.status === 'fulfilled') S.usdtVes = vesRes.value;
  const liveClp = (clpRes.status === 'fulfilled') ? clpRes.value : CLP_FALLBACK + CLP_MARKUP;
  S.usdClp  = liveClp;
  S.usdtClp = liveClp;

  if (S.usdtVes && S.usdClp) {
    S.clpVes = S.usdtVes / S.usdClp;
  }

  S.ready = true;
  document.querySelectorAll('.shimmer').forEach(el => el.classList.remove('shimmer'));
  calculate();
  calculateVES();
}

/* =========================================================
   HELPER: mostrar/ocultar badges
========================================================= */
function showBadges(currId, commId, currLabel, commPct, visible) {
  const bCurr = document.getElementById(currId);
  const bComm = document.getElementById(commId);
  if (visible) {
    bCurr.textContent = currLabel;
    bCurr.style.display = 'inline-flex';
    bComm.textContent = `${(commPct * 100).toFixed(0)}% comisión`;
    bComm.style.display = 'inline-flex';
  } else {
    bCurr.style.display = 'none';
    bComm.style.display = 'none';
  }
}

/* =========================================================
   CLP → VES
========================================================= */
function calculate() {
  if (!S.ready) return;
  const raw   = document.getElementById('amount').value;
  const clp   = parseFloat(raw);
  const empty = !raw || isNaN(clp) || clp <= 0;

  const elVes = document.getElementById('result-ves');
  const elUSD = document.getElementById('local-clp-usd');

  showBadges('badge-clp', 'badge-comm-clp', 'CLP', commCLP(clp || 0), !empty);

  if (empty) { elVes.textContent = '——'; elUSD.textContent = '——'; return; }

  const usd  = clp / S.usdClp;
  const comm = commCLP(clp);
  const ves  = usd * S.usdtVes * (1 - comm);

  elUSD.textContent = fmt(usd, 2);
  elVes.textContent = Math.round(ves).toLocaleString('es-VE');
}

/* =========================================================
   VES → CLP
========================================================= */
function calculateVES() {
  if (!S.ready) return;
  const raw   = document.getElementById('amount-ves').value;
  const ves   = parseFloat(raw);
  const empty = !raw || isNaN(ves) || ves <= 0;

  const elCLP = document.getElementById('result-clp');
  const elUSD = document.getElementById('local-ves-usd');

  showBadges('badge-ves', 'badge-comm-ves', 'Bs.', 0.13, !empty);

  if (empty) { elCLP.textContent = '——'; elUSD.textContent = '——'; return; }

  const usd    = ves / S.usdtVes;
  const clpNet = usd * S.usdtClp * (1 - 0.13);

  elUSD.textContent = fmt(usd, 2);
  elCLP.textContent = Math.round(clpNet).toLocaleString('es-CL');
}

/* =========================================================
   HELPERS
========================================================= */
function fmt(n, dec) {
  return Number(n).toLocaleString('es-VE', {
    minimumFractionDigits: dec,
    maximumFractionDigits: dec,
  });
}

loadRates();
</script>
</body>
</html>
