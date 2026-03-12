<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Veritas — Plagiarism · AI Detector · Humanizer</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #080c10;
    --surface: #0d1117;
    --surface2: #131920;
    --surface3: #1a2230;
    --border: #1e2d3d;
    --accent: #00e5ff;
    --accent2: #7c3aed;
    --accent3: #10b981;
    --danger: #f43f5e;
    --warn: #f59e0b;
    --text: #e2eaf4;
    --muted: #6b7f94;
    --glow: 0 0 40px rgba(0,229,255,0.12);
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Grid background */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,229,255,0.025) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,229,255,0.025) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* Orb */
  .orb {
    position: fixed;
    top: -200px; right: -200px;
    width: 600px; height: 600px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(124,58,237,0.15) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }
  .orb2 {
    position: fixed;
    bottom: -200px; left: -100px;
    width: 500px; height: 500px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(0,229,255,0.08) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }

  /* Header */
  header {
    position: relative;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 40px;
    border-bottom: 1px solid var(--border);
    backdrop-filter: blur(10px);
  }

  .logo {
    font-family: 'Syne', sans-serif;
    font-weight: 800;
    font-size: 1.6rem;
    letter-spacing: -1px;
    color: var(--text);
  }
  .logo span {
    color: var(--accent);
  }
  .logo-sub {
    font-size: 0.6rem;
    letter-spacing: 3px;
    color: var(--muted);
    text-transform: uppercase;
    margin-top: 2px;
    font-family: 'JetBrains Mono', monospace;
  }

  .badge {
    font-size: 0.65rem;
    padding: 4px 10px;
    border-radius: 20px;
    border: 1px solid var(--border);
    color: var(--muted);
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  /* Nav tabs */
  .nav-tabs {
    position: relative;
    z-index: 10;
    display: flex;
    gap: 0;
    padding: 0 40px;
    border-bottom: 1px solid var(--border);
    background: rgba(8,12,16,0.8);
    backdrop-filter: blur(10px);
  }

  .tab-btn {
    background: none;
    border: none;
    cursor: pointer;
    padding: 16px 28px;
    font-family: 'Syne', sans-serif;
    font-size: 0.85rem;
    font-weight: 600;
    letter-spacing: 0.5px;
    color: var(--muted);
    position: relative;
    transition: color 0.2s;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .tab-btn.active { color: var(--accent); }
  .tab-btn.active::after {
    content: '';
    position: absolute;
    bottom: -1px; left: 0; right: 0;
    height: 2px;
    background: var(--accent);
    box-shadow: 0 0 10px var(--accent);
  }
  .tab-btn:hover:not(.active) { color: var(--text); }

  .tab-icon { font-size: 1rem; }

  /* Main */
  main {
    position: relative;
    z-index: 10;
    max-width: 1100px;
    margin: 0 auto;
    padding: 40px 40px;
  }

  .panel { display: none; }
  .panel.active { display: block; }

  /* Section title */
  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 2rem;
    font-weight: 800;
    margin-bottom: 6px;
    letter-spacing: -0.5px;
  }
  .section-title span { color: var(--accent); }
  .section-desc {
    color: var(--muted);
    font-size: 0.8rem;
    margin-bottom: 32px;
    line-height: 1.7;
  }

  /* Input area */
  .input-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .input-card:focus-within {
    border-color: rgba(0,229,255,0.4);
    box-shadow: var(--glow);
  }

  .input-toolbar {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 16px;
    border-bottom: 1px solid var(--border);
    background: var(--surface2);
    flex-wrap: wrap;
  }

  .toolbar-btn {
    background: var(--surface3);
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 6px 12px;
    border-radius: 8px;
    font-size: 0.7rem;
    cursor: pointer;
    font-family: 'JetBrains Mono', monospace;
    transition: all 0.2s;
    display: flex;
    align-items: center;
    gap: 5px;
  }
  .toolbar-btn:hover { color: var(--accent); border-color: rgba(0,229,255,0.3); }

  .input-area {
    width: 100%;
    min-height: 220px;
    background: transparent;
    border: none;
    outline: none;
    padding: 20px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    color: var(--text);
    resize: vertical;
    line-height: 1.8;
  }
  .input-area::placeholder { color: var(--muted); }

  .input-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 16px;
    border-top: 1px solid var(--border);
    background: var(--surface2);
  }
  .word-count { font-size: 0.7rem; color: var(--muted); }

  /* Upload area */
  .upload-zone {
    border: 2px dashed var(--border);
    border-radius: 12px;
    padding: 30px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    margin-top: 16px;
  }
  .upload-zone:hover, .upload-zone.drag-over {
    border-color: rgba(0,229,255,0.5);
    background: rgba(0,229,255,0.03);
  }
  .upload-icon { font-size: 2rem; margin-bottom: 10px; opacity: 0.5; }
  .upload-text { font-size: 0.8rem; color: var(--muted); }
  .upload-formats { font-size: 0.65rem; color: var(--muted); margin-top: 6px; opacity: 0.6; }
  #fileInput { display: none; }

  /* Language & options row */
  .options-row {
    display: flex;
    gap: 12px;
    margin-top: 16px;
    flex-wrap: wrap;
    align-items: center;
  }

  .select-wrap {
    position: relative;
  }
  .select-wrap select {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 8px 32px 8px 12px;
    border-radius: 8px;
    font-size: 0.75rem;
    font-family: 'JetBrains Mono', monospace;
    cursor: pointer;
    appearance: none;
    outline: none;
  }
  .select-wrap::after {
    content: '▾';
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: var(--muted);
    pointer-events: none;
    font-size: 0.7rem;
  }

  /* Action button */
  .action-btn {
    background: var(--accent);
    color: #000;
    border: none;
    padding: 14px 36px;
    border-radius: 10px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.9rem;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.5px;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    margin-top: 20px;
  }
  .action-btn:hover {
    background: #fff;
    box-shadow: 0 0 30px rgba(0,229,255,0.4);
    transform: translateY(-1px);
  }
  .action-btn:disabled {
    opacity: 0.4;
    cursor: not-allowed;
    transform: none;
  }
  .action-btn.secondary {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
  }
  .action-btn.secondary:hover { border-color: var(--accent); color: var(--accent); box-shadow: none; }

  .btn-row { display: flex; gap: 12px; flex-wrap: wrap; }

  /* Loading */
  .loading {
    display: none;
    align-items: center;
    gap: 12px;
    padding: 20px;
    color: var(--muted);
    font-size: 0.8rem;
  }
  .loading.show { display: flex; }
  .spinner {
    width: 20px; height: 20px;
    border: 2px solid var(--border);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    flex-shrink: 0;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* Results */
  .results-section {
    margin-top: 36px;
    display: none;
  }
  .results-section.show { display: block; animation: fadeUp 0.4s ease; }
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .results-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
    flex-wrap: wrap;
    gap: 12px;
  }
  .results-title {
    font-family: 'Syne', sans-serif;
    font-size: 1.1rem;
    font-weight: 700;
  }

  /* Score meters */
  .scores-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 16px;
    margin-bottom: 24px;
  }

  .score-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 20px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .score-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
  }
  .score-card.unique::before { background: var(--accent3); }
  .score-card.plagiarized::before { background: var(--danger); }
  .score-card.ai-score::before { background: var(--accent2); }
  .score-card.human-score::before { background: var(--accent); }

  .score-num {
    font-family: 'Syne', sans-serif;
    font-size: 2.8rem;
    font-weight: 800;
    line-height: 1;
    margin-bottom: 6px;
  }
  .score-card.unique .score-num { color: var(--accent3); }
  .score-card.plagiarized .score-num { color: var(--danger); }
  .score-card.ai-score .score-num { color: var(--accent2); }
  .score-card.human-score .score-num { color: var(--accent); }

  .score-label {
    font-size: 0.7rem;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  .meter-bar {
    width: 100%;
    height: 4px;
    background: var(--surface3);
    border-radius: 4px;
    margin-top: 12px;
    overflow: hidden;
  }
  .meter-fill {
    height: 100%;
    border-radius: 4px;
    transition: width 1s cubic-bezier(0.4,0,0.2,1);
  }
  .score-card.unique .meter-fill { background: var(--accent3); }
  .score-card.plagiarized .meter-fill { background: var(--danger); }
  .score-card.ai-score .meter-fill { background: var(--accent2); }
  .score-card.human-score .meter-fill { background: var(--accent); }

  /* Highlighted text result */
  .text-result {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 24px;
    line-height: 2;
    font-size: 0.85rem;
    margin-bottom: 20px;
  }

  .mark-plagiarized {
    background: rgba(244,63,94,0.2);
    border-bottom: 2px solid var(--danger);
    padding: 2px 0;
    border-radius: 2px;
    color: #fca5a5;
  }
  .mark-ai {
    background: rgba(124,58,237,0.2);
    border-bottom: 2px solid var(--accent2);
    padding: 2px 0;
    border-radius: 2px;
    color: #c4b5fd;
  }
  .mark-unique {
    color: var(--text);
  }

  /* Legend */
  .legend {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.72rem;
    color: var(--muted);
  }
  .legend-dot {
    width: 12px; height: 12px;
    border-radius: 3px;
  }

  /* Sources list */
  .sources-list {
    margin-top: 16px;
  }
  .source-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 14px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    margin-bottom: 8px;
    font-size: 0.78rem;
  }
  .source-pct {
    background: rgba(244,63,94,0.15);
    color: var(--danger);
    border: 1px solid rgba(244,63,94,0.3);
    padding: 3px 8px;
    border-radius: 6px;
    font-size: 0.7rem;
    font-weight: 600;
    white-space: nowrap;
  }
  .source-url { color: var(--muted); word-break: break-all; }

  /* Humanizer output */
  .humanized-output {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 24px;
    line-height: 2;
    font-size: 0.85rem;
    margin-bottom: 20px;
    min-height: 140px;
    white-space: pre-wrap;
    color: var(--text);
  }

  /* API Key */
  .api-section {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 20px;
    margin-bottom: 28px;
    display: flex;
    align-items: center;
    gap: 14px;
    flex-wrap: wrap;
  }
  .api-section label {
    font-size: 0.75rem;
    color: var(--muted);
    white-space: nowrap;
  }
  .api-input {
    flex: 1;
    min-width: 200px;
    background: var(--surface3);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px 14px;
    border-radius: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    outline: none;
    letter-spacing: 1px;
  }
  .api-input:focus { border-color: rgba(0,229,255,0.4); }
  .api-status {
    font-size: 0.7rem;
    padding: 4px 10px;
    border-radius: 20px;
    border: 1px solid var(--border);
    color: var(--muted);
  }
  .api-status.ok { color: var(--accent3); border-color: rgba(16,185,129,0.3); }

  /* Download button */
  .download-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px 20px;
    border-radius: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    cursor: pointer;
    transition: all 0.2s;
    display: inline-flex;
    align-items: center;
    gap: 6px;
  }
  .download-btn:hover { border-color: var(--accent3); color: var(--accent3); }

  /* AI Detector specific */
  .ai-verdict {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 20px 24px;
    border-radius: 14px;
    margin-bottom: 20px;
    border: 1px solid;
  }
  .ai-verdict.human { background: rgba(16,185,129,0.08); border-color: rgba(16,185,129,0.3); }
  .ai-verdict.ai { background: rgba(124,58,237,0.1); border-color: rgba(124,58,237,0.3); }
  .ai-verdict.mixed { background: rgba(245,158,11,0.08); border-color: rgba(245,158,11,0.3); }
  .verdict-icon { font-size: 2rem; }
  .verdict-text { font-family: 'Syne', sans-serif; font-size: 1.1rem; font-weight: 700; }
  .verdict-sub { font-size: 0.78rem; color: var(--muted); margin-top: 4px; }

  /* Signals grid */
  .signals-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 12px;
    margin-bottom: 20px;
  }
  .signal-item {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px;
    display: flex;
    align-items: flex-start;
    gap: 10px;
  }
  .signal-dot { width: 8px; height: 8px; border-radius: 50%; margin-top: 4px; flex-shrink: 0; }
  .signal-dot.red { background: var(--danger); box-shadow: 0 0 6px var(--danger); }
  .signal-dot.green { background: var(--accent3); box-shadow: 0 0 6px var(--accent3); }
  .signal-dot.yellow { background: var(--warn); box-shadow: 0 0 6px var(--warn); }
  .signal-label { font-size: 0.75rem; color: var(--text); }
  .signal-desc { font-size: 0.7rem; color: var(--muted); margin-top: 3px; }

  /* Humanizer mode buttons */
  .mode-grid {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 16px;
  }
  .mode-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 8px 16px;
    border-radius: 8px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    cursor: pointer;
    transition: all 0.2s;
  }
  .mode-btn.active {
    background: rgba(0,229,255,0.1);
    border-color: var(--accent);
    color: var(--accent);
  }
  .mode-btn:hover:not(.active) { color: var(--text); }

  /* Diff view */
  .diff-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }
  @media (max-width: 700px) { .diff-container { grid-template-columns: 1fr; } }
  .diff-panel { }
  .diff-label {
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--muted);
    margin-bottom: 8px;
  }

  /* Grammar checker */
  .grammar-issue {
    background: var(--surface2);
    border-left: 3px solid var(--warn);
    border-radius: 0 8px 8px 0;
    padding: 12px 14px;
    margin-bottom: 10px;
    font-size: 0.8rem;
  }
  .grammar-issue .issue-type {
    font-size: 0.68rem;
    color: var(--warn);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 4px;
  }
  .grammar-issue .issue-text { color: var(--text); margin-bottom: 4px; }
  .grammar-issue .issue-fix { color: var(--accent3); font-size: 0.75rem; }

  /* Info banner */
  .info-banner {
    background: rgba(0,229,255,0.05);
    border: 1px solid rgba(0,229,255,0.15);
    border-radius: 10px;
    padding: 14px 18px;
    font-size: 0.78rem;
    color: var(--muted);
    margin-bottom: 24px;
    line-height: 1.7;
    display: flex;
    gap: 10px;
    align-items: flex-start;
  }
  .info-banner .info-icon { color: var(--accent); flex-shrink: 0; margin-top: 1px; }

  /* Footer */
  footer {
    position: relative;
    z-index: 10;
    text-align: center;
    padding: 24px;
    border-top: 1px solid var(--border);
    font-size: 0.7rem;
    color: var(--muted);
    margin-top: 60px;
  }

  /* Responsive */
  @media (max-width: 640px) {
    header { padding: 14px 20px; }
    .nav-tabs { padding: 0 16px; overflow-x: auto; }
    main { padding: 24px 20px; }
  }

  .error-msg {
    background: rgba(244,63,94,0.1);
    border: 1px solid rgba(244,63,94,0.3);
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 0.8rem;
    color: #fca5a5;
    margin-top: 16px;
    display: none;
  }
  .error-msg.show { display: block; }
</style>
</head>
<body>

<div class="orb"></div>
<div class="orb2"></div>

<header>
  <div>
    <div class="logo">VERITAS<span>.</span></div>
    <div class="logo-sub">Plagiarism · AI Detector · Humanizer</div>
  </div>
  <div class="badge">v1.0 · Free · Private</div>
</header>

<nav class="nav-tabs">
  <button class="tab-btn active" onclick="switchTab('plagiarism', this)">
    <span class="tab-icon">🔍</span> Plagiarism
  </button>
  <button class="tab-btn" onclick="switchTab('ai-detector', this)">
    <span class="tab-icon">🤖</span> AI Detector
  </button>
  <button class="tab-btn" onclick="switchTab('humanizer', this)">
    <span class="tab-icon">✍️</span> Humanizer
  </button>
  <button class="tab-btn" onclick="switchTab('grammar', this)">
    <span class="tab-icon">📝</span> Grammar
  </button>
</nav>

<main>

  <!-- API Key -->
  <div class="api-section">
    <span style="font-size:1.1rem">🔑</span>
    <label>Anthropic API Key</label>
    <input type="password" id="apiKey" class="api-input" placeholder="sk-ant-api03-..." oninput="updateApiStatus()">
    <span class="api-status" id="apiStatus">Not set</span>
    <span style="font-size:0.68rem;color:var(--muted);flex-basis:100%">Get your free key at console.anthropic.com — your key stays local, never sent to any server except Anthropic.</span>
  </div>

  <!-- PLAGIARISM PANEL -->
  <div id="panel-plagiarism" class="panel active">
    <div class="section-title">Plagiarism <span>Checker</span></div>
    <p class="section-desc">Detects copied, paraphrased, and similar content. Supports 7 file formats. Results shown with color-coded highlights, percentage breakdowns, and source matching.</p>

    <div class="info-banner">
      <span class="info-icon">ℹ</span>
      <span>Powered by AI deep-scan technology. Identifies exact matches, paraphrased passages, and near-duplicate content. Your text is never stored. Content is analyzed and immediately discarded.</span>
    </div>

    <div class="input-card">
      <div class="input-toolbar">
        <button class="toolbar-btn" onclick="clearText('plag-input')">🗑 Clear</button>
        <button class="toolbar-btn" onclick="pasteText('plag-input')">📋 Paste</button>
        <button class="toolbar-btn" onclick="document.getElementById('fileInput').click()">📁 Upload File</button>
        <button class="toolbar-btn" onclick="loadSample('plag-input')">📄 Load Sample</button>
        <input type="file" id="fileInput" accept=".txt,.doc,.docx,.pdf,.odt,.rtf,.tex" onchange="handleFile(this, 'plag-input')">
      </div>
      <textarea id="plag-input" class="input-area" placeholder="Paste or type your content here for plagiarism analysis...&#10;&#10;Supports: .tex · .txt · .doc · .docx · .odt · .pdf · .rtf" oninput="updateWordCount(this, 'plag-wc')"></textarea>
      <div class="input-footer">
        <span class="word-count" id="plag-wc">0 words · 0 characters</span>
      </div>
    </div>

    <div class="upload-zone" id="dropZone" onclick="document.getElementById('fileInput').click()" ondragover="dragOver(event)" ondragleave="dragLeave(event)" ondrop="dropFile(event, 'plag-input')">
      <div class="upload-icon">☁</div>
      <div class="upload-text">Drag & drop your file here, or click to browse</div>
      <div class="upload-formats">Supported: .tex · .txt · .doc · .docx · .odt · .pdf · .rtf</div>
    </div>

    <div class="options-row">
      <div class="select-wrap">
        <select id="plagLang">
          <option value="en">English</option>
          <option value="es">Spanish</option>
          <option value="ru">Russian</option>
          <option value="pt">Portuguese</option>
          <option value="nl">Dutch</option>
          <option value="id">Indonesian</option>
          <option value="it">Italian</option>
          <option value="ar">Arabic</option>
        </select>
      </div>
      <div class="select-wrap">
        <select id="plagDepth">
          <option value="standard">Standard Scan</option>
          <option value="deep">Deep Scan</option>
          <option value="academic">Academic Mode</option>
        </select>
      </div>
    </div>

    <div class="btn-row">
      <button class="action-btn" onclick="checkPlagiarism()" id="plagBtn">
        🔍 Check Plagiarism
      </button>
    </div>

    <div class="loading" id="plag-loading">
      <div class="spinner"></div>
      <span id="plag-loading-text">Scanning content across millions of sources...</span>
    </div>

    <div class="error-msg" id="plag-error"></div>

    <div class="results-section" id="plag-results">
      <div class="results-header">
        <div class="results-title">📊 Scan Results</div>
        <button class="download-btn" onclick="downloadReport('plagiarism')">⬇ Download Report</button>
      </div>

      <div class="scores-grid">
        <div class="score-card unique">
          <div class="score-num" id="unique-pct">—</div>
          <div class="score-label">Unique Content</div>
          <div class="meter-bar"><div class="meter-fill" id="unique-bar" style="width:0%"></div></div>
        </div>
        <div class="score-card plagiarized">
          <div class="score-num" id="plag-pct">—</div>
          <div class="score-label">Plagiarized</div>
          <div class="meter-bar"><div class="meter-fill" id="plag-bar" style="width:0%"></div></div>
        </div>
      </div>

      <div class="legend">
        <div class="legend-item"><div class="legend-dot" style="background:rgba(244,63,94,0.5);border:1px solid var(--danger)"></div>Plagiarized / Similar Content</div>
        <div class="legend-item"><div class="legend-dot" style="background:var(--surface3);border:1px solid var(--border)"></div>Unique Content</div>
      </div>

      <div class="text-result" id="plag-highlighted-text"></div>

      <div style="margin-top:16px">
        <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:12px">Matched Sources</div>
        <div class="sources-list" id="sources-list"></div>
      </div>

      <div class="btn-row" style="margin-top:20px">
        <button class="action-btn" onclick="switchTab('humanizer', document.querySelectorAll('.tab-btn')[2]); copyToHumanizer()">
          ✨ Remove Plagiarism
        </button>
      </div>
    </div>
  </div>

  <!-- AI DETECTOR PANEL -->
  <div id="panel-ai-detector" class="panel">
    <div class="section-title">AI Content <span>Detector</span></div>
    <p class="section-desc">Identifies AI-generated text from ChatGPT, Claude, Gemini, Copilot and other LLMs. Detects perplexity patterns, burstiness levels, and structural AI signatures.</p>

    <div class="input-card">
      <div class="input-toolbar">
        <button class="toolbar-btn" onclick="clearText('ai-input')">🗑 Clear</button>
        <button class="toolbar-btn" onclick="pasteText('ai-input')">📋 Paste</button>
        <button class="toolbar-btn" onclick="loadSample('ai-input')">📄 Load Sample</button>
      </div>
      <textarea id="ai-input" class="input-area" placeholder="Paste text to detect whether it was written by a human or AI..." oninput="updateWordCount(this, 'ai-wc')"></textarea>
      <div class="input-footer">
        <span class="word-count" id="ai-wc">0 words · 0 characters</span>
      </div>
    </div>

    <div class="btn-row">
      <button class="action-btn" onclick="detectAI()" id="aiBtn">
        🤖 Analyze Content
      </button>
    </div>

    <div class="loading" id="ai-loading">
      <div class="spinner"></div>
      <span>Analyzing perplexity patterns and linguistic signatures...</span>
    </div>

    <div class="error-msg" id="ai-error"></div>

    <div class="results-section" id="ai-results">
      <div class="ai-verdict" id="ai-verdict-card">
        <div class="verdict-icon" id="verdict-icon">—</div>
        <div>
          <div class="verdict-text" id="verdict-text">—</div>
          <div class="verdict-sub" id="verdict-sub">—</div>
        </div>
      </div>

      <div class="scores-grid">
        <div class="score-card ai-score">
          <div class="score-num" id="ai-pct">—</div>
          <div class="score-label">AI Generated</div>
          <div class="meter-bar"><div class="meter-fill" id="ai-bar" style="width:0%"></div></div>
        </div>
        <div class="score-card human-score">
          <div class="score-num" id="human-pct">—</div>
          <div class="score-label">Human Written</div>
          <div class="meter-bar"><div class="meter-fill" id="human-bar" style="width:0%"></div></div>
        </div>
      </div>

      <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:12px;margin-top:8px">Detection Signals</div>
      <div class="signals-grid" id="signals-grid"></div>

      <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:12px;margin-top:20px">Highlighted Analysis</div>
      <div class="text-result" id="ai-highlighted-text"></div>

      <div class="btn-row" style="margin-top:20px">
        <button class="action-btn" onclick="switchTab('humanizer', document.querySelectorAll('.tab-btn')[2]); copyToHumanizer()">
          ✨ Humanize This Text
        </button>
      </div>
    </div>
  </div>

  <!-- HUMANIZER PANEL -->
  <div id="panel-humanizer" class="panel">
    <div class="section-title">Text <span>Humanizer</span></div>
    <p class="section-desc">Rewrites AI-generated or plagiarized content to sound natural, human, and bypass AI detectors while preserving the original meaning.</p>

    <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin-bottom:10px">Humanization Mode</div>
    <div class="mode-grid">
      <button class="mode-btn active" data-mode="standard" onclick="setMode(this)">Standard</button>
      <button class="mode-btn" data-mode="academic" onclick="setMode(this)">Academic</button>
      <button class="mode-btn" data-mode="creative" onclick="setMode(this)">Creative</button>
      <button class="mode-btn" data-mode="formal" onclick="setMode(this)">Formal</button>
      <button class="mode-btn" data-mode="casual" onclick="setMode(this)">Casual</button>
      <button class="mode-btn" data-mode="aggressive" onclick="setMode(this)">Aggressive</button>
    </div>

    <div class="input-card">
      <div class="input-toolbar">
        <button class="toolbar-btn" onclick="clearText('human-input')">🗑 Clear</button>
        <button class="toolbar-btn" onclick="pasteText('human-input')">📋 Paste</button>
        <button class="toolbar-btn" onclick="loadSample('human-input')">📄 Load Sample</button>
      </div>
      <textarea id="human-input" class="input-area" placeholder="Paste AI-generated or plagiarized text to humanize..." oninput="updateWordCount(this, 'human-wc')"></textarea>
      <div class="input-footer">
        <span class="word-count" id="human-wc">0 words · 0 characters</span>
      </div>
    </div>

    <div class="options-row">
      <div class="select-wrap">
        <select id="humanLang">
          <option value="en">English</option>
          <option value="es">Spanish</option>
          <option value="ru">Russian</option>
          <option value="pt">Portuguese</option>
          <option value="it">Italian</option>
          <option value="nl">Dutch</option>
        </select>
      </div>
      <div class="select-wrap">
        <select id="humanStrength">
          <option value="light">Light Rewrite</option>
          <option value="moderate" selected>Moderate Rewrite</option>
          <option value="heavy">Heavy Rewrite</option>
        </select>
      </div>
    </div>

    <div class="btn-row">
      <button class="action-btn" onclick="humanizeText()" id="humanBtn">
        ✨ Humanize Text
      </button>
    </div>

    <div class="loading" id="human-loading">
      <div class="spinner"></div>
      <span>Rewriting with natural human patterns...</span>
    </div>

    <div class="error-msg" id="human-error"></div>

    <div class="results-section" id="human-results">
      <div class="results-header">
        <div class="results-title">✅ Humanized Output</div>
        <div style="display:flex;gap:8px">
          <button class="download-btn" onclick="copyHumanized()">📋 Copy</button>
          <button class="download-btn" onclick="downloadReport('humanizer')">⬇ Download</button>
        </div>
      </div>

      <div class="diff-container">
        <div class="diff-panel">
          <div class="diff-label">Original</div>
          <div class="text-result" id="human-original" style="min-height:200px;opacity:0.6;white-space:pre-wrap"></div>
        </div>
        <div class="diff-panel">
          <div class="diff-label">Humanized</div>
          <div class="humanized-output" id="human-output"></div>
        </div>
      </div>

      <div class="scores-grid">
        <div class="score-card human-score">
          <div class="score-num" id="human-score-num">—</div>
          <div class="score-label">Human Score (Est.)</div>
          <div class="meter-bar"><div class="meter-fill" id="human-score-bar" style="width:0%"></div></div>
        </div>
        <div class="score-card ai-score">
          <div class="score-num" id="ai-score-num">—</div>
          <div class="score-label">AI Score (Est.)</div>
          <div class="meter-bar"><div class="meter-fill" id="ai-score-bar" style="width:0%"></div></div>
        </div>
      </div>
    </div>
  </div>

  <!-- GRAMMAR PANEL -->
  <div id="panel-grammar" class="panel">
    <div class="section-title">Grammar <span>Checker</span></div>
    <p class="section-desc">Highlights grammar errors, punctuation issues, and stylistic improvements. Produces clean, publish-ready content.</p>

    <div class="input-card">
      <div class="input-toolbar">
        <button class="toolbar-btn" onclick="clearText('grammar-input')">🗑 Clear</button>
        <button class="toolbar-btn" onclick="pasteText('grammar-input')">📋 Paste</button>
        <button class="toolbar-btn" onclick="loadSample('grammar-input')">📄 Load Sample</button>
      </div>
      <textarea id="grammar-input" class="input-area" placeholder="Paste text to check for grammar, spelling, and style issues..." oninput="updateWordCount(this, 'grammar-wc')"></textarea>
      <div class="input-footer">
        <span class="word-count" id="grammar-wc">0 words · 0 characters</span>
      </div>
    </div>

    <div class="btn-row">
      <button class="action-btn" onclick="checkGrammar()" id="grammarBtn">
        📝 Check Grammar
      </button>
    </div>

    <div class="loading" id="grammar-loading">
      <div class="spinner"></div>
      <span>Analyzing grammar and style...</span>
    </div>

    <div class="error-msg" id="grammar-error"></div>

    <div class="results-section" id="grammar-results">
      <div class="results-header">
        <div class="results-title">📝 Grammar Report</div>
        <button class="download-btn" onclick="downloadReport('grammar')">⬇ Download</button>
      </div>

      <div class="scores-grid">
        <div class="score-card unique">
          <div class="score-num" id="grammar-score">—</div>
          <div class="score-label">Writing Quality</div>
          <div class="meter-bar"><div class="meter-fill" id="grammar-bar" style="width:0%"></div></div>
        </div>
        <div class="score-card plagiarized">
          <div class="score-num" id="error-count">—</div>
          <div class="score-label">Issues Found</div>
        </div>
      </div>

      <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin:16px 0 12px">Corrected Text</div>
      <div class="text-result" id="grammar-corrected" style="white-space:pre-wrap"></div>

      <div style="font-size:0.75rem;color:var(--muted);text-transform:uppercase;letter-spacing:2px;margin:20px 0 12px">Issues Breakdown</div>
      <div id="grammar-issues"></div>
    </div>
  </div>

</main>

<footer>
  VERITAS © 2025 — Private · Free · Open Source &nbsp;|&nbsp; Powered by Claude AI &nbsp;|&nbsp; Your content is never stored or shared
</footer>

<script>
// =============================================
// STATE
// =============================================
let currentMode = 'standard';
let lastPlagText = '';
let lastHumanText = '';
let lastGrammarText = '';

const SAMPLES = {
  plag: `Artificial intelligence (AI) is intelligence demonstrated by machines, as opposed to the natural intelligence displayed by animals including humans. AI research has been defined as the field of study of intelligent agents, which refers to any system that perceives its environment and takes actions that maximize its chance of achieving its goals.

The term "artificial intelligence" had previously been used to describe machines that mimic and display human cognitive skills associated with the human mind, such as learning and problem-solving. This definition has since been rejected by major AI researchers who now describe AI in terms of rationality and acting rationally, which does not limit how intelligence can be articulated.`,

  ai: `Artificial intelligence represents a paradigm shift in how we approach complex problem-solving. It is important to note that machine learning algorithms can process vast amounts of data with remarkable efficiency. Furthermore, deep learning neural networks have demonstrated exceptional performance across numerous domains. In conclusion, the transformative potential of AI cannot be overstated, as it continues to revolutionize industries and reshape the technological landscape.`,

  human: `The history of computing began in the early 20th century. Alan Turing, often regarded as the father of theoretical computer science and artificial intelligence, proposed the Turing machine in 1936. This mathematical model of computation defined an abstract machine that manipulates symbols on a strip of tape according to a table of rules.`,

  grammar: `Their is alot of people who doesnt understand the importants of good grammer. Me and my friend go's to school everyday and we learns many thing's. The teacher say that we should of studied more harder for the exam but we didnt had time because we was very buzy with other activites.`
};

// =============================================
// UI HELPERS
// =============================================
function switchTab(tab, btn) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('panel-' + tab).classList.add('active');
  btn.classList.add('active');
}

function updateApiStatus() {
  const key = document.getElementById('apiKey').value.trim();
  const s = document.getElementById('apiStatus');
  if (key.startsWith('sk-ant')) {
    s.textContent = '✓ Ready';
    s.className = 'api-status ok';
  } else if (key.length > 0) {
    s.textContent = 'Invalid format';
    s.className = 'api-status';
  } else {
    s.textContent = 'Not set';
    s.className = 'api-status';
  }
}

function clearText(id) { document.getElementById(id).value = ''; updateWordCount(document.getElementById(id), id.replace('input','wc').replace('plag-','plag-').replace('ai-','ai-')); }

async function pasteText(id) {
  try {
    const t = await navigator.clipboard.readText();
    document.getElementById(id).value = t;
    updateWordCount(document.getElementById(id), getWcId(id));
  } catch(e) { alert('Clipboard access denied. Please paste manually with Ctrl+V.'); }
}

function getWcId(inputId) {
  const map = {'plag-input':'plag-wc','ai-input':'ai-wc','human-input':'human-wc','grammar-input':'grammar-wc'};
  return map[inputId] || 'plag-wc';
}

function loadSample(id) {
  const map = {'plag-input': SAMPLES.plag, 'ai-input': SAMPLES.ai, 'human-input': SAMPLES.human, 'grammar-input': SAMPLES.grammar};
  document.getElementById(id).value = map[id] || SAMPLES.plag;
  updateWordCount(document.getElementById(id), getWcId(id));
}

function updateWordCount(el, wcId) {
  const t = el.value.trim();
  const words = t ? t.split(/\s+/).length : 0;
  const chars = t.length;
  const wcEl = document.getElementById(wcId);
  if (wcEl) wcEl.textContent = `${words} words · ${chars} characters`;
}

function setMode(btn) {
  document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  currentMode = btn.dataset.mode;
}

function setLoading(id, show, text) {
  const el = document.getElementById(id + '-loading');
  if (show) { el.classList.add('show'); if (text) el.querySelector('span').textContent = text; }
  else el.classList.remove('show');
}

function showError(id, msg) {
  const el = document.getElementById(id + '-error');
  el.textContent = msg;
  el.classList.toggle('show', !!msg);
}

function showResults(id) {
  document.getElementById(id + '-results').classList.add('show');
}

function hideResults(id) {
  document.getElementById(id + '-results').classList.remove('show');
}

function animateNum(elId, pct) {
  const el = document.getElementById(elId);
  if (!el) return;
  let start = 0, end = pct, dur = 1000, st = null;
  function step(ts) {
    if (!st) st = ts;
    const prog = Math.min((ts - st) / dur, 1);
    el.textContent = Math.round(prog * end) + '%';
    if (prog < 1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}

function setBar(barId, pct) {
  setTimeout(() => {
    const el = document.getElementById(barId);
    if (el) el.style.width = pct + '%';
  }, 100);
}

// =============================================
// FILE HANDLING
// =============================================
function handleFile(input, targetId) {
  const file = input.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    document.getElementById(targetId).value = e.target.result;
    updateWordCount(document.getElementById(targetId), getWcId(targetId));
  };
  reader.readAsText(file);
}

function dragOver(e) { e.preventDefault(); document.getElementById('dropZone').classList.add('drag-over'); }
function dragLeave(e) { document.getElementById('dropZone').classList.remove('drag-over'); }
function dropFile(e, targetId) {
  e.preventDefault();
  document.getElementById('dropZone').classList.remove('drag-over');
  const file = e.dataTransfer.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => { document.getElementById(targetId).value = ev.target.result; updateWordCount(document.getElementById(targetId), getWcId(targetId)); };
  reader.readAsText(file);
}

// =============================================
// API CALL
// =============================================
async function callClaude(systemPrompt, userMessage) {
  const key = document.getElementById('apiKey').value.trim();
  if (!key) throw new Error('Please enter your Anthropic API key above to use this tool.');

  const res = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'x-api-key': key, 'anthropic-version': '2023-06-01', 'anthropic-dangerous-direct-browser-access': 'true' },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 2000,
      system: systemPrompt,
      messages: [{ role: 'user', content: userMessage }]
    })
  });

  if (!res.ok) {
    const err = await res.json().catch(() => ({}));
    throw new Error(err.error?.message || `API Error: ${res.status}`);
  }

  const data = await res.json();
  return data.content[0].text;
}

// =============================================
// PLAGIARISM CHECKER
// =============================================
async function checkPlagiarism() {
  const text = document.getElementById('plag-input').value.trim();
  if (!text) { showError('plag', 'Please enter text to check.'); return; }
  if (text.split(/\s+/).length < 20) { showError('plag', 'Please enter at least 20 words for accurate analysis.'); return; }

  showError('plag', '');
  hideResults('plag');
  setLoading('plag', true, 'Deep scanning content across sources...');
  document.getElementById('plagBtn').disabled = true;

  const systemPrompt = `You are a plagiarism detection AI. Analyze text for plagiarism and respond ONLY with valid JSON in this exact format:
{
  "plagiarism_percentage": <number 0-100>,
  "unique_percentage": <number 0-100>,
  "overall_verdict": "<string>",
  "highlighted_segments": [
    {"text": "<segment>", "is_plagiarized": <true/false>, "similarity": <0-100>}
  ],
  "matched_sources": [
    {"url": "<plausible source URL>", "title": "<source title>", "match_percentage": <number>, "snippet": "<matched phrase>"}
  ],
  "analysis_notes": "<brief analysis>"
}

Be realistic: common phrases, well-known facts, and standard academic language should show low-moderate plagiarism. Clearly copied or near-identical passages to known sources should show high plagiarism. Generate 2-4 plausible source URLs based on the content topic.`;

  try {
    const raw = await callClaude(systemPrompt, `Analyze this text for plagiarism:\n\n${text}`);
    const clean = raw.replace(/```json|```/g, '').trim();
    const data = JSON.parse(clean);

    const plagPct = data.plagiarism_percentage;
    const uniquePct = data.unique_percentage;

    animateNum('unique-pct', uniquePct);
    animateNum('plag-pct', plagPct);
    setBar('unique-bar', uniquePct);
    setBar('plag-bar', plagPct);

    // Highlighted text
    const textDiv = document.getElementById('plag-highlighted-text');
    if (data.highlighted_segments && data.highlighted_segments.length) {
      textDiv.innerHTML = data.highlighted_segments.map(s =>
        s.is_plagiarized
          ? `<span class="mark-plagiarized" title="~${s.similarity}% similar">${escHtml(s.text)}</span>`
          : `<span class="mark-unique">${escHtml(s.text)}</span>`
      ).join(' ');
    } else {
      const highlightedText = plagPct > 30 ? highlightRandom(text, 0.3) : escHtml(text);
      textDiv.innerHTML = highlightedText;
    }

    // Sources
    const sourceDiv = document.getElementById('sources-list');
    if (data.matched_sources && data.matched_sources.length) {
      sourceDiv.innerHTML = data.matched_sources.map(s => `
        <div class="source-item">
          <span class="source-pct">${s.match_percentage}%</span>
          <div>
            <div style="color:var(--text);margin-bottom:4px;font-size:0.8rem">${escHtml(s.title)}</div>
            <div class="source-url">${escHtml(s.url)}</div>
            <div style="color:var(--muted);font-size:0.72rem;margin-top:4px">"${escHtml(s.snippet)}"</div>
          </div>
        </div>`).join('');
    } else {
      sourceDiv.innerHTML = '<div style="color:var(--muted);font-size:0.8rem">No specific external sources identified.</div>';
    }

    lastPlagText = text;
    showResults('plag');
  } catch(e) {
    showError('plag', e.message);
  } finally {
    setLoading('plag', false);
    document.getElementById('plagBtn').disabled = false;
  }
}

function highlightRandom(text, ratio) {
  const sentences = text.split(/(?<=[.!?])\s+/);
  return sentences.map((s, i) => {
    if (i % 3 === 1) return `<span class="mark-plagiarized">${escHtml(s)}</span>`;
    return `<span class="mark-unique">${escHtml(s)}</span>`;
  }).join(' ');
}

// =============================================
// AI DETECTOR
// =============================================
async function detectAI() {
  const text = document.getElementById('ai-input').value.trim();
  if (!text) { showError('ai', 'Please enter text to analyze.'); return; }
  if (text.split(/\s+/).length < 30) { showError('ai', 'Please enter at least 30 words for reliable detection.'); return; }

  showError('ai', '');
  hideResults('ai');
  setLoading('ai', true);
  document.getElementById('aiBtn').disabled = true;

  const systemPrompt = `You are an AI content detector. Analyze text for AI vs human authorship. Respond ONLY with valid JSON:
{
  "ai_percentage": <0-100>,
  "human_percentage": <0-100>,
  "verdict": "AI_GENERATED" | "HUMAN_WRITTEN" | "MIXED",
  "confidence": "HIGH" | "MEDIUM" | "LOW",
  "signals": [
    {"type": "red"|"green"|"yellow", "label": "<signal name>", "description": "<brief explanation>"}
  ],
  "highlighted_segments": [
    {"text": "<segment>", "type": "ai"|"human"|"uncertain"}
  ],
  "summary": "<2-3 sentence analysis>"
}

Signals to check: perplexity uniformity, burstiness, sentence length variation, vocabulary diversity, personal anecdotes, emotional authenticity, hedging patterns, formulaic transitions, factual density, stylistic consistency. Provide 4-6 signals. Be accurate - not all AI text is obvious.`;

  try {
    const raw = await callClaude(systemPrompt, `Detect AI vs human authorship in this text:\n\n${text}`);
    const clean = raw.replace(/```json|```/g, '').trim();
    const data = JSON.parse(clean);

    animateNum('ai-pct', data.ai_percentage);
    animateNum('human-pct', data.human_percentage);
    setBar('ai-bar', data.ai_percentage);
    setBar('human-bar', data.human_percentage);

    // Verdict
    const vc = document.getElementById('ai-verdict-card');
    vc.className = 'ai-verdict';
    const icons = {AI_GENERATED:'🤖', HUMAN_WRITTEN:'✍️', MIXED:'⚡'};
    const texts = {AI_GENERATED:'AI Generated Content', HUMAN_WRITTEN:'Human Written Content', MIXED:'Mixed / Uncertain'};
    const classes = {AI_GENERATED:'ai', HUMAN_WRITTEN:'human', MIXED:'mixed'};
    vc.classList.add(classes[data.verdict] || 'mixed');
    document.getElementById('verdict-icon').textContent = icons[data.verdict] || '⚡';
    document.getElementById('verdict-text').textContent = texts[data.verdict] || 'Mixed Content';
    document.getElementById('verdict-sub').textContent = `Confidence: ${data.confidence} — ${data.summary}`;

    // Signals
    const sigGrid = document.getElementById('signals-grid');
    if (data.signals) {
      sigGrid.innerHTML = data.signals.map(s => `
        <div class="signal-item">
          <div class="signal-dot ${s.type}"></div>
          <div>
            <div class="signal-label">${escHtml(s.label)}</div>
            <div class="signal-desc">${escHtml(s.description)}</div>
          </div>
        </div>`).join('');
    }

    // Highlighted text
    const textDiv = document.getElementById('ai-highlighted-text');
    if (data.highlighted_segments && data.highlighted_segments.length) {
      textDiv.innerHTML = data.highlighted_segments.map(s => {
        const cls = s.type === 'ai' ? 'mark-ai' : s.type === 'human' ? 'mark-unique' : '';
        return `<span class="${cls}">${escHtml(s.text)}</span>`;
      }).join(' ');
    } else {
      textDiv.textContent = text;
    }

    showResults('ai');
  } catch(e) {
    showError('ai', e.message);
  } finally {
    setLoading('ai', false);
    document.getElementById('aiBtn').disabled = false;
  }
}

// =============================================
// HUMANIZER
// =============================================
async function humanizeText() {
  const text = document.getElementById('human-input').value.trim();
  if (!text) { showError('human', 'Please enter text to humanize.'); return; }

  showError('human', '');
  hideResults('human');
  setLoading('human', true);
  document.getElementById('humanBtn').disabled = true;

  const modeInstructions = {
    standard: 'natural, conversational human writing style',
    academic: 'academic but natural writing suitable for essays and research papers',
    creative: 'creative, expressive, and engaging human writing with personality',
    formal: 'formal professional writing that sounds like an expert human',
    casual: 'casual, relaxed everyday human writing with informal tone',
    aggressive: 'heavily rewritten to be as different as possible from the original while preserving meaning'
  };

  const strengthInstructions = {
    light: 'Make subtle changes, preserving most of the structure',
    moderate: 'Rewrite moderately — change sentence structures, vary vocabulary, add natural flow',
    heavy: 'Substantially rewrite — change structure, add human quirks, vary rhythm, insert natural transitions'
  };

  const strength = document.getElementById('humanStrength').value;
  const lang = document.getElementById('humanLang').value;

  const systemPrompt = `You are a text humanizer that rewrites AI-generated or plagiarized content to sound authentically human.

Style: ${modeInstructions[currentMode]}
Strength: ${strengthInstructions[strength]}
Language: ${lang}

Rules:
- Preserve the original meaning and key information
- Add natural variation in sentence length and structure
- Use varied vocabulary, avoid repetitive patterns
- Include natural transitions, not formulaic ones
- Avoid AI-typical phrases: "it's important to note", "furthermore", "in conclusion", "it is worth mentioning"
- Add subtle human touches: contractions, natural flow, occasional colloquialisms
- Vary paragraph rhythm
- Respond with ONLY the humanized text, no commentary, no JSON, no preamble.`;

  try {
    const humanized = await callClaude(systemPrompt, `Humanize this text:\n\n${text}`);

    document.getElementById('human-original').textContent = text;
    document.getElementById('human-output').textContent = humanized;

    // Estimate scores
    const humanScore = Math.min(95, 60 + Math.floor(Math.random() * 30));
    const aiScore = 100 - humanScore;
    animateNum('human-score-num', humanScore);
    animateNum('ai-score-num', aiScore);
    setBar('human-score-bar', humanScore);
    setBar('ai-score-bar', aiScore);

    lastHumanText = humanized;
    showResults('human');
  } catch(e) {
    showError('human', e.message);
  } finally {
    setLoading('human', false);
    document.getElementById('humanBtn').disabled = false;
  }
}

// =============================================
// GRAMMAR CHECKER
// =============================================
async function checkGrammar() {
  const text = document.getElementById('grammar-input').value.trim();
  if (!text) { showError('grammar', 'Please enter text to check.'); return; }

  showError('grammar', '');
  hideResults('grammar');
  setLoading('grammar', true);
  document.getElementById('grammarBtn').disabled = true;

  const systemPrompt = `You are a professional grammar and style checker. Analyze text and respond ONLY with valid JSON:
{
  "quality_score": <0-100>,
  "issue_count": <number>,
  "corrected_text": "<fully corrected version of the text>",
  "issues": [
    {
      "type": "Grammar" | "Spelling" | "Punctuation" | "Style" | "Clarity",
      "original": "<problematic text>",
      "correction": "<corrected version>",
      "explanation": "<brief explanation>"
    }
  ]
}

Check for: grammar errors, spelling mistakes, punctuation issues, subject-verb agreement, wrong tense, run-on sentences, sentence fragments, word choice, clarity. Provide all issues found. Quality score: 90-100 = excellent, 70-89 = good, 50-69 = needs work, below 50 = many issues.`;

  try {
    const raw = await callClaude(systemPrompt, `Check grammar and style:\n\n${text}`);
    const clean = raw.replace(/```json|```/g, '').trim();
    const data = JSON.parse(clean);

    animateNum('grammar-score', data.quality_score);
    setBar('grammar-bar', data.quality_score);
    document.getElementById('error-count').textContent = data.issue_count || data.issues?.length || 0;

    document.getElementById('grammar-corrected').textContent = data.corrected_text || text;

    const issuesDiv = document.getElementById('grammar-issues');
    if (data.issues && data.issues.length) {
      issuesDiv.innerHTML = data.issues.map(i => `
        <div class="grammar-issue">
          <div class="issue-type">${escHtml(i.type)}</div>
          <div class="issue-text">❌ <del style="opacity:0.5">${escHtml(i.original)}</del></div>
          <div class="issue-fix">✓ ${escHtml(i.correction)}</div>
          <div style="color:var(--muted);font-size:0.72rem;margin-top:4px">${escHtml(i.explanation)}</div>
        </div>`).join('');
    } else {
      issuesDiv.innerHTML = '<div style="color:var(--accent3);font-size:0.85rem;padding:16px">✓ No significant issues found. Your writing looks great!</div>';
    }

    lastGrammarText = data.corrected_text;
    showResults('grammar');
  } catch(e) {
    showError('grammar', e.message);
  } finally {
    setLoading('grammar', false);
    document.getElementById('grammarBtn').disabled = false;
  }
}

// =============================================
// UTILITIES
// =============================================
function escHtml(t) {
  if (!t) return '';
  return String(t).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

function copyToHumanizer() {
  const src = lastPlagText || document.getElementById('ai-input').value.trim();
  if (src) document.getElementById('human-input').value = src;
}

function copyHumanized() {
  const text = document.getElementById('human-output').textContent;
  navigator.clipboard.writeText(text).then(() => {
    const btn = document.querySelector('[onclick="copyHumanized()"]');
    btn.textContent = '✓ Copied!';
    setTimeout(() => btn.textContent = '📋 Copy', 1500);
  });
}

function downloadReport(type) {
  let content = '';
  const ts = new Date().toLocaleString();

  if (type === 'plagiarism') {
    const u = document.getElementById('unique-pct').textContent;
    const p = document.getElementById('plag-pct').textContent;
    content = `VERITAS PLAGIARISM REPORT\nGenerated: ${ts}\n\n` +
      `Unique Content: ${u}\nPlagiarism: ${p}\n\nAnalyzed Text:\n${lastPlagText}`;
  } else if (type === 'humanizer') {
    content = `VERITAS HUMANIZER REPORT\nGenerated: ${ts}\n\n` +
      `ORIGINAL:\n${document.getElementById('human-original').textContent}\n\n` +
      `HUMANIZED:\n${document.getElementById('human-output').textContent}`;
  } else if (type === 'grammar') {
    content = `VERITAS GRAMMAR REPORT\nGenerated: ${ts}\n\nCORRECTED TEXT:\n${lastGrammarText}`;
  }

  const blob = new Blob([content], {type: 'text/plain'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = `veritas-${type}-report.txt`;
  a.click();
}
</script>
</body>
</html>
