---
title: "Software Engineering"
permalink: categories/Software Engineering
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---


{% assign posts = site.categories['Software Engineering']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
