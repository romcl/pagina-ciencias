---
layout: default
---

<nav class="top-menu">
  <a href="#" class="filter-btn" data-filter="all" style="color: #45a29e;">Inicio</a>
  <a href="#astrofisica" class="filter-btn" data-filter="astrofisica">Astrofísica</a>
  <a href="#exploracion" class="filter-btn" data-filter="exploracion">Exploración</a>
  <a href="#energia" class="filter-btn" data-filter="energia">Energía</a>
</nav>

# Bienvenidos al Cosmos

<div class="grid-container" id="post-grid">
  {% for post in site.posts %}
    <div class="card post-card {{ post.categories | join: ' ' }}">
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

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const filterBtns = document.querySelectorAll('.filter-btn');
    const cards = document.querySelectorAll('.post-card');

    filterBtns.forEach(btn => {
      btn.addEventListener('click', function(e) {
        e.preventDefault(); 
        
        // Reiniciar el color de todos los botones
        filterBtns.forEach(b => b.style.color = '#ffffff');
        // Pintar de cyan el botón seleccionado
        this.style.color = '#45a29e'; 

        const filterValue = this.getAttribute('data-filter');

        // Mostrar u ocultar tarjetas según la categoría
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
