---
title: "HTTP 웹 기본 지식"
permalink: categories/HTTP 웹 기본 지식
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---

{% assign posts = site.categories['HTTP 웹 기본 지식']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
