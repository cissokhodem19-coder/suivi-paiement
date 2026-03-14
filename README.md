# suivi-paiement
Application mobile PWA de suivi des paiements pour usage personnel et organisationnel.
<!doctype html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="theme-color" content="#0a0a0a">
  <title>Suivi des Paiements</title>
  <link rel="manifest" href="manifest.json">
  <link rel="apple-touch-icon" href="icon-192.png">
  <meta name="apple-mobile-web-app-title" content="Paiements">
  <meta name="mobile-web-app-capable" content="yes">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0a0a0a;
      --surface: #141414;
      --surface2: #1e1e1e;
      --border: #2a2a2a;
      --accent: #e8ff47;
      --accent2: #47ffd4;
      --text: #f0f0f0;
      --text-dim: #888;
      --text-muted: #555;
      --danger: #ff4757;
      --success: #2ed573;
      --warning: #ffa502;
      --info: #70a1ff;
      --radius: 16px;
      --radius-sm: 10px;
      --bottom-nav: 72px;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

    html, body {
      height: 100%;
      overflow: hidden;
      background: var(--bg);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
    }

    #app {
      height: 100%;
      display: flex;
      flex-direction: column;
      max-width: 480px;
      margin: 0 auto;
      position: relative;
      background: var(--bg);
    }

    /* ── Status Bar ── */
    .status-bar {
      background: var(--surface);
      padding: 12px 20px 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 12px;
      color: var(--text-dim);
      border-bottom: 1px solid var(--border);
      flex-shrink: 0;
    }
    .status-bar .org-name {
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 13px;
      color: var(--accent);
      letter-spacing: 0.5px;
    }
    .status-badge {
      background: var(--accent);
      color: #0a0a0a;
      font-size: 10px;
      font-weight: 700;
      padding: 2px 8px;
      border-radius: 20px;
      font-family: 'Syne', sans-serif;
    }

    /* ── Scrollable Content ── */
    .page {
      display: none;
      flex: 1;
      overflow-y: auto;
      padding-bottom: calc(var(--bottom-nav) + 16px);
      -webkit-overflow-scrolling: touch;
    }
    .page.active { display: block; }

    /* ── Bottom Navigation ── */
    .bottom-nav {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      height: var(--bottom-nav);
      background: var(--surface);
      border-top: 1px solid var(--border);
      display: flex;
      align-items: stretch;
      z-index: 100;
    }
    .nav-item {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 4px;
      cursor: pointer;
      border: none;
      background: none;
      color: var(--text-muted);
      font-size: 10px;
      font-family: 'DM Sans', sans-serif;
      font-weight: 500;
      transition: color 0.2s;
      padding-bottom: 8px;
    }
    .nav-item.active { color: var(--accent); }
    .nav-item svg { width: 22px; height: 22px; stroke-width: 1.8; }
    .nav-item .nav-dot {
      width: 4px; height: 4px;
      border-radius: 50%;
      background: var(--accent);
      opacity: 0;
      transition: opacity 0.2s;
    }
    .nav-item.active .nav-dot { opacity: 1; }

    /* ── Dashboard Page ── */
    .dash-header {
      padding: 24px 20px 16px;
    }
    .dash-greeting {
      font-size: 13px;
      color: var(--text-dim);
      margin-bottom: 4px;
    }
    .dash-title {
      font-family: 'Syne', sans-serif;
      font-size: 26px;
      font-weight: 800;
      color: var(--text);
      line-height: 1.1;
    }
    .dash-title span { color: var(--accent); }

    /* Hero card */
    .hero-card {
      margin: 0 20px 20px;
      background: var(--accent);
      border-radius: var(--radius);
      padding: 24px;
      position: relative;
      overflow: hidden;
    }
    .hero-card::before {
      content: '';
      position: absolute;
      top: -30px; right: -30px;
      width: 120px; height: 120px;
      border-radius: 50%;
      background: rgba(0,0,0,0.08);
    }
    .hero-card::after {
      content: '';
      position: absolute;
      bottom: -20px; left: 40px;
      width: 80px; height: 80px;
      border-radius: 50%;
      background: rgba(0,0,0,0.06);
    }
    .hero-label {
      font-size: 12px;
      font-weight: 600;
      color: rgba(0,0,0,0.6);
      text-transform: uppercase;
      letter-spacing: 1px;
      margin-bottom: 8px;
    }
    .hero-amount {
      font-family: 'Syne', sans-serif;
      font-size: 38px;
      font-weight: 800;
      color: #0a0a0a;
      line-height: 1;
      margin-bottom: 16px;
    }
    .hero-meta {
      display: flex;
      gap: 16px;
    }
    .hero-meta-item {
      display: flex;
      flex-direction: column;
      gap: 2px;
    }
    .hero-meta-label { font-size: 11px; color: rgba(0,0,0,0.5); font-weight: 500; }
    .hero-meta-value { font-size: 16px; font-weight: 700; color: #0a0a0a; font-family: 'Syne', sans-serif; }

    /* Stat chips */
    .stat-row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
      margin: 0 20px 24px;
    }
    .stat-chip {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 16px;
    }
    .stat-chip-label { font-size: 11px; color: var(--text-muted); font-weight: 500; margin-bottom: 6px; text-transform: uppercase; letter-spacing: 0.5px; }
    .stat-chip-value { font-family: 'Syne', sans-serif; font-size: 22px; font-weight: 700; }
    .stat-chip-value.green { color: var(--success); }
    .stat-chip-value.red { color: var(--danger); }
    .stat-chip-value.blue { color: var(--info); }

    /* Recent section */
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 20px 12px;
    }
    .section-title {
      font-family: 'Syne', sans-serif;
      font-size: 16px;
      font-weight: 700;
    }
    .section-link {
      font-size: 12px;
      color: var(--accent);
      cursor: pointer;
      font-weight: 600;
    }

    /* ── Payment Card ── */
    .payment-card {
      margin: 0 20px 10px;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 16px;
      display: flex;
      align-items: center;
      gap: 14px;
      animation: slideUp 0.3s ease forwards;
    }
    @keyframes slideUp {
      from { opacity: 0; transform: translateY(12px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .payment-avatar {
      width: 44px;
      height: 44px;
      border-radius: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: 16px;
      flex-shrink: 0;
    }
    .payment-info { flex: 1; min-width: 0; }
    .payment-name {
      font-weight: 600;
      font-size: 14px;
      color: var(--text);
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    .payment-meta {
      font-size: 11px;
      color: var(--text-dim);
      margin-top: 3px;
    }
    .payment-right { text-align: right; flex-shrink: 0; }
    .payment-amount {
      font-family: 'Syne', sans-serif;
      font-size: 16px;
      font-weight: 700;
      color: var(--text);
    }
    .payment-date {
      font-size: 11px;
      color: var(--text-muted);
      margin-top: 3px;
    }

    /* Status badges */
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      padding: 3px 8px;
      border-radius: 20px;
      font-size: 10px;
      font-weight: 600;
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
    .badge.regle { background: rgba(46,213,115,0.15); color: var(--success); }
    .badge.retard { background: rgba(255,71,87,0.15); color: var(--danger); }
    .badge.virement { background: rgba(112,161,255,0.15); color: var(--info); }
    .badge.carte { background: rgba(200,100,255,0.15); color: #c864ff; }
    .badge.especes { background: rgba(255,165,2,0.15); color: var(--warning); }

    /* ── Add Page ── */
    .form-header {
      padding: 24px 20px 20px;
    }
    .form-title {
      font-family: 'Syne', sans-serif;
      font-size: 24px;
      font-weight: 800;
    }
    .form-subtitle { font-size: 13px; color: var(--text-dim); margin-top: 4px; }

    .form-body { padding: 0 20px; }

    .form-group { margin-bottom: 16px; }
    .form-label {
      display: block;
      font-size: 12px;
      font-weight: 600;
      color: var(--text-dim);
      text-transform: uppercase;
      letter-spacing: 0.8px;
      margin-bottom: 8px;
    }
    .form-input, .form-select {
      width: 100%;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 14px 16px;
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-size: 15px;
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
      appearance: none;
      -webkit-appearance: none;
    }
    .form-input:focus, .form-select:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(232,255,71,0.1);
    }
    .form-input::placeholder { color: var(--text-muted); }
    .form-select option { background: #1e1e1e; }

    .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }

    .btn-primary {
      width: 100%;
      background: var(--accent);
      color: #0a0a0a;
      border: none;
      border-radius: var(--radius-sm);
      padding: 16px;
      font-family: 'Syne', sans-serif;
      font-size: 15px;
      font-weight: 700;
      cursor: pointer;
      margin-top: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: transform 0.15s, opacity 0.15s;
    }
    .btn-primary:active { transform: scale(0.98); opacity: 0.9; }
    .btn-primary:disabled { opacity: 0.5; }

    .success-toast {
      position: fixed;
      top: 20px;
      left: 50%;
      transform: translateX(-50%) translateY(-80px);
      background: var(--success);
      color: #0a0a0a;
      padding: 12px 24px;
      border-radius: 40px;
      font-weight: 700;
      font-size: 14px;
      font-family: 'Syne', sans-serif;
      z-index: 999;
      transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1);
      white-space: nowrap;
    }
    .success-toast.show { transform: translateX(-50%) translateY(0); }

    /* ── History Page ── */
    .history-header {
      padding: 24px 20px 16px;
    }
    .history-title {
      font-family: 'Syne', sans-serif;
      font-size: 24px;
      font-weight: 800;
    }

    .filter-row {
      display: flex;
      gap: 8px;
      padding: 0 20px 16px;
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      scrollbar-width: none;
    }
    .filter-row::-webkit-scrollbar { display: none; }
    .filter-chip {
      flex-shrink: 0;
      padding: 8px 16px;
      border-radius: 40px;
      border: 1px solid var(--border);
      background: var(--surface);
      color: var(--text-dim);
      font-size: 12px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
    }
    .filter-chip.active {
      background: var(--accent);
      color: #0a0a0a;
      border-color: var(--accent);
    }

    .search-bar {
      margin: 0 20px 16px;
      position: relative;
    }
    .search-bar svg {
      position: absolute;
      left: 14px;
      top: 50%;
      transform: translateY(-50%);
      width: 16px;
      height: 16px;
      stroke: var(--text-muted);
    }
    .search-bar input {
      width: 100%;
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 12px 16px 12px 42px;
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-size: 14px;
      outline: none;
    }
    .search-bar input:focus { border-color: var(--accent); }
    .search-bar input::placeholder { color: var(--text-muted); }

    /* Delete swipe */
    .payment-card-wrap {
      position: relative;
      margin: 0 20px 10px;
    }
    .payment-card-wrap .payment-card { margin: 0; }

    .delete-bg {
      position: absolute;
      right: 0; top: 0; bottom: 0;
      width: 80px;
      background: var(--danger);
      border-radius: var(--radius);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 0;
    }
    .delete-bg svg { width: 22px; height: 22px; stroke: white; }

    .delete-confirm-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.8);
      z-index: 500;
      display: flex;
      align-items: flex-end;
      justify-content: center;
      animation: fadeIn 0.2s ease;
    }
    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    .delete-sheet {
      background: var(--surface2);
      border-radius: 24px 24px 0 0;
      padding: 24px 24px 40px;
      width: 100%;
      max-width: 480px;
      animation: slideSheet 0.3s cubic-bezier(0.34,1.56,0.64,1);
    }
    @keyframes slideSheet {
      from { transform: translateY(100%); }
      to { transform: translateY(0); }
    }
    .delete-sheet h3 { font-family: 'Syne', sans-serif; font-size: 20px; font-weight: 700; margin-bottom: 8px; }
    .delete-sheet p { color: var(--text-dim); font-size: 14px; margin-bottom: 24px; }
    .sheet-btns { display: flex; gap: 12px; }
    .btn-danger {
      flex: 1;
      background: var(--danger);
      color: white;
      border: none;
      border-radius: var(--radius-sm);
      padding: 14px;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 14px;
      cursor: pointer;
    }
    .btn-cancel {
      flex: 1;
      background: var(--border);
      color: var(--text);
      border: none;
      border-radius: var(--radius-sm);
      padding: 14px;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 14px;
      cursor: pointer;
    }

    /* Empty state */
    .empty-state {
      text-align: center;
      padding: 60px 40px;
      color: var(--text-muted);
    }
    .empty-icon {
      width: 64px; height: 64px;
      border-radius: 20px;
      background: var(--surface);
      border: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 0 auto 16px;
    }
    .empty-icon svg { width: 28px; height: 28px; stroke: var(--text-muted); }
    .empty-state h3 { font-family: 'Syne', sans-serif; font-size: 16px; font-weight: 700; color: var(--text-dim); margin-bottom: 6px; }
    .empty-state p { font-size: 13px; line-height: 1.5; }

    /* ── Settings Page ── */
    .settings-header {
      padding: 24px 20px 20px;
    }
    .settings-title {
      font-family: 'Syne', sans-serif;
      font-size: 24px;
      font-weight: 800;
    }
    .settings-section { padding: 0 20px 8px; }
    .settings-section-label {
      font-size: 11px;
      font-weight: 700;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 1px;
      padding: 0 4px 10px;
    }
    .settings-item {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 8px;
    }
    .settings-item-left { display: flex; align-items: center; gap: 12px; }
    .settings-icon {
      width: 36px; height: 36px;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .settings-icon svg { width: 18px; height: 18px; }
    .settings-item-label { font-size: 14px; font-weight: 500; }
    .settings-item-sub { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
    .settings-value { font-size: 13px; color: var(--text-dim); }

    .org-input-wrap { margin: 0 20px 20px; }
    .btn-save-settings {
      background: var(--accent);
      color: #0a0a0a;
      border: none;
      border-radius: var(--radius-sm);
      padding: 14px 24px;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 14px;
      cursor: pointer;
      width: 100%;
      margin-top: 8px;
    }

    /* Divider bar */
    .handle-bar {
      width: 36px;
      height: 4px;
      background: var(--border);
      border-radius: 4px;
      margin: 0 auto 20px;
    }

    /* Scroll hint */
    ::-webkit-scrollbar { width: 0; background: transparent; }
  </style>
</head>
<body>
<div id="app">

  <!-- Status Bar -->
  <div class="status-bar">
    <span class="org-name" id="org-display">Mon Organisation</span>
    <span class="status-badge">PRO</span>
  </div>

  <!-- ── DASHBOARD PAGE ── -->
  <div class="page active" id="page-dashboard">
    <div class="dash-header">
      <div class="dash-greeting">Bonjour 👋</div>
      <div class="dash-title">Suivi des<br><span>Paiements</span></div>
    </div>

    <div class="hero-card">
      <div class="hero-label">Montant Total</div>
      <div class="hero-amount" id="hero-total">0,00 €</div>
      <div class="hero-meta">
        <div class="hero-meta-item">
          <span class="hero-meta-label">Enregistrements</span>
          <span class="hero-meta-value" id="hero-count">0</span>
        </div>
        <div class="hero-meta-item">
          <span class="hero-meta-label">Moyenne</span>
          <span class="hero-meta-value" id="hero-avg">0 €</span>
        </div>
      </div>
    </div>

    <div class="stat-row">
      <div class="stat-chip">
        <div class="stat-chip-label">Réglés</div>
        <div class="stat-chip-value green" id="stat-regle">0</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">En retard</div>
        <div class="stat-chip-value red" id="stat-retard">0</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Virements</div>
        <div class="stat-chip-value blue" id="stat-virement">0</div>
      </div>
      <div class="stat-chip">
        <div class="stat-chip-label">Ce mois</div>
        <div class="stat-chip-value" id="stat-mois" style="color:var(--accent)">0</div>
      </div>
    </div>

    <div class="section-header">
      <span class="section-title">Récents</span>
      <span class="section-link" onclick="switchPage('historique')">Voir tout →</span>
    </div>

    <div id="recent-list">
      <div class="empty-state">
        <div class="empty-icon"><svg viewBox="0 0 24 24" fill="none"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
        <h3>Aucun paiement</h3>
        <p>Ajoutez votre premier paiement<br>depuis l'onglet +</p>
      </div>
    </div>
  </div>

  <!-- ── ADD PAGE ── -->
  <div class="page" id="page-ajouter">
    <div class="form-header">
      <div class="form-title">Nouveau Paiement</div>
      <div class="form-subtitle">Remplissez les informations ci-dessous</div>
    </div>

    <div class="form-body">
      <div class="form-row">
        <div class="form-group">
          <label class="form-label">Nom</label>
          <input type="text" id="f-nom" class="form-input" placeholder="Dupont" autocomplete="off">
        </div>
        <div class="form-group">
          <label class="form-label">Prénom</label>
          <input type="text" id="f-prenom" class="form-input" placeholder="Jean" autocomplete="off">
        </div>
      </div>

      <div class="form-group">
        <label class="form-label">Type de paiement</label>
        <select id="f-type" class="form-select">
          <option value="">Sélectionner...</option>
          <option value="Virement">💳 Virement</option>
          <option value="Carte">🃏 Carte</option>
          <option value="Espèces">💵 Espèces</option>
        </select>
      </div>

      <div class="form-group">
        <label class="form-label">Statut</label>
        <select id="f-statut" class="form-select">
          <option value="">Sélectionner...</option>
          <option value="Réglé">✅ Réglé</option>
          <option value="En retard">⚠️ En retard</option>
        </select>
      </div>

      <div class="form-group">
        <label class="form-label">Montant (€)</label>
        <input type="number" id="f-montant" class="form-input" placeholder="50.00" min="0" step="0.01">
      </div>

      <div class="form-group">
        <label class="form-label">Date et heure</label>
        <input type="datetime-local" id="f-date" class="form-input">
      </div>

      <button class="btn-primary" onclick="addPayment()">
        <svg viewBox="0 0 24 24" fill="none" width="18" height="18"><path d="M12 5v14M5 12h14" stroke="currentColor" stroke-width="2.5" stroke-linecap="round"/></svg>
        Enregistrer le paiement
      </button>
    </div>
  </div>

  <!-- ── HISTORY PAGE ── -->
  <div class="page" id="page-historique">
    <div class="history-header">
      <div class="history-title">Historique</div>
    </div>

    <div class="search-bar">
      <svg viewBox="0 0 24 24" fill="none"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35" stroke-linecap="round"/></svg>
      <input type="text" id="search-input" placeholder="Rechercher un nom..." oninput="renderHistory()">
    </div>

    <div class="filter-row">
      <div class="filter-chip active" data-filter="all" onclick="setFilter('all', this)">Tous</div>
      <div class="filter-chip" data-filter="Réglé" onclick="setFilter('Réglé', this)">Réglés</div>
      <div class="filter-chip" data-filter="En retard" onclick="setFilter('En retard', this)">En retard</div>
      <div class="filter-chip" data-filter="Virement" onclick="setFilter('Virement', this)">Virements</div>
      <div class="filter-chip" data-filter="Carte" onclick="setFilter('Carte', this)">Carte</div>
      <div class="filter-chip" data-filter="Espèces" onclick="setFilter('Espèces', this)">Espèces</div>
    </div>

    <div id="history-list"></div>
  </div>

  <!-- ── SETTINGS PAGE ── -->
  <div class="page" id="page-parametres">
    <div class="settings-header">
      <div class="settings-title">Paramètres</div>
    </div>

    <div class="settings-section">
      <div class="settings-section-label">Organisation</div>
      <div class="org-input-wrap">
        <label class="form-label">Nom de l'organisation</label>
        <input type="text" id="s-orgname" class="form-input" placeholder="Mon Organisation">
        <button class="btn-save-settings" onclick="saveSettings()">Sauvegarder</button>
      </div>
    </div>

    <div class="settings-section">
      <div class="settings-section-label">Données</div>
      <div class="settings-item">
        <div class="settings-item-left">
          <div class="settings-icon" style="background:rgba(46,213,115,0.15)">
            <svg viewBox="0 0 24 24" fill="none" style="stroke:var(--success)"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </div>
          <div>
            <div class="settings-item-label">Total paiements</div>
            <div class="settings-item-sub">Stockage local</div>
          </div>
        </div>
        <span class="settings-value" id="s-count">0 entrées</span>
      </div>
      <div class="settings-item">
        <div class="settings-item-left">
          <div class="settings-icon" style="background:rgba(112,161,255,0.15)">
            <svg viewBox="0 0 24 24" fill="none" style="stroke:var(--info)"><path d="M12 2v20M17 5H9.5a3.5 3.5 0 000 7h5a3.5 3.5 0 010 7H6" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </div>
          <div>
            <div class="settings-item-label">Montant cumulé</div>
            <div class="settings-item-sub">Tous les paiements</div>
          </div>
        </div>
        <span class="settings-value" id="s-total">0 €</span>
      </div>
    </div>

    <div class="settings-section">
      <div class="settings-section-label">Actions</div>
      <div class="settings-item" style="cursor:pointer" onclick="exportCSV()">
        <div class="settings-item-left">
          <div class="settings-icon" style="background:rgba(232,255,71,0.15)">
            <svg viewBox="0 0 24 24" fill="none" style="stroke:var(--accent)"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M7 10l5 5 5-5M12 15V3" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </div>
          <div>
            <div class="settings-item-label">Exporter en CSV</div>
            <div class="settings-item-sub">Télécharger les données</div>
          </div>
        </div>
        <svg viewBox="0 0 24 24" fill="none" width="16" style="stroke:var(--text-muted)"><path d="M9 18l6-6-6-6" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <div class="settings-item" style="cursor:pointer" onclick="confirmClear()">
        <div class="settings-item-left">
          <div class="settings-icon" style="background:rgba(255,71,87,0.15)">
            <svg viewBox="0 0 24 24" fill="none" style="stroke:var(--danger)"><path d="M3 6h18M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a1 1 0 011-1h4a1 1 0 011 1v2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          </div>
          <div>
            <div class="settings-item-label" style="color:var(--danger)">Effacer toutes les données</div>
            <div class="settings-item-sub">Action irréversible</div>
          </div>
        </div>
        <svg viewBox="0 0 24 24" fill="none" width="16" style="stroke:var(--text-muted)"><path d="M9 18l6-6-6-6" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
    </div>

    <div style="text-align:center;padding:30px 20px;color:var(--text-muted);font-size:12px;">
      💾 Données sauvegardées localement sur votre appareil
    </div>
  </div>

  <!-- ── BOTTOM NAV ── -->
  <nav class="bottom-nav">
    <button class="nav-item active" onclick="switchPage('dashboard')" id="nav-dashboard">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/></svg>
      Tableau
      <div class="nav-dot"></div>
    </button>
    <button class="nav-item" onclick="switchPage('ajouter')" id="nav-ajouter">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><circle cx="12" cy="12" r="9"/><path d="M12 8v8M8 12h8" stroke-linecap="round"/></svg>
      Ajouter
      <div class="nav-dot"></div>
    </button>
    <button class="nav-item" onclick="switchPage('historique')" id="nav-historique">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2M9 12h6M9 16h4" stroke-linecap="round" stroke-linejoin="round"/></svg>
      Historique
      <div class="nav-dot"></div>
    </button>
    <button class="nav-item" onclick="switchPage('parametres')" id="nav-parametres">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor"><circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 00.33 1.82l.06.06a2 2 0 010 2.83 2 2 0 01-2.83 0l-.06-.06a1.65 1.65 0 00-1.82-.33 1.65 1.65 0 00-1 1.51V21a2 2 0 01-4 0v-.09A1.65 1.65 0 009 19.4a1.65 1.65 0 00-1.82.33l-.06.06a2 2 0 01-2.83-2.83l.06-.06A1.65 1.65 0 004.68 15a1.65 1.65 0 00-1.51-1H3a2 2 0 010-4h.09A1.65 1.65 0 004.6 9a1.65 1.65 0 00-.33-1.82l-.06-.06a2 2 0 012.83-2.83l.06.06A1.65 1.65 0 009 4.68a1.65 1.65 0 001-1.51V3a2 2 0 014 0v.09a1.65 1.65 0 001 1.51 1.65 1.65 0 001.82-.33l.06-.06a2 2 0 012.83 2.83l-.06.06A1.65 1.65 0 0019.4 9a1.65 1.65 0 001.51 1H21a2 2 0 010 4h-.09a1.65 1.65 0 00-1.51 1z" stroke-linecap="round" stroke-linejoin="round"/></svg>
      Paramètres
      <div class="nav-dot"></div>
    </button>
  </nav>
</div>

<!-- Toast -->
<div class="success-toast" id="toast">✓ Paiement enregistré !</div>

<!-- Delete Confirm Sheet -->
<div class="delete-confirm-overlay" id="delete-overlay" style="display:none">
  <div class="delete-sheet">
    <div class="handle-bar"></div>
    <h3>Supprimer ce paiement ?</h3>
    <p id="delete-desc">Cette action est irréversible.</p>
    <div class="sheet-btns">
      <button class="btn-cancel" onclick="closeDeleteOverlay()">Annuler</button>
      <button class="btn-danger" onclick="executeDelete()">Supprimer</button>
    </div>
  </div>
</div>

<!-- Clear All Sheet -->
<div class="delete-confirm-overlay" id="clear-overlay" style="display:none">
  <div class="delete-sheet">
    <div class="handle-bar"></div>
    <h3>Effacer toutes les données ?</h3>
    <p>Tous les paiements enregistrés seront supprimés définitivement.</p>
    <div class="sheet-btns">
      <button class="btn-cancel" onclick="document.getElementById('clear-overlay').style.display='none'">Annuler</button>
      <button class="btn-danger" onclick="clearAll()">Tout effacer</button>
    </div>
  </div>
</div>

<script>
// ── State ──
let payments = [];
let currentFilter = 'all';
let pendingDeleteId = null;
const STORAGE_KEY = 'suivi_paiements_v1';
const ORG_KEY = 'suivi_org_v1';

// ── Init ──
function init() {
  const saved = localStorage.getItem(STORAGE_KEY);
  if (saved) payments = JSON.parse(saved);
  const org = localStorage.getItem(ORG_KEY) || 'Mon Organisation';
  document.getElementById('org-display').textContent = org;
  document.getElementById('s-orgname').value = org;
  const now = new Date();
  document.getElementById('f-date').value = now.toISOString().slice(0, 16);
  render();
}

function save() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(payments));
}

// ── Navigation ──
function switchPage(name) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  document.getElementById('nav-' + name).classList.add('active');
  if (name === 'historique') renderHistory();
  if (name === 'parametres') updateSettings();
}

// ── Add Payment ──
function addPayment() {
  const nom = document.getElementById('f-nom').value.trim();
  const prenom = document.getElementById('f-prenom').value.trim();
  const type = document.getElementById('f-type').value;
  const statut = document.getElementById('f-statut').value;
  const montant = parseFloat(document.getElementById('f-montant').value);
  const date = document.getElementById('f-date').value;

  if (!nom || !prenom || !type || !statut || isNaN(montant) || !date) {
    shakeForm(); return;
  }

  payments.unshift({
    id: Date.now().toString(),
    nom, prenom,
    type_paiement: type,
    statut,
    montant,
    date_paiement: new Date(date).toISOString()
  });

  save();
  render();
  resetForm();
  showToast('✓ Paiement enregistré !');
  setTimeout(() => switchPage('dashboard'), 600);
}

function shakeForm() {
  const btn = document.querySelector('.btn-primary');
  btn.style.animation = 'none';
  btn.style.background = 'var(--danger)';
  setTimeout(() => { btn.style.background = 'var(--accent)'; }, 500);
}

function resetForm() {
  ['f-nom','f-prenom'].forEach(id => document.getElementById(id).value = '');
  ['f-type','f-statut'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('f-montant').value = '';
  document.getElementById('f-date').value = new Date().toISOString().slice(0,16);
}

// ── Render ──
function render() {
  renderDashboard();
  renderHistory();
}

function renderDashboard() {
  const total = payments.reduce((s, p) => s + p.montant, 0);
  const avg = payments.length ? total / payments.length : 0;
  const now = new Date();
  const thisMonth = payments.filter(p => {
    const d = new Date(p.date_paiement);
    return d.getMonth() === now.getMonth() && d.getFullYear() === now.getFullYear();
  }).length;

  document.getElementById('hero-total').textContent = formatMoney(total);
  document.getElementById('hero-count').textContent = payments.length;
  document.getElementById('hero-avg').textContent = formatMoney(avg);
  document.getElementById('stat-regle').textContent = payments.filter(p => p.statut === 'Réglé').length;
  document.getElementById('stat-retard').textContent = payments.filter(p => p.statut === 'En retard').length;
  document.getElementById('stat-virement').textContent = payments.filter(p => p.type_paiement === 'Virement').length;
  document.getElementById('stat-mois').textContent = thisMonth;

  const recent = payments.slice(0, 5);
  const container = document.getElementById('recent-list');
  if (!recent.length) {
    container.innerHTML = `<div class="empty-state">
      <div class="empty-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" stroke-linecap="round" stroke-linejoin="round"/></svg></div>
      <h3>Aucun paiement</h3><p>Ajoutez votre premier paiement<br>depuis l'onglet +</p></div>`;
    return;
  }
  container.innerHTML = recent.map(p => paymentCardHTML(p, false)).join('');
}

function renderHistory() {
  const query = (document.getElementById('search-input')?.value || '').toLowerCase();
  let filtered = payments.filter(p => {
    const matchSearch = !query || p.nom.toLowerCase().includes(query) || p.prenom.toLowerCase().includes(query);
    const matchFilter = currentFilter === 'all' || p.statut === currentFilter || p.type_paiement === currentFilter;
    return matchSearch && matchFilter;
  });

  const container = document.getElementById('history-list');
  if (!container) return;
  if (!filtered.length) {
    container.innerHTML = `<div class="empty-state">
      <div class="empty-icon"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.35-4.35" stroke-linecap="round"/></svg></div>
      <h3>Aucun résultat</h3><p>Aucun paiement ne correspond<br>à votre recherche</p></div>`;
    return;
  }
  container.innerHTML = filtered.map(p => paymentCardHTML(p, true)).join('');
}

function paymentCardHTML(p, withDelete) {
  const d = new Date(p.date_paiement);
  const dateStr = d.toLocaleDateString('fr-FR');
  const timeStr = d.toLocaleTimeString('fr-FR', { hour: '2-digit', minute: '2-digit' });
  const initials = (p.nom[0] || '') + (p.prenom[0] || '');
  const avatarColor = p.statut === 'Réglé' ? 'rgba(46,213,115,0.15);color:var(--success)' : 'rgba(255,71,87,0.15);color:var(--danger)';
  const typeBadge = getTypeBadge(p.type_paiement);
  const statutBadge = getStatutBadge(p.statut);

  const deleteBtn = withDelete ? `<button onclick="askDelete('${p.id}')" style="position:absolute;top:12px;right:12px;background:none;border:none;cursor:pointer;padding:4px;">
    <svg viewBox="0 0 24 24" fill="none" width="18" height="18" stroke="var(--text-muted)" stroke-width="1.8"><path d="M3 6h18M19 6v14a2 2 0 01-2 2H7a2 2 0 01-2-2V6m3 0V4a1 1 0 011-1h4a1 1 0 011 1v2" stroke-linecap="round" stroke-linejoin="round"/></svg>
  </button>` : '';

  return `<div class="payment-card" style="position:relative;${withDelete ? 'padding-right:44px;' : ''}">
    <div class="payment-avatar" style="background:${avatarColor}">${initials.toUpperCase()}</div>
    <div class="payment-info">
      <div class="payment-name">${esc(p.nom)} ${esc(p.prenom)}</div>
      <div class="payment-meta" style="display:flex;gap:6px;margin-top:6px;flex-wrap:wrap;">
        ${typeBadge} ${statutBadge}
      </div>
    </div>
    <div class="payment-right">
      <div class="payment-amount">${formatMoney(p.montant)}</div>
      <div class="payment-date">${dateStr} ${timeStr}</div>
    </div>
    ${deleteBtn}
  </div>`;
}

function getTypeBadge(type) {
  const map = { 'Virement': ['virement','↔'], 'Carte': ['carte','🃏'], 'Espèces': ['especes','💵'] };
  const [cls, icon] = map[type] || ['virement','↔'];
  return `<span class="badge ${cls}">${icon} ${type}</span>`;
}
function getStatutBadge(s) {
  return s === 'Réglé'
    ? `<span class="badge regle">✓ Réglé</span>`
    : `<span class="badge retard">⚠ En retard</span>`;
}

// ── Filter ──
function setFilter(filter, el) {
  currentFilter = filter;
  document.querySelectorAll('.filter-chip').forEach(c => c.classList.remove('active'));
  el.classList.add('active');
  renderHistory();
}

// ── Delete ──
function askDelete(id) {
  const p = payments.find(x => x.id === id);
  if (!p) return;
  pendingDeleteId = id;
  document.getElementById('delete-desc').textContent = `Supprimer le paiement de ${p.nom} ${p.prenom} (${formatMoney(p.montant)}) ?`;
  document.getElementById('delete-overlay').style.display = 'flex';
}
function closeDeleteOverlay() {
  document.getElementById('delete-overlay').style.display = 'none';
  pendingDeleteId = null;
}
function executeDelete() {
  if (!pendingDeleteId) return;
  payments = payments.filter(p => p.id !== pendingDeleteId);
  save(); render();
  closeDeleteOverlay();
  showToast('🗑 Paiement supprimé');
}

// ── Clear All ──
function confirmClear() {
  document.getElementById('clear-overlay').style.display = 'flex';
}
function clearAll() {
  payments = [];
  save(); render();
  document.getElementById('clear-overlay').style.display = 'none';
  updateSettings();
  showToast('🗑 Données effacées');
}

// ── Settings ──
function saveSettings() {
  const org = document.getElementById('s-orgname').value.trim() || 'Mon Organisation';
  localStorage.setItem(ORG_KEY, org);
  document.getElementById('org-display').textContent = org;
  showToast('✓ Paramètres sauvegardés');
}
function updateSettings() {
  const total = payments.reduce((s, p) => s + p.montant, 0);
  document.getElementById('s-count').textContent = `${payments.length} entrée${payments.length > 1 ? 's' : ''}`;
  document.getElementById('s-total').textContent = formatMoney(total);
}

// ── Export CSV ──
function exportCSV() {
  if (!payments.length) { showToast('Aucune donnée à exporter'); return; }
  const header = ['Nom','Prénom','Type','Statut','Montant','Date','Heure','Année'];
  const rows = payments.map(p => {
    const d = new Date(p.date_paiement);
    return [
      p.nom, p.prenom, p.type_paiement, p.statut,
      p.montant.toFixed(2),
      d.toLocaleDateString('fr-FR'),
      d.toLocaleTimeString('fr-FR',{hour:'2-digit',minute:'2-digit'}),
      d.getFullYear()
    ].join(';');
  });
  const csv = [header.join(';'), ...rows].join('\n');
  const blob = new Blob(['\uFEFF' + csv], { type: 'text/csv;charset=utf-8;' });
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'paiements.csv'; a.click();
  URL.revokeObjectURL(url);
  showToast('✓ Export CSV téléchargé');
}

// ── Helpers ──
function formatMoney(v) {
  return v.toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 }) + ' €';
}
function esc(t) {
  const d = document.createElement('div');
  d.textContent = t;
  return d.innerHTML;
}

let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2500);
}

init();

// ── Register Service Worker (PWA) ──
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js')
      .then(() => console.log('SW enregistré'))
      .catch(err => console.log('SW erreur:', err));
  });
}
</script>
</body>
</html>
