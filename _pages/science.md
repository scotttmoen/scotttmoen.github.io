---
layout: archive
permalink: /science/
title: "Science"
author_profile: true
header:
  overlay_image: "images/header_image7.png"
---
{% assign my_array = "" | split: ',' %}
{% for post in site.posts %}
  {% if post.categories contains "science" %}
     {% assign my_array = my_array | push: post %}
  {% endif %}
{% endfor %}


{% include group-by-array collection=my_array
 field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}"
   class="archive__subtitle"><i style="margin-left: 40px">Tag : {{ tag }}</i></h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
