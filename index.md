---
layout: home
---
# Welcome

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

<form action="/index.html" method="get">
  <label for="search-box">Search</label>
  <input type="text" id="search-box" name="query">
  <input type="submit" value="search">
</form>

<div id="search-results"></div>

{% include generate_json.html }

<script src="/js/lunr.min.js"></script>
<script src="/js/search.js"></script>
