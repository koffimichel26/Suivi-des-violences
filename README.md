<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Suivi des Violences en Milieu Scolaire</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;800&family=Source+Sans+3:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --navy: #0a1628;
    --blue: #1a3a6b;
    --blue-mid: #2563a8;
    --blue-light: #3b82f6;
    --blue-pale: #dbeafe;
    --red: #c0392b;
    --red-light: #e74c3c;
    --red-pale: #fde8e8;
    --green: #1a6b3a;
    --green-light: #27ae60;
    --green-pale: #d1fae5;
    --gold: #d4a017;
    --gold-light: #f59e0b;
    --white: #ffffff;
    --gray-50: #f8fafc;
    --gray-100: #f1f5f9;
    --gray-200: #e2e8f0;
    --gray-300: #cbd5e1;
    --gray-500: #64748b;
    --gray-700: #334155;
    --gray-900: #0f172a;
    --shadow-sm: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.08);
    --shadow-md: 0 4px 16px rgba(0,0,0,0.12);
    --shadow-lg: 0 8px 32px rgba(0,0,0,0.16);
    --radius: 12px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

  body {
    font-family: 'Source Sans 3', sans-serif;
    background: var(--gray-50);
    color: var(--gray-900);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── HEADER ── */
  header {
    background: linear-gradient(135deg, var(--navy) 0%, var(--blue) 60%, var(--blue-mid) 100%);
    color: white;
    padding: 0;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: var(--shadow-lg);
  }

  .header-top {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 14px 16px 10px;
    border-bottom: 1px solid rgba(255,255,255,0.1);
  }

  .header-logo {
    width: 44px; height: 44px;
    background: rgba(255,255,255,0.15);
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 22px;
    flex-shrink: 0;
  }

  .header-title h1 {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
    font-weight: 700;
    line-height: 1.2;
    letter-spacing: 0.01em;
  }

  .header-title p {
    font-size: 11px;
    opacity: 0.7;
    margin-top: 2px;
  }

  .header-meta {
    padding: 8px 16px;
    display: flex;
    gap: 8px;
    align-items: center;
  }

  .header-meta input {
    background: rgba(255,255,255,0.12);
    border: 1px solid rgba(255,255,255,0.2);
    color: white;
    padding: 5px 10px;
    border-radius: 6px;
    font-size: 12px;
    font-family: inherit;
    flex: 1;
  }

  .header-meta input::placeholder { color: rgba(255,255,255,0.5); }

  /* ── BOTTOM NAV ── */
  nav {
    position: fixed;
    bottom: 0; left: 0; right: 0;
    background: white;
    display: flex;
    border-top: 2px solid var(--gray-200);
    z-index: 100;
    box-shadow: 0 -4px 20px rgba(0,0,0,0.08);
  }

  .nav-btn {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 10px 4px 8px;
    background: none;
    border: none;
    cursor: pointer;
    color: var(--gray-500);
    font-size: 10px;
    font-family: inherit;
    font-weight: 600;
    letter-spacing: 0.03em;
    transition: color 0.2s;
    gap: 4px;
    -webkit-tap-highlight-color: transparent;
  }

  .nav-btn .nav-icon { font-size: 22px; }

  .nav-btn.active {
    color: var(--blue-mid);
  }

  .nav-btn.active .nav-icon {
    transform: scale(1.1);
  }

  /* ── PAGES ── */
  .page {
    display: none;
    padding: 14px 14px 90px;
    min-height: calc(100vh - 120px);
  }

  .page.active { display: block; }

  /* ── CARDS ── */
  .card {
    background: white;
    border-radius: var(--radius);
    box-shadow: var(--shadow-sm);
    margin-bottom: 14px;
    overflow: hidden;
  }

  .card-header {
    padding: 14px 16px 10px;
    border-bottom: 1px solid var(--gray-100);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .card-header-icon {
    width: 34px; height: 34px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
  }

  .card-header h2 {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
    font-weight: 700;
    color: var(--navy);
    flex: 1;
  }

  .card-body { padding: 14px 16px; }

  /* ── FORM ELEMENTS ── */
  .form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 10px;
  }

  .form-group { display: flex; flex-direction: column; gap: 4px; }

  .form-group label {
    font-size: 11px;
    font-weight: 700;
    color: var(--gray-500);
    letter-spacing: 0.06em;
    text-transform: uppercase;
  }

  .form-group select,
  .form-group input,
  .form-group textarea {
    padding: 10px 12px;
    border: 1.5px solid var(--gray-200);
    border-radius: var(--radius-sm);
    font-family: inherit;
    font-size: 14px;
    color: var(--gray-900);
    background: var(--gray-50);
    transition: border-color 0.2s;
    -webkit-appearance: none;
  }

  .form-group select:focus,
  .form-group input:focus,
  .form-group textarea:focus {
    outline: none;
    border-color: var(--blue-light);
    background: white;
  }

  textarea { resize: vertical; min-height: 70px; }

  /* ── BUTTONS ── */
  .btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
    padding: 11px 18px;
    border: none;
    border-radius: var(--radius-sm);
    font-family: inherit;
    font-weight: 700;
    font-size: 13px;
    cursor: pointer;
    letter-spacing: 0.02em;
    transition: all 0.18s;
    -webkit-tap-highlight-color: transparent;
  }

  .btn-primary { background: var(--blue-mid); color: white; }
  .btn-primary:active { background: var(--blue); transform: scale(0.97); }

  .btn-danger { background: var(--red); color: white; }
  .btn-danger:active { background: #a93226; transform: scale(0.97); }

  .btn-success { background: var(--green-light); color: white; }
  .btn-success:active { background: var(--green); transform: scale(0.97); }

  .btn-gold { background: var(--gold); color: white; }
  .btn-gold:active { transform: scale(0.97); }

  .btn-outline {
    background: transparent;
    border: 2px solid var(--blue-mid);
    color: var(--blue-mid);
  }

  .btn-sm { padding: 7px 12px; font-size: 12px; }
  .btn-full { width: 100%; }

  .btn-group {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }

  .btn-group .btn { flex: 1; min-width: 120px; }

  /* ── DATA TABLE ── */
  .table-wrapper {
    overflow-x: auto;
    border-radius: var(--radius-sm);
    border: 1px solid var(--gray-200);
    -webkit-overflow-scrolling: touch;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 12px;
    min-width: 620px;
  }

  thead { background: var(--navy); color: white; }

  thead th {
    padding: 10px 10px;
    text-align: left;
    font-weight: 700;
    letter-spacing: 0.04em;
    font-size: 11px;
    white-space: nowrap;
  }

  tbody tr { border-bottom: 1px solid var(--gray-100); }
  tbody tr:last-child { border-bottom: none; }
  tbody tr:nth-child(even) { background: var(--gray-50); }

  tbody td {
    padding: 9px 10px;
    vertical-align: middle;
  }

  .badge {
    display: inline-block;
    padding: 3px 8px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 700;
  }

  .badge-t1 { background: var(--blue-pale); color: var(--blue); }
  .badge-t2 { background: var(--green-pale); color: var(--green); }
  .badge-t3 { background: var(--red-pale); color: var(--red); }

  /* ── STAT CARDS ── */
  .stat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 14px;
  }

  .stat-card {
    background: white;
    border-radius: var(--radius);
    padding: 14px 12px;
    box-shadow: var(--shadow-sm);
    display: flex;
    flex-direction: column;
    gap: 4px;
    border-left: 4px solid transparent;
  }

  .stat-card.blue { border-color: var(--blue-light); }
  .stat-card.red { border-color: var(--red-light); }
  .stat-card.green { border-color: var(--green-light); }
  .stat-card.gold { border-color: var(--gold-light); }

  .stat-number {
    font-family: 'Playfair Display', serif;
    font-size: 28px;
    font-weight: 800;
    color: var(--navy);
    line-height: 1;
  }

  .stat-label {
    font-size: 11px;
    font-weight: 600;
    color: var(--gray-500);
    letter-spacing: 0.04em;
    text-transform: uppercase;
  }

  /* ── CHARTS ── */
  .chart-container {
    position: relative;
    height: 220px;
    width: 100%;
    padding: 0 4px;
  }

  .chart-container.tall { height: 260px; }

  /* ── ANALYSIS BOX ── */
  .analysis-box {
    background: linear-gradient(135deg, #eef4ff 0%, #f0fdf4 100%);
    border-radius: var(--radius);
    padding: 14px;
    border: 1px solid var(--blue-pale);
    margin-bottom: 10px;
  }

  .analysis-box h3 {
    font-family: 'Playfair Display', serif;
    font-size: 14px;
    color: var(--navy);
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .analysis-item {
    display: flex;
    gap: 8px;
    margin-bottom: 6px;
    font-size: 13px;
    line-height: 1.4;
    color: var(--gray-700);
  }

  .analysis-item .dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--blue-mid);
    flex-shrink: 0;
    margin-top: 5px;
  }

  /* ── SENSITIVITY MESSAGES ── */
  .trimestre-section {
    border: 1.5px solid var(--gray-200);
    border-radius: var(--radius-sm);
    margin-bottom: 10px;
    overflow: hidden;
  }

  .trimestre-header {
    background: var(--gray-100);
    padding: 10px 14px;
    font-weight: 700;
    font-size: 13px;
    color: var(--navy);
    border-bottom: 1px solid var(--gray-200);
  }

  .trimestre-body { padding: 12px 14px; }

  /* ── RAPPORT SECTION ── */
  .rapport-section {
    background: white;
    border: 1px solid var(--gray-200);
    border-radius: var(--radius);
    padding: 16px;
    margin-bottom: 14px;
  }

  .rapport-section h3 {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
    color: var(--navy);
    margin-bottom: 10px;
    padding-bottom: 8px;
    border-bottom: 2px solid var(--blue-pale);
  }

  .rapport-section p, .rapport-section li {
    font-size: 13px;
    color: var(--gray-700);
    line-height: 1.6;
  }

  .rapport-section ul { padding-left: 18px; margin-top: 6px; }
  .rapport-section li { margin-bottom: 4px; }

  /* ── TOAST ── */
  #toast {
    position: fixed;
    bottom: 80px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: var(--navy);
    color: white;
    padding: 10px 20px;
    border-radius: 30px;
    font-size: 13px;
    font-weight: 600;
    opacity: 0;
    transition: all 0.3s;
    z-index: 999;
    white-space: nowrap;
    pointer-events: none;
  }

  #toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }

  /* ── EMPTY STATE ── */
  .empty-state {
    text-align: center;
    padding: 40px 20px;
    color: var(--gray-500);
  }

  .empty-state .empty-icon { font-size: 48px; margin-bottom: 12px; }
  .empty-state p { font-size: 14px; }

  /* ── LOADING ── */
  #pdfLoading {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(10,22,40,0.85);
    z-index: 999;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 16px;
    color: white;
    font-size: 15px;
    font-weight: 600;
  }

  #pdfLoading.show { display: flex; }

  .spinner {
    width: 44px; height: 44px;
    border: 4px solid rgba(255,255,255,0.2);
    border-top-color: white;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  /* Scrollbar */
  ::-webkit-scrollbar { width: 4px; height: 4px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--gray-300); border-radius: 4px; }

  /* ── PRINT / PDF ── */
  #pdfContent {
    display: none;
    font-family: 'Source Sans 3', Arial, sans-serif;
    font-size: 12px;
    color: #111;
  }

  /* Niveau tags */
  .niveau-tag {
    display: inline-block;
    background: var(--navy);
    color: white;
    padding: 2px 7px;
    border-radius: 4px;
    font-size: 11px;
    font-weight: 700;
  }

  /* Action buttons in table */
  .table-actions { display: flex; gap: 4px; }
  .btn-icon {
    width: 28px; height: 28px;
    border-radius: 6px;
    border: none;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    transition: transform 0.15s;
  }
  .btn-icon:active { transform: scale(0.9); }
  .btn-icon-edit { background: var(--blue-pale); }
  .btn-icon-delete { background: var(--red-pale); }

  /* Divider */
  .divider {
    height: 1px;
    background: var(--gray-200);
    margin: 12px 0;
  }

  /* Progress bar */
  .progress-bar {
    height: 6px;
    background: var(--gray-200);
    border-radius: 10px;
    overflow: hidden;
    margin-top: 4px;
  }
  .progress-fill {
    height: 100%;
    border-radius: 10px;
    transition: width 0.6s ease;
  }

  /* Commentaire inputs */
  .comment-grid { display: grid; gap: 8px; }
  .comment-item { display: flex; flex-direction: column; gap: 4px; }
  .comment-item label { font-size: 12px; font-weight: 700; color: var(--gray-500); }
  .comment-item textarea {
    padding: 8px 10px;
    border: 1.5px solid var(--gray-200);
    border-radius: var(--radius-sm);
    font-family: inherit;
    font-size: 13px;
    resize: vertical;
    min-height: 60px;
  }
  .comment-item textarea:focus { outline: none; border-color: var(--blue-light); }
</style>
</head>
<body>

<header>
  <div class="header-top">
    <div class="header-logo">🏫</div>
    <div class="header-title">
      <h1>Suivi des Violences Scolaires</h1>
      <p>Rapport – Proviseur de Lycée</p>
    </div>
  </div>
  <div class="header-meta">
    <input type="text" id="schoolName" placeholder="Nom du lycée…" style="flex:2">
    <input type="text" id="schoolYear" placeholder="Année scolaire" value="2024–2025" style="flex:1">
  </div>
</header>

<!-- ══════════════ PAGE : SAISIE ══════════════ -->
<div id="page-saisie" class="page active">

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#dbeafe">📝</div>
      <h2>Ajouter un incident</h2>
    </div>
    <div class="card-body">
      <div class="form-row">
        <div class="form-group">
          <label>Trimestre</label>
          <select id="f-trimestre">
            <option value="T1">1er Trimestre</option>
            <option value="T2">2e Trimestre</option>
            <option value="T3">3e Trimestre</option>
          </select>
        </div>
        <div class="form-group">
          <label>Niveau</label>
          <select id="f-niveau">
            <option>6e</option><option>5e</option><option>4e</option>
            <option>3e</option><option>2nde</option><option>1ère</option><option>Tle</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Type de violence</label>
          <select id="f-type">
            <option>Violence physique</option>
            <option>Violence verbale</option>
            <option>Violence psychologique</option>
            <option>Menace / intimidation</option>
            <option>Bagarre</option>
          </select>
        </div>
        <div class="form-group">
          <label>Lieu</label>
          <select id="f-lieu">
            <option>Classe</option>
            <option>Cour</option>
            <option>Sortie école</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Acteurs</label>
          <select id="f-acteurs">
            <option>Élève–Élève</option>
            <option>Élève–Professeur</option>
          </select>
        </div>
        <div class="form-group">
          <label>Nombre d'incidents</label>
          <input type="number" id="f-nb" min="1" value="1" placeholder="ex: 3">
        </div>
      </div>
      <input type="hidden" id="editId" value="">
      <div class="btn-group">
        <button class="btn btn-primary" onclick="addIncident()">➕ Ajouter</button>
        <button class="btn btn-outline btn-sm" onclick="cancelEdit()">✕ Annuler</button>
      </div>
    </div>
  </div>

  <!-- Informations complémentaires par trimestre -->
  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#d1fae5">💬</div>
      <h2>Informations par trimestre</h2>
    </div>
    <div class="card-body" style="padding-top:10px">
      <div class="trimestre-section">
        <div class="trimestre-header">🔵 1er Trimestre</div>
        <div class="trimestre-body">
          <div class="form-group" style="margin-bottom:8px">
            <label>Message de sensibilisation</label>
            <textarea id="msg-T1" placeholder="Campagne de sensibilisation menée…"></textarea>
          </div>
          <div class="form-group">
            <label>Mesures prises</label>
            <textarea id="mesures-T1" placeholder="Sanctions, convocations, médiations…"></textarea>
          </div>
        </div>
      </div>
      <div class="trimestre-section">
        <div class="trimestre-header">🟢 2e Trimestre</div>
        <div class="trimestre-body">
          <div class="form-group" style="margin-bottom:8px">
            <label>Message de sensibilisation</label>
            <textarea id="msg-T2" placeholder="Campagne de sensibilisation menée…"></textarea>
          </div>
          <div class="form-group">
            <label>Mesures prises</label>
            <textarea id="mesures-T2" placeholder="Sanctions, convocations, médiations…"></textarea>
          </div>
        </div>
      </div>
      <div class="trimestre-section">
        <div class="trimestre-header">🔴 3e Trimestre</div>
        <div class="trimestre-body">
          <div class="form-group" style="margin-bottom:8px">
            <label>Message de sensibilisation</label>
            <textarea id="msg-T3" placeholder="Campagne de sensibilisation menée…"></textarea>
          </div>
          <div class="form-group">
            <label>Mesures prises</label>
            <textarea id="mesures-T3" placeholder="Sanctions, convocations, médiations…"></textarea>
          </div>
        </div>
      </div>
      <button class="btn btn-success btn-full" onclick="saveMeta()">💾 Enregistrer</button>
    </div>
  </div>

  <!-- Tableau des incidents -->
  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fde8e8">📋</div>
      <h2>Incidents enregistrés</h2>
      <div style="margin-left:auto;display:flex;gap:6px">
        <button class="btn btn-outline btn-sm" onclick="exportCSV()">📤 CSV</button>
        <label class="btn btn-outline btn-sm" style="cursor:pointer">
          📥 Import
          <input type="file" accept=".csv" style="display:none" onchange="importCSV(event)">
        </label>
      </div>
    </div>
    <div class="card-body" style="padding:0">
      <div id="tableContainer" class="table-wrapper">
        <div class="empty-state">
          <div class="empty-icon">📭</div>
          <p>Aucun incident enregistré.<br>Ajoutez votre premier incident ci-dessus.</p>
        </div>
      </div>
    </div>
  </div>

</div>

<!-- ══════════════ PAGE : STATISTIQUES ══════════════ -->
<div id="page-stats" class="page">

  <div class="stat-grid" id="statCards"></div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#dbeafe">📊</div>
      <h2>Incidents par trimestre</h2>
    </div>
    <div class="card-body">
      <div class="chart-container"><canvas id="chartTrimestre"></canvas></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fde8e8">🔴</div>
      <h2>Types de violence par trimestre</h2>
    </div>
    <div class="card-body">
      <div class="chart-container tall"><canvas id="chartTypes"></canvas></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#d1fae5">🥧</div>
      <h2>Répartition des violences</h2>
    </div>
    <div class="card-body">
      <div class="chart-container"><canvas id="chartPie"></canvas></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fef9c3">📍</div>
      <h2>Lieux et acteurs</h2>
    </div>
    <div class="card-body">
      <div class="chart-container tall"><canvas id="chartLieux"></canvas></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#f3e8ff">🎓</div>
      <h2>Violences par niveau</h2>
    </div>
    <div class="card-body">
      <div class="chart-container"><canvas id="chartNiveaux"></canvas></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#ffedd5">📈</div>
      <h2>Comparaison niveaux / trimestres</h2>
    </div>
    <div class="card-body">
      <div class="chart-container tall"><canvas id="chartComparaison"></canvas></div>
    </div>
  </div>

  <!-- Tableau statistique -->
  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#dbeafe">🔢</div>
      <h2>Tableau statistique</h2>
    </div>
    <div class="card-body" style="padding:0">
      <div id="statTable" class="table-wrapper"></div>
    </div>
  </div>

  <!-- Classement -->
  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fde8e8">🏆</div>
      <h2>Classement des niveaux</h2>
    </div>
    <div class="card-body">
      <div id="rankingList"></div>
    </div>
  </div>

  <!-- Analyse automatique -->
  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#d1fae5">🤖</div>
      <h2>Analyse automatique</h2>
    </div>
    <div class="card-body">
      <div id="autoAnalysis"></div>
    </div>
  </div>

</div>

<!-- ══════════════ PAGE : RAPPORT ══════════════ -->
<div id="page-rapport" class="page">

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fef9c3">✍️</div>
      <h2>Commentaires</h2>
    </div>
    <div class="card-body">
      <div class="form-group" style="margin-bottom:10px">
        <label>Commentaire global</label>
        <textarea id="com-global" placeholder="Analyse globale de la situation de violence…" style="min-height:80px"></textarea>
      </div>
      <div class="divider"></div>
      <p style="font-size:12px;font-weight:700;color:var(--gray-500);letter-spacing:.05em;text-transform:uppercase;margin-bottom:8px">Par niveau</p>
      <div class="comment-grid">
        <div class="comment-item"><label>6e</label><textarea id="com-6e" placeholder="Observations pour la 6e…"></textarea></div>
        <div class="comment-item"><label>5e</label><textarea id="com-5e" placeholder="Observations pour la 5e…"></textarea></div>
        <div class="comment-item"><label>4e</label><textarea id="com-4e" placeholder="Observations pour la 4e…"></textarea></div>
        <div class="comment-item"><label>3e</label><textarea id="com-3e" placeholder="Observations pour la 3e…"></textarea></div>
        <div class="comment-item"><label>2nde</label><textarea id="com-2nde" placeholder="Observations pour la 2nde…"></textarea></div>
        <div class="comment-item"><label>1ère</label><textarea id="com-1ere" placeholder="Observations pour la 1ère…"></textarea></div>
        <div class="comment-item"><label>Tle</label><textarea id="com-tle" placeholder="Observations pour la Tle…"></textarea></div>
      </div>
      <div class="divider"></div>
      <div class="form-group">
        <label>Conclusion générale</label>
        <textarea id="com-conclusion" placeholder="Conclusion et perspectives pour l'année suivante…" style="min-height:90px"></textarea>
      </div>
      <button class="btn btn-success btn-full" onclick="saveComments()">💾 Enregistrer</button>
    </div>
  </div>

  <div class="card">
    <div class="card-header">
      <div class="card-header-icon" style="background:#fde8e8">📄</div>
      <h2>Aperçu du rapport</h2>
    </div>
    <div class="card-body" id="rapportPreview">
      <p style="color:var(--gray-500);font-size:13px">L'aperçu s'affiche ici après génération.</p>
    </div>
  </div>

  <div class="btn-group">
    <button class="btn btn-gold btn-full" onclick="generatePDF()" style="font-size:15px;padding:14px">
      🖨️ Générer rapport PDF
    </button>
  </div>
  <div style="height:8px"></div>
  <div class="btn-group">
    <button class="btn btn-primary" onclick="previewRapport()">👁️ Aperçu</button>
    <button class="btn btn-outline" onclick="exportCSV()">📤 Export CSV</button>
  </div>

</div>

<!-- Loading overlay -->
<div id="pdfLoading">
  <div class="spinner"></div>
  <p>Génération du PDF en cours…</p>
</div>

<!-- Toast -->
<div id="toast"></div>

<!-- Nav -->
<nav>
  <button class="nav-btn active" id="nav-saisie" onclick="showPage('saisie')">
    <span class="nav-icon">📝</span>
    <span>Saisie</span>
  </button>
  <button class="nav-btn" id="nav-stats" onclick="showPage('stats')">
    <span class="nav-icon">📊</span>
    <span>Statistiques</span>
  </button>
  <button class="nav-btn" id="nav-rapport" onclick="showPage('rapport')">
    <span class="nav-icon">📄</span>
    <span>Rapport</span>
  </button>
</nav>

<!-- Hidden PDF canvas area -->
<div id="pdfContent"></div>

<script>
// ══════════════════════════════════════════════
//  DATA STORE
// ══════════════════════════════════════════════
const NIVEAUX = ['6e','5e','4e','3e','2nde','1ère','Tle'];
const TYPES   = ['Violence physique','Violence verbale','Violence psychologique','Menace / intimidation','Bagarre'];
const LIEUX   = ['Classe','Cour','Sortie école'];
const ACTEURS = ['Élève–Élève','Élève–Professeur'];
const TRIM    = ['T1','T2','T3'];

let incidents = [];
let meta      = { T1:{msg:'',mesures:''}, T2:{msg:'',mesures:''}, T3:{msg:'',mesures:''} };
let comments  = { global:'', niveaux:{}, conclusion:'' };
let charts    = {};
let editId    = null;

// ══════════════════════════════════════════════
//  PERSISTENCE
// ══════════════════════════════════════════════
function save() {
  localStorage.setItem('svsData', JSON.stringify({
    incidents, meta, comments,
    school: document.getElementById('schoolName').value,
    year:   document.getElementById('schoolYear').value
  }));
}

function load() {
  const d = localStorage.getItem('svsData');
  if (!d) return;
  const p = JSON.parse(d);
  incidents = p.incidents || [];
  meta      = p.meta      || meta;
  comments  = p.comments  || comments;
  if (p.school) document.getElementById('schoolName').value = p.school;
  if (p.year)   document.getElementById('schoolYear').value = p.year;
  // load meta fields
  ['T1','T2','T3'].forEach(t => {
    if (meta[t]) {
      document.getElementById('msg-'+t).value     = meta[t].msg     || '';
      document.getElementById('mesures-'+t).value = meta[t].mesures || '';
    }
  });
  // load comments
  if (comments.global) document.getElementById('com-global').value = comments.global;
  if (comments.conclusion) document.getElementById('com-conclusion').value = comments.conclusion;
  if (comments.niveaux) {
    NIVEAUX.forEach(n => {
      const el = document.getElementById('com-'+n.replace('è','e').replace('è','e').replace('1ère','1ere').replace('Tle','tle').replace('2nde','2nde'));
      if (el && comments.niveaux[n]) el.value = comments.niveaux[n];
    });
  }
}

// ══════════════════════════════════════════════
//  NAVIGATION
// ══════════════════════════════════════════════
function showPage(p) {
  document.querySelectorAll('.page').forEach(el => el.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(el => el.classList.remove('active'));
  document.getElementById('page-'+p).classList.add('active');
  document.getElementById('nav-'+p).classList.add('active');
  if (p === 'stats')   { updateStats(); }
  if (p === 'rapport') { loadComments(); previewRapport(); }
}

// ══════════════════════════════════════════════
//  INCIDENTS CRUD
// ══════════════════════════════════════════════
function addIncident() {
  const nb = parseInt(document.getElementById('f-nb').value);
  if (!nb || nb < 1) { toast('⚠️ Nombre invalide'); return; }

  const inc = {
    id:        editId || Date.now().toString(),
    trimestre: document.getElementById('f-trimestre').value,
    niveau:    document.getElementById('f-niveau').value,
    type:      document.getElementById('f-type').value,
    lieu:      document.getElementById('f-lieu').value,
    acteurs:   document.getElementById('f-acteurs').value,
    nb
  };

  if (editId) {
    const idx = incidents.findIndex(i => i.id === editId);
    if (idx > -1) incidents[idx] = inc;
    editId = null;
    document.getElementById('editId').value = '';
  } else {
    incidents.push(inc);
  }

  document.getElementById('f-nb').value = 1;
  save();
  renderTable();
  toast('✅ Incident enregistré');
}

function editIncident(id) {
  const inc = incidents.find(i => i.id === id);
  if (!inc) return;
  editId = id;
  document.getElementById('f-trimestre').value = inc.trimestre;
  document.getElementById('f-niveau').value    = inc.niveau;
  document.getElementById('f-type').value      = inc.type;
  document.getElementById('f-lieu').value      = inc.lieu;
  document.getElementById('f-acteurs').value   = inc.acteurs;
  document.getElementById('f-nb').value        = inc.nb;
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function deleteIncident(id) {
  incidents = incidents.filter(i => i.id !== id);
  save();
  renderTable();
  toast('🗑️ Supprimé');
}

function cancelEdit() {
  editId = null;
  document.getElementById('f-nb').value = 1;
}

// ══════════════════════════════════════════════
//  RENDER TABLE
// ══════════════════════════════════════════════
function renderTable() {
  const c = document.getElementById('tableContainer');
  if (!incidents.length) {
    c.innerHTML = `<div class="empty-state"><div class="empty-icon">📭</div><p>Aucun incident enregistré.<br>Ajoutez votre premier incident ci-dessus.</p></div>`;
    return;
  }
  const trimColor = { T1:'badge-t1', T2:'badge-t2', T3:'badge-t3' };
  const trimLabel = { T1:'T1', T2:'T2', T3:'T3' };
  c.innerHTML = `
    <table>
      <thead>
        <tr>
          <th>Trim.</th><th>Niveau</th><th>Type</th><th>Lieu</th><th>Acteurs</th><th>Nb</th><th>Actions</th>
        </tr>
      </thead>
      <tbody>
        ${incidents.map(i => `
          <tr>
            <td><span class="badge ${trimColor[i.trimestre]||''}">${i.trimestre}</span></td>
            <td><span class="niveau-tag">${i.niveau}</span></td>
            <td style="font-size:11px">${i.type}</td>
            <td style="font-size:11px">${i.lieu}</td>
            <td style="font-size:11px">${i.acteurs}</td>
            <td><strong>${i.nb}</strong></td>
            <td>
              <div class="table-actions">
                <button class="btn-icon btn-icon-edit" onclick="editIncident('${i.id}')">✏️</button>
                <button class="btn-icon btn-icon-delete" onclick="deleteIncident('${i.id}')">🗑️</button>
              </div>
            </td>
          </tr>
        `).join('')}
      </tbody>
    </table>`;
}

// ══════════════════════════════════════════════
//  META SAVE
// ══════════════════════════════════════════════
function saveMeta() {
  ['T1','T2','T3'].forEach(t => {
    meta[t].msg     = document.getElementById('msg-'+t).value;
    meta[t].mesures = document.getElementById('mesures-'+t).value;
  });
  save();
  toast('✅ Informations enregistrées');
}

// ══════════════════════════════════════════════
//  STATISTICS
// ══════════════════════════════════════════════
function getTotalByTrim(t) {
  return incidents.filter(i => i.trimestre === t).reduce((s,i) => s+i.nb, 0);
}

function getTotalByNiveau(n) {
  return incidents.filter(i => i.niveau === n).reduce((s,i) => s+i.nb, 0);
}

function getTotalByType(tp) {
  return incidents.filter(i => i.type === tp).reduce((s,i) => s+i.nb, 0);
}

function getTotalByLieu(l) {
  return incidents.filter(i => i.lieu === l).reduce((s,i) => s+i.nb, 0);
}

function getTotalByActeurs(a) {
  return incidents.filter(i => i.acteurs === a).reduce((s,i) => s+i.nb, 0);
}

function grandTotal() {
  return incidents.reduce((s,i) => s+i.nb, 0);
}

function updateStats() {
  if (!incidents.length) {
    document.getElementById('statCards').innerHTML = `<div class="empty-state" style="grid-column:1/-1"><div class="empty-icon">📊</div><p>Ajoutez des incidents pour voir les statistiques.</p></div>`;
    return;
  }
  const gt = grandTotal();
  const t1 = getTotalByTrim('T1');
  const t2 = getTotalByTrim('T2');
  const t3 = getTotalByTrim('T3');

  // Stat cards
  document.getElementById('statCards').innerHTML = `
    <div class="stat-card blue">
      <div class="stat-number">${gt}</div>
      <div class="stat-label">Total incidents</div>
    </div>
    <div class="stat-card red">
      <div class="stat-number">${t1}</div>
      <div class="stat-label">1er Trimestre</div>
    </div>
    <div class="stat-card green">
      <div class="stat-number">${t2}</div>
      <div class="stat-label">2e Trimestre</div>
    </div>
    <div class="stat-card gold">
      <div class="stat-number">${t3}</div>
      <div class="stat-label">3e Trimestre</div>
    </div>`;

  renderCharts(t1,t2,t3,gt);
  renderStatTable(t1,t2,t3,gt);
  renderRanking(gt);
  renderAutoAnalysis(t1,t2,t3,gt);
}

// ══════════════════════════════════════════════
//  CHARTS
// ══════════════════════════════════════════════
const COLORS = {
  blue:   '#2563a8', red: '#c0392b', green: '#27ae60',
  gold:   '#d4a017', purple: '#7c3aed', orange: '#ea580c',
  teal:   '#0d9488', pink: '#db2777'
};
const PAL = Object.values(COLORS);

function destroyChart(id) { if (charts[id]) { charts[id].destroy(); delete charts[id]; } }

function renderCharts(t1,t2,t3,gt) {
  // Chart 1 – Barres trimestres
  destroyChart('chartTrimestre');
  charts['chartTrimestre'] = new Chart(document.getElementById('chartTrimestre'), {
    type: 'bar',
    data: {
      labels: ['1er Trimestre','2e Trimestre','3e Trimestre'],
      datasets: [{
        label: 'Incidents',
        data: [t1, t2, t3],
        backgroundColor: [COLORS.blue, COLORS.green, COLORS.red],
        borderRadius: 6
      }]
    },
    options: chartOpts('Incidents')
  });

  // Chart 2 – Types par trimestre (groupé)
  destroyChart('chartTypes');
  charts['chartTypes'] = new Chart(document.getElementById('chartTypes'), {
    type: 'bar',
    data: {
      labels: ['T1','T2','T3'],
      datasets: TYPES.map((tp, idx) => ({
        label: tp,
        data: TRIM.map(t => incidents.filter(i => i.trimestre===t && i.type===tp).reduce((s,i)=>s+i.nb,0)),
        backgroundColor: PAL[idx],
        borderRadius: 4
      }))
    },
    options: { ...chartOpts('Incidents'), plugins: { legend: { display: true, position: 'bottom', labels: { font: { size: 10 }, boxWidth: 12 } } } }
  });

  // Chart 3 – Camembert types
  destroyChart('chartPie');
  const typeData = TYPES.map(tp => getTotalByType(tp));
  charts['chartPie'] = new Chart(document.getElementById('chartPie'), {
    type: 'doughnut',
    data: {
      labels: TYPES,
      datasets: [{ data: typeData, backgroundColor: PAL, borderWidth: 2, borderColor: '#fff' }]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { position: 'bottom', labels: { font: { size: 10 }, boxWidth: 12 } } }
    }
  });

  // Chart 4 – Lieux + Acteurs (empilé)
  destroyChart('chartLieux');
  charts['chartLieux'] = new Chart(document.getElementById('chartLieux'), {
    type: 'bar',
    data: {
      labels: [...LIEUX, ...ACTEURS],
      datasets: [{
        label: 'Incidents',
        data: [...LIEUX.map(l => getTotalByLieu(l)), ...ACTEURS.map(a => getTotalByActeurs(a))],
        backgroundColor: [...LIEUX.map((_,i) => PAL[i]), ...ACTEURS.map((_,i) => PAL[i+4])],
        borderRadius: 6
      }]
    },
    options: chartOpts('Incidents')
  });

  // Chart 5 – Par niveau
  destroyChart('chartNiveaux');
  charts['chartNiveaux'] = new Chart(document.getElementById('chartNiveaux'), {
    type: 'bar',
    data: {
      labels: NIVEAUX,
      datasets: [{
        label: 'Total incidents',
        data: NIVEAUX.map(n => getTotalByNiveau(n)),
        backgroundColor: NIVEAUX.map((_,i) => PAL[i % PAL.length]),
        borderRadius: 6
      }]
    },
    options: chartOpts('Incidents')
  });

  // Chart 6 – Niveaux par trimestre
  destroyChart('chartComparaison');
  charts['chartComparaison'] = new Chart(document.getElementById('chartComparaison'), {
    type: 'bar',
    data: {
      labels: NIVEAUX,
      datasets: TRIM.map((t,i) => ({
        label: t === 'T1' ? '1er Trimestre' : t === 'T2' ? '2e Trimestre' : '3e Trimestre',
        data: NIVEAUX.map(n => incidents.filter(inc => inc.trimestre===t && inc.niveau===n).reduce((s,i)=>s+i.nb,0)),
        backgroundColor: [COLORS.blue, COLORS.green, COLORS.red][i],
        borderRadius: 4
      }))
    },
    options: { ...chartOpts('Incidents'), plugins: { legend: { display: true, position: 'bottom', labels: { font: { size: 10 }, boxWidth: 12 } } } }
  });
}

function chartOpts(yLabel) {
  return {
    responsive: true, maintainAspectRatio: false,
    plugins: { legend: { display: false } },
    scales: {
      y: { beginAtZero: true, ticks: { precision: 0, font: { size: 10 } }, grid: { color: '#f1f5f9' } },
      x: { ticks: { font: { size: 10 } }, grid: { display: false } }
    }
  };
}

// ══════════════════════════════════════════════
//  STAT TABLE
// ══════════════════════════════════════════════
function renderStatTable(t1,t2,t3,gt) {
  const el = document.getElementById('statTable');
  if (!gt) { el.innerHTML = ''; return; }
  el.innerHTML = `
    <table>
      <thead>
        <tr>
          <th>Niveau</th>
          <th>T1</th><th>T2</th><th>T3</th>
          <th>Total</th><th>%</th>
        </tr>
      </thead>
      <tbody>
        ${NIVEAUX.map(n => {
          const nt1 = incidents.filter(i=>i.trimestre==='T1'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
          const nt2 = incidents.filter(i=>i.trimestre==='T2'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
          const nt3 = incidents.filter(i=>i.trimestre==='T3'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
          const tot = nt1+nt2+nt3;
          const pct = gt ? ((tot/gt)*100).toFixed(1) : 0;
          return `<tr>
            <td><span class="niveau-tag">${n}</span></td>
            <td>${nt1}</td><td>${nt2}</td><td>${nt3}</td>
            <td><strong>${tot}</strong></td>
            <td><span style="color:var(--blue-mid);font-weight:700">${pct}%</span></td>
          </tr>`;
        }).join('')}
        <tr style="background:var(--navy);color:white;font-weight:700">
          <td>TOTAL</td>
          <td>${t1}</td><td>${t2}</td><td>${t3}</td>
          <td>${gt}</td><td>100%</td>
        </tr>
      </tbody>
    </table>`;
}

// ══════════════════════════════════════════════
//  RANKING
// ══════════════════════════════════════════════
function renderRanking(gt) {
  const ranked = NIVEAUX.map(n => ({ n, tot: getTotalByNiveau(n) })).sort((a,b) => b.tot-a.tot);
  const el = document.getElementById('rankingList');
  el.innerHTML = ranked.map((r,i) => {
    const pct = gt ? ((r.tot/gt)*100).toFixed(1) : 0;
    const colors = ['#c0392b','#e67e22','#f39c12','#27ae60','#2563a8','#7c3aed','#0d9488'];
    return `
      <div style="margin-bottom:10px">
        <div style="display:flex;align-items:center;gap:8px;margin-bottom:3px">
          <span style="font-weight:800;color:${colors[i]};font-size:16px;width:20px">${i+1}</span>
          <span class="niveau-tag">${r.n}</span>
          <span style="font-size:13px;color:var(--gray-700);flex:1">${r.tot} incident${r.tot>1?'s':''}</span>
          <span style="font-size:12px;font-weight:700;color:var(--gray-500)">${pct}%</span>
        </div>
        <div class="progress-bar" style="margin-left:28px">
          <div class="progress-fill" style="width:${pct}%;background:${colors[i]}"></div>
        </div>
      </div>`;
  }).join('');
}

// ══════════════════════════════════════════════
//  AUTO ANALYSIS
// ══════════════════════════════════════════════
function renderAutoAnalysis(t1,t2,t3,gt) {
  const el = document.getElementById('autoAnalysis');
  if (!gt) { el.innerHTML = '<p style="color:var(--gray-500);font-size:13px">Données insuffisantes pour l\'analyse.</p>'; return; }

  const insights = [];

  // Tendance trimestrielle
  if (t2 < t1 && t1 > 0) insights.push(`Baisse observée au 2e trimestre (${t1} → ${t2} incidents, −${t1-t2}).`);
  if (t2 > t1 && t1 > 0) insights.push(`Hausse au 2e trimestre (${t1} → ${t2} incidents, +${t2-t1}).`);
  if (t3 > t2 && t2 > 0) insights.push(`Hausse au 3e trimestre (${t2} → ${t3} incidents, +${t3-t2}).`);
  if (t3 < t2 && t2 > 0) insights.push(`Baisse au 3e trimestre (${t2} → ${t3} incidents, −${t2-t3}).`);
  if (t1 === 0 && t2 === 0 && t3 === 0) insights.push('Aucun incident enregistré sur les trois trimestres.');

  // Niveau le plus touché
  const ranked = NIVEAUX.map(n => ({ n, tot: getTotalByNiveau(n) })).sort((a,b)=>b.tot-a.tot);
  if (ranked[0].tot > 0) insights.push(`Le niveau ${ranked[0].n} est le plus touché avec ${ranked[0].tot} incidents.`);
  if (ranked[ranked.length-1].tot === 0) insights.push(`Aucun incident enregistré en ${ranked[ranked.length-1].n}.`);

  // Type dominant
  const typeRanked = TYPES.map(t => ({ t, tot: getTotalByType(t) })).sort((a,b)=>b.tot-a.tot);
  if (typeRanked[0].tot > 0) insights.push(`Type de violence le plus fréquent : « ${typeRanked[0].t} » (${typeRanked[0].tot} cas).`);

  // Lieu dominant
  const lieuRanked = LIEUX.map(l => ({ l, tot: getTotalByLieu(l) })).sort((a,b)=>b.tot-a.tot);
  if (lieuRanked[0].tot > 0) insights.push(`Lieu le plus touché : « ${lieuRanked[0].l} » (${lieuRanked[0].tot} incidents).`);

  // Amélioration
  const improved = NIVEAUX.filter(n => {
    const byTrim = TRIM.map(t => incidents.filter(i=>i.trimestre===t&&i.niveau===n).reduce((s,i)=>s+i.nb,0));
    return byTrim[1] < byTrim[0] && byTrim[0] > 0;
  });
  if (improved.length) insights.push(`Amélioration constatée au 2e trimestre en : ${improved.join(', ')}.`);

  el.innerHTML = `
    <div class="analysis-box">
      <h3>🔍 Analyse globale</h3>
      ${insights.map(i => `<div class="analysis-item"><div class="dot"></div><span>${i}</span></div>`).join('')}
    </div>`;
}

// ══════════════════════════════════════════════
//  COMMENTS
// ══════════════════════════════════════════════
function loadComments() {
  document.getElementById('com-global').value      = comments.global || '';
  document.getElementById('com-conclusion').value  = comments.conclusion || '';
  const map = {'6e':'6e','5e':'5e','4e':'4e','3e':'3e','2nde':'2nde','1ère':'1ere','Tle':'tle'};
  NIVEAUX.forEach(n => {
    const el = document.getElementById('com-'+map[n]);
    if (el) el.value = (comments.niveaux && comments.niveaux[n]) || '';
  });
}

function saveComments() {
  const map = {'6e':'6e','5e':'5e','4e':'4e','3e':'3e','2nde':'2nde','1ère':'1ere','Tle':'tle'};
  comments.global     = document.getElementById('com-global').value;
  comments.conclusion = document.getElementById('com-conclusion').value;
  comments.niveaux = {};
  NIVEAUX.forEach(n => {
    const el = document.getElementById('com-'+map[n]);
    if (el) comments.niveaux[n] = el.value;
  });
  save();
  toast('✅ Commentaires enregistrés');
}

// ══════════════════════════════════════════════
//  RAPPORT PREVIEW
// ══════════════════════════════════════════════
function previewRapport() {
  saveComments();
  const school = document.getElementById('schoolName').value || '[Nom du lycée]';
  const year   = document.getElementById('schoolYear').value || '';
  const gt     = grandTotal();
  const t1 = getTotalByTrim('T1'), t2 = getTotalByTrim('T2'), t3 = getTotalByTrim('T3');

  const el = document.getElementById('rapportPreview');
  el.innerHTML = `
    <div class="rapport-section">
      <h3>📋 En-tête</h3>
      <p><strong>Établissement :</strong> ${school}</p>
      <p><strong>Année scolaire :</strong> ${year}</p>
      <p><strong>Date de génération :</strong> ${new Date().toLocaleDateString('fr-FR')}</p>
    </div>

    <div class="rapport-section">
      <h3>📊 Résumé global</h3>
      <p>Total d'incidents enregistrés sur l'année : <strong>${gt}</strong></p>
      <ul>
        <li>1er Trimestre : <strong>${t1}</strong> incident${t1>1?'s':''}</li>
        <li>2e Trimestre : <strong>${t2}</strong> incident${t2>1?'s':''}</li>
        <li>3e Trimestre : <strong>${t3}</strong> incident${t3>1?'s':''}</li>
      </ul>
    </div>

    ${comments.global ? `<div class="rapport-section"><h3>💬 Commentaire global</h3><p>${comments.global.replace(/\n/g,'<br>')}</p></div>` : ''}

    <div class="rapport-section">
      <h3>🎓 Analyse par niveau</h3>
      ${NIVEAUX.map(n => {
        const tot = getTotalByNiveau(n);
        const com = comments.niveaux && comments.niveaux[n] ? comments.niveaux[n] : '';
        return `<p style="margin-bottom:6px"><span class="niveau-tag">${n}</span> — ${tot} incident${tot>1?'s':''}.${com ? ' '+com : ''}</p>`;
      }).join('')}
    </div>

    ${[1,2,3].map(q => {
      const t = 'T'+q;
      const m = meta[t];
      return (m && (m.msg || m.mesures)) ? `
        <div class="rapport-section">
          <h3>📅 ${q===1?'1er':q+'e'} Trimestre – Informations</h3>
          ${m.msg ? `<p><strong>Sensibilisation :</strong> ${m.msg.replace(/\n/g,'<br>')}</p>` : ''}
          ${m.mesures ? `<p style="margin-top:6px"><strong>Mesures prises :</strong> ${m.mesures.replace(/\n/g,'<br>')}</p>` : ''}
        </div>` : '';
    }).join('')}

    ${comments.conclusion ? `<div class="rapport-section"><h3>🏁 Conclusion générale</h3><p>${comments.conclusion.replace(/\n/g,'<br>')}</p></div>` : ''}
  `;
}

// ══════════════════════════════════════════════
//  PDF GENERATION
// ══════════════════════════════════════════════
async function generatePDF() {
  if (!incidents.length) { toast('⚠️ Aucun incident à inclure'); return; }
  saveComments();

  document.getElementById('pdfLoading').classList.add('show');

  await new Promise(r => setTimeout(r, 100));

  try {
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF({ unit: 'mm', format: 'a4' });
    const W = 210, H = 297;
    const margin = 15;
    let y = 20;

    const school = document.getElementById('schoolName').value || 'Lycée';
    const year   = document.getElementById('schoolYear').value || '';
    const gt     = grandTotal();
    const t1=getTotalByTrim('T1'), t2=getTotalByTrim('T2'), t3=getTotalByTrim('T3');

    // Header
    doc.setFillColor(10, 22, 40);
    doc.rect(0, 0, W, 38, 'F');
    doc.setTextColor(255, 255, 255);
    doc.setFontSize(16);
    doc.setFont('helvetica', 'bold');
    doc.text('Suivi des Violences en Milieu Scolaire', W/2, 14, { align: 'center' });
    doc.setFontSize(12);
    doc.setFont('helvetica', 'normal');
    doc.text(school, W/2, 22, { align: 'center' });
    doc.setFontSize(10);
    doc.text(`Année scolaire : ${year}  |  Généré le ${new Date().toLocaleDateString('fr-FR')}`, W/2, 30, { align: 'center' });
    doc.setTextColor(0, 0, 0);
    y = 46;

    // Section helper
    function sectionTitle(title, color=[26,58,107]) {
      checkPage();
      doc.setFillColor(...color);
      doc.rect(margin, y, W-margin*2, 7, 'F');
      doc.setTextColor(255,255,255);
      doc.setFont('helvetica','bold');
      doc.setFontSize(10);
      doc.text(title, margin+3, y+5);
      doc.setTextColor(0,0,0);
      doc.setFont('helvetica','normal');
      y += 11;
    }

    function checkPage(extra=0) {
      if (y + extra > H-20) { doc.addPage(); y = 15; }
    }

    function addText(text, size=10, bold=false, indent=0) {
      checkPage(8);
      doc.setFontSize(size);
      doc.setFont('helvetica', bold ? 'bold' : 'normal');
      const lines = doc.splitTextToSize(text, W-margin*2-indent);
      doc.text(lines, margin+indent, y);
      y += lines.length * (size * 0.45) + 2;
    }

    // Résumé global
    sectionTitle('RÉSUMÉ GLOBAL');
    addText(`Total d'incidents enregistrés : ${gt}`, 11, true);
    addText(`1er Trimestre : ${t1}  |  2e Trimestre : ${t2}  |  3e Trimestre : ${t3}`, 10);
    y += 4;

    // Tableau des incidents
    sectionTitle('TABLEAU DES INCIDENTS');
    const headers = ['Trim.','Niveau','Type de violence','Lieu','Acteurs','Nb'];
    const colW = [18,16,54,30,32,14];
    let x = margin;
    doc.setFillColor(10,22,40);
    doc.rect(margin, y, W-margin*2, 7, 'F');
    doc.setTextColor(255,255,255);
    doc.setFont('helvetica','bold');
    doc.setFontSize(8);
    headers.forEach((h,i) => { doc.text(h, x+2, y+5); x += colW[i]; });
    doc.setTextColor(0,0,0);
    doc.setFont('helvetica','normal');
    y += 7;

    incidents.forEach((inc, idx) => {
      checkPage(7);
      if (idx%2===0) { doc.setFillColor(245,248,252); doc.rect(margin, y, W-margin*2, 6, 'F'); }
      x = margin;
      doc.setFontSize(8);
      const row = [inc.trimestre, inc.niveau, inc.type, inc.lieu, inc.acteurs, ''+inc.nb];
      row.forEach((cell, i) => {
        const txt = doc.splitTextToSize(cell, colW[i]-2);
        doc.text(txt[0], x+2, y+4);
        x += colW[i];
      });
      y += 6;
    });
    y += 4;

    // Statistiques par niveau
    sectionTitle('STATISTIQUES PAR NIVEAU');
    const sHeaders = ['Niveau','T1','T2','T3','Total','%'];
    const sColW    = [24,20,20,20,24,20];
    x = margin;
    doc.setFillColor(10,22,40);
    doc.rect(margin, y, W-margin*2, 7, 'F');
    doc.setTextColor(255,255,255);
    doc.setFont('helvetica','bold');
    doc.setFontSize(8);
    sHeaders.forEach((h,i) => { doc.text(h, x+2, y+5); x += sColW[i]; });
    doc.setTextColor(0,0,0);
    y += 7;

    NIVEAUX.forEach((n,idx) => {
      checkPage(7);
      const nt1=incidents.filter(i=>i.trimestre==='T1'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
      const nt2=incidents.filter(i=>i.trimestre==='T2'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
      const nt3=incidents.filter(i=>i.trimestre==='T3'&&i.niveau===n).reduce((s,i)=>s+i.nb,0);
      const tot=nt1+nt2+nt3;
      const pct=gt?((tot/gt)*100).toFixed(1):'0.0';
      if (idx%2===0) { doc.setFillColor(245,248,252); doc.rect(margin, y, W-margin*2, 6, 'F'); }
      x = margin;
      doc.setFontSize(8);
      doc.setFont('helvetica','normal');
      [''+n, ''+nt1, ''+nt2, ''+nt3, ''+tot, pct+'%'].forEach((v,i) => { doc.text(v, x+2, y+4); x+=sColW[i]; });
      y += 6;
    });
    // Total row
    checkPage(7);
    doc.setFillColor(10,22,40);
    doc.rect(margin, y, W-margin*2, 7, 'F');
    doc.setTextColor(255,255,255);
    doc.setFont('helvetica','bold');
    doc.setFontSize(8);
    x = margin;
    ['TOTAL',''+t1,''+t2,''+t3,''+gt,'100%'].forEach((v,i) => { doc.text(v, x+2, y+5); x+=sColW[i]; });
    doc.setTextColor(0,0,0);
    y += 10;

    // Analyse automatique
    sectionTitle('ANALYSE AUTOMATIQUE');
    const t1v=t1,t2v=t2,t3v=t3;
    if (t2v<t1v&&t1v>0) addText(`• Baisse au 2e trimestre : ${t1v} → ${t2v} (−${t1v-t2v})`, 10);
    if (t2v>t1v&&t1v>0) addText(`• Hausse au 2e trimestre : ${t1v} → ${t2v} (+${t2v-t1v})`, 10);
    if (t3v>t2v&&t2v>0) addText(`• Hausse au 3e trimestre : ${t2v} → ${t3v} (+${t3v-t2v})`, 10);
    if (t3v<t2v&&t2v>0) addText(`• Baisse au 3e trimestre : ${t2v} → ${t3v} (−${t2v-t3v})`, 10);
    const topN = NIVEAUX.map(n=>({n,tot:getTotalByNiveau(n)})).sort((a,b)=>b.tot-a.tot)[0];
    if (topN.tot>0) addText(`• Niveau le plus touché : ${topN.n} (${topN.tot} incidents)`, 10);
    const topT = TYPES.map(t=>({t,tot:getTotalByType(t)})).sort((a,b)=>b.tot-a.tot)[0];
    if (topT.tot>0) addText(`• Type dominant : ${topT.t} (${topT.tot} cas)`, 10);
    y += 4;

    // Commentaire global
    if (comments.global) {
      sectionTitle('COMMENTAIRE GLOBAL');
      addText(comments.global, 10);
      y += 2;
    }

    // Commentaires par niveau
    const hasNivCom = NIVEAUX.some(n => comments.niveaux && comments.niveaux[n]);
    if (hasNivCom) {
      sectionTitle('COMMENTAIRES PAR NIVEAU');
      NIVEAUX.forEach(n => {
        if (comments.niveaux && comments.niveaux[n]) {
          checkPage(12);
          addText(n + ' :', 10, true);
          addText(comments.niveaux[n], 10, false, 4);
          y += 2;
        }
      });
    }

    // Informations trimestrielles
    const hasMeta = [1,2,3].some(q => { const m=meta['T'+q]; return m&&(m.msg||m.mesures); });
    if (hasMeta) {
      sectionTitle('INFORMATIONS TRIMESTRIELLES');
      [1,2,3].forEach(q => {
        const t='T'+q, m=meta[t];
        if (m&&(m.msg||m.mesures)) {
          checkPage(20);
          addText(`${q===1?'1er':q+'e'} Trimestre`, 10, true);
          if (m.msg)     { addText('Sensibilisation : '+m.msg, 9, false, 4); }
          if (m.mesures) { addText('Mesures prises : '+m.mesures, 9, false, 4); }
          y += 2;
        }
      });
    }

    // Conclusion
    if (comments.conclusion) {
      sectionTitle('CONCLUSION GÉNÉRALE', [26,107,58]);
      addText(comments.conclusion, 10);
    }

    // Footer on each page
    const pageCount = doc.getNumberOfPages();
    for (let i=1; i<=pageCount; i++) {
      doc.setPage(i);
      doc.setFontSize(8);
      doc.setTextColor(150);
      doc.text(`Page ${i} / ${pageCount}  –  ${school}  –  ${year}`, W/2, H-7, { align:'center' });
    }

    doc.save(`rapport-violences-${year||'lycee'}.pdf`);
    toast('✅ PDF généré avec succès !');
  } catch(e) {
    console.error(e);
    toast('❌ Erreur lors de la génération');
  } finally {
    document.getElementById('pdfLoading').classList.remove('show');
  }
}

// ══════════════════════════════════════════════
//  CSV EXPORT / IMPORT
// ══════════════════════════════════════════════
function exportCSV() {
  if (!incidents.length) { toast('⚠️ Aucune donnée'); return; }
  const rows = [['Trimestre','Niveau','Type','Lieu','Acteurs','Nombre']];
  incidents.forEach(i => rows.push([i.trimestre, i.niveau, i.type, i.lieu, i.acteurs, i.nb]));
  const csv = rows.map(r => r.join(';')).join('\n');
  const blob = new Blob(['\ufeff'+csv], { type:'text/csv;charset=utf-8' });
  const url  = URL.createObjectURL(blob);
  const a    = document.createElement('a');
  a.href = url; a.download = 'incidents-violences.csv'; a.click();
  URL.revokeObjectURL(url);
  toast('✅ CSV exporté');
}

function importCSV(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    try {
      const lines = e.target.result.split('\n').filter(Boolean);
      const imported = [];
      lines.slice(1).forEach(line => {
        const [trimestre,niveau,type,lieu,acteurs,nb] = line.split(';');
        if (trimestre && niveau) {
          imported.push({ id: Date.now().toString()+Math.random(), trimestre:trimestre.trim(), niveau:niveau.trim(), type:type.trim(), lieu:lieu.trim(), acteurs:acteurs.trim(), nb:parseInt(nb)||1 });
        }
      });
      incidents = [...incidents, ...imported];
      save();
      renderTable();
      toast(`✅ ${imported.length} ligne(s) importée(s)`);
    } catch(err) { toast('❌ Erreur d\'import'); }
  };
  reader.readAsText(file, 'utf-8');
  event.target.value = '';
}

// ══════════════════════════════════════════════
//  TOAST
// ══════════════════════════════════════════════
function toast(msg) {
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.classList.add('show');
  setTimeout(() => el.classList.remove('show'), 2500);
}

// ══════════════════════════════════════════════
//  AUTO-SAVE ON INPUT
// ══════════════════════════════════════════════
document.getElementById('schoolName').addEventListener('input', save);
document.getElementById('schoolYear').addEventListener('input', save);

// ══════════════════════════════════════════════
//  INIT
// ══════════════════════════════════════════════
load();
renderTable();
</script>
</body>
</html>
