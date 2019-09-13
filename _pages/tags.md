---
layout: archive
title: "Tags"
permalink: /tags/
author_profile: true
---

{% include group-by-array collection=site.posts field="tags" %}

<ul>
{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
	<li><a href="#{{ tag }}">{{ tag }} ({{ posts.size }})</a></li>
{% endfor %}
</ul>



{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
<h2 id="{{ tag | slugify }}">{{ tag }}</h2>
<ul> 
	{% for post in posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
{% endfor %}