<template>
  <section id="process" class="who-we-are-section">
    <!-- Background Moving Text (X-Shape Effect) -->
    <div class="bg-marquee-container">
      <div class="bg-marquee-row row-1">
        <div class="marquee-inner">
          <span v-for="n in 10" :key="'bg-l1-'+n">HELIFY — INNOVACIÓN — INGENIERÍA — FUTURO — IMPACTO — </span>
        </div>
        <div class="marquee-inner">
          <span v-for="n in 10" :key="'bg-l2-'+n">HELIFY — INNOVACIÓN — INGENIERÍA — FUTURO — IMPACTO — </span>
        </div>
      </div>
      <div class="bg-marquee-row row-2">
        <div class="marquee-inner">
          <span v-for="n in 10" :key="'bg-r1-'+n">CREATIVIDAD — ESTRATEGIA — DISEÑO — EXCELENCIA — SIMPLE — </span>
        </div>
        <div class="marquee-inner">
          <span v-for="n in 10" :key="'bg-r2-'+n">CREATIVIDAD — ESTRATEGIA — DISEÑO — EXCELENCIA — SIMPLE — </span>
        </div>
      </div>
    </div>

    <div class="container relative-content">
      <!-- Massive Heading (Minimalist) -->
      <div class="header-content fade-up" ref="headerRef">
        <h2 class="massive-title">
          IDEALIZAMOS<br/>
          EL FUTURO
        </h2>
        <div class="intro-box">
          <p class="intro-text">
            Somos una boutique de ingeniería digital enfocada en la simplicidad, la utilidad y el alto rendimiento.
          </p>
        </div>
      </div>

      <!-- Split Layout: Story & Values -->
      <div class="content-grid">
        <div class="story-column fade-up">
          <h3 class="side-heading">Nuestra Filosofía</h3>
          <p class="story-text">
            No somos una agencia convencional. En Helify, creemos que la tecnología debe ser invisible: una herramienta tan bien diseñada que simplemente funciona sin que tengas que pensar en ella.
          </p>
          <p class="story-text">
            Combinamos el rigor técnico con una visión de negocio disruptiva para entregar productos que no solo cumplen expectativas, sino que redefinen estándares.
          </p>
        </div>

        <div class="values-column">
          <div v-for="(value, index) in values" :key="index" class="value-item fade-up">
            <div class="value-top">
              <span class="value-index">0{{ index + 1 }}</span>
              <h4 class="value-title">{{ value.title }}</h4>
            </div>
            <p class="value-desc">{{ value.desc }}</p>
          </div>
        </div>
      </div>

      <!-- Premium Visual Element (Innovative Separator) -->
      <div class="visual-footer fade-up">
        <div class="line-reveal"></div>
        <div class="footer-meta">
          <span>BASADOS EN LA EXCELENCIA</span>
          
          <span>IMPULSANDO EL ÉXITO</span>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const headerRef = ref(null);
const values = [
  { title: 'Innovación Real', desc: 'No usamos tecnología por moda, la usamos para resolver necesidades que otros no pueden.' },
  { title: 'Ingeniería Ética', desc: 'Código limpio, arquitecturas escalables y transparencia total en cada paso.' },
  { title: 'Visión Global', desc: 'Diseñamos soluciones pensadas para crecer y dominar mercados competitivos.' }
];

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.fade-up').forEach(el => observer.observe(el));
});
</script>

<style scoped>
.who-we-are-section {
  background-color: #ffffff;
  color: #000000;
  padding: 160px 0;
  min-height: 100vh;
  position: relative;
  overflow: hidden; /* Critical for background marquee */
}

.relative-content {
  position: relative;
  z-index: 10;
}

/* Background X Marquee */
.bg-marquee-container {
  position: absolute;
  inset: 0;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0.08; /* Very subtle background presence */
  pointer-events: none;
}

.bg-marquee-row {
  display: flex;
  white-space: nowrap;
  font-family: var(--font-heading);
  font-size: 8rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: -2px;
}

.row-1 {
  transform: rotate(-12deg) translateY(-100px);
}

.row-2 {
  transform: rotate(8deg) translateY(100px);
  margin-top: 0; /* Removed negative margin to allow more natural separation */
}

.row-1 .marquee-inner {
  display: flex;
  animation: scroll-left-bg 120s linear infinite;
}

.row-2 .marquee-inner {
  display: flex;
  animation: scroll-right-bg 120s linear infinite;
}

.marquee-inner span {
  padding-right: 80px;
}

@keyframes scroll-left-bg {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

@keyframes scroll-right-bg {
  0% { transform: translateX(-50%); }
  100% { transform: translateX(0); }
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 5%;
}

.header-content {
  margin-bottom: 100px;
  display: flex;
  flex-direction: column;
  gap: 40px;
}

.massive-title {
  font-family: var(--font-heading);
  font-size: clamp(4rem, 15vw, 12rem);
  font-weight: 300;
  line-height: 0.85;
  letter-spacing: -0.06em;
  color: #000;
  margin: 0;
}

.intro-box {
  border-left: 2px solid #000;
  padding-left: 40px;
  max-width: 600px;
}

.intro-text {
  font-size: 1.5rem;
  line-height: 1.4;
  font-weight: 500;
  color: #000;
}

/* Content Grid */
.content-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 120px;
  margin-top: 60px;
}

.side-heading {
  font-family: var(--font-heading);
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: #888;
  margin-bottom: 30px;
}

.story-text {
  font-size: 1.25rem;
  line-height: 1.7;
  color: #333;
  margin-bottom: 30px;
  max-width: 500px;
}

.values-column {
  display: flex;
  flex-direction: column;
  gap: 60px;
}

.value-item {
  border-bottom: 1px solid #000;
  padding-bottom: 30px;
  transition: all 0.4s ease;
}

.value-item:hover {
  padding-left: 20px;
  border-bottom-width: 3px;
}

.value-top {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 15px;
}

.value-index {
  font-family: var(--font-heading);
  font-size: 0.9rem;
  font-weight: 700;
  color: #000;
}

.value-title {
  font-family: var(--font-heading);
  font-size: 1.8rem;
  font-weight: 700;
  margin: 0;
}

.value-desc {
  font-size: 1rem;
  color: #555;
  line-height: 1.6;
  max-width: 450px;
}

/* Visual Footer */
.visual-footer {
  margin-top: 140px;
}

.line-reveal {
  width: 100%;
  height: 1px;
  background-color: #000;
  margin-bottom: 30px;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 1.5s cubic-bezier(0.16, 1, 0.3, 1);
}

.fade-up.visible .line-reveal {
  transform: scaleX(1);
}

.footer-meta {
  display: flex;
  justify-content: space-between;
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 2px;
  color: #888;
}

/* Base Animations */
.fade-up {
  opacity: 0;
  transform: translateY(40px);
  transition: all 1s cubic-bezier(0.16, 1, 0.3, 1);
}

.fade-up.visible {
  opacity: 1;
  transform: translateY(0);
}

@media (max-width: 1024px) {
  .content-grid {
    grid-template-columns: 1fr;
    gap: 80px;
  }
  .massive-title { letter-spacing: -0.04em; }
}

@media (max-width: 768px) {
  .who-we-are-section { padding: 100px 0; }
  .intro-text { font-size: 1.2rem; }
  .massive-title { font-size: 4rem; }
  .footer-meta { flex-direction: column; gap: 10px; align-items: center; }
}
</style>
