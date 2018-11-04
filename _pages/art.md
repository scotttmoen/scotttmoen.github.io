---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
header:
  overlay_image: "/images/header_image6.png"
---
{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.portfolio %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
