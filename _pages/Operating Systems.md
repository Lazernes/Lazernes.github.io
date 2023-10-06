---
title: "Operating Systems"
permalink: /categories/Operating Systems/
layout: archive # category
authoe_profile: true
sidebar_main: true
# types: posts
# taxononmy: Javascript
---

운영체제

{% assign posts = site.categories['Operating Systems']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
