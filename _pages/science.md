---
layout: archive
permalink: /science/
title: "Science"
author_profile: true
header:
  overlay_image: "images/header_image7.png"
---


{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}"
   class="archive__subtitle"><i>   Tag : {{ tag }}</i></h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
