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
    padding: 0.4rem 0.7rem;
    border: 1px solid #555;
    border-radius: 4px;
    background: #1e1e1e;
    color: inherit;
    font-size: 0.9rem;
    flex: 1;
    min-width: 180px;
  }
  .db-count { margin-left: auto; font-size: 0.85rem; color: #888; }

  .db-table { width: 100%; border-collapse: collapse; font-size: 0.9rem; table-layout: fixed; }
  .db-table col.col-title   { width: 22%; }
  .db-table col.col-summary { width: 38%; }
  .db-table col.col-comment { width: 22%; }
  .db-table col.col-url     { width: 18%; }
  .db-table th {
    text-align: left;
    padding: 0.55rem 0.75rem;
    background: #2a2a2a;
    border-bottom: 2px solid #e8a87c;
    cursor: pointer;
    user-select: none;
    white-space: nowrap;
  }
  .db-table th:hover { background: #333; }
  .db-table th .sort-icon { margin-left: 4px; color: #888; font-size: 0.75rem; }
  .db-table td {
    padding: 0.55rem 0.75rem;
    border-bottom: 1px solid #2e2e2e;
    vertical-align: top;
    overflow: hidden;
    word-break: break-word;
  }
  .db-table tr:hover td { background: rgba(255,255,255,0.03); }
  .db-table td.title-cell a { font-weight: 600; text-decoration: none; }
  .db-table td.title-cell a:hover { text-decoration: underline; }
  .db-table td.summary-cell { color: #ccc; font-size: 0.85rem; }
  .db-table td.comment-cell { color: #aaa; font-size: 0.85rem; }
  .db-table td.url-cell { font-size: 0.82rem; }
  .db-table td.url-cell a { color: #e8a87c; word-break: break-all; }
  .no-results { text-align: center; padding: 2rem; color: #888; }
</style>

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}

<div class="db-toolbar">
  <input type="text" id="search" placeholder="タイトル・要約・コメントを検索...">
  <span class="db-count" id="count"></span>
</div>

<table class="db-table" id="db-table">
  <colgroup>
    <col class="col-title">
    <col class="col-summary">
    <col class="col-comment">
    <col class="col-url">
  </colgroup>
  <thead>
    <tr>
      <th data-col="title">タイトル <span class="sort-icon">↕</span></th>
      <th data-col="summary">要約 <span class="sort-icon">↕</span></th>
      <th data-col="comment">コメント <span class="sort-icon">↕</span></th>
      <th data-col="url">URL <span class="sort-icon">↕</span></th>
    </tr>
  </thead>
  <tbody id="db-body">
    {% for p in knowledge_pages %}
    <tr
      data-title="{{ p.title | default: p.name | downcase }}"
      data-summary="{{ p.summary | downcase }}"
      data-comment="{{ p.comment | downcase }}"
      data-url="{{ p.source_url | downcase }}"
    >
      <td class="title-cell"><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></td>
      <td class="summary-cell">{{ p.summary | truncate: 120 }}</td>
      <td class="comment-cell">{{ p.comment | truncate: 60 }}</td>
      <td class="url-cell">{% if p.source_url %}<a href="{{ p.source_url }}" target="_blank" rel="noopener">{{ p.source_url | truncate: 50 }}</a>{% endif %}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>
<p class="no-results" id="no-results" style="display:none">該当するナレッジが見つかりません。</p>

<script>
(function () {
  const search = document.getElementById('search');
  const count = document.getElementById('count');
  const noResults = document.getElementById('no-results');
  const tbody = document.getElementById('db-body');
  const rows = Array.from(tbody.querySelectorAll('tr'));
  const total = rows.length;
  let sortCol = null, sortAsc = true;

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
    count.textContent = `${visible} / ${total} 件`;
    noResults.style.display = visible === 0 ? '' : 'none';
  }

  function sort(col) {
    if (sortCol === col) { sortAsc = !sortAsc; } else { sortCol = col; sortAsc = true; }
    rows.sort((a, b) => {
      const va = a.dataset[col] || '';
      const vb = b.dataset[col] || '';
      return sortAsc ? va.localeCompare(vb) : vb.localeCompare(va);
    });
    rows.forEach(r => tbody.appendChild(r));
    document.querySelectorAll('.db-table th').forEach(th => {
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
  filter();
})();
</script>
