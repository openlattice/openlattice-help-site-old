---
layout: page
title: Guides
permalink: /guides/
description: Reference our step-by-step guides and tutorials on how to use OpenLattice's platform.
menu: true
---
{% assign guides = site.guides | sort:"weight" | reverse %}
{% for guide in guides %}
  <div>
    <a href="{{guide.url}}">
    <h3>{{ guide.title }}</h3>
    </a>
    {{ guide.description | strip_html}} <a href="{{ guide.url }}">Learn More »</a>
  </div>
{% endfor %}
