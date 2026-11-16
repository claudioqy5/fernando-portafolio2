<template>
  <section id="about" class="services-section">
    <div class="container-fluid">
      <!-- Header with DitherVeil on left & 'NUESTROS SERVICIOS' on right -->
      <div class="header-container container">
        <div class="header-content fade-up" ref="headerRef">
          <div class="header-dither-col">
            <div class="dither-card">
              <DitherVeil
                src="/statue-sculpture.jpg"
                fit="contain"
                pattern="floyd"
                :pixelSize="2"
                :levels="2"
                palette="duotone"
                inkColor="#b0b1a2"
                paperColor="#ffffff"
                :contrast="1.3"
                :brightness="0.05"
                :revealRadius="260"
                :softness="0.6"
                :linger="1"
                rimColor="#ffffff"
                :rim="0"
                :reverse="false"
                :wander="false"
                clickBurst
              />
            </div>
          </div>

          <div class="header-text-col">
            <h2 class="massive-title">
              NUESTROS<br/>
              SERVICIOS
            </h2>
            <p class="subtitle-text">
              Cada servicio está pensado para resolver una necesidad real, no para usar tecnología por moda.
            </p>
          </div>
        </div>
      </div>

      <!-- Infinite Marquee Services (Interactive Drag & Accelerate) -->
      <div class="services-marquee">
        <div 
          class="marquee-track" 
          ref="marqueeTrackRef"
          :class="{ 'is-dragging': isDragging }"
          @pointerdown="onPointerDown"
          @pointermove="onPointerMove"
          @pointerup="onPointerUp"
          @pointercancel="onPointerUp"
        >
          <!-- Set A -->
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
          <!-- Set B -->
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
          <!-- Set C -->
          <div v-for="(service, index) in services" :key="'c-' + index" class="service-card">
            <div class="card-header">
              <span class="service-index">0{{ index + 1 }}</span>
              <div class="card-line"></div>
            </div>
            <div class="card-body">
              <h3 class="service-name">{{ service.title }}</h3>
              <p class="service-desc">{{ service.description }}</p>
            </div>
          </div>
          <!-- Set D -->
          <div v-for="(service, index) in services" :key="'d-' + index" class="service-card">
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
import { onMounted, onBeforeUnmount, ref } from 'vue';
import DitherVeil from './DitherVeil.vue';

const headerRef = ref(null);
const marqueeTrackRef = ref(null);
const isDragging = ref(false);

let currentX = 0;
let currentSpeed = -1.2;
const baseSpeed = -1.2;
let oneSetWidth = 0;
let rafId = null;

let isPointerDown = false;
let lastX = 0;
let lastTime = 0;
let velocity = 0;
let activePointerId = null;

const measureWidth = () => {
  if (marqueeTrackRef.value) {
    // 4 sets rendered, so 1 set is 1/4 of total scroll width
    oneSetWidth = marqueeTrackRef.value.scrollWidth / 4;
  }
};

const animate = () => {
  if (!isPointerDown) {
    // Smoothly return towards baseSpeed if it was boosted by a flick
    currentSpeed += (baseSpeed - currentSpeed) * 0.045;
    currentX += currentSpeed;

    if (oneSetWidth > 0) {
      while (currentX <= -oneSetWidth) currentX += oneSetWidth;
      while (currentX > 0) currentX -= oneSetWidth;
    }

    if (marqueeTrackRef.value) {
      marqueeTrackRef.value.style.transform = `translate3d(${currentX}px, 0, 0)`;
    }
  }

  rafId = requestAnimationFrame(animate);
};

const onPointerDown = (e) => {
  isPointerDown = true;
  isDragging.value = true;
  activePointerId = e.pointerId;
  lastX = e.clientX;
  lastTime = performance.now();
  velocity = 0;

  if (marqueeTrackRef.value) {
    try {
      marqueeTrackRef.value.setPointerCapture(e.pointerId);
    } catch {}
  }
};

const onPointerMove = (e) => {
  if (!isPointerDown) return;
  const now = performance.now();
  const dt = Math.max(8, now - lastTime);
  const dx = e.clientX - lastX;

  // Instant velocity in px/frame (~16ms)
  const instantVel = (dx / dt) * 16.6;
  velocity = velocity * 0.3 + instantVel * 0.7;

  currentX += dx;

  if (oneSetWidth > 0) {
    while (currentX <= -oneSetWidth) currentX += oneSetWidth;
    while (currentX > 0) currentX -= oneSetWidth;
  }

  if (marqueeTrackRef.value) {
    marqueeTrackRef.value.style.transform = `translate3d(${currentX}px, 0, 0)`;
  }

  lastX = e.clientX;
  lastTime = now;
};

const onPointerUp = (e) => {
  if (!isPointerDown) return;
  isPointerDown = false;
  isDragging.value = false;

  // When released, if dragged/pulled with speed, accelerate in that direction with momentum!
  if (Math.abs(velocity) > 0.4) {
    const dir = Math.sign(velocity);
    const boost = Math.min(Math.abs(velocity) * 1.8, 32);
    currentSpeed = dir * boost;
  } else {
    currentSpeed = baseSpeed;
  }

  if (marqueeTrackRef.value && activePointerId !== null) {
    try {
      marqueeTrackRef.value.releasePointerCapture(activePointerId);
    } catch {}
  }
  activePointerId = null;
};

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

  measureWidth();
  rafId = requestAnimationFrame(animate);
  window.addEventListener('resize', measureWidth);
  document.fonts?.ready?.then(measureWidth);
});

onBeforeUnmount(() => {
  if (rafId) cancelAnimationFrame(rafId);
  window.removeEventListener('resize', measureWidth);
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
  margin-bottom: 0;
  position: relative;
  z-index: 2;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 40px;
}

.header-dither-col {
  flex: 1 1 52%;
  display: flex;
  justify-content: flex-start;
  margin-bottom: -110px;
  position: relative;
  z-index: 3;
}

.dither-card {
  width: 100%;
  max-width: 680px;
  height: 640px;
  overflow: visible;
  position: relative;
  background: transparent;
  box-shadow: none;
  border: none;
  -webkit-mask-image: linear-gradient(to bottom, black 75%, transparent 98%);
  mask-image: linear-gradient(to bottom, black 75%, transparent 98%);
}

.header-text-col {
  flex: 1 1 48%;
  max-width: 620px;
  padding-left: 20px;
  z-index: 3;
}

.massive-title {
  font-family: var(--font-heading);
  font-size: clamp(3.2rem, 6.8vw, 7.5rem);
  font-weight: 800;
  line-height: 0.88;
  letter-spacing: -0.05em;
  color: #000;
  margin: 0;
  text-transform: uppercase;
}

.subtitle-text {
  font-size: 1.25rem;
  color: #000;
  margin-top: 24px;
  max-width: 500px;
  font-weight: 400;
  line-height: 1.4;
  opacity: 0.85;
}

@media (max-width: 992px) {
  .services-section {
    padding: 120px 0 60px 0;
  }

  .header-container {
    margin-bottom: 30px;
  }

  .header-content {
    flex-direction: column-reverse;
    align-items: center;
    gap: 24px;
  }
  
  .header-dither-col {
    width: 100%;
    max-width: 100%;
    justify-content: center;
    margin-bottom: -40px;
    position: relative;
    z-index: 2;
  }

  .dither-card {
    max-width: 500px;
    height: 420px;
    margin: 0 auto;
  }

  .header-text-col {
    max-width: 100%;
    padding-left: 0;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    z-index: 3;
  }

  .massive-title {
    text-align: center;
    font-size: clamp(2.8rem, 10vw, 5rem);
    line-height: 0.88;
  }

  .subtitle-text {
    text-align: center;
    font-size: clamp(0.95rem, 3.4vw, 1.2rem);
    line-height: 1.4;
    margin-top: 14px;
    margin-left: auto;
    margin-right: auto;
    max-width: 600px;
  }
}

@media (max-width: 576px) {
  .services-section {
    padding: 95px 0 50px 0;
  }

  .container {
    padding: 0 5%;
  }

  .dither-card {
    max-width: 100%;
    height: 380px;
  }

  .massive-title {
    font-size: clamp(2.3rem, 11vw, 3.4rem);
  }

  .subtitle-text {
    font-size: 0.95rem;
    margin-top: 10px;
  }
}

/* Marquee Logic (Interactive drag, velocity & smooth infinite loop) */
.services-marquee {
  position: relative;
  width: 100%;
  padding: 40px 0;
  overflow: hidden;
  touch-action: pan-y;
}

.marquee-track {
  display: flex;
  width: max-content;
  will-change: transform;
  cursor: grab;
  user-select: none;
  -webkit-user-select: none;
  touch-action: pan-y;
}

.marquee-track.is-dragging {
  cursor: grabbing;
}

/* Hover effect on cards when not dragging */
.marquee-track:not(.is-dragging):hover .service-card {
  filter: blur(6px);
  opacity: 0.35;
  transform: scale(0.96);
}

.marquee-track:not(.is-dragging) .service-card:hover {
  filter: blur(0);
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.4s ease, filter 0.4s ease;
  opacity: 1;
  transform: scale(1.06);
  z-index: 10;
}

.service-card {
  width: 400px;
  margin-right: 60px;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 30px;
  user-select: none;
  -webkit-user-select: none;
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
