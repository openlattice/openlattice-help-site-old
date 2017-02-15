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

<script>
  window.store = {
    {% for page in site.pages %}
      "{{ page.url | slugify }}": {
        "title": "{{ page.title | xml_escape }}",
        "content": {{ page.content | strip_html | strip_newlines | jsonify }},
        "url": "{{ page.url | xml_escape }}"
      }
      {% unless forloop.last %},{% endunless %}
    {% endfor %}
  };

  {% for guide in site.guides %}
    window.store["{{guide.url | slugify}}"] = {
      "title": "{{ guide.title | xml_escape }}",
      "content": {{ guide.content | strip_html | strip_newlines | jsonify }},
      "url": "{{ guide.url | xml_escape }}"
    }
  {% endfor %}

  {% for doc in site.info %}
    window.store["{{doc.url | slugify}}"] = {
      "title": "{{ doc.title | xml_escape }}",
      "content": {{ doc.content | strip_html | strip_newlines | jsonify }},
      "url": "{{ doc.url | xml_escape }}"
    }
  {% endfor %}

</script>
<script src="/js/lunr.min.js"></script>
<script src="/js/search.js"></script>
