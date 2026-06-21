<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Man in the Mirror</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #ffffff;
    --bg2: #f7f7f6;
    --bg3: #f0f0ee;
    --border: rgba(0,0,0,0.08);
    --border2: rgba(0,0,0,0.14);
    --txt: #1a1a1a;
    --txt2: #6b6b6b;
    --txt3: #a0a0a0;
    --radius: 12px;
    --radius-sm: 8px;
    --fe: #7B72E9;
    --cuerpo: #2DB88F;
    --mente: #3A8FE8;
    --crear: #E8931A;
    --mision: #E05A32;
    --rel: #E05080;
    --car: #888884;
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0f0f0f;
      --bg2: #1a1a1a;
      --bg3: #222222;
      --border: rgba(255,255,255,0.07);
      --border2: rgba(255,255,255,0.12);
      --txt: #f0f0f0;
      --txt2: #999999;
      --txt3: #555555;
    }
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    background: var(--bg);
    color: var(--txt);
    font-size: 13px;
    font-weight: 300;
    line-height: 1.5;
    padding: 24px 20px 40px;
    max-width: 680px;
    margin: 0 auto;
  }

  .serif {
    font-family: 'Cormorant Garamond', Georgia, serif;
  }

  /* ── header ── */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 24px;
  }
  .header-label {
    font-size: 10px;
    color: var(--txt3);
    letter-spacing: 0.18em;
    text-transform: uppercase;
    margin-bottom: 5px;
    font-family: 'Inter', sans-serif;
    font-weight: 400;
  }
  .header-title {
    font-size: 36px;
    font-weight: 300;
    letter-spacing: -0.5px;
    color: var(--txt);
    font-family: 'Cormorant Garamond', Georgia, serif;
    line-height: 1.1;
  }
  .header-sub {
    font-size: 11px;
    color: var(--txt3);
    margin-top: 5px;
    font-weight: 300;
    letter-spacing: 0.02em;
  }
  .header-score {
    text-align: right;
  }
  .header-score-num {
    font-size: 42px;
    font-weight: 300;
    letter-spacing: -2px;
    line-height: 1;
    font-family: 'Cormorant Garamond', Georgia, serif;
  }
  .header-score-label {
    font-size: 10px;
    color: var(--txt3);
    margin-top: 3px;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    font-weight: 400;
  }

  /* ── tabs ── */
  .tabs {
    display: flex;
    gap: 4px;
    margin-bottom: 20px;
    padding-bottom: 16px;
    border-bottom: 0.5px solid var(--border);
  }
  .tab {
    padding: 5px 14px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 400;
    cursor: pointer;
    border: 0.5px solid var(--border2);
    background: transparent;
    color: var(--txt2);
    font-family: 'Inter', sans-serif;
    letter-spacing: 0.04em;
    transition: all .15s;
  }
  .tab:hover { background: var(--bg2); }
  .tab.active {
    background: var(--txt);
    color: var(--bg);
    border-color: var(--txt);
    font-weight: 500;
  }

  /* ── panels ── */
  .panel { display: none; }
  .panel.visible { display: block; }

  /* ── metric cards ── */
  .metrics {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    margin-bottom: 12px;
  }
  .metric {
    background: var(--bg2);
    border-radius: var(--radius-sm);
    padding: 12px 14px;
  }
  .metric-label {
    font-size: 10px;
    color: var(--txt3);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 6px;
    font-weight: 400;
  }
  .metric-val {
    font-size: 20px;
    font-weight: 300;
    color: var(--txt);
    line-height: 1.3;
    font-family: 'Cormorant Garamond', Georgia, serif;
  }

  /* ── card ── */
  .card {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 16px 18px;
    margin-bottom: 10px;
  }
  .card-label {
    font-size: 10px;
    color: var(--txt3);
    margin-bottom: 12px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    font-weight: 400;
  }

  /* ── bar chart ── */
  .bars {
    display: flex;
    align-items: flex-end;
    gap: 5px;
    height: 64px;
  }
  .bar-col {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 3px;
    height: 100%;
    justify-content: flex-end;
  }
  .bar-val { font-size: 9px; color: var(--txt3); font-weight: 500; }
  .bar-rect {
    width: 100%;
    border-radius: 3px 3px 0 0;
    transition: height .4s ease;
    min-height: 2px;
  }
  .bar-day { font-size: 9px; color: var(--txt3); margin-top: 3px; }

  /* ── area cards grid ── */
  .areas-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
    gap: 8px;
  }
  .area-card {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 14px 16px;
  }
  .area-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 10px;
  }
  .area-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    margin-bottom: 5px;
  }
  .area-name {
    font-size: 14px;
    font-weight: 400;
    font-family: 'Cormorant Garamond', Georgia, serif;
    color: var(--txt);
    max-width: 100px;
    line-height: 1.35;
  }
  .area-trend {
    font-size: 10px;
    margin-top: 3px;
    font-weight: 300;
  }

  /* ── ring svg ── */
  .ring-wrap { flex-shrink: 0; }

  /* ── spark ── */
  .spark { margin-top: 4px; }

  /* ── tracker ── */
  .tracker-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }
  .tracker-title {
    font-size: 20px;
    font-weight: 400;
    font-family: 'Cormorant Garamond', Georgia, serif;
    letter-spacing: 0.01em;
  }
  .btn-add {
    font-size: 11px;
    padding: 4px 12px;
    border-radius: 7px;
    border: 0.5px solid var(--border2);
    background: transparent;
    cursor: pointer;
    color: var(--txt2);
    font-family: inherit;
    transition: background .15s;
  }
  .btn-add:hover { background: var(--bg2); }

  .day-row {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    margin-bottom: 6px;
    overflow: hidden;
  }
  .day-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    cursor: pointer;
    user-select: none;
  }
  .day-header:hover { background: var(--bg2); }
  .day-left { display: flex; align-items: center; gap: 10px; }
  .day-name { font-size: 18px; font-weight: 400; font-family: 'Cormorant Garamond', Georgia, serif; }
  .day-dots { display: flex; gap: 3px; align-items: center; }
  .day-dot-bar {
    width: 4px;
    height: 18px;
    border-radius: 2px;
  }
  .day-right { display: flex; align-items: center; gap: 10px; }
  .day-score { font-size: 13px; font-weight: 500; }
  .chevron { font-size: 11px; color: var(--txt3); transition: transform .2s; }
  .chevron.open { transform: rotate(180deg); }

  .day-body {
    display: none;
    padding: 0 16px 14px;
    border-top: 0.5px solid var(--border);
    padding-top: 12px;
  }
  .day-body.open { display: block; }

  .q-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
    margin-bottom: 8px;
  }
  .q-area-dot {
    width: 5px; height: 5px; border-radius: 50%; flex-shrink: 0;
  }
  .q-text {
    flex: 1;
    font-size: 11px;
    color: var(--txt2);
  }
  .q-btns { display: flex; gap: 3px; }
  .vbtn {
    padding: 3px 9px;
    border-radius: 20px;
    font-size: 10px;
    cursor: pointer;
    border: 0.5px solid var(--border);
    background: transparent;
    color: var(--txt2);
    font-family: inherit;
    transition: all .1s;
    white-space: nowrap;
  }
  .vbtn.si.active   { background: #E1F5EE; color: #085041; border-color: #E1F5EE; font-weight: 500; }
  .vbtn.par.active  { background: #FAEEDA; color: #633806; border-color: #FAEEDA; font-weight: 500; }
  .vbtn.no.active   { background: #FCEBEB; color: #791F1F; border-color: #FCEBEB; font-weight: 500; }

  /* new entry form */
  .new-entry {
    background: var(--bg2);
    border-radius: var(--radius);
    padding: 14px 16px;
    margin-bottom: 10px;
    display: none;
  }
  .new-entry.open { display: block; }
  .new-entry-label { font-size: 11px; color: var(--txt3); margin-bottom: 8px; }
  .new-entry input[type=text] {
    width: 100%;
    border: 0.5px solid var(--border2);
    border-radius: 7px;
    padding: 7px 10px;
    font-size: 12px;
    font-family: inherit;
    background: var(--bg);
    color: var(--txt);
    margin-bottom: 12px;
    outline: none;
  }
  .new-entry input[type=text]:focus { border-color: #534AB7; }
  .form-btns { display: flex; gap: 6px; margin-top: 10px; }
  .btn-save {
    padding: 6px 16px;
    border-radius: 7px;
    background: var(--txt);
    color: var(--bg);
    border: none;
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
    font-weight: 500;
  }
  .btn-cancel {
    padding: 6px 14px;
    border-radius: 7px;
    background: transparent;
    color: var(--txt2);
    border: 0.5px solid var(--border2);
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
  }

  /* ── heatmap ── */
  .heat-wrap { overflow-x: auto; }
  .heat-inner { min-width: 420px; }
  .heat-days-header {
    display: flex;
    margin-left: 154px;
    gap: 4px;
    margin-bottom: 5px;
  }
  .heat-day-label {
    width: 28px;
    font-size: 9px;
    color: var(--txt3);
    text-align: center;
    flex-shrink: 0;
  }
  .heat-row {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 4px;
  }
  .heat-area-label {
    width: 146px;
    font-size: 11px;
    color: var(--txt2);
    text-align: right;
    flex-shrink: 0;
  }
  .heat-cells { display: flex; gap: 4px; }
  .heat-cell {
    width: 28px;
    height: 28px;
    border-radius: 5px;
    flex-shrink: 0;
    transition: background .3s;
    cursor: default;
  }
  .heat-legend {
    display: flex;
    gap: 12px;
    margin-top: 14px;
    margin-left: 154px;
    font-size: 10px;
    color: var(--txt3);
    align-items: center;
  }
  .heat-legend-item { display: flex; align-items: center; gap: 4px; }
  .heat-legend-swatch {
    width: 12px; height: 12px; border-radius: 3px;
  }

  /* ── radar svg ── */
  #radar-svg {
    display: block;
    margin: 0 auto;
    max-width: 210px;
  }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-label">man in the mirror</div>
    <div class="header-title">Estadísticas de vida</div>
    <div class="header-sub" id="sub-label">7 días registrados esta semana</div>
  </div>
  <div class="header-score">
    <div class="header-score-num" id="global-score">—</div>
    <div class="header-score-label">promedio global</div>
  </div>
</div>

<div class="tabs">
  <button class="tab active" data-tab="resumen">Resumen</button>
  <button class="tab" data-tab="areas">Áreas</button>
  <button class="tab" data-tab="habitos">Hábitos</button>
  <button class="tab" data-tab="tracker">Reflexión</button>
  <button class="tab" data-tab="heatmap">Heatmap</button>
</div>

<!-- RESUMEN -->
<div class="panel visible" id="panel-resumen">
  <div class="metrics">
    <div class="metric">
      <div class="metric-label">mejor día</div>
      <div class="metric-val" id="m-best">—</div>
    </div>
    <div class="metric">
      <div class="metric-label">racha</div>
      <div class="metric-val" id="m-racha">—</div>
    </div>
    <div class="metric">
      <div class="metric-label">área débil</div>
      <div class="metric-val" id="m-weak">—</div>
    </div>
  </div>
  <div class="card">
    <div class="card-label">Puntuación diaria</div>
    <div class="bars" id="daily-bars"></div>
  </div>
  <div class="card">
    <div class="card-label">Radar de áreas · promedio semanal</div>
    <svg id="radar-svg" viewBox="0 0 240 240" width="100%"></svg>
  </div>
</div>

<!-- ÁREAS -->
<div class="panel" id="panel-areas">
  <div class="areas-grid" id="areas-grid"></div>
</div>

<!-- HÁBITOS -->
<div class="panel" id="panel-habitos">
  <div class="tracker-header">
    <div class="tracker-title">Hábitos del día</div>
    <div style="font-size:11px;color:var(--txt3)">Marca al completar</div>
  </div>
  <div id="habitos-grid"></div>
</div>

<!-- TRACKER REFLEXIÓN -->
<div class="panel" id="panel-tracker">
  <div class="tracker-header">
    <div class="tracker-title">Reflexión nocturna</div>
    <button class="btn-add" id="btn-add-day">+ Nuevo día</button>
  </div>
  <div class="new-entry" id="new-entry-form">
    <div class="new-entry-label">Nuevo registro</div>
    <input type="text" id="new-day-name" placeholder="Ej: Lun 16">
    <div id="new-q-rows"></div>
    <div class="form-btns">
      <button class="btn-save" id="btn-save-new">Guardar</button>
      <button class="btn-cancel" id="btn-cancel-new">Cancelar</button>
    </div>
  </div>
  <div id="tracker-rows"></div>
</div>

<!-- HEATMAP -->
<div class="panel" id="panel-heatmap">
  <div class="card">
    <div class="card-label">Mapa de calor · cada celda es un día, más oscuro es mejor</div>
    <div class="heat-wrap">
      <div class="heat-inner" id="heat-inner"></div>
    </div>
  </div>
</div>

<script>
const SHEET_URL = 'https://script.google.com/macros/s/AKfycbw1mzp5cYddTLKVyS04LUqSOOHOkxcjhznLNBblBly6U94FTRFBtYeg6BiqJOR2RMUbow/exec';

const AREAS = [
  { k:"fe",     label:"Fe & espíritu",         color:"#7B72E9" },
  { k:"mente",  label:"Mente & aprendizaje",   color:"#3A8FE8" },
  { k:"mision", label:"Misión & negocio",      color:"#E05A32" },
  { k:"cuerpo", label:"Cuerpo & salud",        color:"#2DB88F" },
  { k:"car",    label:"Carácter & disciplina", color:"#888884" },
];

// ── Hábitos reales por área ──
const HABITOS = {
  fe: [
    "Leer la Biblia 30 min",
    "Reflexión nocturna (agradecimientos + qué mejorar)",
    "Ver Smallville",
  ],
  mente: [
    "Leer You Can't Hurt Me 30 min",
    "Estudiar Heroes Academia 1 hora",
    "Medir cortisol (1–10, 10 = bajo cortisol)",
  ],
  mision: [
    "Trabajar 2.5h sesión 1",
    "Trabajar 2.5h sesión 2",
    "Hopecast",
    "Ayudar en casa (cocinar o arreglar)",
  ],
  cuerpo: [
    "Entrenar",
    "4 litros de agua",
    "130g proteína",
    "250g carbohidratos",
    "55g grasas",
    "Ducha fría",
    "Dormir 8–9 horas",
    "Medir energía (1–10)",
  ],
  car: [
    "Cumplir horario del día",
    "Mantener espacio limpio y ordenado",
    "Evitar distracciones innecesarias",
  ],
};

// ── Preguntas de reflexión (3 por área) ──
const QS = [
  // Fe & espíritu
  { q:"¿Conecté con Dios hoy?",              area:"fe"     },
  { q:"¿Fui agradecido durante el día?",     area:"fe"     },
  { q:"¿Viví con propósito y paz interior?", area:"fe"     },
  // Mente & aprendizaje
  { q:"¿Aprendí algo nuevo hoy?",            area:"mente"  },
  { q:"¿Sostuve mi cortisol bajo?",          area:"mente"  },
  { q:"¿Mi mente estuvo enfocada y clara?",  area:"mente"  },
  // Misión & negocio
  { q:"¿Completé mis 5 horas de trabajo?",   area:"mision" },
  { q:"¿Avancé en Hopecast hoy?",            area:"mision" },
  { q:"¿Aportó valor mi trabajo de hoy?",    area:"mision" },
  // Cuerpo & salud
  { q:"¿Entrené con intensidad?",            area:"cuerpo" },
  { q:"¿Cumplí mi nutrición del día?",       area:"cuerpo" },
  { q:"¿Mi energía fue alta hoy (7+/10)?",   area:"cuerpo" },
  // Carácter & disciplina
  { q:"¿Cumplí mis horarios?",               area:"car"    },
  { q:"¿Fui íntegro cuando nadie miraba?",   area:"car"    },
  { q:"¿Cumplí todo lo que prometí?",        area:"car"    },
];

const QS_AREA = {
  fe:     QS.filter(q=>q.area==="fe"    ).map(q=>q.q),
  mente:  QS.filter(q=>q.area==="mente" ).map(q=>q.q),
  mision: QS.filter(q=>q.area==="mision").map(q=>q.q),
  cuerpo: QS.filter(q=>q.area==="cuerpo").map(q=>q.q),
  car:    QS.filter(q=>q.area==="car"   ).map(q=>q.q),
};

const VAL = { "Sí":2, "Parcial":1, "No":0 };
let DATA = [];
let LOADING = true;

// ── Google Sheets ──
async function loadFromSheet() {
  try {
    const res = await fetch(SHEET_URL);
    const rows = await res.json();
    DATA = rows.map(r => {
      const obj = { d: r['Día'] || r['Dia'] || '' };
      QS.forEach(({q}) => { obj[q] = r[q] || null; });
      return obj;
    }).filter(r => r.d);
    LOADING = false;
    render();
  } catch(e) {
    LOADING = false;
    document.getElementById('sub-label').textContent = 'Error cargando datos — revisa la conexión';
    render();
  }
}

async function saveToSheet(entry) {
  const body = { dia: entry.d, fecha: new Date().toISOString().split('T')[0] };
  QS.forEach(({q}) => { body[q] = entry[q] || ''; });
  try {
    await fetch(SHEET_URL, {
      method: 'POST',
      mode: 'no-cors',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(body)
    });
  } catch(e) { console.error('Error guardando:', e); }
}

// ── Calculations ──
function mean(arr){ const v=arr.filter(x=>x!=null); return v.length ? v.reduce((a,b)=>a+b,0)/v.length : null; }
function p(v){ return v==null?null:Math.round((v/2)*100); }
function dayScore(row){ const vs=QS.map(({q})=>VAL[row[q]]??null).filter(v=>v!=null); return vs.length?Math.round((mean(vs)/2)*100):null; }
function areaScore(row,ak){ const qs=QS_AREA[ak]; const vs=qs.map(q=>VAL[row[q]]??null).filter(v=>v!=null); return vs.length?p(mean(vs)):null; }
function areaAvg(ak){ const vs=DATA.map(r=>areaScore(r,ak)).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function globalAvg(){ const vs=DATA.map(dayScore).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function scoreColor(s){ return s>=80?"#2DB88F":s>=60?"#E8931A":"#E05A32"; }
function hex2alpha(hex,a){ const r=parseInt(hex.slice(1,3),16),g=parseInt(hex.slice(3,5),16),b=parseInt(hex.slice(5,7),16); return `rgba(${r},${g},${b},${a})`; }

// ── Render helpers ──
function ring(score, color, r){
  r = r||28;
  const sz=(r+5)*2, cx=r+5, circ=2*Math.PI*r;
  const dash = score==null ? 0 : (score/100)*circ;
  const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  const textColor = isDark ? '#f0f0f0' : '#1a1a1a';
  const trackColor = isDark ? 'rgba(255,255,255,0.08)' : 'rgba(0,0,0,0.07)';
  return `<svg width="${sz}" height="${sz}" viewBox="0 0 ${sz} ${sz}" class="ring-wrap">
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="${trackColor}" stroke-width="3.5"/>
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="${color}" stroke-width="3.5"
      stroke-dasharray="${dash.toFixed(1)} ${circ.toFixed(1)}" stroke-linecap="round"
      transform="rotate(-90 ${cx} ${cx})" style="transition:stroke-dasharray .5s ease"/>
    <text x="${cx}" y="${cx}" text-anchor="middle" dominant-baseline="central"
      font-size="${Math.round(r*.52)}" font-weight="500" fill="${textColor}">${score!=null?score+'%':'—'}</text>
  </svg>`;
}

function spark(vals, color){
  const W=130, H=32, pad=2;
  const valid = vals.filter(v=>v!=null);
  if(!valid.length) return `<div style="height:${H}px"></div>`;
  const n=vals.length;
  const xs = vals.map((_,i)=>pad+i*(W-2*pad)/Math.max(n-1,1));
  const ys = vals.map(v=>v==null?null: H-pad-(v/100)*(H-2*pad));
  const pts = xs.reduce((acc,x,i)=> ys[i]!=null ? acc+(acc?`L${x.toFixed(1)},${ys[i].toFixed(1)}`:`M${x.toFixed(1)},${ys[i].toFixed(1)}`) : acc ,'');
  return `<svg width="${W}" height="${H}" viewBox="0 0 ${W} ${H}" class="spark" style="width:100%">
    <path d="${pts}" fill="none" stroke="${color}" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" opacity=".9"/>
  </svg>`;
}

// ── Tab switching ──
document.querySelectorAll('.tab').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('visible'));
    btn.classList.add('active');
    document.getElementById('panel-'+btn.dataset.tab).classList.add('visible');
    render();
  });
});

function activeTab(){ return document.querySelector('.tab.active').dataset.tab; }

// ── Hábitos locales (solo sesión, no se guardan en Sheet) ──
let HABITOS_CHECK = {};
function toggleHabito(area, idx){
  const key = area+'_'+idx;
  HABITOS_CHECK[key] = !HABITOS_CHECK[key];
  renderHabitos();
}

function renderHabitos(){
  const grid = document.getElementById('habitos-grid');
  grid.innerHTML = AREAS.map(a=>{
    const lista = HABITOS[a.k] || [];
    const completados = lista.filter((_,i)=>HABITOS_CHECK[a.k+'_'+i]).length;
    const pctH = lista.length ? Math.round((completados/lista.length)*100) : 0;
    const items = lista.map((h,i)=>{
      const checked = HABITOS_CHECK[a.k+'_'+i];
      return `<div onclick="toggleHabito('${a.k}',${i})" style="display:flex;align-items:center;gap:10px;padding:8px 0;cursor:pointer;border-bottom:0.5px solid var(--border);user-select:none;" class="habito-row">
        <div style="width:18px;height:18px;border-radius:5px;border:1.5px solid ${checked?a.color:'var(--border2)'};background:${checked?a.color:'transparent'};flex-shrink:0;display:flex;align-items:center;justify-content:center;transition:all .15s;">
          ${checked?`<svg width="10" height="10" viewBox="0 0 10 10"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>`:''}
        </div>
        <div style="font-size:12px;color:${checked?'var(--txt3)':'var(--txt)'};text-decoration:${checked?'line-through':'none'};transition:all .15s;">${h}</div>
      </div>`;
    }).join('');
    return `<div class="card" style="margin-bottom:8px;">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <div style="display:flex;align-items:center;gap:8px;">
          <div style="width:7px;height:7px;border-radius:50%;background:${a.color};flex-shrink:0;"></div>
          <div style="font-size:12px;font-weight:500;color:var(--txt);">${a.label}</div>
        </div>
        <div style="font-size:11px;color:${pctH===100?a.color:'var(--txt3)'};">${completados}/${lista.length}</div>
      </div>
      <div style="height:3px;background:var(--bg3);border-radius:2px;margin-bottom:12px;">
        <div style="height:3px;background:${a.color};border-radius:2px;width:${pctH}%;transition:width .3s;"></div>
      </div>
      ${items}
    </div>`;
  }).join('');
}

// ── Master render ──
function render(){
  renderHeader();
  if(LOADING) return;
  const t = activeTab();
  if(t==='resumen')  renderResumen();
  if(t==='areas')    renderAreas();
  if(t==='habitos')  renderHabitos();
  if(t==='tracker')  renderTracker();
  if(t==='heatmap')  renderHeatmap();
}

function renderHeader(){
  const g = globalAvg();
  const el = document.getElementById('global-score');
  if(LOADING){ el.textContent='…'; el.style.color='var(--txt3)'; document.getElementById('sub-label').textContent='Cargando datos…'; return; }
  el.textContent = g!=null ? g+'%' : '—';
  el.style.color = g!=null ? scoreColor(g) : 'var(--txt3)';
  document.getElementById('sub-label').textContent = DATA.length+' días registrados';
}

function renderResumen(){
  if(!DATA.length){ document.getElementById('daily-bars').innerHTML='<div style="color:var(--txt3);font-size:12px;padding:20px 0">Sin datos aún — agrega tu primer día en Tracker</div>'; return; }
  const scores = DATA.map(dayScore);
  const bestI = scores.reduce((bi,s,i)=>(s??-1)>(scores[bi]??-1)?i:bi, 0);
  const best = DATA[bestI];
  document.getElementById('m-best').textContent = best ? best.d+' · '+dayScore(best)+'%' : '—';
  document.getElementById('m-racha').textContent = DATA.length+' días';
  const weak = AREAS.filter(a=>{ const s=areaAvg(a.k); return s!=null&&s<70; }).sort((a,b)=>areaAvg(a.k)-areaAvg(b.k));
  const wEl = document.getElementById('m-weak');
  wEl.textContent = weak.length ? weak[0].label.split('&')[0].trim() : '—';
  wEl.style.color = weak.length ? weak[0].color : 'var(--txt)';
  const barsEl = document.getElementById('daily-bars');
  barsEl.innerHTML = scores.map((s,i)=>{
    const h = s==null ? 2 : Math.max(2, Math.round((s/100)*56));
    return `<div class="bar-col">
      <div class="bar-val">${s!=null?s+'%':''}</div>
      <div class="bar-rect" style="height:${h}px;background:#7B72E9;opacity:${s==null?.15:.82}"></div>
      <div class="bar-day">${DATA[i].d}</div>
    </div>`;
  }).join('');
  renderRadar();
}

function renderRadar(){
  const svg = document.getElementById('radar-svg');
  const cx=120,cy=120,R=82,n=AREAS.length;
  const angle = i=>(i/n)*2*Math.PI-Math.PI/2;
  const pt = (i,f)=>({ x:cx+R*f*Math.cos(angle(i)), y:cy+R*f*Math.sin(angle(i)) });
  const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  const gridColor = isDark ? 'rgba(255,255,255,0.08)' : 'rgba(0,0,0,0.07)';
  const labelColor = isDark ? '#555555' : '#a0a0a0';
  let html = '';
  [.25,.5,.75,1].forEach(f=>{
    const pts = AREAS.map((_,i)=>{ const p=pt(i,f); return `${p.x.toFixed(1)},${p.y.toFixed(1)}`; }).join(' ');
    html += `<polygon points="${pts}" fill="none" stroke="${gridColor}" stroke-width="0.5"/>`;
  });
  AREAS.forEach((_,i)=>{ const e=pt(i,1); html+=`<line x1="${cx}" y1="${cy}" x2="${e.x.toFixed(1)}" y2="${e.y.toFixed(1)}" stroke="${gridColor}" stroke-width="0.5"/>`; });
  const avgs = AREAS.map(a=>areaAvg(a.k));
  const spoly = AREAS.map((a,i)=>{ const f=(avgs[i]??0)/100; const p=pt(i,f); return `${p.x.toFixed(1)},${p.y.toFixed(1)}`; }).join(' ');
  html += `<polygon points="${spoly}" fill="#7B72E9" fill-opacity="0.15" stroke="#7B72E9" stroke-width="1.5"/>`;
  AREAS.forEach((a,i)=>{ const f=(avgs[i]??0)/100; const p2=pt(i,f); html+=`<circle cx="${p2.x.toFixed(1)}" cy="${p2.y.toFixed(1)}" r="3" fill="${a.color}"/>`; });
  AREAS.forEach((a,i)=>{
    const lp=pt(i,1.22);
    const name=a.label.split('&')[0].trim();
    html+=`<text x="${lp.x.toFixed(1)}" y="${lp.y.toFixed(1)}" text-anchor="middle" dominant-baseline="middle" font-size="10" fill="${labelColor}">${name}</text>`;
  });
  svg.innerHTML = html;
}

function renderAreas(){
  const grid = document.getElementById('areas-grid');
  if(!DATA.length){ grid.innerHTML='<div style="color:var(--txt3);font-size:12px">Sin datos aún</div>'; return; }
  grid.innerHTML = AREAS.map(a=>{
    const s = areaAvg(a.k);
    const days = DATA.map(r=>areaScore(r,a.k));
    const last=days[days.length-1], prev=days[days.length-2];
    const trend = (last!=null&&prev!=null) ? last-prev : null;
    const trendHtml = trend!=null
      ? `<div class="area-trend" style="color:${trend>0?'#2DB88F':trend<0?'#E05A32':'var(--txt3)'}">${trend>0?'↑':trend<0?'↓':' '}${Math.abs(trend)}% vs ayer</div>`
      : '';
    return `<div class="area-card">
      <div class="area-top">
        <div>
          <div class="area-dot" style="background:${a.color}"></div>
          <div class="area-name">${a.label}</div>
          ${trendHtml}
        </div>
        ${ring(s, a.color, 24)}
      </div>
      ${spark(days, a.color)}
    </div>`;
  }).join('');
}

function renderTracker(){
  const container = document.getElementById('tracker-rows');
  container.innerHTML = DATA.map((row,di)=>{
    const s = dayScore(row);
    const sColor = s!=null ? scoreColor(s) : 'var(--txt3)';
    const dots = AREAS.map(a=>{
      const v = areaScore(row,a.k);
      const op = v!=null ? Math.max(.15, v/100) : .1;
      return `<div class="day-dot-bar" style="background:${a.color};opacity:${op}"></div>`;
    }).join('');
    const qRows = QS.map(({q,area})=>{
      const ar = AREAS.find(a=>a.k===area);
      const cur = row[q];
      return `<div class="q-row">
        <div class="q-area-dot" style="background:${ar.color}"></div>
        <div class="q-text">${q}</div>
        <div class="q-btns">
          <button class="vbtn si${cur==='Sí'?' active':''}" data-di="${di}" data-q="${q}" data-v="Sí">Sí</button>
          <button class="vbtn par${cur==='Parcial'?' active':''}" data-di="${di}" data-q="${q}" data-v="Parcial">Parcial</button>
          <button class="vbtn no${cur==='No'?' active':''}" data-di="${di}" data-q="${q}" data-v="No">No</button>
        </div>
      </div>`;
    }).join('');
    return `<div class="day-row" id="dayrow-${di}">
      <div class="day-header" data-di="${di}">
        <div class="day-left">
          <div class="day-name">${row.d}</div>
          <div class="day-dots">${dots}</div>
        </div>
        <div class="day-right">
          <div class="day-score" style="color:${sColor}">${s!=null?s+'%':'—'}</div>
          <div class="chevron" id="chev-${di}">▾</div>
        </div>
      </div>
      <div class="day-body" id="body-${di}">${qRows}</div>
    </div>`;
  }).join('');

  container.querySelectorAll('.day-header').forEach(h=>{
    h.addEventListener('click',()=>{
      const di = h.dataset.di;
      document.getElementById('body-'+di).classList.toggle('open');
      document.getElementById('chev-'+di).classList.toggle('open');
    });
  });

  container.querySelectorAll('.vbtn').forEach(btn=>{
    btn.addEventListener('click',e=>{
      e.stopPropagation();
      const {di,q,v} = btn.dataset;
      DATA[+di][q] = v;
      renderTracker();
      renderHeader();
    });
  });
}

function renderHeatmap(){
  const inner = document.getElementById('heat-inner');
  if(!DATA.length){ inner.innerHTML='<div style="color:var(--txt3);font-size:12px">Sin datos aún</div>'; return; }
  const dayLabels = `<div class="heat-days-header">${DATA.map(r=>`<div class="heat-day-label">${r.d}</div>`).join('')}</div>`;
  const rows = AREAS.map(a=>{
    const cells = DATA.map(r=>{
      const s = areaScore(r,a.k);
      const bg = s==null ? 'var(--bg3)'
        : s>=90 ? a.color
        : s>=65 ? hex2alpha(a.color,.75)
        : s>=35 ? hex2alpha(a.color,.4)
        : hex2alpha(a.color,.15);
      return `<div class="heat-cell" style="background:${bg}" title="${a.label}: ${s!=null?s+'%':'—'}"></div>`;
    }).join('');
    return `<div class="heat-row">
      <div class="heat-area-label">${a.label}</div>
      <div class="heat-cells">${cells}</div>
    </div>`;
  }).join('');
  const legend = `<div class="heat-legend">
    ${[['bajo','rgba(123,114,233,.15)'],['medio','rgba(123,114,233,.4)'],['alto','rgba(123,114,233,.75)'],['óptimo','#7B72E9']].map(([l,bg])=>`<div class="heat-legend-item"><div class="heat-legend-swatch" style="background:${bg}"></div>${l}</div>`).join('')}
  </div>`;
  inner.innerHTML = dayLabels + rows + legend;
}

// ── New entry form ──
const newForm = document.getElementById('new-entry-form');
const newQRows = document.getElementById('new-q-rows');
let newEntry = { d:'' };

function renderNewForm(){
  newQRows.innerHTML = QS.map(({q,area})=>{
    const ar = AREAS.find(a=>a.k===area);
    const cur = newEntry[q];
    return `<div class="q-row">
      <div class="q-area-dot" style="background:${ar.color}"></div>
      <div class="q-text">${q}</div>
      <div class="q-btns">
        <button class="vbtn si${cur==='Sí'?' active':''}" data-q="${q}" data-v="Sí">Sí</button>
        <button class="vbtn par${cur==='Parcial'?' active':''}" data-q="${q}" data-v="Parcial">Parcial</button>
        <button class="vbtn no${cur==='No'?' active':''}" data-q="${q}" data-v="No">No</button>
      </div>
    </div>`;
  }).join('');
  newQRows.querySelectorAll('.vbtn').forEach(btn=>{
    btn.addEventListener('click',()=>{
      newEntry[btn.dataset.q] = btn.dataset.v;
      renderNewForm();
    });
  });
}

document.getElementById('btn-add-day').addEventListener('click',()=>{
  newEntry = { d:'' };
  newForm.classList.add('open');
  renderNewForm();
  document.getElementById('new-day-name').focus();
});

document.getElementById('new-day-name').addEventListener('input',e=>{ newEntry.d = e.target.value; });

document.getElementById('btn-save-new').addEventListener('click', async ()=>{
  if(!newEntry.d) return;
  const entry = {...newEntry};

  // Guardar en Sheet
  const saveBtn = document.getElementById('btn-save-new');
  saveBtn.textContent = 'Guardando…';
  saveBtn.disabled = true;
  await saveToSheet(entry);

  // Actualizar DATA local
  DATA.push(entry);
  newEntry = { d:'' };
  newForm.classList.remove('open');
  saveBtn.textContent = 'Guardar';
  saveBtn.disabled = false;
  render();
});

document.getElementById('btn-cancel-new').addEventListener('click',()=>{
  newForm.classList.remove('open');
});

// ── Init — carga desde Sheet ──
render(); // muestra "Cargando…"
loadFromSheet();
</script>
</body>
</html>
