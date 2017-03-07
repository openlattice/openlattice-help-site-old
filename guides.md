---
layout: page
title: Guides
permalink: /guides/
description: Reference our step-by-step guides and tutorials on how to use Loom's platform.
menu: true
---

{% for guide in site.guides %}
  <div>
    <a href="{{guide.url}}">
    <h2>{{ guide.title }}</h2>
    </a>
    {{ guide.description }}
  </div>
{% endfor %}
