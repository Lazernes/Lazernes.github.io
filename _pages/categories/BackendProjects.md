---
title: "Backend Projects"
permalink: categories/BackendProjects
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---

{% assign posts = site.categories['BackendProjects']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
