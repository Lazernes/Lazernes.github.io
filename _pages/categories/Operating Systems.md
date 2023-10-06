---
title: "Operating Systems"
permalink: categories/Operating Systems
layout: archive # category
author_profile: true
sidebar:
  nav: "docs"
# types: posts
# taxononmy: Javascript
---


{% assign posts = site.categories['Operating Systems']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
