---
layout: default
---

<nav class="top-menu">
  <a href="{{ site.baseurl }}/">Inicio</a>
  <a href="#astrofisica">Astrofísica</a>
  <a href="#exploracion">Exploración</a>
  <a href="#energia">Energía</a>
</nav>

# Bienvenidos al Cosmos

<div class="grid-container">
  {% for post in site.posts %}
    <div class="card">
      {% if post.image %}
        <img src="{{ post.image }}" alt="{{ post.title }}" class="card-img">
      {% endif %}
      <div class="card-content">
        <h3>{{ post.title }}</h3>
        <p>{{ post.excerpt | strip_html | truncatewords: 15 }}</p>
        <a href="{{ post.url | relative_url }}" class="btn-leer">Leer investigación →</a>
      </div>
    </div>
  {% endfor %}
</div>
