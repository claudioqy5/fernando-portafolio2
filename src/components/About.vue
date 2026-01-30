<template>
  <section id="about" class="about">
    <div class="container about-grid">
      <div class="about-text fade-up" ref="aboutText">
        <h2 class="section-title text-left">SERVICIOS</h2>
        <p class="highlight-p">
          Soy Fernando, un apasionado de la tecnología con un enfoque práctico: 
          <strong>creo soluciones que funcionan.</strong>
        </p>
        <p>
          No solo escribo código; diseño estrategias digitales que resuelven problemas reales. Mi objetivo es que la tecnología sea tu mejor aliada, no un obstáculo.
        </p>
        <div class="skills-mini">
          <div class="skill-tag">Desarrollo Web</div>
          <div class="skill-tag">Automatizaciones</div>
          <div class="skill-tag">Estrategia Digital</div>
          <div class="skill-tag">Arquitectura de Software</div>
        </div>
      </div>
      <div class="about-stats glass fade-up" ref="aboutStats">
        <div class="stat-item">
          <span class="stat-num">+5</span>
          <span class="stat-label">Años de experiencia</span>
        </div>
        <div class="stat-item">
          <span class="stat-num">+50</span>
          <span class="stat-label">Proyectos entregados</span>
        </div>
        <div class="stat-item">
          <span class="stat-num">100%</span>
          <span class="stat-label">Enfoque en resultados</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const aboutText = ref(null);
const aboutStats = ref(null);

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.2 });

  if (aboutText.value) observer.observe(aboutText.value);
  if (aboutStats.value) observer.observe(aboutStats.value);
});
</script>

<style scoped>
.about-grid {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 4rem;
  align-items: center;
}

.text-left {
  text-align: left;
  margin-bottom: 2rem;
}

.highlight-p {
  font-size: 1.5rem;
  color: var(--text-primary);
  margin-bottom: 1.5rem;
  line-height: 1.4;
}

.highlight-p strong {
  color: var(--accent-primary);
}

p {
  color: var(--text-secondary);
  margin-bottom: 2rem;
  font-size: 1.1rem;
}

.skills-mini {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.skill-tag {
  padding: 0.5rem 1.2rem;
  background: var(--bg-secondary);
  border: 1px solid var(--glass-border);
  border-radius: 50px;
  font-size: 0.9rem;
  color: var(--text-secondary);
  transition: var(--transition-speed);
}

.skill-tag:hover {
  border-color: var(--accent-primary);
  color: var(--text-primary);
}

.about-stats {
  padding: 3rem;
  display: flex;
  flex-direction: column;
  gap: 2.5rem;
}

.stat-item {
  display: flex;
  flex-direction: column;
}

.stat-num {
  font-family: var(--font-heading);
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--accent-primary);
}

.stat-label {
  color: var(--text-secondary);
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 1px;
}

@media (max-width: 992px) {
  .about-grid {
    grid-template-columns: 1fr;
    gap: 3rem;
  }
}
</style>
