---
layout: archive
title: "Latest Posts"
permalink: /posts/
author_profile: true
---

{% for post in site.posts %}
   {% include archive-single.html %}
  {% endfor %}
{% endfor %}