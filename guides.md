---
layout: page
title: Guides
permalink: /guides/
description: For step-by-step guides and tutorials on how to use Loom's platform.
menu: true
---
<div class="flex-container">
{% for guide in site.guides %}
<a href="{{guide.url}}" class="item">
<div>
  <h2>{{ guide.title }}</h2>
  <span class="small">{{ guide.description }}</span>
</div>
</a>
{% endfor %}
</div>
