<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#000000" id="meta-theme-color">
<title>Man in the Mirror - Elite System</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; overflow-wrap: break-word; word-wrap: break-word; }

  :root {
    --bg: #ffffff; --bg2: #f7f7f6; --bg3: #f0f0ee;
    --border: rgba(0,0,0,0.08); --border2: rgba(0,0,0,0.14);
    --txt: #1a1a1a; --txt2: #6b6b6b; --txt3: #a0a0a0;
    --radius: 12px; --radius-sm: 8px;
    --fe: #7B72E9; --cuerpo: #2DB88F; --mente: #3A8FE8;
    --mision: #E05A32; --car: #888884; --sos: #D32F2F;
    --gold: #D4AF37;
  }

  body.dark-mode {
    --bg: #0f0f0f; --bg2: #1a1a1a; --bg3: #222222;
    --border: rgba(255,255,255,0.07); --border2: rgba(255,255,255,0.12);
    --txt: #f0f0f0; --txt2: #999999; --txt3: #555555;
  }

  ::-webkit-scrollbar { width: 6px; height: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 4px; }

  body { 
    font-family: 'Inter', sans-serif; background: var(--bg); color: var(--txt); 
    font-size: 14px; font-weight: 400; line-height: 1.5; 
    padding: 24px 20px 80px; max-width: 680px; margin: 0 auto; 
    transition: background 0.2s ease, color 0.2s ease; 
    touch-action: manipulation;
    -webkit-font-smoothing: antialiased;
  }

  .header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 20px; }
  .header-label { font-size: 10px; color: var(--txt3); letter-spacing: 0.18em; text-transform: uppercase; margin-bottom: 5px; font-weight: 600;}
  .header-title { font-size: 36px; font-weight: 300; letter-spacing: -0.5px; font-family: 'Cormorant Garamond', serif; line-height: 1.1; }
  .rank-badge { display: inline-block; font-size: 10px; font-weight: 700; padding: 3px 10px; border-radius: 6px; background: var(--bg2); border: 1px solid var(--border2); text-transform: uppercase; letter-spacing: 1px; margin-top: 8px; color: var(--gold); }
  .quote-display { font-style: italic; font-size: 12px; color: var(--txt2); margin-top: 8px; max-width: 250px; }
  
  .header-score { text-align: right; }
  .header-score-num { font-size: 46px; font-weight: 300; letter-spacing: -2px; line-height: 1; font-family: 'Cormorant Garamond', serif; transition: color 0.3s ease; }
  
  .top-btns { display: flex; gap: 8px; margin-top: 12px; }
  .icon-btn { background: var(--bg2); border: 1px solid var(--border); border-radius: 50%; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; cursor: pointer; color: var(--txt); font-size: 16px; transition: transform 0.1s ease, background 0.2s; }
  .icon-btn:active { transform: scale(0.9); }

  .tabs { position: sticky; top: 0; background: var(--bg); z-index: 50; display: flex; gap: 6px; padding: 12px 0; margin-bottom: 16px; border-bottom: 1px solid var(--border); overflow-x: auto; scrollbar-width: none; }
  .tabs::-webkit-scrollbar { display: none; }
  .tab { padding: 8px 16px; border-radius: 20px; font-size: 12px; font-weight: 500; cursor: pointer; border: 1px solid var(--border2); background: transparent; color: var(--txt2); white-space: nowrap; transition: all 0.2s ease; }
  .tab.active { background: var(--txt); color: var(--bg); border-color: var(--txt); font-weight: 600; }

  .panel { display: none; opacity: 0; }
  .panel.visible { display: block; opacity: 1; animation: fadeIn 0.2s ease forwards; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

  .card { background: var(--bg); border: 1px solid var(--border); border-radius: 14px; padding: 18px 20px; margin-bottom: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.02); }
  .card-label { font-size: 11px; font-weight: 600; color: var(--txt3); margin-bottom: 14px; letter-spacing: 0.08em; text-transform: uppercase; display: flex; justify-content: space-between; align-items: center;}
  
  .metrics { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 12px; }
  .metric { background: var(--bg2); border-radius: 10px; padding: 14px; }
  .metric-label { font-size: 10px; font-weight: 600; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 6px; }
  .metric-val { font-size: 22px; font-weight: 300; font-family: 'Cormorant Garamond', serif; }

  #radar-svg, #trend-svg { display: block; margin: 0 auto; width: 100%; max-width: 260px; }
  
  .heat-wrap { overflow-x: auto; padding-bottom: 10px; }
  .heat-inner { min-width: 450px; }
  .heat-days-header { display: flex; margin-left: 140px; gap: 4px; margin-bottom: 6px; }
  .heat-day-label { width: 30px; font-size: 9px; color: var(--txt3); text-align: center; flex-shrink: 0; }
  .heat-row { display: flex; align-items: center; gap: 8px; margin-bottom: 6px; }
  .heat-area-label { width: 132px; font-size: 12px; font-weight: 500; color: var(--txt2); text-align: right; flex-shrink: 0; }
  .heat-cells { display: flex; gap: 4px; }
  .heat-cell { width: 30px; height: 30px; border-radius: 6px; flex-shrink: 0; cursor: pointer; transition: transform 0.1s; }
  .heat-cell:active { transform: scale(0.9); }
  .heat-legend { display: flex; gap: 12px; margin-top: 16px; justify-content: center; font-size: 11px; font-weight: 500; color: var(--txt3); }
  .heat-legend-swatch { width: 14px; height: 14px; border-radius: 4px; margin-right: 6px; display: inline-block; vertical-align: middle; }

  .day-row { background: var(--bg); border: 1px solid var(--border); border-radius: 14px; margin-bottom: 8px; overflow: hidden; }
  .day-header { display: flex; justify-content: space-between; align-items: center; padding: 16px 20px; cursor: pointer; background: var(--bg2); }
  .day-body { display: none; padding: 0 20px 20px; border-top: 1px solid var(--border); animation: slideDown 0.2s ease forwards; }
  .day-body.open { display: block; }
  .day-title-edit { outline: none; border-bottom: 1px dashed transparent; transition: border 0.2s; padding-bottom: 2px;}
  .day-title-edit:focus { border-bottom-color: var(--txt); }
  @keyframes slideDown { from { opacity: 0; transform: translateY(-5px); } to { opacity: 1; transform: translateY(0); } }

  .section-title { font-size: 11px; font-weight: 700; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.05em; margin: 20px 0 12px; display: flex; justify-content: space-between; align-items: center;}

  .habit-row-wrap { margin-bottom: 12px; border-bottom: 1px solid var(--border); padding-bottom: 12px; }
  .habit-top { display: flex; align-items: center; gap: 12px; margin-bottom: 8px; min-height: 44px; }
  .habit-chk { width: 32px; height: 32px; border-radius: 8px; border: 2px solid var(--border2); cursor: pointer; display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: transform 0.1s; }
  .habit-chk:active { transform: scale(0.85); }
  .habit-name { font-size: 14px; font-weight: 500; flex: 1; cursor: pointer; user-select: none; }
  .habit-streak { font-size: 11px; font-weight: 700; display: inline-flex; align-items: center; background: var(--bg3); padding: 2px 6px; border-radius: 4px; margin-left: 6px; }
  .btn-timer { background: var(--txt); border: none; border-radius: 8px; width: 44px; height: 44px; font-size: 18px; cursor: pointer; color: var(--bg); display: flex; justify-content: center; align-items: center;}
  .btn-timer:active { opacity: 0.8; }
  
  .text-input, .journal-input { width: 100%; border: 1px solid var(--border2); border-radius: 10px; padding: 12px 14px; font-size: 14px; font-family: inherit; background: var(--bg); color: var(--txt); outline: none; margin-bottom: 8px; transition: border-color 0.2s ease; }
  .text-input:focus, .journal-input:focus { border-color: var(--fe); }
  .journal-input { min-height: 100px; resize: vertical; line-height: 1.6; }
  .excuse-warning { color: var(--sos); font-size: 11px; font-weight: 700; text-transform: uppercase; margin-top: 4px; display: none; }

  .photo-gallery { display: flex; gap: 10px; overflow-x: auto; margin-top: 10px; padding-bottom: 10px; }
  .photo-preview { height: 100px; border-radius: 10px; border: 1px solid var(--border); object-fit: cover; }
  
  .btn-add { font-size: 13px; font-weight: 600; padding: 10px 18px; border-radius: 10px; border: none; background: var(--txt); color: var(--bg); cursor: pointer; display: inline-flex; align-items: center; justify-content: center; gap: 6px;}
  .btn-add:active { opacity: 0.8; transform: scale(0.98); }

  .cookie-jar-item { border-left: 4px solid var(--gold); background: var(--bg2); padding: 14px 16px; border-radius: 0 10px 10px 0; margin-bottom: 10px; font-size: 14px; line-height: 1.6; }

  .quick-task-btn { position: fixed; bottom: 24px; left: 24px; width: 56px; height: 56px; border-radius: 28px; background: var(--txt); color: var(--bg); font-weight: 600; border: none; box-shadow: 0 6px 16px rgba(0,0,0,0.2); cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 24px; z-index: 100; }
  .sos-btn { position: fixed; bottom: 24px; right: 24px; width: 56px; height: 56px; border-radius: 28px; background: var(--sos); color: white; border: none; box-shadow: 0 6px 16px rgba(211,47,47,0.4); cursor: pointer; display: none; align-items: center; justify-content: center; font-size: 24px; z-index: 100; }
  .auto-badge { display: inline-block; background: var(--fe); color: white; font-size: 10px; padding: 4px 10px; border-radius: 12px; font-weight: 600; }

  /* Modals */
  .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); backdrop-filter: blur(5px); display: none; align-items: center; justify-content: center; z-index: 1000; padding: 20px; opacity: 0; transition: opacity 0.2s; }
  .modal-overlay.active { display: flex; opacity: 1; }
  .modal-box { background: var(--bg); border-radius: 20px; padding: 28px; max-width: 420px; width: 100%; text-align: center; border: 1px solid var(--border); transform: scale(0.95); transition: transform 0.2s; position: relative; }
  .modal-overlay.active .modal-box { transform: scale(1); }
  
  .timer-display { font-size: 64px; font-weight: 300; font-family: 'Cormorant Garamond', serif; margin: 10px 0; font-variant-numeric: tabular-nums; display: flex; justify-content: center; align-items: center; }
  .timer-input { background: transparent; border: none; border-bottom: 2px solid var(--txt); color: var(--txt); font-size: 64px; font-weight: 300; font-family: inherit; width: 90px; text-align: center; outline: none; }
  
  .sos-title { color: var(--sos); font-size: 28px; font-weight: 700; text-transform: uppercase; margin-bottom: 15px; }
  .modal-btn { background: var(--txt); color: var(--bg); border: none; padding: 14px 20px; border-radius: 10px; font-size: 14px; font-weight: 600; cursor: pointer; margin-top: 10px; width: 100%; transition: opacity 0.2s; }
  .modal-btn:hover { opacity: 0.85; }

  .delete-btn { background: var(--bg3); border: none; color: var(--txt); font-weight: bold; cursor: pointer; padding: 6px 12px; border-radius: 8px; display: flex; align-items: center; justify-content: center; transition: all 0.2s; }
  .delete-btn:active { transform: scale(0.9); }
  
  /* Toasts */
  #toast-container { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); z-index: 9999; display: flex; flex-direction: column; gap: 10px; pointer-events: none; }
  .toast { background: var(--txt); color: var(--bg); padding: 12px 24px; border-radius: 30px; font-size: 13px; font-weight: 600; box-shadow: 0 8px 16px rgba(0,0,0,0.2); animation: toastIn 0.2s, toastOut 0.2s 2.8s forwards; }
  @keyframes toastIn { from { opacity: 0; transform: translateY(-20px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes toastOut { from { opacity: 1; } to { opacity: 0; } }

  .breathe-circle { width: 120px; height: 120px; border-radius: 50%; background: var(--fe); opacity: 0.2; margin: 30px auto; animation: breathe 8s infinite ease-in-out; }
  @keyframes breathe { 0%, 100% { transform: scale(1); opacity: 0.2; } 50% { transform: scale(2); opacity: 0.6; } }

  .color-swatch { width: 32px; height: 32px; border-radius: 50%; cursor: pointer; display: inline-block; margin: 6px; border: 2px solid transparent; }
  
  .ref-btn { flex: 1; padding: 10px 6px; font-size: 12px; font-weight: 600; border: 1px solid var(--border); border-radius: 8px; background: var(--bg2); color: var(--txt2); cursor: pointer; transition: background 0.1s; }
  .ref-btn.selected { background: var(--txt); color: var(--bg); border-color: var(--txt); }
</style>
</head>
<body>

<div id="toast-container"></div>

<div class="header">
  <div>
    <div class="header-label">man in the mirror</div>
    <div class="header-title">Estadísticas de Vida</div>
    <div class="rank-badge" id="rank-badge">Recluta</div>
    <div class="quote-display" id="daily-quote"></div>
    <div class="top-btns">
      <button class="icon-btn" id="btn-theme">🌙</button>
      <button class="icon-btn" id="btn-future">🚪</button>
      <button class="icon-btn" id="btn-quick" style="color:var(--mision); border-color:var(--mision);">⚡</button>
    </div>
  </div>
  <div class="header-score">
    <div class="header-score-num" id="global-score">—</div>
    <div class="header-score-label" style="font-size:12px; font-weight:500; color:var(--txt3);">promedio global</div>
    <div style="font-size: 11px; font-weight: 700; color: var(--fe); text-transform: uppercase; margin-top: 6px;" id="rank-title">Aprendiz</div>
  </div>
</div>

<div class="tabs" id="nav-tabs">
  <button class="tab active" data-tab="resumen">Resumen</button>
  <button class="tab" data-tab="habitos">Hoy</button>
  <button class="tab" data-tab="tracker">Diario</button>
  <button class="tab" data-tab="areas">Áreas</button>
  <button class="tab" data-tab="heatmap">Heatmap</button>
  <button class="tab" data-tab="cookiejar">Tarro 🍪</button>
  <button class="tab" data-tab="boveda">Bóveda</button>
  <button class="tab" data-tab="config">⚙️</button>
</div>

<!-- PANEL: RESUMEN -->
<div class="panel visible" id="panel-resumen">
  <div class="card" style="background:var(--fe); color:white; text-align:center; padding:24px; border:none;">
    <div style="font-size:13px; font-weight:500; opacity:0.9;">Día de hoy</div>
    <div style="font-size:32px; font-family:'Cormorant Garamond',serif; font-weight:600; margin-top:4px;" id="today-label">—</div>
    <div style="font-size:12px; font-weight:600; background:rgba(0,0,0,0.2); display:inline-block; padding:4px 12px; border-radius:12px; margin-top:10px;" id="today-status">Cargando...</div>
  </div>

  <div class="metrics">
    <div class="metric"><div class="metric-label">mejor día</div><div class="metric-val" id="m-best">—</div></div>
    <div class="metric"><div class="metric-label">días >80%</div><div class="metric-val" id="m-elite">—</div></div>
    <div class="metric"><div class="metric-label">área débil</div><div class="metric-val" id="m-weak" style="font-size:16px; font-weight:600;">—</div></div>
  </div>
  
  <div class="card" id="insights-card">
    <div class="card-label">🧠 Inteligencia de Rendimiento</div>
    <div id="insights-content" style="font-size:13px; color:var(--txt2); line-height:1.6;">Analizando datos...</div>
  </div>

  <div class="card">
    <div class="card-label">📈 Tendencia (Últimos 14 días)</div>
    <svg id="trend-svg" viewBox="0 0 240 60" style="height:60px; overflow:visible;"></svg>
  </div>

  <div class="card">
    <div class="card-label">📊 Puntuación diaria</div>
    <div id="daily-bars" style="display:flex; gap:6px; height:80px; align-items:flex-end; margin-top:16px;"></div>
  </div>

  <div class="card">
    <div class="card-label">Radar de áreas · Promedio Total</div>
    <svg id="radar-svg" viewBox="0 0 240 240" width="100%"></svg>
  </div>
</div>

<!-- PANEL: ÁREAS -->
<div class="panel" id="panel-areas"><div id="areas-grid" style="display:grid; gap:12px;"></div></div>

<!-- PANEL: HÁBITOS (HOY) -->
<div class="panel" id="panel-habitos">
  <div id="habitos-grid"></div>
</div>

<!-- PANEL: TRACKER (DIARIO) -->
<div class="panel" id="panel-tracker">
  <div style="display:flex; justify-content:space-between; margin-bottom:16px; align-items:center;">
    <div class="header-title" style="font-size:28px;">Historial</div>
    <div style="display:flex; gap:8px;">
      <span class="auto-badge" id="hell-mode-badge" style="display:none; background:var(--sos);">🔥 INF</span>
      <button class="btn-add" id="btn-add-day">✚ Añadir Día Manual</button>
    </div>
  </div>
  <div id="tracker-rows"></div>
</div>

<!-- PANEL: HEATMAP -->
<div class="panel" id="panel-heatmap">
  <div class="card">
    <div class="card-label">
      <span>Mapa de calor de Constancia</span>
      <div style="display:flex; gap:4px;">
        <button class="tab" onclick="renderHeatmap(30)" style="padding:4px 10px; font-size:10px;">30D</button>
        <button class="tab" onclick="renderHeatmap(0)" style="padding:4px 10px; font-size:10px;">Todo</button>
      </div>
    </div>
    <div class="heat-wrap"><div class="heat-inner" id="heat-inner"></div></div>
  </div>
</div>

<!-- PANEL: COOKIE JAR -->
<div class="panel" id="panel-cookiejar">
  <div class="card">
    <div class="card-label">🍪 El Tarro de Galletas</div>
    <p style="font-size:13px; color:var(--txt2); margin-bottom:16px; line-height:1.5;">Registra tus victorias más difíciles. Léelas cuando tu mente te diga que no puedes más. David Goggins style.</p>
    <div style="display:flex; gap:8px; margin-bottom:20px;">
      <input type="text" id="cookie-input" class="text-input" placeholder="Ej: Entrené enfermo y cansado..." style="margin-bottom:0;">
      <button class="btn-add" id="btn-add-cookie" style="flex-shrink:0;">Guardar</button>
    </div>
    <div id="cookie-list"></div>
  </div>
</div>

<!-- PANEL: BÓVEDA -->
<div class="panel" id="panel-boveda">
  <div style="display:flex; gap:8px; margin-bottom:12px; overflow-x:auto; padding-bottom:4px;">
    <button class="btn-add" style="white-space:nowrap; background:var(--bg2); color:var(--txt); border:1px solid var(--border);" onclick="document.getElementById('vault-search').value='#victoria'; document.getElementById('vault-search').dispatchEvent(new Event('input'))">🍪 Victorias</button>
    <button class="btn-add" style="white-space:nowrap; background:var(--bg2); color:var(--txt); border:1px solid var(--border);" onclick="exportVault()">📄 Exportar Todo</button>
  </div>
  <div class="card">
    <div class="card-label">📖 Base de Conocimiento</div>
    <input type="text" id="vault-search" class="text-input" placeholder="Buscar aprendizajes, dietas, notas..." style="margin-bottom:16px;">
    <div id="vault-wordcloud" style="font-size:12px; color:var(--txt3); margin-bottom:16px; line-height:1.8;"></div>
    <div id="vault-content" style="display:flex; flex-direction:column; gap:10px;"></div>
  </div>
</div>

<!-- PANEL: CONFIG -->
<div class="panel" id="panel-config">
  <div class="card">
    <div class="card-label">📝 Editor de Hábitos Maestros</div>
    <div style="font-size:12px; color:var(--txt2); margin-bottom:14px;">Añade o elimina los hábitos que aparecerán en los nuevos días.</div>
    <select id="habit-area-select" class="text-input" style="margin-bottom:10px; font-weight:600;" onchange="renderConfigHabits()">
      <option value="fe">Fe & Espíritu</option>
      <option value="mente">Mente & Aprendizaje</option>
      <option value="mision">Misión & Negocio</option>
      <option value="cuerpo">Cuerpo & Salud</option>
      <option value="car">Carácter & Disciplina</option>
    </select>
    <div style="display:flex; gap:8px; margin-bottom:16px;">
      <input type="text" id="new-habit-input" class="text-input" placeholder="Nuevo hábito..." style="margin-bottom:0;">
      <button class="btn-add" onclick="addNewHabit()" style="flex-shrink:0;">Añadir</button>
    </div>
    <div id="config-habit-list"></div>
  </div>

  <div class="card">
    <div class="card-label">🎨 Color Principal</div>
    <div id="color-palette" style="display:flex; flex-wrap:wrap; gap:4px; justify-content:center; padding:10px 0;">
      <div class="color-swatch" style="background:#7B72E9" onclick="setPrimaryColor('#7B72E9')"></div>
      <div class="color-swatch" style="background:#2DB88F" onclick="setPrimaryColor('#2DB88F')"></div>
      <div class="color-swatch" style="background:#E05A32" onclick="setPrimaryColor('#E05A32')"></div>
      <div class="color-swatch" style="background:#D32F2F" onclick="setPrimaryColor('#D32F2F')"></div>
      <div class="color-swatch" style="background:#3A8FE8" onclick="setPrimaryColor('#3A8FE8')"></div>
      <div class="color-swatch" style="background:#888884" onclick="setPrimaryColor('#888884')"></div>
      <div class="color-swatch" style="background:#D4AF37" onclick="setPrimaryColor('#D4AF37')"></div>
    </div>
  </div>

  <div class="card">
    <div class="card-label">💾 Datos y Seguridad</div>
    <div style="font-size:12px; color:var(--txt3); margin-bottom:14px; line-height:1.5;">(Tu información vive solo en tu dispositivo. Exporta un backup si vas a borrar el historial del navegador.)</div>
    <button class="modal-btn" onclick="exportData()" style="margin-bottom:8px;">⬇️ Descargar Backup (.json)</button>
    <label class="modal-btn" style="display:block; text-align:center; background:var(--bg2); color:var(--txt); border:1px solid var(--border);">
      ⬆️ Restaurar Backup
      <input type="file" style="display:none" accept=".json" onchange="importData(event)">
    </label>
  </div>

  <div class="card" style="border-color:rgba(239, 68, 68, 0.3);">
    <div class="card-label" style="color:var(--sos);">🔥 Modo Infierno</div>
    <div style="font-size:13px; font-weight:500; margin-bottom:12px; color:var(--txt2);">Si fallas el 80% de puntuación, tu racha vuelve a 0. No hay piedad.</div>
    <button class="modal-btn" id="btn-hell-mode" style="background:var(--sos);" onclick="toggleHellMode()">Activar Modo Infierno</button>
  </div>
</div>

<button class="sos-btn" id="btn-sos" title="Protocolo de Emergencia">🚨</button>
<button class="quick-task-btn" id="btn-quick-fab" style="display:none;">⚡</button>

<!-- MODALS GLOBALES -->
<div class="modal-overlay" id="modal-add-day">
  <div class="modal-box">
    <div class="card-label" style="justify-content:center; font-size:14px;">Añadir Día Histórico</div>
    <p style="font-size:13px; color:var(--txt2); margin-bottom:16px;">Introduce la fecha o el nombre del día.</p>
    <input type="text" id="input-new-day-name" class="text-input" style="text-align:center; font-weight:bold; font-size:16px; padding:12px;">
    <button class="modal-btn" id="btn-confirm-add-day" style="font-size:16px; padding:14px; margin-top:20px;">✚ Crear Día</button>
    <button class="modal-btn" style="background:transparent; color:var(--txt3); margin-top:8px;" onclick="document.getElementById('modal-add-day').classList.remove('active')">Cancelar</button>
  </div>
</div>

<div class="modal-overlay" id="modal-timer">
  <div class="modal-box">
    <div class="card-label" id="timer-title" style="justify-content:center; font-size:14px;">Modo Enfoque</div>
    <div class="timer-display">
      <input type="number" id="timer-input-m" class="timer-input" value="45" min="1" max="180"> 
      <span>:</span> 
      <span id="timer-display-s" style="width:100px; text-align:center;">00</span>
    </div>
    <button class="modal-btn" id="btn-timer-action" style="font-size:18px; padding:18px; margin-top:20px;">▶ Iniciar</button>
    <button class="modal-btn" style="background:transparent; color:var(--txt3); margin-top:8px;" id="btn-timer-close">Cerrar</button>
  </div>
</div>

<div class="modal-overlay" id="modal-sos">
  <div class="modal-box" style="border: 2px solid var(--sos);">
    <div class="sos-title">Reseteo Táctico</div>
    <div class="sos-list">
      1. Suelta el teléfono y ponte de pie.<br>
      2. Bebe 1 vaso grande de agua.<br>
      3. Lávate la cara con agua fría.<br>
      4. Haz 20 flexiones estrictas.<br>
      5. "No te detengas cuando estés cansado."
    </div>
    <button class="modal-btn" style="background:var(--sos); padding:16px;" onclick="document.getElementById('modal-sos').classList.remove('active'); showToast('De vuelta al trabajo.');">Entendido. A trabajar.</button>
    <button class="modal-btn" style="background:var(--bg2); color:var(--txt); margin-top:12px;" onclick="lockApp()">Bloquear App 15 min</button>
  </div>
</div>

<div class="modal-overlay" id="modal-mirror">
  <div class="modal-box">
    <div class="mirror-title">Confrontación del Espejo</div>
    <p style="font-size:14px; color:var(--txt2); margin-bottom:20px; line-height:1.6;" id="mirror-text"></p>
    <input type="text" id="mirror-input" class="text-input" placeholder="Escribe 'NO ME RENDIRÉ'" style="text-align:center; font-weight:800; font-size:16px; padding:14px;">
    <button class="modal-btn" id="btn-mirror-unlock" style="background:var(--txt3); cursor:not-allowed; margin-top:16px; padding:14px;" disabled>Desbloquear App</button>
  </div>
</div>

<div class="modal-overlay" id="modal-future">
  <div class="modal-box">
    <div class="card-label" style="justify-content:center;">Proyección a 1 Año 🚪</div>
    <div id="future-content" style="font-size:16px; line-height:1.6; margin-bottom:24px;"></div>
    <button class="modal-btn" onclick="document.getElementById('modal-future').classList.remove('active')">Volver al presente</button>
  </div>
</div>

<div class="modal-overlay" id="modal-quick">
  <div class="modal-box">
    <div class="card-label" style="justify-content:center;">Regla de 2 Minutos ⚡</div>
    <p style="font-size:13px; margin-bottom:16px; color:var(--txt2);">Si la tarea toma menos de 2 minutos, hazla AHORA MISMO y táchala.</p>
    <input type="text" id="quick-input" class="text-input" placeholder="Ej: Limpiar escritorio, enviar email..." style="padding:14px;">
    <div id="quick-list" style="text-align:left; margin-top:16px; max-height:200px; overflow-y:auto; padding-right:4px;"></div>
    <button class="modal-btn" style="background:transparent; color:var(--txt); border:1px solid var(--border); margin-top:20px;" onclick="document.getElementById('modal-quick').classList.remove('active')">Cerrar</button>
  </div>
</div>

<div class="modal-overlay" id="modal-breathe">
  <div class="modal-box">
    <div class="card-label" style="color:var(--fe); justify-content:center; font-size:14px;">Alerta de Cortisol</div>
    <p style="font-size:14px; color:var(--txt2); margin-top:10px;">Respira con el círculo. Inhala al expandir, exhala al contraer. Baja revoluciones.</p>
    <div class="breathe-circle"></div>
    <button class="modal-btn" onclick="document.getElementById('modal-breathe').classList.remove('active')">Me siento en control</button>
  </div>
</div>

<script>
// =========================================================================
// CORE STATE & CONFIG
// =========================================================================
const STORAGE_KEY = 'man_in_the_mirror_ultimate';

let openDays = new Set();
let appLockedUntil = 0;
try { appLockedUntil = localStorage.getItem('mitm_lock') || 0; } catch(e){}

let settings = { color: '#7B72E9', hellMode: false, currentStreak: 0 };
try { settings = JSON.parse(localStorage.getItem('mitm_settings')) || settings; } catch(e){}

const AREAS = [
  { k:"fe",     label:"Fe & espíritu",         color: () => settings.color },
  { k:"mente",  label:"Mente & aprendizaje",   color: () => "#3A8FE8" },
  { k:"mision", label:"Misión & negocio",      color: () => "#E05A32" },
  { k:"cuerpo", label:"Cuerpo & salud",        color: () => "#2DB88F" },
  { k:"car",    label:"Carácter & disciplina", color: () => "#888884" },
];

let HABITOS = {
  fe: ["Leer la Biblia 30 min", "Reflexión nocturna"],
  mente: ["Leer You Can't Hurt Me 30 min", "Estudiar 1 hora"],
  mision: ["Avanzar aplicación", "El diario del chef"],
  cuerpo: ["Entrenar", "3 litros de agua", "Cumplir Macros", "Ducha fría", "Dormir 8–9 horas"],
  car: ["Cumplir horario del día", "Mantener espacio limpio"]
};
try { HABITOS = JSON.parse(localStorage.getItem('mitm_habitos')) || HABITOS; } catch(e){}

const QS = [
  { q:"¿Oré/medité profundamente hoy?", area:"fe" },
  { q:"¿Mantuve mi propósito por encima de mi ego?", area:"fe" },
  { q:"¿Aprendí algo nuevo y aplicable hoy?", area:"mente" },
  { q:"¿Mantuve concentración profunda (Deep Work)?", area:"mente" },
  { q:"¿Avancé en mi aprendizaje de forma táctica?", area:"mision" },
  { q:"¿Tomé decisiones pensando a largo plazo?", area:"mision" },
  { q:"¿Entrené con la intensidad necesaria?", area:"cuerpo" },
  { q:"¿Evité la comida chatarra/azúcares inútiles?", area:"cuerpo" },
  { q:"¿Cumplí mi horario sin posponer (Procrastinar)?", area:"car" },
  { q:"¿Hice lo difícil aunque no quería hacerlo?", area:"car" }
];

const QUOTES = [
  "No te detengas cuando estés cansado, detente cuando hayas terminado.",
  "El dolor que sientes hoy es la fuerza que sentirás mañana.",
  "Eres el capitán de tu alma y el amo de tu destino.",
  "La disciplina equivale a la libertad.",
  "La motivación te pone en marcha, la disciplina te hace llegar.",
  "The man in the mirror has to change his ways."
];

const VAL = { "Sí":2, "Parcial":1, "No":0 };
let DATA = { days: [], cookies: [] }; 
let quickTasks = [];
try { quickTasks = JSON.parse(localStorage.getItem('mitm_quick')) || []; } catch(e){}

const DIAS_ES = ["Dom","Lun","Mar","Mié","Jue","Vie","Sáb"];
const MESES_ES = ["Ene","Feb","Mar","Abr","May","Jun","Jul","Ago","Sep","Oct","Nov","Dic"];
const EXCUSE_REGEX = /(cansad[oa]|mañana lo hago|no tuve tiempo|difícil|pereza|excusa|muy duro|no pude|no alcancé|frustrad[oa])/i;

// =========================================================================
// UTILITIES & SAFE STORAGE
// =========================================================================
function showToast(msg) {
  const c = document.getElementById('toast-container');
  const t = document.createElement('div');
  t.className = 'toast'; t.textContent = msg;
  c.appendChild(t);
  if(navigator.vibrate) try { navigator.vibrate(40); } catch(e){}
  setTimeout(() => t.remove(), 3000);
}

function playDing() {
  try {
    const ctx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain); gain.connect(ctx.destination);
    osc.type = 'sine'; osc.frequency.setValueAtTime(880, ctx.currentTime);
    gain.gain.setValueAtTime(1, ctx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.5);
    osc.start(ctx.currentTime); osc.stop(ctx.currentTime + 0.5);
  } catch(e) {}
}

function saveData() { 
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify(DATA)); } 
  catch(e) { console.warn("Storage warning:", e); } 
}

function setPrimaryColor(hex) {
  settings.color = hex;
  document.documentElement.style.setProperty('--fe', hex);
  document.getElementById('meta-theme-color').content = document.body.classList.contains('dark-mode') ? '#000' : hex;
  try { localStorage.setItem('mitm_settings', JSON.stringify(settings)); } catch(e){}
  render();
}

function toggleHellMode() {
  settings.hellMode = !settings.hellMode;
  if(settings.hellMode) settings.currentStreak = 0; 
  try { localStorage.setItem('mitm_settings', JSON.stringify(settings)); } catch(e){}
  showToast(settings.hellMode ? "Modo Infierno Activado 🔥" : "Modo Infierno Desactivado");
  render();
}

function lockApp() {
  appLockedUntil = Date.now() + 15 * 60000;
  try { localStorage.setItem('mitm_lock', appLockedUntil); } catch(e){}
  document.getElementById('modal-sos').classList.remove('active');
  checkLock();
}

function checkLock() {
  if (Date.now() < appLockedUntil) {
    document.body.innerHTML = `<div style="display:flex; height:100vh; align-items:center; justify-content:center; flex-direction:column; background:var(--sos); color:white; font-family:'Inter',sans-serif; text-align:center; padding:20px;">
      <div style="font-size:64px; margin-bottom:20px;">🔒</div>
      <h1 style="margin:0 0 10px; font-weight:800;">APLICACIÓN BLOQUEADA</h1>
      <p style="font-size:16px; font-weight:500;">Ve a hacer el trabajo. El bloqueo se levantará en unos minutos.</p>
    </div>`;
    setTimeout(() => location.reload(), appLockedUntil - Date.now());
  }
}

// Swipe Gestures for Tabs
let touchstartX = 0;
document.addEventListener('touchstart', e => touchstartX = e.changedTouches[0].screenX, {passive:true});
document.addEventListener('touchend', e => {
  let touchendX = e.changedTouches[0].screenX;
  if (touchendX < touchstartX - 100) swipe(1);
  if (touchendX > touchstartX + 100) swipe(-1);
});
function swipe(dir) {
  const tabs = Array.from(document.querySelectorAll('.tab:not([data-tab="config"])'));
  const activeIdx = tabs.findIndex(t => t.classList.contains('active'));
  if (activeIdx >= 0) {
    let next = activeIdx + dir;
    if (next >= 0 && next < tabs.length) {
      tabs[next].scrollIntoView({behavior: 'smooth', block: 'nearest', inline: 'center'});
      tabs[next].click();
    }
  }
}

// =========================================================================
// CORE SCORING & LOGIC
// =========================================================================
function getTodayLabel() {
  const now = new Date();
  return `${DIAS_ES[now.getDay()]} ${now.getDate()} ${MESES_ES[now.getMonth()]}`;
}

function autoCreateToday() {
  const today = getTodayLabel();
  if(!DATA.days) DATA.days = [];
  const exists = DATA.days.some(r => r.d === today);
  if (!exists) {
    DATA.days.push({ d: today, note:'', qs:{}, habits: defaultHabits(), auto: true, imgs: [], weight: '', restDay: false });
    saveData();
  }
  const el = document.getElementById('today-label');
  if (el) el.textContent = today + ' ' + (exists ? '✓' : '✨');
}

function defaultHabits() {
  let h = {};
  Object.values(HABITOS).flat().forEach(hab => h[hab] = { done: false, note: '' });
  return h;
}

function getHabScore(r, ak) {
  if (r.restDay) return 100; 
  let habs = ak ? HABITOS[ak] : Object.values(HABITOS).flat();
  if(!habs || habs.length === 0) return null;
  let done = habs.filter(h => r.habits && r.habits[h]?.done).length;
  return (done/habs.length)*100;
}

function getRefScore(r, ak) {
  if (r.restDay) return 100;
  let qsList = ak ? QS.filter(q=>q.area===ak).map(q=>q.q) : QS.map(q=>q.q);
  let vals = qsList.map(q => VAL[r.qs?.[q]]).filter(v => v!=null);
  return vals.length ? (vals.reduce((a,b)=>a+b,0)/(vals.length*2))*100 : null;
}

function dayScore(r) { 
  if (r.restDay) return null; 
  let h=getHabScore(r), rf=getRefScore(r); 
  if(h==null && rf==null) return null;
  return Math.round(((h||0)*0.75) + ((rf||0)*0.25)); 
}

function areaScore(r, ak) {
  if(r.restDay) return null;
  let h = getHabScore(r, ak), rf = getRefScore(r, ak);
  if(h == null && rf == null) return null;
  return Math.round(((h || 0) * 0.75) + ((rf || 0) * 0.25));
}

function areaAvg(ak){ 
  const vs = DATA.days.map(r=>areaScore(r,ak)).filter(v=>v!=null); 
  return vs.length ? Math.round(vs.reduce((a,b)=>a+b,0)/vs.length) : null; 
}

function getStreak(habit) {
  let s = 0;
  for(let i=DATA.days.length-1; i>=0; i--) { 
    if(DATA.days[i].restDay) continue; 
    if(DATA.days[i].habits?.[habit]?.done) s++; else break; 
  }
  return s;
}

function getRank(eliteDays) {
  if (eliteDays < 7) return "Recluta";
  if (eliteDays < 21) return "Soldado";
  if (eliteDays < 60) return "Comandante";
  return "Chef Implacable 🔥";
}

// =========================================================================
// INITIALIZATION
// =========================================================================
function init() {
  checkLock();
  document.getElementById('daily-quote').textContent = `"${QUOTES[Math.floor(Math.random() * QUOTES.length)]}"`;
  
  // 1. Cargar Theme antes que nada
  let theme = 'dark';
  try { theme = localStorage.getItem('theme') || 'dark'; } catch(e){}
  
  if(theme === 'light') {
    document.body.classList.remove('dark-mode');
    document.getElementById('btn-theme').textContent = '🌙';
  } else {
    document.body.classList.add('dark-mode');
    document.getElementById('btn-theme').textContent = '☀️';
  }

  let saved = null;
  try { saved = localStorage.getItem(STORAGE_KEY); } catch(e){}

  if (saved) {
    try {
      let parsed = JSON.parse(saved);
      if(Array.isArray(parsed)) { DATA = { days: parsed, cookies: [] }; }
      else { DATA = parsed; }
      if(!DATA.days) DATA.days = [];
      if(!DATA.cookies) DATA.cookies = [];
    } catch(e) { DATA = {days: [], cookies: []}; }
  } else {
    DATA = { days: [], cookies: [] };
  }
  
  setPrimaryColor(settings.color);
  autoCreateToday();
  
  checkMirror();
  renderConfigHabits();
  render();

  setInterval(() => {
    const today = getTodayLabel();
    if (DATA.days && !DATA.days.some(r => r.d === today)) { autoCreateToday(); render(); }
  }, 60000);
}

// =========================================================================
// RENDER & DOM UPDATES
// =========================================================================
document.querySelectorAll('.tab').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('visible'));
    btn.classList.add('active');
    
    requestAnimationFrame(() => {
      document.getElementById('panel-'+btn.dataset.tab).classList.add('visible');
      render();
    });
  });
});

function render() {
  const scores = DATA.days.map(dayScore);
  const validScores = scores.filter(v=>v!=null);
  const eliteDaysCount = validScores.filter(v => v >= 80).length;
  
  const globalAvg = validScores.length ? Math.round(validScores.reduce((a,b)=>a+b,0)/validScores.length) : 0;
  const globalNumEl = document.getElementById('global-score');
  const isPerfect = validScores.length && validScores[validScores.length - 1] === 100;
  
  globalNumEl.textContent = validScores.length ? globalAvg+'%' : '—';
  globalNumEl.style.color = isPerfect ? 'var(--gold)' : 'inherit';
  
  document.getElementById('rank-title').textContent = getRank(eliteDaysCount);
  document.getElementById('rank-badge').textContent = getRank(eliteDaysCount);

  // Modo Infierno Lógica
  document.getElementById('btn-hell-mode').textContent = settings.hellMode ? "Desactivar Modo Infierno" : "Activar Modo Infierno";
  if (settings.hellMode && validScores.length > 0) {
    if (validScores[validScores.length-1] < 80 && !DATA.days[DATA.days.length-1].restDay) settings.currentStreak = 0;
    else settings.currentStreak = validScores.length; 
  }

  const today = getTodayLabel();
  document.getElementById('today-status').textContent = DATA.days.some(r => r.d === today && r.auto) 
    ? '⚡ Día en progreso' : (DATA.days.some(r => r.d === today) ? '📌 Registrado manualmente' : '');
  
  checkSOS();
  
  // RENDER TAB: RESUMEN
  if(document.getElementById('panel-resumen').classList.contains('visible')) {
    document.getElementById('m-best').textContent = validScores.length ? Math.max(...validScores)+'%' : '—';
    document.getElementById('m-racha').textContent = settings.hellMode ? `${settings.currentStreak} d (Inf)` : `${DATA.days.length} d`;
    document.getElementById('m-elite').textContent = eliteDaysCount;
    
    let minArea = null, minVal = 101;
    AREAS.forEach(a => { let v = areaAvg(a.k); if(v != null && v < minVal) { minVal = v; minArea = a.label; } });
    document.getElementById('m-weak').textContent = minArea || '—';
    
    generateInsights(validScores);
    renderTrendLine(validScores.slice(-14));
    
    document.getElementById('daily-bars').innerHTML = DATA.days.map((r,i) => {
      let s = dayScore(r);
      let isRest = r.restDay;
      let h = s==null ? 2 : Math.max(2, (s/100)*60);
      let c = isRest ? 'var(--cuerpo)' : (r.auto ? 'var(--fe)' : 'var(--txt)');
      return `<div style="flex:1; display:flex; flex-direction:column; align-items:center; justify-content:flex-end; gap:4px; cursor:crosshair;">
        <div style="font-size:10px; font-weight:600; color:var(--txt3)">${s!=null?s+'%':(isRest?'💤':'')}</div>
        <div style="width:100%; background:${c}; height:${h}px; opacity:${s==null&&!isRest?0.15:0.9}; border-radius:4px 4px 0 0;"></div>
      </div>`;
    }).slice(-30).join('');
      
    renderRadar();
  }
  
  // RENDER TAB: ÁREAS
  if(document.getElementById('panel-areas').classList.contains('visible')) {
    document.getElementById('areas-grid').innerHTML = AREAS.map(a => {
      let avg = areaAvg(a.k) || 0;
      let trophy = (avg>=80) ? '🔥' : '';
      return `<div class="card" style="display:flex; justify-content:space-between; align-items:center;">
        <div>
          <div style="font-weight:700; font-size:15px; color:${a.color()}; margin-bottom:4px;">${a.label} ${trophy}</div>
          <div style="font-size:12px; color:var(--txt3)">Promedio histórico</div>
        </div>
        <div style="font-size:32px; font-family:'Cormorant Garamond', serif;">${avg}%</div>
      </div>`;
    }).join('');
  }

  // RENDER TAB: HOY (Hábitos)
  if(document.getElementById('panel-habitos').classList.contains('visible')) {
    if(!DATA.days.length) return document.getElementById('habitos-grid').innerHTML = '<div style="color:var(--txt3);text-align:center;">No hay días aún.</div>';
    document.getElementById('habitos-grid').innerHTML = renderHabitList(DATA.days.length-1);
  }

  // RENDER TAB: TRACKER (Diario)
  if(document.getElementById('panel-tracker').classList.contains('visible')) {
    let displayDays = DATA.days.slice(-30).reverse();
    let renderIndexOffset = DATA.days.length - 1;
    
    document.getElementById('tracker-rows').innerHTML = displayDays.map((r, ri) => {
      const di = renderIndexOffset - ri;
      const isAuto = r.auto ? ' <span class="auto-badge">⚡</span>' : '';
      const sc = r.restDay ? '💤' : (dayScore(r)!=null?dayScore(r)+'%':'—');
      let excuseWarn = EXCUSE_REGEX.test(r.note || '') ? 'block' : 'none';
      return `
      <div class="day-row" id="day-row-${di}">
        <div class="day-header" onclick="toggleBody(${di})">
          <div style="font-weight:700; font-size:15px; display:flex; align-items:center; gap:8px;" onclick="event.stopPropagation()">
             <div contenteditable="true" onblur="updateDayTitle(${di}, this.innerText)" style="outline:none; border-bottom:1px dashed var(--border2); padding-bottom:2px;">${r.d}</div>
             ${isAuto}
          </div>
          <div style="display:flex; align-items:center; gap:12px;">
            <span style="font-weight:600; font-size:15px;">${sc}</span>
            <button class="delete-btn" onclick="event.stopPropagation(); deleteDaySafe(${di}, this)">✕</button>
          </div>
        </div>
        <div class="day-body ${openDays.has(di) ? 'open' : ''}" id="body-${di}">
          
          <div class="section-title">
            Diario de Vida
            <label style="font-size:12px; font-weight:600; display:flex; align-items:center; gap:6px; color:var(--txt); cursor:pointer;">
              <input type="checkbox" ${r.restDay?'checked':''} onchange="toggleRestDay(${di}, this.checked)" style="width:16px;height:16px;"> Día de Descanso
            </label>
          </div>
          <textarea class="journal-input" data-di="${di}" placeholder="Anota aquí... Si notas excusas o quejas, el sistema te alertará. Cero víctimas.">${r.note||''}</textarea>
          <div class="excuse-warning" id="warn-${di}" style="display:${excuseWarn}">⚠️ Alerta: Detectamos una justificación de debilidad en tu texto. Destrúyela.</div>

          <div style="display:flex; gap:10px; margin-top:12px;">
            <input type="text" class="text-input" placeholder="Peso o Récord de Gimnasio..." value="${r.weight||''}" onchange="updateWeight(${di}, this.value)">
          </div>

          <div class="section-title" style="margin-top:24px;">Reflexión Nocturna</div>
          ${AREAS.map(a => {
            let areaQs = QS.filter(q => q.area === a.k);
            if(!areaQs.length) return '';
            return `
              <div style="margin-bottom: 16px; padding-left: 10px; border-left: 4px solid ${a.color()};">
                <div style="font-weight:700; font-size:12px; color:${a.color()}; margin-bottom:10px; letter-spacing:0.5px;">${a.label.toUpperCase()}</div>
                ${areaQs.map(q => `
                  <div style="margin-bottom:12px;">
                    <div style="font-size:13px; font-weight:500; margin-bottom:8px; color:var(--txt2);">${q.q}</div>
                    <div style="display:flex; gap:6px;">
                      ${['Sí', 'Parcial', 'No'].map(opt => {
                        let sel = r.qs && r.qs[q.q] === opt;
                        let cls = sel ? 'selected' : '';
                        return `<button class="ref-btn ${cls}" onclick="setReflexion(${di}, '${q.q.replace(/'/g, "\\'")}', '${opt}')">${opt}</button>`;
                      }).join('')}
                    </div>
                  </div>
                `).join('')}
              </div>
            `;
          }).join('')}
          
          <div class="section-title" style="margin-top:24px;">Foto del Día</div>
          <input type="file" style="font-size:12px; margin-bottom:10px;" accept="image/*" onchange="handleImage(this, ${di})">
          <div class="photo-gallery">
            ${(r.imgs||[]).map(src => `<img class="photo-preview" src="${src}">`).join('')}
          </div>
          
          <div style="margin-top:20px; text-align:right;">
             <button class="btn-add" style="background:transparent; border:1px solid var(--txt); color:var(--txt);" onclick="shareDay(${di})">Compartir Reporte 📤</button>
          </div>
        </div>
      </div>`;
    }).join('');
    
    if (DATA.days.length > 30) {
        document.getElementById('tracker-rows').innerHTML += `<div style="text-align:center; color:var(--txt3); font-size:12px; margin-top:20px;">Mostrando los últimos 30 días. Los datos antiguos están seguros.</div>`;
    }
  }
  
  if(document.getElementById('panel-heatmap').classList.contains('visible')) renderHeatmap(30);
  if(document.getElementById('panel-boveda').classList.contains('visible')) renderVault();
  if(document.getElementById('panel-cookiejar').classList.contains('visible')) renderCookieJar();
  if(document.getElementById('panel-config').classList.contains('visible')) renderConfigHabits();
}

function shareDay(di) {
  const r = DATA.days[di];
  let text = `🔥 Informe: ${r.d}\nPuntuación: ${r.restDay ? 'Descanso Activo 💤' : (dayScore(r) + '%')}\n\n`;
  if(r.note) text += `Diario: ${r.note}\n`;
  navigator.clipboard.writeText(text);
  showToast("Reporte copiado al portapapeles 📤");
}

function updateDayTitle(di, val) { DATA.days[di].d = val; saveData(); }
function toggleRestDay(di, val) { DATA.days[di].restDay = val; saveData(); render(); showToast(val ? "Día de Descanso. Recupérate." : "Descanso desactivado."); }
function updateWeight(di, val) { DATA.days[di].weight = val; saveData(); }

function deleteDaySafe(di, btn) {
  if(btn.dataset.confirm !== 'true') {
    btn.dataset.confirm = 'true';
    const oldHtml = btn.innerHTML;
    btn.innerHTML = '⚠️'; btn.style.background = 'var(--sos)'; btn.style.color = 'white';
    setTimeout(() => {
      if(btn) { btn.dataset.confirm = 'false'; btn.innerHTML = oldHtml; btn.style.background = ''; btn.style.color = ''; }
    }, 3000);
    return;
  }
  DATA.days.splice(di, 1); openDays.delete(di); saveData(); render(); showToast("Día eliminado");
}

function checkSOS() {
  const btnSOS = document.getElementById('btn-sos');
  if (DATA.days.length < 3) { btnSOS.style.display = 'none'; return; }
  const last3 = DATA.days.slice(-3).filter(r => !r.restDay && dayScore(r) != null);
  if (last3.length < 3) { btnSOS.style.display = 'none'; return; }
  let avg = Math.round(last3.reduce((a,b)=>a+dayScore(b),0)/last3.length);
  btnSOS.style.display = (avg < 50) ? 'flex' : 'none';
}

function setReflexion(di, q, val) {
  if(!DATA.days[di].qs) DATA.days[di].qs = {};
  DATA.days[di].qs[q] = val; saveData(); render();
}

function renderRadar() {
  const svg = document.getElementById('radar-svg');
  const cx=120, cy=120, R=82, avgs=AREAS.map(a=>areaAvg(a.k)), isDark=document.body.classList.contains('dark-mode');
  const p = (i,f) => { const a=(i/AREAS.length)*2*Math.PI-Math.PI/2; return `${(cx+R*f*Math.cos(a)).toFixed(1)},${(cy+R*f*Math.sin(a)).toFixed(1)}`; };
  let html = [.25,.5,.75,1].map(f => `<polygon points="${AREAS.map((_,i)=>p(i,f)).join(' ')}" fill="none" stroke="${isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.07)'}" stroke-width="0.5"/>`).join('');
  AREAS.forEach((_,i)=>html+=`<line x1="${cx}" y1="${cy}" x2="${p(i,1).split(',')[0]}" y2="${p(i,1).split(',')[1]}" stroke="${isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.07)'}" stroke-width="0.5"/>`);
  const poly = AREAS.map((_,i)=>p(i,(avgs[i]??0)/100)).join(' ');
  html += `<polygon points="${poly}" fill="${settings.color}" fill-opacity="0.15" stroke="${settings.color}" stroke-width="1.5"/>`;
  AREAS.forEach((a,i)=> {
    const coords = p(i,(avgs[i]??0)/100).split(',');
    html+=`<circle cx="${coords[0]}" cy="${coords[1]}" r="4" fill="${a.color()}"/>`;
    const lc = p(i,1.25).split(',');
    html+=`<text x="${lc[0]}" y="${lc[1]}" text-anchor="middle" dominant-baseline="middle" font-weight="600" font-size="10" fill="${isDark?'#999':'#666'}">${a.label.split('&')[0].trim()}</text>`;
  });
  svg.innerHTML = html;
}

function renderTrendLine(recentData) {
  const svg = document.getElementById('trend-svg');
  if(recentData.length < 2) { svg.innerHTML = ''; return; }
  const maxW = 240, maxH = 50;
  const step = maxW / (recentData.length - 1);
  let pts = recentData.map((s, i) => `${i*step},${maxH - (s/100)*maxH}`).join(' ');
  svg.innerHTML = `<polyline points="${pts}" fill="none" stroke="var(--fe)" stroke-width="2" stroke-linecap="round"/>
    ${recentData.map((s,i) => `<circle cx="${i*step}" cy="${maxH - (s/100)*maxH}" r="3" fill="var(--bg)" stroke="var(--fe)" stroke-width="2"/>`).join('')}`;
}

function renderHeatmap(limit = 0) {
  const inner = document.getElementById('heat-inner');
  let mapData = limit ? DATA.days.slice(-limit) : DATA.days;
  if(!mapData.length) { inner.innerHTML='<div style="color:var(--txt3);font-size:13px;padding:10px 0;">Aún no hay días registrados.</div>'; return; }
  inner.innerHTML = `<div class="heat-days-header">${mapData.map(r => `<div class="heat-day-label" title="${r.d}">${r.d.split(' ')[0]}</div>`).join('')}</div>` +
    AREAS.map(a => `<div class="heat-row"><div class="heat-area-label" style="color:${a.color()}">${a.label}</div><div class="heat-cells">
      ${mapData.map(r => { 
        if(r.restDay) return `<div class="heat-cell" style="background:var(--bg3); display:flex;align-items:center;justify-content:center;font-size:10px;" title="${r.d} - Descanso">💤</div>`;
        const s = areaScore(r,a.k); 
        const c = s==null ? 'var(--bg3)' : s>=90 ? a.color() : s>=65 ? a.color()+'bf' : s>=35 ? a.color()+'66' : a.color()+'26'; 
        return `<div class="heat-cell" style="background:${c}" title="${r.d}: ${s!=null?s+'%':'—'}"></div>`;
      }).join('')}
    </div></div>`).join('');
}

function renderHabitList(di) {
  const row = DATA.days[di];
  if(!row.habits) row.habits = defaultHabits();
  return AREAS.map(a => {
    if(!HABITOS[a.k] || !HABITOS[a.k].length) return '';
    let html = `<div style="font-weight:700; font-size:12px; color:${a.color()}; margin:16px 0 8px; letter-spacing:0.5px; text-transform:uppercase;">${a.label}</div>`;
    HABITOS[a.k].forEach(h => {
      if(!row.habits[h]) row.habits[h] = {done:false, note:''};
      let hd = row.habits[h];
      
      let safeH = h.replace(/'/g, "\\'");
      let safeHInput = h.replace(/"/g, "&quot;");
      let streak = getStreak(h);
      let fire = streak > 2 ? `<span class="habit-streak">🔥 ${streak}</span>` : '';
      let timerBtn = `<button class="btn-timer" onclick="openTimer('${safeH}', ${di})">⏲️</button>`;
      let bg = hd.done ? a.color() : 'transparent';
      let chk = hd.done ? `<svg width="16" height="16"><polyline points="3,8 7,12 14,4" stroke="white" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>` : '';
      
      html += `
      <div class="habit-row-wrap">
        <div class="habit-top">
          <div class="habit-chk" style="background:${bg}; border-color:${hd.done?a.color():'var(--border2)'}" onclick="toggleHabit(${di}, '${safeH}')">${chk}</div>
          <div class="habit-name" style="text-decoration:${hd.done?'line-through':'none'}; color:${hd.done?'var(--txt3)':'var(--txt)'}" onclick="toggleHabit(${di}, '${safeH}')">${h} ${fire}</div>
          ${timerBtn}
        </div>
        <input type="text" class="text-input habit-note" data-di="${di}" data-h="${safeHInput}" value="${hd.note||''}" placeholder="Medición o Sub-tareas...">
      </div>`;
    });
    return html;
  }).join('');
}

function toggleBody(di) { 
  if (openDays.has(di)) {
    openDays.delete(di);
    document.getElementById('body-'+di).classList.remove('open');
  } else {
    openDays.add(di);
    document.getElementById('body-'+di).classList.add('open');
  }
}

function toggleHabit(di, h) { 
  if (!DATA.days[di].habits) DATA.days[di].habits = defaultHabits();
  if (!DATA.days[di].habits[h]) DATA.days[di].habits[h] = {done: false, note: ''};
  DATA.days[di].habits[h].done = !DATA.days[di].habits[h].done; 
  if(DATA.days[di].habits[h].done) { playDing(); showToast('¡Hábito marcado! 🔥'); }
  saveData(); render(); 
}

// ─── Añadir Día Manual con Modal Personalizado ───────────────
document.getElementById('btn-add-day').addEventListener('click', () => {
  document.getElementById('input-new-day-name').value = getTodayLabel();
  document.getElementById('modal-add-day').classList.add('active');
});

document.getElementById('btn-confirm-add-day').addEventListener('click', () => {
  let d = document.getElementById('input-new-day-name').value.trim();
  if(d) {
    DATA.days.push({ d, note:'', qs:{}, habits:defaultHabits(), auto: false, imgs: [], weight: '', restDay: false }); 
    const newDi = DATA.days.length - 1;
    openDays.add(newDi); 
    saveData(); 
    render(); 
    document.getElementById('modal-add-day').classList.remove('active');
    showToast('Día añadido al historial');
    setTimeout(()=> { 
      const row = document.getElementById('day-row-'+newDi);
      if(row) row.scrollIntoView({behavior: 'smooth', block: 'center'}); 
    }, 150);
  }
});

// Event Delegation for inputs
document.addEventListener('input', e => {
  if(e.target.classList.contains('journal-input')) { 
    let val = e.target.value;
    DATA.days[e.target.dataset.di].note = val; 
    saveData(); 
    let warnEl = document.getElementById('warn-'+e.target.dataset.di);
    if(warnEl) warnEl.style.display = EXCUSE_REGEX.test(val) ? 'block' : 'none';
  }
  if(e.target.classList.contains('habit-note')) { 
    if (!DATA.days[e.target.dataset.di].habits) DATA.days[e.target.dataset.di].habits = defaultHabits();
    if (!DATA.days[e.target.dataset.di].habits[e.target.dataset.h]) DATA.days[e.target.dataset.di].habits[e.target.dataset.h] = {done: false, note: ''};
    DATA.days[e.target.dataset.di].habits[e.target.dataset.h].note = e.target.value; 
    saveData(); 
  }
  if(e.target.id === 'vault-search') renderVault(e.target.value);
});

// =========================================================================
// CONFIGURACIÓN (Hábitos y Colores)
// =========================================================================
function renderConfigHabits() {
  const area = document.getElementById('habit-area-select').value;
  const list = document.getElementById('config-habit-list');
  list.innerHTML = HABITOS[area].map((h, i) => {
    return `
    <div style="display:flex; justify-content:space-between; align-items:center; background:var(--bg2); padding:10px 14px; margin-bottom:6px; border-radius:8px; border:1px solid var(--border); font-size:13px; font-weight:500;">
      <span>${h}</span>
      <button class="delete-btn" onclick="deleteHabitSafe('${area}', ${i}, this)">✕</button>
    </div>`;
  }).join('');
}
function addNewHabit() {
  const area = document.getElementById('habit-area-select').value;
  const val = document.getElementById('new-habit-input').value.trim();
  if(val && !HABITOS[area].includes(val)) {
    HABITOS[area].push(val);
    try { localStorage.setItem('mitm_habitos', JSON.stringify(HABITOS)); } catch(e){}
    document.getElementById('new-habit-input').value = '';
    renderConfigHabits(); render(); showToast('Hábito añadido al sistema');
  }
}
function deleteHabitSafe(area, index, btn) {
  if(btn.dataset.confirm !== 'true') {
    btn.dataset.confirm = 'true';
    const oldHtml = btn.innerHTML;
    btn.innerHTML = '⚠️'; btn.style.background = 'var(--sos)'; btn.style.color = 'white';
    setTimeout(() => { if(btn) { btn.dataset.confirm = 'false'; btn.innerHTML = oldHtml; btn.style.background = ''; btn.style.color = ''; } }, 3000);
    return;
  }
  HABITOS[area].splice(index, 1);
  try { localStorage.setItem('mitm_habitos', JSON.stringify(HABITOS)); } catch(e){}
  renderConfigHabits(); render(); showToast('Hábito eliminado');
}

// =========================================================================
// COOKIE JAR, VAULT, IMAGES, EXPORT
// =========================================================================
document.getElementById('btn-add-cookie').addEventListener('click', () => {
  const input = document.getElementById('cookie-input');
  if(input.value.trim()) {
    DATA.cookies.push({ text: input.value.trim(), date: new Date().toLocaleDateString() });
    input.value = ''; saveData(); renderCookieJar(); showToast('Victoria Inmortalizada 🍪');
  }
});

function renderCookieJar() {
  const list = document.getElementById('cookie-list');
  if(!DATA.cookies.length) { list.innerHTML = "<div style='color:var(--txt3); font-size:13px; padding:20px; text-align:center;'>Tu tarro está vacío. Supera algo que odies y regístralo aquí.</div>"; return; }
  list.innerHTML = DATA.cookies.slice().reverse().map(c => 
    `<div class="cookie-jar-item">"${c.text}" <span style="display:block; font-size:10px; color:var(--txt3); margin-top:6px; font-weight:600;">- ${c.date}</span></div>`
  ).join('');
}

function handleImage(input, di) {
  if(!input.files || !input.files[0]) return;
  const reader = new FileReader();
  reader.onload = e => {
    const img = new Image();
    img.onload = () => {
      const cvs = document.createElement('canvas');
      const MAX_W = 600; const scale = Math.min(MAX_W / img.width, 1); 
      cvs.width = img.width * scale; cvs.height = img.height * scale;
      cvs.getContext('2d').drawImage(img, 0, 0, cvs.width, cvs.height);
      const b64 = cvs.toDataURL('image/jpeg', 0.6); 
      if(!DATA.days[di].imgs) DATA.days[di].imgs = [];
      DATA.days[di].imgs.push(b64); saveData(); render();
    }
    img.src = e.target.result;
  }
  reader.readAsDataURL(input.files[0]);
}

function generateInsights(validScores) {
  const el = document.getElementById('insights-content');
  if(DATA.days.length < 3) return el.innerHTML = "Recolectando datos... Necesitas al menos 3 días.";
  
  let sleepDays = DATA.days.filter(r => r.habits?.['Dormir 8–9 horas']?.done);
  let sleepScoreAvg = sleepDays.length ? Math.round(sleepDays.reduce((a,b)=>a+(dayScore(b)||0),0)/sleepDays.length) : 0;
  
  let txt = `Cuando duermes tus horas, rindes al <b>${sleepScoreAvg}%</b> de tu capacidad. `;
  
  if(validScores.length >= 14) {
    let last7 = validScores.slice(-7).reduce((a,b)=>a+b,0)/7;
    let prev7 = validScores.slice(-14, -7).reduce((a,b)=>a+b,0)/7;
    if(last7 > prev7) txt += `<br><span style="color:var(--cuerpo)">📈 Has mejorado respecto a la semana pasada.</span>`;
    else txt += `<br><span style="color:var(--sos)">📉 Tu rendimiento cayó respecto a la semana anterior.</span>`;
  }
  el.innerHTML = txt;
}

function renderVault(filter = "") {
  const el = document.getElementById('vault-content');
  let html = ""; let f = filter.toLowerCase();
  let words = {};
  
  DATA.days.slice().reverse().forEach(r => {
    if(r.note) {
      r.note.split(/\s+/).forEach(w => { 
        w = w.toLowerCase().replace(/[^a-zñáéíóú]/g,''); 
        if(w.length > 4) words[w] = (words[w]||0)+1; 
      });
    }

    if(r.note && r.note.toLowerCase().includes(f) && f) {
      html += `<div style="background:var(--bg2); padding:14px; border-radius:10px; border:1px solid var(--border);">
          <div style="font-size:11px; color:var(--txt3); font-weight:700; margin-bottom:8px; text-transform:uppercase;">${r.d} · Diario</div>
          <div style="font-size:14px; line-height:1.6;">${r.note.replace(/#victoria/gi, '<b>#victoria</b> 🍪')}</div>
        </div>`;
    }

    if(!r.habits) return;
    Object.keys(HABITOS).forEach(ak => {
      HABITOS[ak].forEach(h => {
        let note = r.habits[h]?.note;
        if(note && note.trim() && note.toLowerCase().includes(f)) {
          html += `<div style="background:var(--bg2); padding:14px; border-radius:10px; border:1px solid var(--border);">
            <div style="font-size:11px; color:${AREAS.find(a=>a.k===ak)?.color()}; font-weight:700; margin-bottom:8px; text-transform:uppercase;">${r.d} · ${h}</div>
            <div style="font-size:14px; line-height:1.6;">${note}</div>
          </div>`;
        }
      });
    });
  });

  let sortedWords = Object.entries(words).sort((a,b)=>b[1]-a[1]).slice(0, 10);
  document.getElementById('vault-wordcloud').innerHTML = "<b>Foco mental reciente: </b>" + sortedWords.map(w => `<span style="font-weight:600; opacity:${Math.min(1, w[1]*0.1 + 0.5)}">${w[0]}</span>`).join(', ');

  el.innerHTML = html || "<div style='color:var(--txt3); font-size:13px; padding:20px; text-align:center;'>No hay resultados. (Escribe para buscar)</div>";
}

function exportData() {
  const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(DATA));
  const dl = document.createElement('a'); dl.setAttribute("href", dataStr); dl.setAttribute("download", "mitm_backup.json"); dl.click();
}

function importData(e) {
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const importedData = JSON.parse(ev.target.result);
      if(importedData.days) { DATA = importedData; saveData(); showToast("Sistema Restaurado"); setTimeout(()=>location.reload(),1000); } 
      else if (Array.isArray(importedData)) { DATA = { days: importedData, cookies: [] }; saveData(); showToast("Datos migrados"); setTimeout(()=>location.reload(),1000); } 
      else { showToast("Error: Archivo JSON no válido"); }
    } catch(err) { showToast("Error: JSON corrupto"); }
  };
  reader.readAsText(file);
}

function exportVault() {
  let text = "Bóveda de Conocimiento - Man in the Mirror\n\n";
  DATA.days.forEach(r => { if(r.note) text += `[ ${r.d} ]\n${r.note}\n\n`; });
  const dataStr = "data:text/plain;charset=utf-8," + encodeURIComponent(text);
  const dl = document.createElement('a'); dl.setAttribute("href", dataStr); dl.setAttribute("download", "boveda_mitm.txt"); dl.click();
}

// =========================================================================
// MODALS LOGIC (Timer, Mirror, Future, Quick)
// =========================================================================
let timerInterval = null, timerSeconds = 0, timerRunning = false, timerDi = -1, timerHabit = '';

function openTimer(habit, di) {
  const modal = document.getElementById('modal-timer');
  timerHabit = habit; timerDi = di; timerRunning = false;
  document.getElementById('timer-input-m').value = 45; 
  document.getElementById('timer-display-s').textContent = "00";
  document.getElementById('timer-title').textContent = `Enfoque: ${habit}`;
  document.getElementById('btn-timer-action').textContent = '▶ Iniciar';
  if (timerInterval) clearInterval(timerInterval);
  modal.classList.add('active');
}

document.getElementById('btn-timer-action').addEventListener('click', () => {
  const btn = document.getElementById('btn-timer-action');
  if (!timerRunning) {
    let m = parseInt(document.getElementById('timer-input-m').value) || 25;
    if(timerSeconds === 0) timerSeconds = m * 60;
    timerRunning = true; btn.textContent = '⏸ Pausar';
    document.getElementById('timer-input-m').disabled = true;
    
    timerInterval = setInterval(() => {
      timerSeconds--;
      document.getElementById('timer-input-m').value = Math.floor(timerSeconds / 60);
      document.getElementById('timer-display-s').textContent = String(timerSeconds % 60).padStart(2,'0');
      
      if (timerSeconds <= 0) {
        clearInterval(timerInterval); timerInterval = null; timerRunning = false;
        btn.textContent = '✅ Misión Cumplida'; playDing();
        if (timerDi >= 0 && timerHabit && DATA.days[timerDi]) {
          if (!DATA.days[timerDi].habits) DATA.days[timerDi].habits = defaultHabits();
          DATA.days[timerDi].habits[timerHabit].done = true; saveData(); render();
        }
        setTimeout(() => { document.getElementById('modal-timer').classList.remove('active'); document.getElementById('timer-input-m').disabled = false;}, 1500);
      }
    }, 1000);
  } else {
    timerRunning = false; btn.textContent = '▶ Reanudar'; clearInterval(timerInterval);
  }
});

document.getElementById('btn-timer-close').addEventListener('click', () => {
  clearInterval(timerInterval); timerRunning = false; timerSeconds = 0;
  document.getElementById('timer-input-m').disabled = false;
  document.getElementById('modal-timer').classList.remove('active');
});

function checkMirror() {
  if (DATA.days.length === 0) return;
  const last = DATA.days[DATA.days.length - 1];
  if(last.restDay) return;
  const hs = Object.values(HABITOS).flat();
  let doneCount = hs.filter(h => last.habits?.[h]?.done).length;
  if (doneCount < Math.floor(hs.length * 0.4) && DATA.days.length >= 2) {
    document.getElementById('mirror-text').innerHTML = `
      El espejo no miente. Tú sabes que estás rindiendo muy por debajo de tu potencial.<br><br>
      <i>"Nadie va a venir a salvarte."</i>
    `;
    document.getElementById('modal-mirror').classList.add('active');
  }
}

document.getElementById('mirror-input').addEventListener('input', function() {
  const btn = document.getElementById('btn-mirror-unlock');
  if (this.value.trim().toUpperCase() === "NO ME RENDIRÉ") {
    btn.disabled = false; btn.style.background = 'var(--txt)'; btn.style.color = 'var(--bg)'; btn.style.cursor = 'pointer';
  } else {
    btn.disabled = true; btn.style.background = 'var(--txt3)'; btn.style.cursor = 'not-allowed';
  }
});
document.getElementById('btn-mirror-unlock').addEventListener('click', () => { document.getElementById('modal-mirror').classList.remove('active'); });

document.getElementById('btn-future').addEventListener('click', () => {
  const avg = DATA.days.map(dayScore).filter(v=>v!=null);
  const avgScore = avg.length ? Math.round(avg.reduce((a,b)=>a+b,0)/avg.length) : 0;
  let text = avgScore >= 80 ? "Te volverás imparable. Tus metas actuales se verán ridículamente pequeñas." : 
             avgScore >= 60 ? "Avanzarás, pero la mediocridad seguirá cerca. Aprieta las tuercas." :
             "Si sigues así, en 1 año estarás exactamente en el mismo maldito lugar que odias hoy.";
  document.getElementById('future-content').innerHTML = `
    <div style="margin-bottom:16px; font-size:14px; font-weight:600;">Proyección a 1 año basada en tu constancia (${avgScore}%):</div>
    <div style="color:var(--txt2); font-size:16px; font-style:italic;">"${text}"</div>
  `;
  document.getElementById('modal-future').classList.add('active');
});

document.getElementById('btn-quick').addEventListener('click', () => {
  document.getElementById('modal-quick').classList.add('active');
  renderQuickTasks();
});

document.getElementById('quick-input').addEventListener('keypress', e => {
  if(e.key === 'Enter' && e.target.value.trim()) {
    quickTasks.push(e.target.value.trim());
    try { localStorage.setItem('mitm_quick', JSON.stringify(quickTasks)); } catch(e){}
    e.target.value = ''; renderQuickTasks();
  }
});

function renderQuickTasks() {
  document.getElementById('quick-list').innerHTML = quickTasks.map((t, i) => `
    <div style="display:flex; justify-content:space-between; background:var(--bg2); padding:12px; border-radius:8px; margin-bottom:8px; font-size:14px; align-items:center;">
      <span>${t}</span>
      <button class="delete-btn" style="color:var(--cuerpo); font-size:18px;" onclick="removeQuick(${i})">✓</button>
    </div>
  `).join('');
}

function removeQuick(i) {
  quickTasks.splice(i, 1);
  try { localStorage.setItem('mitm_quick', JSON.stringify(quickTasks)); } catch(e){}
  renderQuickTasks(); showToast("Tarea fulminada ⚡");
}

document.getElementById('btn-sos').addEventListener('click', () => { document.getElementById('modal-sos').classList.add('active'); });

// Keyboard Shortcuts (PC)
document.addEventListener('keydown', e => {
  if(e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;
  if(e.key.toLowerCase() === 'h') document.querySelector('.tab[data-tab="habitos"]').click();
  if(e.key.toLowerCase() === 'd') document.querySelector('.tab[data-tab="tracker"]').click();
});

// START
init();

/* =========================================================================
   PWA INJECTION (OFFLINE MAGIC) 
   ========================================================================= */
(function initPWA() {
  if ('serviceWorker' in navigator) {
    const swCode = `
      const CACHE = 'mitm-elite-v1';
      self.addEventListener('install', e => { self.skipWaiting(); });
      self.addEventListener('activate', e => { self.clients.claim(); });
      self.addEventListener('fetch', e => {
        e.respondWith(fetch(e.request).catch(() => new Response("App Offline Ready", {status:200, headers:{'Content-Type':'text/html'}})));
      });
    `;
    const blob = new Blob([swCode], {type: 'application/javascript'});
    navigator.serviceWorker.register(URL.createObjectURL(blob)).catch(()=>{});
  }
})();
</script>
</body>
</html>
