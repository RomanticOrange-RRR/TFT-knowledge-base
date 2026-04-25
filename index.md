---
layout: default
title: TFT My Portal
---
<style>
  /* ── CSS変数 ── */
  :root {
    --bg:         #070b14;
    --surface:    #0d1321;
    --surface-2:  #111c2e;
    --border:     #1c2d4a;
    --gold:       #c89b3c;
    --gold-light: #e8c97a;
    --blue:       #4da8ff;
    --red:        #e53935;
    --text:       #cdd6f4;
    --text-muted: #5a7099;
    --glow-gold:  0 0 18px rgba(200,155,60,0.45);
    --glow-blue:  0 0 18px rgba(77,168,255,0.4);
    --glow-red:   0 0 12px rgba(229,57,53,0.5);
  }

  /* ── グローバル上書き ── */
  body, .site-header, .site-footer, .page-content, .wrapper {
    background: var(--bg) !important;
    color: var(--text) !important;
    border-color: var(--border) !important;
  }
  .site-header {
    border-bottom: 1px solid var(--border) !important;
    box-shadow: 0 2px 24px rgba(0,0,0,0.7) !important;
  }
  .site-title, .site-title:visited {
    color: var(--gold) !important;
    font-weight: 800 !important;
    letter-spacing: 0.06em !important;
    text-shadow: var(--glow-gold) !important;
  }
  a { color: var(--blue); }

  /* ── ツールバー ── */
  .db-toolbar {
    display: flex;
    gap: 0.75rem;
    margin-bottom: 1rem;
    flex-wrap: wrap;
    align-items: center;
  }
  .db-toolbar input {
    padding: 0.55rem 1rem;
    border: 1px solid var(--border);
    border-radius: 6px;
    background: var(--surface);
    color: var(--text);
    font-size: 0.95rem;
    flex: 1;
    min-width: 180px;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .db-toolbar input::placeholder { color: var(--text-muted); }
  .db-toolbar input:focus {
    outline: none;
    border-color: var(--gold);
    box-shadow: var(--glow-gold);
  }
  .db-count {
    font-size: 0.88rem;
    color: var(--gold);
    font-weight: 700;
    letter-spacing: 0.04em;
    white-space: nowrap;
  }
  .btn-token {
    padding: 0.5rem 1rem;
    border: 1px solid var(--border);
    border-radius: 6px;
    background: var(--surface);
    color: var(--text-muted);
    font-size: 0.85rem;
    cursor: pointer;
    transition: all 0.2s;
  }
  .btn-token:hover { border-color: var(--gold); color: var(--gold); }

  /* ── タグ・年・月フィルター ── */
  .tag-filters, .year-filters, .month-filters {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 0.85rem;
    flex-wrap: wrap;
    align-items: center;
  }
  .filter-label {
    font-size: 0.72rem;
    color: var(--text-muted);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    white-space: nowrap;
    margin-right: 0.15rem;
  }
  .tag-btn, .year-btn, .month-btn {
    padding: 0.35rem 0.9rem;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text-muted);
    font-size: 0.8rem;
    cursor: pointer;
    transition: all 0.18s;
    font-weight: 500;
  }
  .tag-btn:hover  { border-color: var(--gold); color: var(--gold); }
  .year-btn:hover { border-color: #a78bfa; color: #a78bfa; }
  .month-btn:hover { border-color: var(--blue); color: var(--blue); }
  .tag-btn.active {
    background: linear-gradient(135deg, var(--gold), var(--gold-light));
    color: #07090f;
    border-color: transparent;
    box-shadow: var(--glow-gold);
    font-weight: 700;
  }
  .year-btn.active {
    background: linear-gradient(135deg, #7c3aed, #a78bfa);
    color: #07090f;
    border-color: transparent;
    box-shadow: 0 0 16px rgba(167,139,250,0.45);
    font-weight: 700;
  }
  .month-btn.active {
    background: linear-gradient(135deg, #2979ff, var(--blue));
    color: #07090f;
    border-color: transparent;
    box-shadow: var(--glow-blue);
    font-weight: 700;
  }
  .month-filters:empty { display: none; }

  /* ── カードグリッド ── */
  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.1rem;
  }
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    position: relative;
    transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
  }
  .card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 10px;
    background: linear-gradient(135deg, rgba(200,155,60,0.05), transparent 60%);
    pointer-events: none;
    z-index: 0;
  }
  .card:hover {
    border-color: var(--gold);
    box-shadow: var(--glow-gold), 0 8px 32px rgba(0,0,0,0.5);
    transform: translateY(-3px);
  }

  /* ── サムネイル ── */
  .card-thumb {
    width: 100%;
    aspect-ratio: 16/9;
    object-fit: cover;
    background: var(--surface-2);
    display: block;
    transition: opacity 0.2s;
  }
  .card:hover .card-thumb { opacity: 0.9; }
  .card-thumb-placeholder {
    width: 100%;
    aspect-ratio: 16/9;
    background: var(--surface-2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.5rem;
  }

  /* ── ツイートプレビュー ── */
  .tweet-preview {
    padding: 0.8rem;
    background: #080d1a;
    border-bottom: 1px solid var(--border);
    font-size: 0.85rem;
    color: var(--text);
    line-height: 1.5;
    min-height: 80px;
  }
  .tweet-preview .tweet-author {
    color: var(--blue);
    font-weight: 700;
    margin-bottom: 0.3rem;
    font-size: 0.82rem;
  }

  /* ── カードボディ ── */
  .card-body { padding: 0.8rem; flex: 1; display: flex; flex-direction: column; gap: 0.45rem; position: relative; z-index: 1; }
  .card-title a {
    font-weight: 700;
    font-size: 0.9rem;
    color: var(--blue);
    text-decoration: none;
    line-height: 1.45;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    transition: color 0.15s;
  }
  .card-title a:hover { color: var(--gold); text-decoration: none; }
  .card-summary {
    font-size: 0.8rem;
    color: #7a8fb0;
    line-height: 1.6;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
    flex: 1;
  }
  .card-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 0.5rem;
    padding-top: 0.5rem;
    border-top: 1px solid var(--border);
    font-size: 0.73rem;
    color: var(--text-muted);
  }

  /* ── バッジ ── */
  .media-badge {
    font-size: 0.68rem;
    padding: 0.15rem 0.5rem;
    border-radius: 4px;
    font-weight: 800;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }
  .badge-youtube { background: #7f0000; color: #ef9a9a; box-shadow: 0 0 8px rgba(183,28,28,0.4); }
  .badge-twitter { background: #0d2a5e; color: #90caf9; box-shadow: 0 0 8px rgba(13,71,161,0.4); }
  .badge-note    { background: #004d40; color: #80cbc4; box-shadow: 0 0 8px rgba(0,105,92,0.4); }
  .badge-other   { background: #1c2d4a; color: #6b8cbf; }
  .badge-qa      { background: #3e0d5e; color: #ce93d8; box-shadow: 0 0 8px rgba(100,28,130,0.4); }

  /* ── 削除ボタン ── */
  .btn-del {
    position: absolute;
    top: 0.5rem;
    right: 0.5rem;
    background: rgba(127,0,0,0.85);
    border: none;
    color: #fff;
    border-radius: 5px;
    padding: 0.2rem 0.55rem;
    font-size: 0.75rem;
    cursor: pointer;
    display: none;
    backdrop-filter: blur(4px);
    transition: background 0.15s, box-shadow 0.15s;
    z-index: 10;
  }
  .btn-del:hover { background: var(--red); box-shadow: var(--glow-red); }
  .token-set .btn-del { display: block; }

  /* ── その他 ── */
  .no-results { text-align: center; padding: 3rem; color: var(--text-muted); }

  /* ── モーダル ── */
  .modal-overlay {
    display: none;
    position: fixed; inset: 0;
    background: rgba(0,0,0,0.85);
    backdrop-filter: blur(6px);
    z-index: 1000;
    align-items: center;
    justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--surface-2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.75rem;
    width: 420px;
    max-width: 90vw;
    box-shadow: 0 24px 60px rgba(0,0,0,0.8), var(--glow-gold);
  }
  .modal h3 { margin: 0 0 0.75rem; color: var(--gold); font-size: 1rem; letter-spacing: 0.04em; text-shadow: var(--glow-gold); }
  .modal p  { font-size: 0.85rem; color: var(--text-muted); margin: 0 0 1rem; }
  .modal input {
    width: 100%; padding: 0.6rem 0.8rem; box-sizing: border-box;
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 6px; color: var(--text); font-size: 0.9rem; margin-bottom: 1rem;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .modal input:focus { outline: none; border-color: var(--gold); box-shadow: var(--glow-gold); }
  .modal-btns { display: flex; gap: 0.5rem; justify-content: flex-end; }
  .modal-btns button { padding: 0.45rem 1.1rem; border-radius: 6px; border: none; font-size: 0.9rem; cursor: pointer; font-weight: 600; transition: all 0.15s; }
  .btn-save-token { background: linear-gradient(135deg, var(--gold), var(--gold-light)); color: #07090f; }
  .btn-save-token:hover { box-shadow: var(--glow-gold); }
  .btn-cancel { background: var(--surface); color: var(--text-muted); border: 1px solid var(--border); }
  .btn-cancel:hover { border-color: var(--text-muted); color: var(--text); }

  /* ── タブバー ── */
  .tab-bar {
    display: flex;
    gap: 0;
    margin-bottom: 1.5rem;
    border-bottom: 1px solid var(--border);
  }
  .tab-btn {
    padding: 0.65rem 1.4rem;
    background: none;
    border: none;
    border-bottom: 2px solid transparent;
    color: var(--text-muted);
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.18s;
    margin-bottom: -1px;
    letter-spacing: 0.02em;
  }
  .tab-btn:hover { color: var(--text); }
  .tab-btn.active {
    color: var(--gold);
    border-bottom-color: var(--gold);
    text-shadow: var(--glow-gold);
  }

  /* ── 攻略サイトグリッド ── */
  .sites-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.1rem;
    margin-top: 0.5rem;
  }
  .site-card {
    display: flex;
    flex-direction: column;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    text-decoration: none;
    overflow: hidden;
    transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
    position: relative;
  }
  .site-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(200,155,60,0.05), transparent 60%);
    pointer-events: none;
    z-index: 0;
  }
  .site-card:hover {
    border-color: var(--gold);
    box-shadow: var(--glow-gold), 0 8px 32px rgba(0,0,0,0.5);
    transform: translateY(-3px);
    text-decoration: none;
  }
  .site-banner {
    width: 100%;
    aspect-ratio: 16/7;
    object-fit: cover;
    background: var(--surface-2);
    display: block;
    transition: opacity 0.2s;
  }
  .site-card:hover .site-banner { opacity: 0.9; }
  .site-banner-placeholder {
    width: 100%;
    aspect-ratio: 16/7;
    background: var(--surface-2);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
  }
  .site-banner-placeholder img {
    width: 48px;
    height: 48px;
    border-radius: 8px;
  }
  .site-banner-placeholder span {
    font-size: 1.1rem;
    font-weight: 800;
    color: var(--text);
    letter-spacing: 0.04em;
  }
  .site-body {
    padding: 0.8rem 0.9rem;
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 0.35rem;
    position: relative;
    z-index: 1;
  }
  .site-header-row {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .site-favicon {
    width: 20px;
    height: 20px;
    border-radius: 4px;
    object-fit: cover;
    flex-shrink: 0;
  }
  .site-name {
    font-weight: 700;
    font-size: 0.95rem;
    color: var(--text);
  }
  .site-arrow {
    margin-left: auto;
    font-size: 1rem;
    color: var(--gold);
    opacity: 0.5;
    transition: opacity 0.2s, transform 0.2s;
  }
  .site-card:hover .site-arrow { opacity: 1; transform: translateX(3px); }
  .site-desc {
    font-size: 0.79rem;
    color: var(--text-muted);
    line-height: 1.5;
  }
  .site-tags {
    display: flex;
    gap: 0.35rem;
    flex-wrap: wrap;
    margin-top: 0.1rem;
  }
  .site-tag {
    font-size: 0.65rem;
    padding: 0.1rem 0.45rem;
    border-radius: 4px;
    font-weight: 700;
    letter-spacing: 0.04em;
    background: var(--surface-2);
    border: 1px solid var(--border);
    color: var(--text-muted);
  }
  .site-url {
    font-size: 0.71rem;
    color: var(--blue);
    opacity: 0.75;
    margin-top: 0.15rem;
  }
  .sites-section-title {
    font-size: 0.75rem;
    color: var(--text-muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin: 1.5rem 0 0.75rem;
    padding-bottom: 0.4rem;
    border-bottom: 1px solid var(--border);
  }
  .sites-section-title:first-child { margin-top: 0; }

  /* ── Tips タブ ── */
  .tips-subtab-bar {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 1.5rem;
    flex-wrap: wrap;
  }
  .tips-subtab-btn {
    padding: 0.45rem 1.1rem;
    border-radius: 20px;
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text-muted);
    font-size: 0.85rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.18s;
    letter-spacing: 0.02em;
  }
  .tips-subtab-btn:hover { border-color: var(--gold); color: var(--gold); }
  .tips-subtab-btn.active {
    background: linear-gradient(135deg, var(--gold), var(--gold-light));
    color: #07090f;
    border-color: transparent;
    box-shadow: var(--glow-gold);
  }
  .tips-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.1rem;
    margin-bottom: 1.5rem;
  }
  .tip-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 0.9rem 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    position: relative;
    transition: transform 0.2s, border-color 0.2s, box-shadow 0.2s;
  }
  .tip-card::before {
    content: '';
    position: absolute;
    inset: 0;
    border-radius: 10px;
    background: linear-gradient(135deg, rgba(200,155,60,0.05), transparent 60%);
    pointer-events: none;
  }
  .tip-card:hover {
    border-color: var(--gold);
    box-shadow: var(--glow-gold), 0 8px 32px rgba(0,0,0,0.5);
    transform: translateY(-3px);
  }
  .tip-tags { display: flex; gap: 0.4rem; flex-wrap: wrap; }
  .tip-tag {
    font-size: 0.68rem;
    padding: 0.15rem 0.5rem;
    border-radius: 4px;
    font-weight: 800;
    letter-spacing: 0.04em;
  }
  .tip-tag-s      { background: rgba(229,57,53,0.15);  color: #ef9a9a; box-shadow: 0 0 8px rgba(229,57,53,0.2); }
  .tip-tag-a      { background: rgba(200,155,60,0.15); color: #e8c97a; }
  .tip-tag-b      { background: rgba(77,168,255,0.15); color: #90caf9; }
  .tip-tag-source { background: rgba(167,139,250,0.15); color: #ce93d8; }
  .tip-title      { font-weight: 700; font-size: 0.92rem; color: var(--text); line-height: 1.4; }
  .tip-body       { font-size: 0.8rem; color: #7a8fb0; line-height: 1.65; flex: 1; white-space: pre-wrap; }
  .tip-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-top: 0.5rem;
    border-top: 1px solid var(--border);
    font-size: 0.72rem;
    color: var(--text-muted);
  }
  .tip-del {
    background: none;
    border: none;
    color: var(--text-muted);
    cursor: pointer;
    font-size: 0.78rem;
    padding: 0.1rem 0.4rem;
    border-radius: 4px;
    transition: color 0.15s;
  }
  .tip-del:hover { color: var(--red); }

  .tips-add-form {
    background: var(--surface);
    border: 1px dashed var(--border);
    border-radius: 10px;
    padding: 1.1rem 1.2rem;
  }
  .tips-add-form h4 {
    font-size: 0.82rem;
    color: var(--text-muted);
    margin-bottom: 0.9rem;
    letter-spacing: 0.06em;
    text-transform: uppercase;
  }
  .tips-form-row { display: flex; gap: 0.65rem; margin-bottom: 0.65rem; flex-wrap: wrap; }
  .tips-form-row input,
  .tips-form-row select,
  .tips-form-row textarea {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 6px;
    color: var(--text);
    padding: 0.5rem 0.75rem;
    font-size: 0.88rem;
    font-family: inherit;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .tips-form-row input::placeholder,
  .tips-form-row textarea::placeholder { color: var(--text-muted); }
  .tips-form-row input:focus,
  .tips-form-row select:focus,
  .tips-form-row textarea:focus { outline: none; border-color: var(--gold); box-shadow: var(--glow-gold); }
  .tips-form-row input.f-title { flex: 1; min-width: 180px; }
  .tips-form-row select { min-width: 120px; }
  .tips-form-row textarea { width: 100%; min-height: 76px; resize: vertical; }
  .tips-form-row select option { background: var(--surface); }
  .btn-tips-add {
    background: linear-gradient(135deg, var(--gold), var(--gold-light));
    color: #07090f;
    border: none;
    padding: 0.5rem 1.2rem;
    border-radius: 6px;
    font-weight: 700;
    font-size: 0.88rem;
    cursor: pointer;
    transition: box-shadow 0.2s;
    white-space: nowrap;
  }
  .btn-tips-add:hover { box-shadow: var(--glow-gold); }
  .tips-empty {
    text-align: center;
    padding: 2.5rem 1rem;
    color: var(--text-muted);
    font-size: 0.88rem;
    grid-column: 1 / -1;
  }
</style>

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | where_exp: "p", "p.media_type != 'qa'" | where_exp: "p", "p.media_type != 'tips'" | sort: "date" | reverse %}
{% assign tips_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/tips/'" | sort: "date" | reverse %}

<div class="tab-bar">
  <button class="tab-btn active" data-tab="knowledge">📚 ナレッジ</button>
  <button class="tab-btn" data-tab="tips">💡 Tips</button>
  <button class="tab-btn" data-tab="sites">🌐 攻略サイト</button>
</div>

<div id="tab-knowledge">
<div class="db-toolbar">
  <input type="text" id="search" placeholder="タイトル・要約・コメントを検索...">
  <span class="db-count" id="count"></span>
  <button class="btn-token" id="btn-token">🔑 削除モード</button>
</div>

<div class="tag-filters" id="tag-filters">
  <button class="tag-btn active" data-type="all">すべて</button>
  <button class="tag-btn" data-type="youtube">YouTube</button>
  <button class="tag-btn" data-type="twitter">X / Twitter</button>
  <button class="tag-btn" data-type="note">note</button>
  <button class="tag-btn" data-type="other">Web</button>
</div>

<div class="year-filters" id="year-filters"></div>
<div class="month-filters" id="month-filters"></div>

<div class="card-grid" id="card-grid">
{% for p in knowledge_pages %}
<div class="card"
  data-title="{{ p.title | default: p.name | downcase }}"
  data-summary="{{ p.summary | downcase }}"
  data-comment="{{ p.comment | downcase }}"
  data-media-type="{{ p.media_type | default: 'other' }}"
  data-date="{{ p.date | date: "%Y-%m" }}"
>
  <button class="btn-del" data-path="{{ p.path }}" data-title="{{ p.title | default: p.name }}">🗑 削除</button>

  {% if p.media_type == "youtube" and p.thumbnail_url and p.thumbnail_url != "" %}
    <a href="{{ p.url | relative_url }}">
      <img class="card-thumb" src="{{ p.thumbnail_url }}" alt="{{ p.title }}" loading="lazy"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="card-thumb-placeholder" style="display:none">▶️</div>
    </a>
  {% elsif p.media_type == "youtube" %}
    <a href="{{ p.url | relative_url }}"><div class="card-thumb-placeholder">▶️</div></a>
  {% elsif p.media_type == "twitter" %}
    {% if p.thumbnail_url and p.thumbnail_url != "" %}
      <a href="{{ p.source_url | default: p.url | relative_url }}" target="_blank" rel="noopener noreferrer">
        <img class="card-thumb" src="{{ p.thumbnail_url }}" alt="{{ p.title }}" loading="lazy"
             onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
        <div class="card-thumb-placeholder" style="display:none">🐦</div>
      </a>
    {% else %}
      <div class="tweet-preview">
        {% assign parts = p.title | split: ": " %}
        <div class="tweet-author">🐦 {{ parts[0] }}</div>
        <div>{{ p.summary | truncate: 140 }}</div>
      </div>
    {% endif %}
  {% elsif p.thumbnail_url and p.thumbnail_url != "" %}
    <a href="{{ p.source_url }}" target="_blank" rel="noopener noreferrer">
      <img class="card-thumb" src="{{ p.thumbnail_url }}" alt="{{ p.title }}" loading="lazy"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="card-thumb-placeholder" style="display:none">{% if p.media_type == "note" %}📝{% else %}📄{% endif %}</div>
    </a>
  {% else %}
    <a href="{{ p.source_url | default: p.url | relative_url }}" {% if p.source_url %}target="_blank" rel="noopener noreferrer"{% endif %}>
      <div class="card-thumb-placeholder">{% if p.media_type == "note" %}📝{% else %}📄{% endif %}</div>
    </a>
  {% endif %}

  <div class="card-body">
    <div class="card-title">
      {% if p.source_url %}
        <a href="{{ p.source_url }}" target="_blank" rel="noopener noreferrer">{{ p.title | default: p.name }}</a>
      {% else %}
        <a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a>
      {% endif %}
    </div>
    {% if p.media_type != "twitter" and p.summary and p.summary != "" %}
    <div class="card-summary">{{ p.summary }}</div>
    {% endif %}
    {% if p.comment and p.comment != "" %}
    <div style="font-size:0.78rem;color:var(--text-muted);font-style:italic">💬 {{ p.comment | truncate: 60 }}</div>
    {% endif %}
    <div class="card-footer">
      <span class="media-badge badge-{{ p.media_type | default: 'other' }}">
        {% if p.media_type == "youtube" %}YouTube
        {% elsif p.media_type == "twitter" %}X / Twitter
        {% elsif p.media_type == "note" %}note
        {% elsif p.media_type == "qa" %}Q&A
        {% else %}Web{% endif %}
      </span>
      <span>{{ p.date | date: "%Y-%m-%d" }}</span>
    </div>
  </div>
</div>
{% endfor %}
</div>
<p class="no-results" id="no-results" style="display:none">該当するナレッジが見つかりません。</p>
</div><!-- /tab-knowledge -->

<div id="tab-tips" style="display:none">
  <div class="tips-subtab-bar" id="tips-subtabs">
    <button class="tips-subtab-btn active" data-cat="all">すべて <span id="cnt-all"></span></button>
    <button class="tips-subtab-btn" data-cat="chara">⚔️ 強いキャラ <span id="cnt-chara"></span></button>
    <button class="tips-subtab-btn" data-cat="augment">💎 オーグメント <span id="cnt-augment"></span></button>
    <button class="tips-subtab-btn" data-cat="strategy">🧠 立ち回り <span id="cnt-strategy"></span></button>
    <button class="tips-subtab-btn" data-cat="comp">🏆 コンプ <span id="cnt-comp"></span></button>
  </div>

  <div class="tips-grid" id="tips-grid">
  {% if tips_pages.size == 0 %}
    <div class="tips-empty" style="grid-column:1/-1">
      まだTipsがありません。DiscordでBotに <code>@bot tip [内容]</code> と送ると自動で追加されます。
    </div>
  {% else %}
    {% for p in tips_pages %}
    <div class="tip-card"
      data-cat="{{ p.tips_category | default: 'strategy' }}"
      data-rank="{{ p.rank | default: 'A' }}"
    >
      <div class="tip-tags">
        {% assign rank = p.rank | default: 'A' %}
        {% assign cat  = p.tips_category | default: 'strategy' %}
        {% if rank == 'S' %}<span class="tip-tag tip-tag-s">
          {% if cat == 'chara' or cat == 'comp' %}S ランク{% elsif cat == 'augment' %}強い{% else %}重要{% endif %}
        </span>
        {% elsif rank == 'A' %}<span class="tip-tag tip-tag-a">
          {% if cat == 'chara' or cat == 'comp' %}A ランク{% elsif cat == 'augment' %}普通{% else %}参考{% endif %}
        </span>
        {% else %}<span class="tip-tag tip-tag-b">
          {% if cat == 'chara' or cat == 'comp' %}B ランク{% elsif cat == 'augment' %}弱い{% else %}メモ{% endif %}
        </span>{% endif %}
        {% if p.source and p.source != "" %}
        <span class="tip-tag tip-tag-source">📡 {{ p.source }}</span>
        {% endif %}
        {% if p.discord_user and p.discord_user != "" %}
        <span class="tip-tag" style="background:rgba(77,168,255,0.1);color:#4da8ff">🎮 {{ p.discord_user }}</span>
        {% endif %}
      </div>
      <div class="tip-title">{{ p.title }}</div>
      <div class="tip-body">{{ p.body | newline_to_br }}</div>
      <div class="tip-footer">
        <span>{{ p.date | date: "%Y-%m-%d" }}</span>
        <span style="font-size:0.7rem;color:var(--text-muted)">
          {% if cat == 'chara' %}⚔️ 強いキャラ
          {% elsif cat == 'augment' %}💎 オーグメント
          {% elsif cat == 'strategy' %}🧠 立ち回り
          {% elsif cat == 'comp' %}🏆 コンプ
          {% endif %}
        </span>
      </div>
    </div>
    {% endfor %}
  {% endif %}
  </div>

  <div class="tips-add-form" style="margin-top:1.5rem">
    <h4>💬 Discord から追加する方法</h4>
    <div style="font-size:0.85rem;color:var(--text-muted);line-height:1.8;padding:0.5rem 0">
      TFT Bot に <strong style="color:var(--text)">@bot tip [内容]</strong> と送るだけで自動で保存されます。<br>
      <code style="background:var(--bg);padding:0.15rem 0.5rem;border-radius:4px;font-size:0.82rem">tip ジンはSランク。スペルシェイパー2との相性が抜群</code><br>
      <code style="background:var(--bg);padding:0.15rem 0.5rem;border-radius:4px;font-size:0.82rem">tip 8-1ロールダウンはHP80以上の時だけ行う</code>
    </div>
  </div>
</div><!-- /tab-tips -->

<div id="tab-sites" style="display:none">
  <p class="sites-section-title">📊 メタ・統計サイト</p>
  <div class="sites-grid">

    <a class="site-card" href="https://www.metatft.com/" target="_blank" rel="noopener noreferrer">
      <img class="site-banner"
           src="https://www.metatft.com/MetaTFTLogo.png"
           alt="MetaTFT"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="site-banner-placeholder" style="display:none">
        <img src="https://www.google.com/s2/favicons?domain=metatft.com&sz=64" alt="">
        <span>MetaTFT</span>
      </div>
      <div class="site-body">
        <div class="site-header-row">
          <img class="site-favicon" src="https://www.google.com/s2/favicons?domain=metatft.com&sz=32" alt="">
          <span class="site-name">MetaTFT</span>
          <span class="site-arrow">→</span>
        </div>
        <div class="site-desc">メタコンプ・アイテム統計・ランキング。現パッチで最も強い構成を数値で確認できる</div>
        <div class="site-tags">
          <span class="site-tag">メタ統計</span>
          <span class="site-tag">ランキング</span>
          <span class="site-tag">アイテム</span>
        </div>
        <div class="site-url">metatft.com</div>
      </div>
    </a>

    <a class="site-card" href="https://tactics.tools/ja" target="_blank" rel="noopener noreferrer">
      <div class="site-banner-placeholder">
        <img src="https://www.google.com/s2/favicons?domain=tactics.tools&sz=64" alt="">
        <span>Tactics.tools</span>
      </div>
      <div class="site-body">
        <div class="site-header-row">
          <img class="site-favicon" src="https://www.google.com/s2/favicons?domain=tactics.tools&sz=32" alt="">
          <span class="site-name">Tactics.tools</span>
          <span class="site-arrow">→</span>
        </div>
        <div class="site-desc">コンプビルダー・勝率統計・プレイヤー検索。日本語完全対応</div>
        <div class="site-tags">
          <span class="site-tag">ビルダー</span>
          <span class="site-tag">勝率統計</span>
          <span class="site-tag">日本語</span>
        </div>
        <div class="site-url">tactics.tools/ja</div>
      </div>
    </a>

    <a class="site-card" href="https://op.gg/ja/tft" target="_blank" rel="noopener noreferrer">
      <img class="site-banner"
           src="https://c-tft-web.op.gg/images/Opengraph.png"
           alt="OP.GG TFT"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="site-banner-placeholder" style="display:none">
        <img src="https://www.google.com/s2/favicons?domain=op.gg&sz=64" alt="">
        <span>OP.GG TFT</span>
      </div>
      <div class="site-body">
        <div class="site-header-row">
          <img class="site-favicon" src="https://www.google.com/s2/favicons?domain=op.gg&sz=32" alt="">
          <span class="site-name">OP.GG TFT</span>
          <span class="site-arrow">→</span>
        </div>
        <div class="site-desc">プレイヤー統計・ランキング・チャンピオン別情報。日本語対応</div>
        <div class="site-tags">
          <span class="site-tag">プレイヤー検索</span>
          <span class="site-tag">ランキング</span>
          <span class="site-tag">日本語</span>
        </div>
        <div class="site-url">op.gg/ja/tft</div>
      </div>
    </a>

  </div>

  <p class="sites-section-title">📖 攻略・ガイドサイト</p>
  <div class="sites-grid">

    <a class="site-card" href="https://tftips.app/" target="_blank" rel="noopener noreferrer">
      <img class="site-banner"
           src="https://tftips.b-cdn.net/site/logo.webp"
           alt="TFTips"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="site-banner-placeholder" style="display:none">
        <img src="https://www.google.com/s2/favicons?domain=tftips.app&sz=64" alt="">
        <span>TFTips</span>
      </div>
      <div class="site-body">
        <div class="site-header-row">
          <img class="site-favicon" src="https://www.google.com/s2/favicons?domain=tftips.app&sz=32" alt="">
          <span class="site-name">TFTips</span>
          <span class="site-arrow">→</span>
        </div>
        <div class="site-desc">構成ガイド・アイテムチートシート・パッチ別攻略情報。初心者にも見やすい</div>
        <div class="site-tags">
          <span class="site-tag">ガイド</span>
          <span class="site-tag">チートシート</span>
          <span class="site-tag">構成</span>
        </div>
        <div class="site-url">tftips.app</div>
      </div>
    </a>


    <a class="site-card" href="https://tftacademy.com/" target="_blank" rel="noopener noreferrer">
      <img class="site-banner"
           src="https://tftacademy.com/og-image.png"
           alt="TFT Academy"
           onerror="this.style.display='none';this.nextElementSibling.style.display='flex'">
      <div class="site-banner-placeholder" style="display:none">
        <img src="https://www.google.com/s2/favicons?domain=tftacademy.com&sz=64" alt="">
        <span>TFT Academy</span>
      </div>
      <div class="site-body">
        <div class="site-header-row">
          <img class="site-favicon" src="https://www.google.com/s2/favicons?domain=tftacademy.com&sz=32" alt="">
          <span class="site-name">TFT Academy</span>
          <span class="site-arrow">→</span>
        </div>
        <div class="site-desc">構成ティアリスト・リロール構成ガイドを詳しく解説。Set17対応</div>
        <div class="site-tags">
          <span class="site-tag">ティアリスト</span>
          <span class="site-tag">構成</span>
          <span class="site-tag">ガイド</span>
        </div>
        <div class="site-url">tftacademy.com</div>
      </div>
    </a>

  </div>
</div><!-- /tab-sites -->

<div class="modal-overlay" id="modal">
  <div class="modal">
    <h3>GitHub トークンを入力</h3>
    <p>このブラウザにのみ保存されます。Contents の write 権限があるトークンを使用してください。</p>
    <input type="password" id="token-input" placeholder="ghp_xxxxxxxxxxxx">
    <div class="modal-btns">
      <button class="btn-cancel" id="btn-cancel">キャンセル</button>
      <button class="btn-save-token" id="btn-save-token">保存して有効化</button>
    </div>
  </div>
</div>

<script>
(function () {
  const REPO = 'RomanticOrange-RRR/TFT-knowledge-base';
  const BRANCH = 'main';
  const TOKEN_KEY = 'gh_delete_token';

  const search    = document.getElementById('search');
  const count     = document.getElementById('count');
  const noResults = document.getElementById('no-results');
  const grid      = document.getElementById('card-grid');
  const modal     = document.getElementById('modal');
  const tokenInput= document.getElementById('token-input');
  const btnToken  = document.getElementById('btn-token');

  let cards = Array.from(grid.querySelectorAll('.card'));
  let activeType  = 'all';
  let activeYear  = 'all';
  let activeMonth = 'all';

  function getToken() { return localStorage.getItem(TOKEN_KEY); }

  function updateBadges() {
    const counts = { all: 0, youtube: 0, twitter: 0, note: 0, other: 0 };
    cards.forEach(c => {
      const t = c.dataset.mediaType || 'other';
      counts.all++;
      if (t in counts) counts[t]++;
      else counts.other++;
    });
    document.querySelector('[data-type="all"]').textContent     = `すべて (${counts.all})`;
    document.querySelector('[data-type="youtube"]').textContent = `YouTube (${counts.youtube})`;
    document.querySelector('[data-type="twitter"]').textContent = `X / Twitter (${counts.twitter})`;
    document.querySelector('[data-type="note"]').textContent    = `note (${counts.note})`;
    document.querySelector('[data-type="other"]').textContent   = `Web (${counts.other})`;
  }

  function buildYearFilter() {
    const years = [...new Set(
      cards.map(c => (c.dataset.date || '').split('-')[0]).filter(Boolean)
    )].sort().reverse();
    const container = document.getElementById('year-filters');
    if (years.length === 0) return;

    const label = document.createElement('span');
    label.className = 'filter-label';
    label.textContent = '年';
    container.appendChild(label);

    const allBtn = document.createElement('button');
    allBtn.className = 'year-btn active';
    allBtn.dataset.year = 'all';
    allBtn.textContent = 'すべて';
    container.appendChild(allBtn);

    years.forEach(y => {
      const btn = document.createElement('button');
      btn.className = 'year-btn';
      btn.dataset.year = y;
      btn.textContent = `${y}年`;
      container.appendChild(btn);
    });

    container.addEventListener('click', e => {
      const btn = e.target.closest('.year-btn');
      if (!btn) return;
      document.querySelectorAll('.year-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      activeYear  = btn.dataset.year;
      activeMonth = 'all';
      rebuildMonthFilter();
      filter();
    });
  }

  function rebuildMonthFilter() {
    const container = document.getElementById('month-filters');
    container.innerHTML = '';

    const months = [...new Set(
      cards
        .map(c => c.dataset.date)
        .filter(d => d && (activeYear === 'all' || d.startsWith(activeYear)))
    )].sort().reverse();

    if (months.length === 0) return;

    const label = document.createElement('span');
    label.className = 'filter-label';
    label.textContent = '月';
    container.appendChild(label);

    const allBtn = document.createElement('button');
    allBtn.className = 'month-btn active';
    allBtn.dataset.month = 'all';
    allBtn.textContent = 'すべて';
    container.appendChild(allBtn);

    months.forEach(m => {
      const btn = document.createElement('button');
      btn.className = 'month-btn';
      btn.dataset.month = m;
      const [, mo] = m.split('-');
      btn.textContent = `${parseInt(mo)}月`;
      container.appendChild(btn);
    });

    container.addEventListener('click', e => {
      const btn = e.target.closest('.month-btn');
      if (!btn) return;
      document.querySelectorAll('.month-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      activeMonth = btn.dataset.month;
      filter();
    });
  }

  function applyTokenMode() {
    if (getToken()) {
      grid.classList.add('token-set');
      btnToken.textContent = '🔑 削除モード ON';
      btnToken.style.borderColor = 'var(--gold)';
      btnToken.style.color = 'var(--gold)';
    } else {
      grid.classList.remove('token-set');
      btnToken.textContent = '🔑 削除モード';
      btnToken.style.borderColor = '';
      btnToken.style.color = '';
    }
  }

  btnToken.addEventListener('click', () => {
    if (getToken()) {
      if (confirm('削除モードを解除しますか？')) { localStorage.removeItem(TOKEN_KEY); applyTokenMode(); }
    } else {
      tokenInput.value = '';
      modal.classList.add('open');
      tokenInput.focus();
    }
  });
  document.getElementById('btn-cancel').addEventListener('click', () => modal.classList.remove('open'));
  document.getElementById('btn-save-token').addEventListener('click', () => {
    const t = tokenInput.value.trim();
    if (!t) return alert('トークンを入力してください');
    localStorage.setItem(TOKEN_KEY, t);
    modal.classList.remove('open');
    applyTokenMode();
  });

  document.getElementById('tag-filters').addEventListener('click', (e) => {
    const btn = e.target.closest('.tag-btn');
    if (!btn) return;
    document.querySelectorAll('.tag-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    activeType = btn.dataset.type;
    filter();
  });

  grid.addEventListener('click', async (e) => {
    const btn = e.target.closest('.btn-del');
    if (!btn) return;
    const token = getToken();
    if (!token) return;
    const path  = btn.dataset.path;
    const title = btn.dataset.title;
    const card  = btn.closest('.card');
    if (!confirm(`「${title}」を削除しますか？`)) return;
    btn.textContent = '削除中...';
    btn.disabled = true;
    try {
      const metaRes = await fetch(
        `https://api.github.com/repos/${REPO}/contents/${path}?ref=${BRANCH}`,
        { headers: { Authorization: `token ${token}`, Accept: 'application/vnd.github+json' } }
      );
      if (!metaRes.ok) throw new Error(`SHA取得失敗: ${metaRes.status}`);
      const meta = await metaRes.json();
      const delRes = await fetch(
        `https://api.github.com/repos/${REPO}/contents/${path}`,
        {
          method: 'DELETE',
          headers: { Authorization: `token ${token}`, Accept: 'application/vnd.github+json', 'Content-Type': 'application/json' },
          body: JSON.stringify({ message: `delete: ${path}`, sha: meta.sha, branch: BRANCH })
        }
      );
      if (!delRes.ok) throw new Error(`削除失敗: ${delRes.status}`);
      card.style.transition = 'opacity 0.3s';
      card.style.opacity = '0';
      setTimeout(() => { card.remove(); cards = cards.filter(c => c !== card); filter(); }, 300);
    } catch (err) {
      alert(`エラー: ${err.message}`);
      btn.textContent = '🗑 削除';
      btn.disabled = false;
    }
  });

  function filter() {
    const q = search.value.toLowerCase();
    let visible = 0;
    cards.forEach(c => {
      const typeMatch  = activeType  === 'all' || c.dataset.mediaType === activeType;
      const yearMatch  = activeYear  === 'all' || (c.dataset.date || '').startsWith(activeYear);
      const monthMatch = activeMonth === 'all' || c.dataset.date === activeMonth;
      const textMatch  = !q || c.dataset.title.includes(q) || c.dataset.summary.includes(q) || c.dataset.comment.includes(q);
      const match = typeMatch && yearMatch && monthMatch && textMatch;
      c.style.display = match ? '' : 'none';
      if (match) visible++;
    });
    count.textContent = `${visible} / ${cards.length} 件`;
    noResults.style.display = visible === 0 ? '' : 'none';
  }

  search.addEventListener('input', filter);
  applyTokenMode();
  updateBadges();
  buildYearFilter();
  rebuildMonthFilter();
  filter();

  // タブ切り替え
  document.querySelector('.tab-bar').addEventListener('click', e => {
    const btn = e.target.closest('.tab-btn');
    if (!btn) return;
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    const tab = btn.dataset.tab;
    document.getElementById('tab-knowledge').style.display = tab === 'knowledge' ? '' : 'none';
    document.getElementById('tab-tips').style.display      = tab === 'tips'      ? '' : 'none';
    document.getElementById('tab-sites').style.display     = tab === 'sites'     ? '' : 'none';
  });
})();

// ── Tips サブタブフィルタリング ──
(function () {
  const grid  = document.getElementById('tips-grid');
  if (!grid) return;

  const cards = Array.from(grid.querySelectorAll('.tip-card'));

  function updateCounts() {
    const totals = { all: cards.length, chara: 0, augment: 0, strategy: 0, comp: 0 };
    cards.forEach(c => { const cat = c.dataset.cat; if (cat in totals) totals[cat]++; });
    Object.keys(totals).forEach(k => {
      const el = document.getElementById('cnt-' + k);
      if (el) el.textContent = '(' + totals[k] + ')';
    });
  }

  function filterByCat(cat) {
    cards.forEach(c => {
      c.style.display = (cat === 'all' || c.dataset.cat === cat) ? '' : 'none';
    });
  }

  document.getElementById('tips-subtabs').addEventListener('click', e => {
    const btn = e.target.closest('.tips-subtab-btn');
    if (!btn) return;
    document.querySelectorAll('.tips-subtab-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    filterByCat(btn.dataset.cat);
  });

  updateCounts();
})();
</script>
