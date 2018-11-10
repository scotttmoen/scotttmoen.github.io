---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
header:
  overlay_image: "/images/header_image6.png"
---


{% for collect in site.portfolio %}
  <div class="collection">
    <h1><img src="{{ site.url }}{{site.baseurl }}/{{collect.image_path}}" alt="{{ collect.title }}" />  <br>
  <a href="https://github.com/scotttmoen/Art">{{ "Download" }}</a><img src="{{ site.url }}{{site.baseurl }}/images/blenderlogocolor.png" alt="Blender logo" width="100"/><h1> </h1>  {{ collect.title }}</h1>
    {{ collect.content }}
  </div>
{% endfor %}
