---
layout: default
---

<nav class="top-menu">
  <a href="#" class="filter-btn" data-filter="all" style="color: #45a29e;">Inicio</a>
  <a href="#astrofisica" class="filter-btn" data-filter="astrofisica">Astrofísica</a>
  <a href="#exploracion" class="filter-btn" data-filter="exploracion">Exploración</a>
  <a href="#energia" class="filter-btn" data-filter="energia">Energía</a>
</nav>

<div class="telemetry-panel">
  <div class="stat-box">
    <span class="stat-number">0{{ site.posts.size }}</span>
    <span class="stat-label">Archivos Desclasificados</span>
  </div>
  <div class="stat-box">
    <span class="stat-number">100%</span>
    <span class="stat-label">Eficiencia Teórica</span>
  </div>
  <div class="stat-box">
    <span class="stat-number">∞</span>
    <span class="stat-label">Frontera Cósmica</span>
  </div>
</div>

# Base de Datos Activa

<div class="grid-container" id="post-grid">
  {% for post in site.posts %}
    <div class="card post-card {{ post.categories | join: ' ' }}">
      {% if post.image %}
        <img src="{{ post.image }}" alt="{{ post.title }}" class="card-img">
      {% endif %}
      <div class="card-content">
        <h3>{{ post.title }}</h3>
        <p>{{ post.excerpt | strip_html | truncatewords: 15 }}</p>
        <a href="{{ post.url | relative_url }}" class="btn-leer">Acceder al registro →</a>
      </div>
    </div>
  {% endfor %}
</div>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const filterBtns = document.querySelectorAll('.filter-btn');
    const cards = document.querySelectorAll('.post-card');

    filterBtns.forEach(btn => {
      btn.addEventListener('click', function(e) {
        e.preventDefault(); 
        filterBtns.forEach(b => b.style.color = '#ffffff');
        this.style.color = '#45a29e'; 

        const filterValue = this.getAttribute('data-filter');

        cards.forEach(card => {
          if (filterValue === 'all' || card.classList.contains(filterValue)) {
            card.style.display = 'flex';
          } else {
            card.style.display = 'none';
          }
        });
      });
    });
  });
</script>
