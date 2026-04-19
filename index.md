---
layout: default
title: TFT ナレッジベース
---
<style>
  .db-toolbar {
    display: flex;
    gap: 0.75rem;
    margin-bottom: 1rem;
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
    padding: 0.4rem 0.8rem;
    border: 1px solid #888;
    border-radius: 4px;
    background: #2a2a2a;
    color: #ccc;
    font-size: 0.85rem;
    cursor: pointer;
  }
  .btn-token:hover { background: #3a3a3a; }

  .db-table { width: 100%; border-collapse: collapse; font-size: 0.9rem; table-layout: fixed; }
  .db-table col.col-title   { width: 20%; }
  .db-table col.col-summary { width: 35%; }
  .db-table col.col-comment { width: 20%; }
  .db-table col.col-url     { width: 17%; }
  .db-table col.col-del     { width: 8%; }
  .db-table th {
    text-align: left;
    padding: 0.6rem 0.75rem;
    background: #333;
    color: #f0f0f0;
    border-bottom: 2px solid #e8a87c;
    cursor: pointer;
    user-select: none;
    white-space: nowrap;
    font-size: 0.92rem;
    font-weight: 700;
  }
  .db-table th.no-sort { cursor: default; }
  .db-table th:not(.no-sort):hover { background: #444; }
  .db-table th .sort-icon { margin-left: 4px; color: #e8a87c; font-size: 0.8rem; }
  .db-table td {
    padding: 0.55rem 0.75rem;
    border-bottom: 1px solid #3a3a3a;
    vertical-align: top;
    overflow: hidden;
    word-break: break-word;
  }
  .db-table tr:hover td { background: rgba(255,255,255,0.05); }
  .db-table td.title-cell a { font-weight: 600; text-decoration: none; color: #7eb8f7; }
  .db-table td.title-cell a:hover { text-decoration: underline; }
  .db-table td.summary-cell { color: #ddd; font-size: 0.85rem; }
  .db-table td.comment-cell { color: #ccc; font-size: 0.85rem; }
  .db-table td.url-cell { font-size: 0.82rem; }
  .db-table td.url-cell a { color: #e8a87c; word-break: break-all; }
  .db-table td.del-cell { text-align: center; }
  .btn-del {
    background: none;
    border: 1px solid #c0392b;
    color: #e74c3c;
    border-radius: 4px;
    padding: 0.2rem 0.5rem;
    font-size: 0.8rem;
    cursor: pointer;
    display: none;
  }
  .btn-del:hover { background: #c0392b; color: #fff; }
  .token-set .btn-del { display: inline-block; }
  .no-results { text-align: center; padding: 2rem; color: #888; }

  /* トークン入力モーダル */
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
  .modal p { font-size: 0.85rem; color: #aaa; margin: 0 0 1rem; }
  .modal input {
    width: 100%; padding: 0.5rem; box-sizing: border-box;
    background: #1e1e1e; border: 1px solid #666;
    border-radius: 4px; color: #f0f0f0; font-size: 0.9rem;
    margin-bottom: 1rem;
  }
  .modal-btns { display: flex; gap: 0.5rem; justify-content: flex-end; }
  .modal-btns button {
    padding: 0.4rem 1rem; border-radius: 4px; border: none;
    font-size: 0.9rem; cursor: pointer;
  }
  .btn-save-token { background: #e8a87c; color: #111; font-weight: 700; }
  .btn-cancel { background: #444; color: #ccc; }
  .btn-clear-token { background: none; border: 1px solid #c0392b !important; color: #e74c3c; font-size: 0.78rem; padding: 0.3rem 0.6rem; border-radius: 4px; cursor: pointer; }
</style>

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}

<div class="db-toolbar">
  <input type="text" id="search" placeholder="タイトル・要約・コメントを検索...">
  <span class="db-count" id="count"></span>
  <button class="btn-token" id="btn-token">🔑 削除モード</button>
</div>

<table class="db-table" id="db-table">
  <colgroup>
    <col class="col-title">
    <col class="col-summary">
    <col class="col-comment">
    <col class="col-url">
    <col class="col-del">
  </colgroup>
  <thead>
    <tr>
      <th data-col="title">タイトル <span class="sort-icon">↕</span></th>
      <th data-col="summary">要約 <span class="sort-icon">↕</span></th>
      <th data-col="comment">コメント <span class="sort-icon">↕</span></th>
      <th data-col="url">URL <span class="sort-icon">↕</span></th>
      <th class="no-sort"></th>
    </tr>
  </thead>
  <tbody id="db-body">
    {% for p in knowledge_pages %}
    <tr
      data-title="{{ p.title | default: p.name | downcase }}"
      data-summary="{{ p.summary | downcase }}"
      data-comment="{{ p.comment | downcase }}"
      data-url="{{ p.source_url | downcase }}"
      data-path="{{ p.path }}"
    >
      <td class="title-cell"><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></td>
      <td class="summary-cell">{{ p.summary | truncate: 120 }}</td>
      <td class="comment-cell">{{ p.comment | truncate: 60 }}</td>
      <td class="url-cell">{% if p.source_url %}<a href="{{ p.source_url }}" target="_blank" rel="noopener">{{ p.source_url | truncate: 50 }}</a>{% endif %}</td>
      <td class="del-cell"><button class="btn-del" data-path="{{ p.path }}">削除</button></td>
    </tr>
    {% endfor %}
  </tbody>
</table>
<p class="no-results" id="no-results" style="display:none">該当するナレッジが見つかりません。</p>

<!-- トークン入力モーダル -->
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

  const search = document.getElementById('search');
  const count = document.getElementById('count');
  const noResults = document.getElementById('no-results');
  const tbody = document.getElementById('db-body');
  const modal = document.getElementById('modal');
  const tokenInput = document.getElementById('token-input');
  const btnToken = document.getElementById('btn-token');
  const table = document.getElementById('db-table');

  let rows = Array.from(tbody.querySelectorAll('tr'));
  let sortCol = null, sortAsc = true;

  // --- トークン管理 ---
  function getToken() { return localStorage.getItem(TOKEN_KEY); }

  function applyTokenMode() {
    if (getToken()) {
      table.classList.add('token-set');
      btnToken.textContent = '🔑 削除モード ON';
      btnToken.style.borderColor = '#e8a87c';
      btnToken.style.color = '#e8a87c';
    } else {
      table.classList.remove('token-set');
      btnToken.textContent = '🔑 削除モード';
      btnToken.style.borderColor = '';
      btnToken.style.color = '';
    }
  }

  btnToken.addEventListener('click', () => {
    if (getToken()) {
      if (confirm('削除モードを解除しますか？（トークンを削除します）')) {
        localStorage.removeItem(TOKEN_KEY);
        applyTokenMode();
      }
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

  // --- 削除処理 ---
  tbody.addEventListener('click', async (e) => {
    const btn = e.target.closest('.btn-del');
    if (!btn) return;
    const token = getToken();
    if (!token) return;

    const path = btn.dataset.path;
    const row = btn.closest('tr');
    const title = row.querySelector('.title-cell a').textContent;

    if (!confirm(`「${title}」を削除しますか？`)) return;

    btn.textContent = '削除中...';
    btn.disabled = true;

    try {
      // SHA取得
      const metaRes = await fetch(
        `https://api.github.com/repos/${REPO}/contents/${path}?ref=${BRANCH}`,
        { headers: { Authorization: `token ${token}`, Accept: 'application/vnd.github+json' } }
      );
      if (!metaRes.ok) throw new Error(`SHA取得失敗: ${metaRes.status}`);
      const meta = await metaRes.json();

      // 削除
      const delRes = await fetch(
        `https://api.github.com/repos/${REPO}/contents/${path}`,
        {
          method: 'DELETE',
          headers: { Authorization: `token ${token}`, Accept: 'application/vnd.github+json', 'Content-Type': 'application/json' },
          body: JSON.stringify({ message: `delete: ${path}`, sha: meta.sha, branch: BRANCH })
        }
      );
      if (!delRes.ok) throw new Error(`削除失敗: ${delRes.status}`);

      row.style.transition = 'opacity 0.3s';
      row.style.opacity = '0';
      setTimeout(() => {
        row.remove();
        rows = rows.filter(r => r !== row);
        filter();
      }, 300);
    } catch (err) {
      alert(`エラー: ${err.message}`);
      btn.textContent = '削除';
      btn.disabled = false;
    }
  });

  // --- 検索 ---
  function filter() {
    const q = search.value.toLowerCase();
    let visible = 0;
    rows.forEach(r => {
      const match = !q ||
        r.dataset.title.includes(q) ||
        r.dataset.summary.includes(q) ||
        r.dataset.comment.includes(q);
      r.style.display = match ? '' : 'none';
      if (match) visible++;
    });
    count.textContent = `${visible} / ${rows.length} 件`;
    noResults.style.display = visible === 0 ? '' : 'none';
  }

  // --- ソート ---
  function sort(col) {
    if (sortCol === col) { sortAsc = !sortAsc; } else { sortCol = col; sortAsc = true; }
    rows.sort((a, b) => {
      const va = a.dataset[col] || '';
      const vb = b.dataset[col] || '';
      return sortAsc ? va.localeCompare(vb) : vb.localeCompare(va);
    });
    rows.forEach(r => tbody.appendChild(r));
    document.querySelectorAll('.db-table th[data-col]').forEach(th => {
      const icon = th.querySelector('.sort-icon');
      if (th.dataset.col === col) icon.textContent = sortAsc ? '↑' : '↓';
      else icon.textContent = '↕';
    });
    filter();
  }

  document.querySelectorAll('.db-table th[data-col]').forEach(th => {
    th.addEventListener('click', () => sort(th.dataset.col));
  });
  search.addEventListener('input', filter);

  applyTokenMode();
  filter();
})();
</script>
