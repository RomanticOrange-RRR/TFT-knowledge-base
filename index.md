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
  .db-toolbar input, .db-toolbar select {
    padding: 0.4rem 0.7rem;
    border: 1px solid #555;
    border-radius: 4px;
    background: #1e1e1e;
    color: inherit;
    font-size: 0.9rem;
  }
  .db-toolbar input { flex: 1; min-width: 180px; }
  .db-count { margin-left: auto; font-size: 0.85rem; color: #888; }

  .db-table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
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
  }
  .db-table tr:hover td { background: rgba(255,255,255,0.03); }
  .db-table td.title-cell a { font-weight: 500; text-decoration: none; }
  .db-table td.title-cell a:hover { text-decoration: underline; }
  .db-table td.comment-cell { color: #aaa; font-size: 0.85rem; max-width: 220px; }
  .db-table td.date-cell { white-space: nowrap; color: #999; font-size: 0.85rem; }
  .db-table td.ch-cell, .db-table td.user-cell { white-space: nowrap; font-size: 0.85rem; color: #bbb; }
  .tag-ch {
    display: inline-block;
    padding: 0.1rem 0.5rem;
    border-radius: 3px;
    background: rgba(232,168,124,0.15);
    color: #e8a87c;
    font-size: 0.8rem;
  }
  .no-results { text-align: center; padding: 2rem; color: #888; }
</style>

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}

<div class="db-toolbar">
  <input type="text" id="search" placeholder="タイトル・コメントを検索...">
  <select id="ch-filter">
    <option value="">すべてのチャンネル</option>
    {% assign channels = knowledge_pages | map: "discord_channel" | uniq | sort %}
    {% for ch in channels %}
    {% if ch and ch != "" %}<option value="{{ ch }}">#{{ ch }}</option>{% endif %}
    {% endfor %}
  </select>
  <span class="db-count" id="count"></span>
</div>

<table class="db-table" id="db-table">
  <thead>
    <tr>
      <th data-col="title">タイトル <span class="sort-icon">↕</span></th>
      <th data-col="date">日付 <span class="sort-icon">↕</span></th>
      <th data-col="channel">チャンネル <span class="sort-icon">↕</span></th>
      <th data-col="user">投稿者 <span class="sort-icon">↕</span></th>
      <th data-col="comment">コメント <span class="sort-icon">↕</span></th>
    </tr>
  </thead>
  <tbody id="db-body">
    {% for p in knowledge_pages %}
    <tr
      data-title="{{ p.title | default: p.name | downcase }}"
      data-date="{{ p.date | date: '%Y-%m-%d' }}"
      data-channel="{{ p.discord_channel }}"
      data-user="{{ p.discord_user }}"
      data-comment="{{ p.comment | downcase }}"
    >
      <td class="title-cell"><a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a></td>
      <td class="date-cell">{{ p.date | date: "%Y-%m-%d" }}</td>
      <td class="ch-cell">{% if p.discord_channel %}<span class="tag-ch">#{{ p.discord_channel }}</span>{% endif %}</td>
      <td class="user-cell">{{ p.discord_user }}</td>
      <td class="comment-cell">{{ p.comment | truncate: 60 }}</td>
    </tr>
    {% endfor %}
  </tbody>
</table>
<p class="no-results" id="no-results" style="display:none">該当するナレッジが見つかりません。</p>

<script>
(function () {
  const search = document.getElementById('search');
  const chFilter = document.getElementById('ch-filter');
  const count = document.getElementById('count');
  const noResults = document.getElementById('no-results');
  const tbody = document.getElementById('db-body');
  const rows = Array.from(tbody.querySelectorAll('tr'));
  const total = rows.length;
  let sortCol = 'date', sortAsc = false;

  function filter() {
    const q = search.value.toLowerCase();
    const ch = chFilter.value;
    let visible = 0;
    rows.forEach(r => {
      const matchText = !q || r.dataset.title.includes(q) || r.dataset.comment.includes(q);
      const matchCh = !ch || r.dataset.channel === ch;
      const show = matchText && matchCh;
      r.style.display = show ? '' : 'none';
      if (show) visible++;
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
  chFilter.addEventListener('change', filter);

  filter();
})();
</script>
