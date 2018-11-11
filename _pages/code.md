---
layout: archive
permalink: /code/
title: "Code"
author_profile: true
header:
  overlay_image: "/images/header_image2.png"
---

test7

{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% if tag == "code" %}
    {% assign posts = group_items[forloop.index0] %}
    <h2 id="{{ tag | slugify }}"
     class="archive__subtitle"><i style="margin-left: 40px">Tag : {{tag }}</i></h2>
    {% for post in posts %}
        {% include archive-single.html %}
    {% endfor %}
  {% endif %}
{% endfor %}
