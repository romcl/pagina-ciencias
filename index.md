---
layout: default
---

<nav class="top-menu">
  <a href="#" class="filter-btn" data-filter="all" style="color: #45a29e;">Inicio</a>
  <a href="#astrofisica" class="filter-btn" data-filter="astrofisica">Astrofísica</a>
  <a href="#exploracion" class="filter-btn" data-filter="exploracion">Exploración</a>
  <a href="#energia" class="filter-btn" data-filter="energia">Energía</a>
</nav>

<div id="science-archive"></div>

<script id="posts-data" type="application/json">
[
  {% for post in site.posts %}
  {
    "title": {{ post.title | jsonify }},
    "url": {{ post.url | relative_url | jsonify }},
    "image": {{ post.image | jsonify }},
    "excerpt": {{ post.excerpt | strip_html | truncatewords: 20 | jsonify }},
    "categories": {{ post.categories | jsonify }}
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
]
</script>

<h2 class="section-title">Herramienta de Exploración</h2>
<div class="terminal-wrapper">
  <div class="terminal-header">
    <span class="pulse"></span> SISTEMA DE ESCANEO DE PROFUNDIDAD (ORIGEN: PLANETA TIERRA)
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
      <a href="https://instagram.com/" target="_blank" class="social-link">Instagram</a>
      <a href="https://youtube.com/" target="_blank" class="social-link">YouTube</a>
      <a href="https://tiktok.com/" target="_blank" class="social-link">TikTok</a>
    </div>
  </div>
  <div class="footer-bottom">
    <p>&copy; 2026. Todos los derechos reservados.</p>
  </div>
</footer>

<script>
document.addEventListener('DOMContentLoaded', function() {
  // --- 1. RENDERIZADO DEL ARCHIVO (HERO + BENTO) ---
  const postsRaw = document.getElementById('posts-data').textContent;
  const posts = JSON.parse(postsRaw);
  const container = document.getElementById('science-archive');

  function renderArchive(filter = 'all') {
    let filteredPosts = filter === 'all' ? posts : posts.filter(p => p.categories.includes(filter));
    
    if (filteredPosts.length === 0) {
      container.innerHTML = '<p style="text-align:center; padding:50px; color:#c5c6c7;">No hay registros en esta categoría.</p>';
      return;
    }

    const randomIndex = Math.floor(Math.random() * filteredPosts.length);
    const heroPost = filteredPosts[randomIndex];
    const secondaryPosts = filteredPosts.filter((_, i) => i !== randomIndex);

    let html = `
      <h2 class="section-title" style="margin-top: -30px;">Investigación Destacada</h2>
      <div class="hero-section">
        <div class="hero-card">
          <img src="${heroPost.image}" class="hero-img">
          <div class="hero-content">
            <span class="badge">SISTEMA RANDOM ACTIVO</span>
            <h2>${heroPost.title}</h2>
            <p>${heroPost.excerpt}</p>
            <a href="${heroPost.url}" class="btn-leer">Acceder al Registro →</a>
          </div>
        </div>
      </div>
      <h2 class="section-title" style="font-size: 1.2rem; margin-top: 20px;">Otros Archivos</h2>
      <div class="bento-grid">
    `;

    secondaryPosts.forEach(post => {
      html += `
        <a href="${post.url}" class="bento-card">
          <img src="${post.image}" class="bento-img">
          <div class="bento-content">
            <h3>${post.title}</h3>
          </div>
        </a>
      `;
    });

    html += `</div>`;
    container.innerHTML = html;
  }

  renderArchive();

  document.querySelectorAll('.filter-btn').forEach(btn => {
    btn.addEventListener('click', (e) => {
      e.preventDefault();
      document.querySelectorAll('.filter-btn').forEach(b => b.style.color = '#ffffff');
      btn.style.color = '#45a29e';
      renderArchive(btn.getAttribute('data-filter'));
    });
  });

  // --- 2. LÓGICA DEL RADAR ---
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
