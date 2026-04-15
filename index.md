---
layout: home
title: TFT ナレッジベース
---

# TFT ナレッジベース

Discordで収集したTFTに関するナレッジ集です。

## ナレッジ一覧

{% assign knowledge_pages = site.pages | where_exp: "p", "p.path contains 'knowledge/'" | sort: "date" | reverse %}
{% if knowledge_pages.size > 0 %}
{% for p in knowledge_pages %}
- [{{ p.title | default: p.name }}]({{ p.url }}) {% if p.date %} — {{ p.date | date: "%Y-%m-%d" }}{% endif %}
{% endfor %}
{% else %}
まだナレッジが登録されていません。Discordでメッセージに 📌 を付けて保存を開始してください。
{% endif %}
