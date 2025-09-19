---
title: Publications
permalink: /publications/
---

# Publications

<div class="grid">
{%- assign sorted = site. publications | sort: 'weight' %}
{%- for publication in sorted %}
  {%- include  publication-card.html  publication= publication %}
{%- endfor %}
</div>