---
title: "AIML"
permalink: categories/AIML
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---

{% assign posts = site.categories['AIML']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
