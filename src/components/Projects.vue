<template>
  <section id="projects" class="projects">
    <!-- Crossed Marquee Background -->
    <div class="marquee-x-background">
      <div class="marquee-container pos-rotated">
        <div class="marquee-track">
          <div class="marquee-content">
            <span v-for="(item, index) in [...items, ...items, ...items]" :key="index" class="marquee-item">
              {{ item }} <span class="separator">/</span>
            </span>
          </div>
        </div>
      </div>
      
      <div class="marquee-container neg-rotated">
        <div class="marquee-track reverse-anim">
          <div class="marquee-content">
            <span v-for="(item, index) in [...itemsReverse, ...itemsReverse, ...itemsReverse]" :key="index" class="marquee-item italic">
              {{ item }} <span class="separator">/</span>
            </span>
          </div>
        </div>
      </div>
    </div>

    <!-- Content Overlay -->
    <div class="container content-overlay">
      <div class="text-block-right" :class="{ 'visible': isVisible }">
        <h2 class="section-title">¿Que hacemos?</h2>
        <p class="description">
          En Helify fusionamos <span>diseño editorial</span> con <span>arquitectura de software</span> de vanguardia. 
          Desarrollamos ecosistemas digitales que no solo capturan la atención, sino que están construidos para escalar y perdurar.
        </p>
        
        <div class="mini-grid">
          <div class="mini-item">
            <span class="dot"></span>
            <div>
              <h4>Desarrollo Web</h4>
              <p>Experiencias inmersivas y optimizadas.</p>
            </div>
          </div>
          <div class="mini-item">
            <span class="dot"></span>
            <div>
              <h4>Automatización</h4>
              <p>Eficiencia pura para tus procesos.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const isVisible = ref(false);

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true;
  }, 100);
});

const items = ["LANDING PAGES", "AUTOMATIZACIONES", "APLICACIONES WEB", "eCOMMERCE", "SISTEMAS SaaS"];
const itemsReverse = ["UI/UX DESIGN", "OPTIMIZACIÓN SEO", "INTEGRACIONES API", "CLOUD ARCHITECTURE", "CYBERSECURITY"];
</script>

<style scoped>
.projects {
  padding: 15rem 0;
  min-height: 90vh;
  background-color: var(--bg-primary);
  overflow: hidden;
  position: relative;
  display: flex;
  align-items: center;
  scroll-margin-top: 100px;
}

/* Marquee X Style */
.marquee-x-background {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 150vw;
  z-index: 1;
  pointer-events: none;
  opacity: 0.15; /* Subdued background effect */
}

.marquee-container {
  width: 100%;
  overflow: hidden;
  display: flex;
  padding: 1rem 0;
}

.pos-rotated {
  transform: rotate(10deg) translateY(-50PX);
}

.neg-rotated {
  transform: rotate(-10deg) translateY(50PX);
}

.marquee-track {
  display: flex;
  white-space: nowrap;
  animation: marquee 50s linear infinite;
}

.reverse-anim {
  animation-direction: reverse;
}

@keyframes marquee {
  0% { transform: translateX(0); }
  100% { transform: translateX(-33.33%); }
}

.marquee-item {
  font-family: var(--font-heading);
  font-size: clamp(2rem, 5vw, 4rem);
  font-weight: 800;
  color: var(--text-primary);
  padding: 0 2rem;
  letter-spacing: 2px;
}

.italic {
  font-style: italic;
  font-weight: 400;
  -webkit-text-stroke: 1px var(--text-primary);
  color: transparent;
}

.separator {
  margin-left: 2rem;
  color: var(--accent-primary);
}

/* Content Overlay */
.content-overlay {
  position: relative;
  z-index: 2;
  display: flex;
  justify-content: flex-end;
  width: 100%;
}

.text-block-right {
  max-width: 500px;
  text-align: right;
  background: rgba(10, 10, 10, 0.95);
  backdrop-filter: blur(20px);
  padding: 3.5rem;
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  opacity: 0;
  transform: translateX(50px);
  transition: all 1s cubic-bezier(0.2, 1, 0.3, 1);
}

.text-block-right.visible {
  opacity: 1;
  transform: translateX(0);
}

.section-title {
  font-size: clamp(2.5rem, 5vw, 4rem);
  margin-bottom: 2rem;
  color: var(--text-primary);
  text-transform: uppercase;
}

.description {
  font-size: 1.1rem;
  line-height: 1.8;
  color: var(--text-secondary);
  margin-bottom: 3rem;
}

.description span {
  color: var(--accent-primary);
  font-weight: 600;
}

.mini-grid {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  align-items: flex-end;
}

.mini-item {
  display: flex;
  gap: 1.5rem;
  align-items: flex-start;
  text-align: right;
}

.mini-item h4 {
  font-size: 1.1rem;
  color: var(--text-primary);
  margin-bottom: 0.3rem;
}

.mini-item p {
  font-size: 0.9rem;
  color: var(--text-secondary);
}

.dot {
  width: 8px;
  height: 8px;
  background: var(--accent-primary);
  border-radius: 50%;
  margin-top: 0.5rem;
}

@media (max-width: 1024px) {
  .projects {
    padding: 10rem 0;
  }
  
  .content-overlay {
    justify-content: center;
  }
  
  .text-block-right {
    text-align: center;
    max-width: 100%;
  }
  
  .mini-grid {
    align-items: center;
  }
  
  .mini-item {
    text-align: center;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
  }
  
  .marquee-x-background {
    width: 200vw;
  }
}
</style>
