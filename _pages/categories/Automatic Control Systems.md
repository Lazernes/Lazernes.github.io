---
title: "Automatic Control Systems"
permalink: categories/Automatic Control Systems
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---

{% assign posts = site.categories['Automatic Control Systems']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
