---
title: Projects
permalink: /projects/
---

# Projects

<div class="grid">
{%- assign sorted = site.projects | sort: 'weight' | reverse %}
{%- for project in sorted %}
  {%- include project-card.html project=project %}
{%- endfor %}
</div>