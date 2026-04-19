---
layout: default
title: TFT ナレッジベース
---
<style>
  .db-toolbar {
    display: flex;
    gap: 0.75rem;
    margin-bottom: 1.25rem;
    flex-wrap: wrap;
    align-items: center;
  }
  .db-toolbar input {
    padding: 0.5rem 0.8rem;
    border: 1px solid #888;
    border-radius: 4px;
    background: #2a2a2a;
    color: #f0f0f0;
    font-size: 0.95rem;
    flex: 1;
    min-width: 180px;
  }
  .db-toolbar input::placeholder { color: #aaa; }
  .db-count { font-size: 0.9rem; color: #ccc; font-weight: 600; white-space: nowrap; }
  .btn-token {
    padding: 0.45rem 0.8rem;
    border: 1px solid #888;
    border-radius: 4px;
    background: #2a2a2a;
    color: #ccc;
    font-size: 0.85rem;
    cursor: pointer;
  }
  .btn-token:hover { background: #3a3a3a; }

  /* カードグリッド */
  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1rem;
  }
  .card {
    background: #1e1e1e;
    border: 1px solid #3a3a3a;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    transition: border-color 0.15s;
    position: relative;
  }
  .card:hover { border-color: #e8a87c; }

  /* サムネイル */
  .card-thumb {
    width: 100%;
    aspect-ratio: 16/9;
    object-fit: cover;
    background: #2a2a2a;
    display: block;
  }
  .card-thumb-placeholder {
    width: 100%;
    aspect-ratio: 16/9;
    background: #2a2a2a;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.5rem;
  }

  /* ツイートプレビュー */
  .tweet-preview {
    padding: 0.75rem;
    background: #1a1a2e;
    border-bottom: 1px solid #3a3a3a;
    font-size: 0.85rem;
    color: #ddd;
    line-height: 1.5;
    min-height: 80px;
  }
  .tweet-preview .tweet-author {
    color: #7eb8f7;
    font-weight: 700;
    margin-bottom: 0.3rem;
    font-size: 0.82rem;
  }

  .card-body { padding: 0.75rem; flex: 1; display: flex; flex-direction: column; gap: 0.4rem; }
  .card-title a {
    font-weight: 700;
    font-size: 0.9rem;
    color: #7eb8f7;
    text-decoration: none;
    line-height: 1.4;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .card-title a:hover { text-decoration: underline; }
  .card-summary {
    font-size: 0.8rem;
    color: #bbb;
    line-height: 1.5;
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
    border-top: 1px solid #2e2e2e;
    font-size: 0.75rem;
    color: #888;
  }
  .card-source a { color: #e8a87c; text-decoration: none; font-size: 0.75rem; }
  .card-source a:hover { text-decoration: underline; }
  .media-badge {
    font-size: 0.7rem;
    padding: 0.1rem 0.4rem;
    border-radius: 3px;
    font-weight: 700;
  }
  .badge-youtube { background: #c0392b; color: #fff; }
  .badge-twitter { background: #1a8cd8; color: #fff; }
  .badge-other   { background: #555; color: #ccc; }

  .btn-del {
    position: absolute;
    top: 0.4rem;
    right: 0.4rem;
    background: rgba(192,57,43,0.85);
    border: none;
    color: #fff;
    border-radius: 4px;
    padding: 0.2rem 0.5rem;
    font-size: 0.75rem;
    cursor: pointer;
    display: none;
  }
  .btn-del:hover { background: #c0392b; }
  .token-set .btn-del { display: block; }

  .no-results { text-align: center; padding: 3rem; color: #888; }

  .modal-overlay {
    display: none;
    position: fixed; inset: 0;
    background: rgba(0,0,0,0.7);
    z-index: 1000;
    align-items: center;
    justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: #2a2a2a;
    border: 1px solid #555;
    border-radius: 8px;
    padding: 1.5rem;
    width: 420px;
    max-width: 90vw;
  }
  .modal h3 { margin: 0 0 0.75rem; color: #f0f0f0; font-size: 1rem; }
  .modal p  { font-size: 0.85rem; color: #aaa; margin: 0 0 1rem; }
  .modal input {
    width: 100%; padding: 0.5rem; box-sizing: border-box;
    background: #1e1e1e; border: 1px solid #666;
    border-radius: 4px; color: #f0f0f0; font-size: 0.9rem; margin-bottom: 1rem;
  }
  .modal-btns { display: flex; gap: 0.5rem; justify-content: flex-end; }
  .modal-btns button { padding: 0.4rem 1rem; border-radius: 4px; border: none; font-size: 0.9rem; cursor: pointer; }
  .btn-save-token { background: #e8a87c; color: #111; font-weight: 700; }
  .btn-cancel { background: #444; color: #ccc; }
</style>

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}

<div class="db-toolbar">
  <input type="text" id="search" placeholder="タイトル・要約・コメントを検索...">
  <span class="db-count" id="count"></span>
  <button class="btn-token" id="btn-token">🔑 削除モード</button>
</div>

<div class="card-grid" id="card-grid">
{% for p in knowledge_pages %}
<div class="card"
  data-title="{{ p.title | default: p.name | downcase }}"
  data-summary="{{ p.summary | downcase }}"
  data-comment="{{ p.comment | downcase }}"
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
    <div class="tweet-preview">
      {% assign parts = p.title | split: ": " %}
      <div class="tweet-author">🐦 {{ parts[0] }}</div>
      <div>{{ p.summary | truncate: 140 }}</div>
    </div>
  {% else %}
    <a href="{{ p.url | relative_url }}"><div class="card-thumb-placeholder">📄</div></a>
  {% endif %}

  <div class="card-body">
    <div class="card-title"><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></div>
    {% if p.media_type != "twitter" and p.summary and p.summary != "" %}
    <div class="card-summary">{{ p.summary }}</div>
    {% endif %}
    {% if p.comment and p.comment != "" %}
    <div style="font-size:0.78rem;color:#aaa;font-style:italic">💬 {{ p.comment | truncate: 60 }}</div>
    {% endif %}
    <div class="card-footer">
      <span class="media-badge badge-{{ p.media_type | default: 'other' }}">
        {% if p.media_type == "youtube" %}YouTube
        {% elsif p.media_type == "twitter" %}X / Twitter
        {% else %}Web{% endif %}
      </span>
      <span>{{ p.date | date: "%Y-%m-%d" }}</span>
      {% if p.source_url %}<span class="card-source"><a href="{{ p.source_url }}" target="_blank" rel="noopener">元リンク →</a></span>{% endif %}
    </div>
  </div>
</div>
{% endfor %}
</div>
<p class="no-results" id="no-results" style="display:none">該当するナレッジが見つかりません。</p>

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

  function getToken() { return localStorage.getItem(TOKEN_KEY); }

  function applyTokenMode() {
    if (getToken()) {
      grid.classList.add('token-set');
      btnToken.textContent = '🔑 削除モード ON';
      btnToken.style.borderColor = '#e8a87c';
      btnToken.style.color = '#e8a87c';
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
      const match = !q || c.dataset.title.includes(q) || c.dataset.summary.includes(q) || c.dataset.comment.includes(q);
      c.style.display = match ? '' : 'none';
      if (match) visible++;
    });
    count.textContent = `${visible} / ${cards.length} 件`;
    noResults.style.display = visible === 0 ? '' : 'none';
  }

  search.addEventListener('input', filter);
  applyTokenMode();
  filter();
})();
</script>
