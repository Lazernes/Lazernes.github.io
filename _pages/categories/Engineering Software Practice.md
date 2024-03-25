---
title: "Engineering Software Practice"
permalink: categories/Engineering Software Practice
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---

{% assign posts = site.categories['Engineering Software Practice']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
