---
layout: archive
permalink: /art/
title: "Art"
author_profile: true
<<<<<<< HEAD
=======
collection:portfolio
entries_layout: grid
>>>>>>> 29222c5b8d6dd49c2e323b19c0325ad9567ebaee
header:
  overlay_image: "/images/header_image6.png"
---

Art coming soon



{% for collect in site.portfolio %}
  <div class="collection">
    <h2><img src="{{ site.url }}{{site.baseurl }}/{{collect.image_path}}" alt="{{ collect.title }}" />  

    {{ collect.title }}</h2>
    {{ collect.content }}
  </div>
{% endfor %}
