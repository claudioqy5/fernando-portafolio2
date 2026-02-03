<template>
  <section id="about" class="services-section">
    <div class="container-fluid">
      <!-- Massive Title as 'NUESTROS SERVICIOS' -->
      <div class="header-container container">
        <div class="header-content fade-up" ref="headerRef">
          <h2 class="massive-title">
            NUESTROS<br/>
            SERVICIOS
          </h2>
          <p class="subtitle-text">
            Cada servicio está pensado para resolver una necesidad real, no para usar tecnología por moda.
          </p>
        </div>
      </div>

      <!-- Infinite Marquee Services -->
      <div class="services-marquee">
        <div class="marquee-track">
          <!-- First set of services -->
          <div v-for="(service, index) in services" :key="'a-' + index" class="service-card">
            <div class="card-header">
              <span class="service-index">0{{ index + 1 }}</span>
              <div class="card-line"></div>
            </div>
            <div class="card-body">
              <h3 class="service-name">{{ service.title }}</h3>
              <p class="service-desc">{{ service.description }}</p>
            </div>
          </div>
          <!-- Duplicated set for seamless loop -->
          <div v-for="(service, index) in services" :key="'b-' + index" class="service-card">
            <div class="card-header">
              <span class="service-index">0{{ index + 1 }}</span>
              <div class="card-line"></div>
            </div>
            <div class="card-body">
              <h3 class="service-name">{{ service.title }}</h3>
              <p class="service-desc">{{ service.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const headerRef = ref(null);

const services = [
  {
    title: 'Desarrollo web',
    description: 'Sitios web modernos, rápidos y optimizados para convertir visitantes en clientes.'
  },
  {
    title: 'Aplicaciones web',
    description: 'Plataformas interactivas y escalables que resuelven necesidades específicas del negocio.'
  },
  {
    title: 'Sistemas a medida',
    description: 'Software personalizado diseñado para integrarse perfectamente con tus flujos de trabajo.'
  },
  {
    title: 'Automatizaciones con IA',
    description: 'Implementación de agentes y herramientas inteligentes para ahorrar tiempo y reducir errores.'
  },
  {
    title: 'Integraciones y procesos',
    description: 'Conectamos tus herramientas favoritas para que la información fluya sin fricciones.'
  },
  {
    title: 'Estrategia digital',
    description: 'Consultoría experta para escalar tu presencia online con resultados medibles.'
  }
];

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.1 });

  if (headerRef.value) observer.observe(headerRef.value);
});
</script>

<style scoped>
.services-section {
  background-color: #b0b1a2;
  color: #000000;
  padding: 160px 0;
  min-height: 100vh;
  overflow: hidden;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 5%;
}

.header-container {
  margin-bottom: 80px;
}

.massive-title {
  font-family: var(--font-heading);
  font-size: clamp(4rem, 15vw, 12rem);
  font-weight: 800;
  line-height: 0.85;
  letter-spacing: -0.06em;
  color: #000;
  margin: 0;
  text-transform: uppercase;
}

.subtitle-text {
  font-size: 1.5rem;
  color: #000;
  margin-top: 30px;
  max-width: 600px;
  font-weight: 400;
  line-height: 1.4;
  opacity: 0.8;
}

/* Marquee Logic */
.services-marquee {
  position: relative;
  width: 100%;
  padding: 40px 0;
}

.marquee-track {
  display: flex;
  width: max-content;
  animation: scroll 40s linear infinite;
  transition: all 0.5s ease;
}

/* Blur effect on hover for the other cards */
.marquee-track:hover .service-card {
  filter: blur(8px);
  opacity: 0.3;
  transform: scale(0.95);
}

.marquee-track .service-card:hover {
  filter: blur(0);
  transition: all 0.5s ease;
  opacity: 1;
  transform: scale(1.1);
  z-index: 10;
}

.marquee-track:hover {
  animation-play-state: paused;
}

.service-card {
  width: 400px;
  margin-right: 60px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 20px;
}

.service-index {
  font-family: var(--font-heading);
  font-size: 1rem;
  font-weight: 700;
  color: #000;
}

.card-line {
  flex-grow: 1;
  height: 1px;
  background: #000;
}

.service-name {
  font-family: var(--font-heading);
  font-size: 2.2rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #000;
  letter-spacing: -1px;
}

.service-desc {
  font-size: 1.1rem;
  color: #333;
  line-height: 1.5;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(calc(-50%));
  }
}

/* Animations */
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
  .massive-title {
    letter-spacing: -2px;
  }
  .service-card {
    width: 320px;
  }
}

@media (max-width: 768px) {
  .services-section {
    padding: 100px 0;
  }
  .header-container {
    margin-bottom: 40px;
  }
  .service-name {
    font-size: 1.8rem;
  }
}
</style>
