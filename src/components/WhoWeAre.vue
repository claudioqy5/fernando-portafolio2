<template>
  <section id="process" class="who-we-are">
    <div class="container content-wrapper">
      <div class="text-side fade-up">
        <h2 class="section-title text-left">¿QUIÉNES SOMOS?</h2>
        <div class="editorial-text">
          <p class="large-text">
            Un equipo de <span>expertos digitales</span> dedicados a convertir ideas en arquitecturas tecnológicas de alto rendimiento.
          </p>
          <p class="description">
            En Helify, no solo construimos software, diseñamos el futuro de tu negocio. Nuestra pasión por la ingeniería y el diseño nos permite crear herramientas que no solo se ven bien, sino que impulsan resultados tangibles.
          </p>
        </div>
        
        <div class="values-grid">
          <div class="value-card glass">
            <span class="value-index">01</span>
            <h4>Innovación</h4>
            <p>Siempre a la vanguardia de las últimas tecnologías.</p>
          </div>
          <div class="value-card glass">
            <span class="value-index">02</span>
            <h4>Calidad</h4>
            <p>Estructuras robustas y código limpio por defecto.</p>
          </div>
          <div class="value-card glass">
            <span class="value-index">03</span>
            <h4>Compromiso</h4>
            <p>Tu éxito es nuestra principal métrica de desempeño.</p>
          </div>
        </div>
      </div>
      
      <div class="visual-side fade-up">
        <div class="floating-image-container">
          <!-- Placeholder or decoration to maintain the editorial look -->
          <div class="tech-shape shape-1"></div>
          <div class="tech-shape shape-2"></div>
          <div class="glass burst-shape">
            <span class="big-number">8+</span>
            <span class="label">Años creando impacto</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
// Logic for reveal animations if needed
</script>

<style scoped>
.who-we-are {
  background-color: var(--bg-primary);
  position: relative;
  overflow: hidden;
}

.content-wrapper {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 6rem;
  align-items: center;
}

.large-text {
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  font-weight: 600;
  line-height: 1.3;
  margin-bottom: 2rem;
  color: var(--text-primary);
}

.large-text span {
  color: var(--accent-primary);
}

.description {
  font-size: 1.15rem;
  color: var(--text-secondary);
  line-height: 1.8;
  margin-bottom: 3.5rem;
  max-width: 650px;
}

.values-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}

.value-card {
  padding: 1.5rem;
  border-radius: 12px;
  transition: transform 0.3s ease;
}

.value-card:hover {
  transform: translateY(-5px);
}

.value-index {
  font-family: var(--font-heading);
  font-size: 0.8rem;
  color: var(--accent-primary);
  font-weight: 700;
  margin-bottom: 1rem;
  display: block;
}

.value-card h4 {
  margin-bottom: 0.5rem;
  font-size: 1.1rem;
}

.value-card p {
  font-size: 0.85rem;
  color: var(--text-secondary);
  line-height: 1.5;
}

/* Visual Side Decoration */
.visual-side {
  position: relative;
}

.floating-image-container {
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.tech-shape {
  position: absolute;
  border: 1px solid rgba(191, 162, 128, 0.3); /* #bfa280 with transparency */
  width: 300px;
  height: 300px;
  border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%;
  animation: morphing 10s infinite alternate;
}

.shape-2 {
  width: 250px;
  height: 250px;
  border-color: rgba(255, 255, 255, 0.1);
  animation-delay: -5s;
}

.burst-shape {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.big-number {
  font-size: 3.5rem;
  font-weight: 800;
  color: var(--accent-primary);
}

.label {
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: var(--text-secondary);
}

@keyframes morphing {
  0% { border-radius: 30% 70% 70% 30% / 30% 30% 70% 70%; transform: rotate(0deg); }
  100% { border-radius: 41% 59% 41% 59% / 54% 48% 52% 46%; transform: rotate(180deg); }
}

@media (max-width: 1024px) {
  .content-wrapper {
    grid-template-columns: 1fr;
    text-align: center;
    gap: 4rem;
  }
  
  .description {
    margin-inline: auto;
  }
  
  .text-left {
    text-align: center;
  }
  
  .values-grid {
    grid-template-columns: 1fr;
  }
  
  .visual-side {
    order: -1;
  }
}
</style>
