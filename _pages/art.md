---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
header:
  overlay_image: "/images/header_image6.png"
---
<img src="{{ site.url }}{{site.baseurl }}/images/Excellogo.png" alt="Excel logo" width="50"/>
art coming soon.



{% for collect in site.collections %}
  <div class="collection">
    <h2><img src="{{ site.url }}{{site.baseurl }}/{{collection.image_path}} alt="{{ collect.title }}" />{{ collect.title }}</h2>
    {{ collect.content }}
  </div>
{% endfor %}
