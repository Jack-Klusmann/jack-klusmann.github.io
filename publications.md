---
title: Publications
permalink: /publications/
---

# Publications

<div class="pub-list">
{%- assign sorted = site.publications | sort: 'weight' | reverse -%}
{%- for publication in sorted -%}
  {%- include publication-card.html publication=publication -%}
{%- endfor -%}
</div>