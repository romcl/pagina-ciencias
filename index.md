---
layout: default
---

<nav class="top-menu">
  <a href="#" class="filter-btn" data-filter="all" style="color: #45a29e;">Inicio</a>
  <a href="#astrofisica" class="filter-btn" data-filter="astrofisica">Astrofísica</a>
  <a href="#exploracion" class="filter-btn" data-filter="exploracion">Exploración</a>
  <a href="#energia" class="filter-btn" data-filter="energia">Energía</a>
</nav>

<h2 class="section-title" style="margin-top: -30px; position: relative; z-index: 10;">Archivo Principal</h2>

<div class="hero-section" id="hero-container">
  {% for post in site.posts limit: 1 %}
    <div class="hero-card post-item {{ post.categories | join: ' ' }}">
      {% if post.image %}
        <img src="{{ post.image }}" alt="{{ post.title }}" class="hero-img">
      {% endif %}
      <div class="hero-content">
        <span class="badge">ÚLTIMA DESCLASIFICACIÓN</span>
        <h2>{{ post.title }}</h2>
        <p>{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
        <a href="{{ post.url | relative_url }}" class="btn-leer">Acceder al Registro Completo →</a>
      </div>
    </div>
  {% endfor %}
</div>

<div class="bento-grid" id="bento-container">
  {% for post in site.posts offset: 1 %}
    <div class="bento-card post-item {{ post.categories | join: ' ' }}">
      {% if post.image %}
        <img src="{{ post.image }}" alt="{{ post.title }}" class="bento-img">
      {% endif %}
      <div class="bento-content">
        <h3>{{ post.title }}</h3>
        <a href="{{ post.url | relative_url }}" class="btn-leer btn-small">Analizar Datos →</a>
      </div>
    </div>
  {% endfor %}
</div>

<h2 class="section-title" style="margin-top: 40px;">Herramienta de Exploración</h2>

<div class="terminal-wrapper">
  <div class="terminal-header">
    <span class="pulse"></span> SISTEMA DE ESCANEO DE PROFUNDIDAD (ORIGEN: CALDERA, ATACAMA)
  </div>
  
  <div class="radar-layout">
    <div class="radar-controls">
      <h4>OBJETIVOS</h4>
      <button class="target-btn active" data-target="luna">LUNA</button>
      <button class="target-btn" data-target="sol">SOL</button>
      <button class="target-btn" data-target="venus">VENUS</button>
      <button class="target-btn" data-target="marte">MARTE</button>
      <button class="target-btn" data-target="jupiter">JÚPITER</button>
    </div>

    <div class="radar-display">
      <div class="radar-circle">
        <div class="radar-sweep"></div>
        <div class="earth-center"></div>
      </div>
    </div>

    <div class="radar-data">
      <h4>TELEMETRÍA EN VIVO</h4>
      <div class="data-group">
        <span class="data-label">OBJETIVO:</span>
        <span class="data-value highlight" id="data-name">LUNA</span>
      </div>
      <div class="data-group">
        <span class="data-label">DISTANCIA PROMEDIO:</span>
        <span class="data-value" id="data-dist">384,400 km</span>
      </div>
      <div class="data-group">
        <span class="data-label">RETRASO (LUZ):</span>
        <span class="data-value" id="data-light">1.28 segundos</span>
      </div>
      <div class="data-group">
        <span class="data-label">VIAJE DE SONDA:</span>
        <span class="data-value" id="data-probe">9.0 días</span>
      </div>
      <div class="data-group mt-auto">
        <span class="data-label">ESTADO DEL ANÁLISIS:</span>
        <span class="data-value text-cyan" id="data-status">Órbita estable. Anomalías: 0.</span>
      </div>
    </div>
  </div>
</div>

<footer class="custom-footer">
  <div class="footer-content">
    <div class="footer-logo">
      <h3>EXPLORACIÓN Y CIENCIA</h3>
      <p>Divulgación científica desde Caldera, Región de Atacama.</p>
    </div>
    <div class="footer-social">
      <a href="https://instagram.com/jamsroam" target="_blank" class="social-link">Instagram</a>
      <a href="https://youtube.com/" target="_blank" class="social-link">YouTube</a>
      <a href="https://tiktok.com/" target="_blank" class="social-link">TikTok</a>
    </div>
  </div>
  <div class="footer-bottom">
    <p>&copy; 2026 Raúl Araya Morales. Todos los derechos reservados.</p>
  </div>
</footer>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    // Lógica del Filtro (Ahora aplica a post-item en general)
    const filterBtns = document.querySelectorAll('.filter-btn');
    const items = document.querySelectorAll('.post-item');
    
    filterBtns.forEach(btn => {
      btn.addEventListener('click', function(e) {
        e.preventDefault(); 
        filterBtns.forEach(b => b.style.color = '#ffffff');
        this.style.color = '#45a29e'; 
        
        const filterValue = this.getAttribute('data-filter');
        
        items.forEach(item => {
          if (filterValue === 'all' || item.classList.contains(filterValue)) {
            item.style.display = 'flex';
          } else {
            item.style.display = 'none';
          }
        });
      });
    });

    // Base de Datos del Radar Cósmico
    const astroData = {
      luna: { name: "LUNA", dist: 384400, status: "Órbita estable. Anomalías: 0." },
      sol: { name: "SOL", dist: 149600000, status: "Actividad solar moderada." },
      venus: { name: "VENUS", dist: 41400000, status: "Atmósfera tóxica detectada." },
      marte: { name: "MARTE", dist: 78300000, status: "Rastreo de rovers activo." },
      jupiter: { name: "JÚPITER", dist: 628700000, status: "Fuerte radiación magnética." }
    };
    const speedOfLight = 299792; 
    const probeSpeed = 60000; 

    const targetBtns = document.querySelectorAll('.target-btn');
    const dName = document.getElementById('data-name');
    const dDist = document.getElementById('data-dist');
    const dLight = document.getElementById('data-light');
    const dProbe = document.getElementById('data-probe');
    const dStatus = document.getElementById('data-status');

    targetBtns.forEach(btn => {
      btn.addEventListener('click', function() {
        targetBtns.forEach(b => b.classList.remove('active'));
        this.classList.add('active');

        const key = this.getAttribute('data-target');
        const data = astroData[key];
        
        const lightSeconds = (data.dist / speedOfLight);
        let lightText = lightSeconds < 60 ? `${lightSeconds.toFixed(2)} segundos` : `${(lightSeconds/60).toFixed(2)} minutos`;

        const probeHours = data.dist / probeSpeed;
        let probeText = probeHours < 24 ? `${probeHours.toFixed(1)} horas` : `${(probeHours/24).toFixed(1)} días`;
        if ((probeHours/24) > 365) probeText = `${(probeHours/24/365).toFixed(2)} años`;

        dName.textContent = data.name;
        dDist.textContent = data.dist.toLocaleString('es-ES') + " km";
        dLight.textContent = lightText;
        dProbe.textContent = probeText;
        dStatus.textContent = data.status;
      });
    });
  });
</script>
