---
layout: archive
permalink: /code/
title: "Code"
author_profile: true
header:
  overlay_image: "/images/header_image2.png"
---

test19

{% for post in site.posts%}
  {% if post.category == "code" %}
    {% include group-by-array collection=site.posts field="tags" %}
  {% endif %}
{% endfor %}


{% for post in site.posts%}
  {% if post.category == "code" %}

    {% for tag in group_names %}
     {% assign posts = group_items[forloop.index0] %}
      <h2 id="{{ tag | slugify }}"
        class="archive__subtitle"><i style="margin-left: 40px">Tag : {{ tag }}</i></h2>
      {% for post in posts %}
       {% if post.category == 'code' %}
         {% include archive-single.html %}
       {% endif %}
      {% endfor %}
    {% endfor %}

  {% endif %}
{% endfor %}
