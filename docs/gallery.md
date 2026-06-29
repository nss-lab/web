---
layout: page
permalink: /gallery/
title: Gallery
subtitle: Moments from the lab.
---

<p class="gallery-note muted">Use the arrows to browse — or swipe / scroll.</p>
<div class="gallery">
  <button class="gallery-nav gallery-prev" type="button" aria-label="Previous photos">‹</button>
  <div class="gallery-strip" id="galleryStrip">
  {% for img in site.data.gallery %}<figure class="gallery-item"><img src="{{ '/assets/img/gallery/' | append: img.file | relative_url }}" alt="{{ img.caption }}" loading="lazy">{% if img.caption %}<figcaption>{{ img.caption }}</figcaption>{% endif %}</figure>
  {% endfor %}</div>
  <button class="gallery-nav gallery-next" type="button" aria-label="Next photos">›</button>
</div>
