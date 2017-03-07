---
layout: home
---
# What can we help you with?

<div class="tagline">We want to make sure that you have all the tools you need
to successfully tackle your data challenges.</div>

<form action="/index.html" method="get">
  <input type="text" id="search-box" name="query" placeholder="Search for a topic">
  <input type="submit" value="Search">
</form>

<div id="search-results"></div>

<div class="flex-container main-menu">
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

{% include generate_json.html %}

<script src="/js/lunr.min.js"></script>
<script src="/js/search.js"></script>
