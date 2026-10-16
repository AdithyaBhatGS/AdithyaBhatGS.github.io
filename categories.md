---
layout: page
title: Categories
permalink: /categories/
---

<h2>All Categories</h2>
<ul>
  {% for category in site.categories %}
    <li>
      <a href="{{ site.baseurl }}/categories/{{ category | first | slugify }}/">
        {{ category | first }} ({{ category | last | size }})
      </a>
    </li>
  {% endfor %}
</ul>
