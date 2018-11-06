---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
header:
  overlay_image: "/images/header_image6.png"
---

Art coming soon



{% for collect in site.portfolio %}
  <div class="collection">
    <h2><img src="{{ site.url }}{{site.baseurl }}/{{collect.image_path}}" alt="{{ collect.title }}" />  <br>
  <img src="{{ site.url }}{{site.baseurl }}/images/blenderlogocolor.png" alt="SVG logo" width="50"/>  {{ collect.title }}</h2>
    {{ collect.content }}
  </div>
{% endfor %}
