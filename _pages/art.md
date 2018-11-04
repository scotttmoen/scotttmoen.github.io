---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
header:
  overlay_image: "/images/header_image6.png"
---


{% for collect in collections %}
  <<div class="collection">
    <h2><<img src="{{collect.image_path}}" alt="{{ collect.title }}"/> /*{{collect.title }}*/h2>{{ collect.content }}
  </div>
{%endfor%}
