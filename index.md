---
layout: home
---
# Welcome to Loom's Help Center

<center>We want to make sure that you have all the tools you need to successfully tackle your data challenges.</center>

<div class="flex-container">
{% for page in site.pages %}
  {% if page.menu == true %}
  <a href="{{ page.url | relative_url }}" class="item">
    <div class="large">{{page.title}}</div>
    <div class="line-break"></div>
    <div class="small">{{page.description}}</div>
  </a>
  {% endif %}
{% endfor %}
</div>
