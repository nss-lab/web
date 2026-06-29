---
layout: page
permalink: /gallery/
title: Gallery
subtitle: Moments from the lab.
---

<p class="gallery-note muted">Scroll sideways to browse →</p>
<div class="gallery-strip">
{% for img in site.data.gallery %}<img src="{{ '/assets/img/gallery/' | append: img.file | relative_url }}" alt="{{ img.alt }}" loading="lazy">
{% endfor %}</div>
