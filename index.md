---
layout: default
title: TFT ナレッジベース
---
<style>
  .knowledge-list { list-style: none; padding: 0; margin: 0; }
  .knowledge-item {
    padding: 0.9rem 1rem;
    margin-bottom: 0.6rem;
    border-left: 4px solid #e8a87c;
    background: rgba(255,255,255,0.04);
  }
  .knowledge-item a { font-weight: bold; text-decoration: none; }
  .knowledge-item a:hover { text-decoration: underline; }
  .knowledge-meta-line {
    font-size: 0.82rem;
    color: #888;
    margin-top: 0.3rem;
  }
  .knowledge-meta-line span { margin-right: 1rem; }
</style>

Discordで収集したTFTに関するナレッジ集です。

## ナレッジ一覧

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}
{% if knowledge_pages.size > 0 %}
<ul class="knowledge-list">
{% for p in knowledge_pages %}
<li class="knowledge-item">
  <a href="{{ p.url | relative_url }}">{{ p.title | default: p.name }}</a>
  <div class="knowledge-meta-line">
    {% if p.date %}<span>📅 {{ p.date | date: "%Y-%m-%d" }}</span>{% endif %}
    {% if p.discord_channel %}<span>💬 #{{ p.discord_channel }}</span>{% endif %}
    {% if p.discord_user %}<span>👤 {{ p.discord_user }}</span>{% endif %}
    {% if p.comment and p.comment != "" %}<span>📝 {{ p.comment | truncate: 40 }}</span>{% endif %}
  </div>
</li>
{% endfor %}
</ul>
{% else %}
まだナレッジが登録されていません。Discordでメッセージに 📌 を付けて保存を開始してください。
{% endif %}
