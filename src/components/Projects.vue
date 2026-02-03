<template>
  <div id="projects" class="projects-wrapper">
    <!-- Static Title Section -->
    <div class="title-section">
      <div class="container">
        <div class="text-reveal fade-up">
          <h2 class="section-title">CASOS DE ÉXITO</h2>
          <p class="section-subtitle">Transformamos problemas reales en soluciones digitales que funcionan. Cada caso de éxito refleja nuestro enfoque: entender la necesidad, diseñar con propósito y entregar resultados.</p>
        </div>
      </div>
    </div>

    <!-- Main Showcase Grid -->
    <section class="projects-grid-section">
      <div class="container-wide">
        <div class="projects-masonry">
          <div 
            v-for="(service, index) in services" 
            :key="index"
            class="project-tile fade-up"
            :class="`tile-${index + 1}`"
            @click="openGallery(index)"
          >
            <!-- Background Gallery -->
            <div class="tile-bg">
              <div class="slideshow-inner">
                <template v-for="(asset, aIndex) in service.assets" :key="asset.src">
                  <div 
                    class="asset-layer"
                    :class="{ 'active': isAssetActive(index, aIndex) }"
                  >
                    <video 
                      v-if="asset.type === 'video'"
                      autoplay muted loop playsinline class="tile-media"
                    >
                      <source :src="asset.src" type="video/mp4">
                    </video>
                    <img 
                      v-else 
                      :src="asset.src" 
                      class="tile-media" 
                      loading="lazy"
                    />
                  </div>
                </template>
              </div>
              <!-- Lightened overlay for better background visibility -->
              <div class="tile-overlay"></div>
            </div>

            <!-- Content Overlay -->
            <div class="tile-content">
              <div class="content-header">
                <span class="count">{{ service.assets.length }} ARCHIVOS</span>
                <h3 class="tile-title">{{ service.title }}</h3>
              </div>
              <div class="content-footer">
                <p class="tile-desc">{{ service.description }}</p>
                <div class="view-btn">
                  <span>Ver Galería</span>
                  <div class="arrow">→</div>
                </div>
              </div>
            </div>

            <!-- Asset Progress Indicator -->
            <div class="progress-bar-container">
              <div 
                v-for="(_, pIdx) in service.assets" 
                :key="pIdx"
                class="progress-segment"
                :class="{ 'active': previewCycles[index] === pIdx }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Fullscreen Gallery Modal (Teleported to body to avoid transform issues) -->
    <Teleport to="body">
      <Transition name="modal">
        <div v-if="selectedIndex !== null && services[selectedIndex]" class="gallery-modal" @click.self="resetSelection">
          <div class="modal-header">
            <div class="modal-info">
              <span class="modal-index">CATEGORÍA 0{{ selectedIndex + 1 }}</span>
              <h2 class="modal-title">{{ services[selectedIndex].title }}</h2>
            </div>
            <button class="close-gallery" @click="resetSelection">
              <span>VOLVER</span>
              <div class="close-icon-circle">×</div>
            </button>
          </div>

          <div class="gallery-body">
            <div class="active-media-container">
              <Transition :name="transitionName" mode="out-in">
                <div :key="services[selectedIndex].assets[currentAssetIndex].src" class="active-media">
                  <video 
                    v-if="services[selectedIndex].assets[currentAssetIndex].type === 'video'"
                    autoplay muted loop playsinline class="media-content"
                  >
                    <source :src="services[selectedIndex].assets[currentAssetIndex].src" type="video/mp4">
                  </video>
                  <img v-else :src="services[selectedIndex].assets[currentAssetIndex].src" class="media-content" />
                </div>
              </Transition>
            </div>
            
            <!-- Thumbnails Track -->
            <div class="thumbnails-container">
              <div class="thumbnails-track">
                <div 
                  v-for="(asset, idx) in services[selectedIndex].assets" 
                  :key="idx" 
                  class="thumb"
                  :class="{ 'active': currentAssetIndex === idx }"
                  @click="goToAsset(idx)"
                >
                  <video v-if="asset.type === 'video'" class="thumb-media">
                    <source :src="asset.src" type="video/mp4">
                  </video>
                  <img v-else :src="asset.src" class="thumb-media" />
                  <div v-if="asset.type === 'video'" class="play-hint">▶</div>
                </div>
              </div>
            </div>
          </div>

          <!-- Navigation -->
          <button class="nav-arrow prev" @click="prevAsset">‹</button>
          <button class="nav-arrow next" @click="nextAsset">›</button>
        </div>
      </Transition>
    </Teleport>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// Assets Imports
import web1 from '../assets/materiales (fotos y videos)/Pagina web/Elegance Eventos/principal.JPG';
import web2 from '../assets/materiales (fotos y videos)/Pagina web/Elegance Eventos/descripcion.JPG';
import web3 from '../assets/materiales (fotos y videos)/Pagina web/Patitas spa/patitasSpa.JPG';
import app1 from '../assets/materiales (fotos y videos)/Aplicaciones Web/NiñoSano/dashboard.JPG';
import app2 from '../assets/materiales (fotos y videos)/Aplicaciones Web/NiñoSano/gestion de usuarios.JPG';
import app3 from '../assets/materiales (fotos y videos)/Aplicaciones Web/NiñoSano/login.JPG';
import app4 from '../assets/materiales (fotos y videos)/Aplicaciones Web/Colegio-X/dashboard.JPG';
import app5 from '../assets/materiales (fotos y videos)/Aplicaciones Web/Colegio-X/cursos.JPG';
import app6 from '../assets/materiales (fotos y videos)/Aplicaciones Web/Colegio-X/gestion de usuarios.JPG';
import app7 from '../assets/materiales (fotos y videos)/Aplicaciones Web/Colegio-X/login.JPG';
import eco1 from '../assets/materiales (fotos y videos)/E-commerce/principal.JPG';
import eco2 from '../assets/materiales (fotos y videos)/E-commerce/categorias.JPG';
import eco3 from '../assets/materiales (fotos y videos)/E-commerce/productos.JPG';
import land1 from '../assets/materiales (fotos y videos)/Landing pages/ericka/efecto.mp4';
import land2 from '../assets/materiales (fotos y videos)/Landing pages/ericka/principal.JPG';
import port1 from '../assets/materiales (fotos y videos)/Portafolio digital/Claudio Portafolio digital.mp4';
import port2 from '../assets/materiales (fotos y videos)/Portafolio digital/carlos Portafolio digital.mp4';

const isVisible = ref(false);
const selectedIndex = ref(null);
const currentAssetIndex = ref(0);
const previewCycles = ref([0, 0, 0, 0, 0]);
const transitionName = ref('media-fade-next');

const services = [
  {
    title: "Páginas web",
    description: "Diseños corporativos de alto impacto visual y elegancia.",
    assets: [
      { src: web1, type: 'image' },
      { src: web2, type: 'image' },
      { src: web3, type: 'image' }
    ]
  },
  {
    title: "Apps Institucionales",
    description: "Sistemas robustos y dashboards de gestión avanzada.",
    assets: [
      { src: app3, type: 'image' }, { src: app2, type: 'image' }, { src: app1, type: 'image' },
      { src: app7, type: 'image' }, { src: app4, type: 'image' }, { src: app5, type: 'image' }, 
      { src: app6, type: 'image' }      
    ]
  },
  {
    title: "E-commerce",
    description: "Tiendas virtuales optimizadas para máxima conversión.",
    assets: [
      { src: eco1, type: 'image' }, { src: eco2, type: 'image' }, { src: eco3, type: 'image' }
    ]
  },
  {
    title: "Landing Page",
    description: "Páginas de aterrizaje con tecnologías de última generación.",
    assets: [
      { src: land2, type: 'image' }, { src: land1, type: 'video' }
    ]
  },
  {
    title: "Portafolio digital",
    description: "Vitrinas interactivas para una marca personal de élite.",
    assets: [
      { src: port1, type: 'video' }, { src: port2, type: 'video' }
    ]
  }
];

let cycleInterval;
const startCycle = () => {
  cycleInterval = setInterval(() => {
    previewCycles.value = previewCycles.value.map((current, idx) => {
      return (current + 1) % services[idx].assets.length;
    });
  }, 3500);
};

const isAssetActive = (serviceIdx, assetIdx) => {
  return previewCycles.value[serviceIdx] === assetIdx;
};

const openGallery = (idx) => {
  selectedIndex.value = idx;
  currentAssetIndex.value = 0;
  document.body.style.overflow = 'hidden';
};

const resetSelection = () => {
  selectedIndex.value = null;
  document.body.style.overflow = 'auto';
};

const goToAsset = (idx) => {
  transitionName.value = idx > currentAssetIndex.value ? 'media-fade-next' : 'media-fade-prev';
  currentAssetIndex.value = idx;
};

const nextAsset = () => {
  transitionName.value = 'media-fade-next';
  currentAssetIndex.value = (currentAssetIndex.value + 1) % services[selectedIndex.value].assets.length;
};

const prevAsset = () => {
  transitionName.value = 'media-fade-prev';
  currentAssetIndex.value = (currentAssetIndex.value - 1 + services[selectedIndex.value].assets.length) % services[selectedIndex.value].assets.length;
};

onMounted(() => {
  startCycle();
});

onUnmounted(() => {
  clearInterval(cycleInterval); 
});
</script>

<style scoped>
/* CSS styles truncated for brevity as they are presumed unchanged from previous successful intent */
.projects-wrapper { background-color: #050505; min-height: 100vh; padding-bottom: 15vh; }
.title-section { padding: 140px 0 60px; }
.section-title { font-family: var(--font-heading); font-size: clamp(3rem, 10vw, 7.5rem); font-weight:300 ; color: #fff; letter-spacing: -4px; line-height: 0.85; margin-bottom: 2rem; }
.section-subtitle { 
  font-size: clamp(1.1rem, 2vw, 1.4rem); 
  color: rgba(255,255,255,0.5);   
  line-height: 1.6; 
  margin-top: 2.5rem;
  font-weight: 300;
  letter-spacing: -0.01em;
}
.projects-grid-section { padding: 0 40px; }
.container-wide { max-width: 1900px; margin: 0 auto; }
.projects-masonry { display: grid; grid-template-columns: repeat(12, 1fr); grid-auto-rows: 240px; gap: 1.5rem; }
.project-tile { position: relative; border-radius: 32px; overflow: hidden; cursor: pointer; background: #111; }
.tile-1 { grid-column: span 8; grid-row: span 2; }
.tile-2 { grid-column: span 4; grid-row: span 3; }
.tile-3 { grid-column: span 4; grid-row: span 2; }
.tile-4 { grid-column: span 4; grid-row: span 1; }
.tile-5 { grid-column: span 8; grid-row: span 1; }
.tile-bg { position: absolute; inset: 0; z-index: 1; }
.slideshow-inner { position: relative; width: 100%; height: 100%; }
.asset-layer { position: absolute; inset: 0; opacity: 0; transition: opacity 1.2s ease-in-out, transform 8s linear; }
.asset-layer.active { opacity: 1; transform: scale(1.05); }
.tile-media { width: 100%; height: 100%; object-fit: cover; }
.tile-media.bg-img { background-size: cover; background-position: center; }
.tile-overlay {
  position: absolute;
  inset: 0;
  /* Darkened overlay to improve text contrast */
  background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0.4) 50%, rgba(0,0,0,0.2) 100%);
  z-index: 2;
  transition: opacity 0.4s ease;
}
.project-tile:hover .tile-overlay { background: linear-gradient(to top, rgba(0,0,0,0.9) 0%, rgba(0,0,0,0.5) 60%, rgba(0,0,0,0.3) 100%); }
.tile-content { position: absolute; inset: 0; z-index: 5; padding: 3rem; display: flex; flex-direction: column; justify-content: space-between; }
.count { font-family: var(--font-heading); font-size: 0.75rem; font-weight: 700; color: var(--accent-primary); letter-spacing: 2px; }
.tile-title { font-size: clamp(1.8rem, 3vw, 3.5rem); font-weight: 800; color: #fff; line-height: 0.95; margin-top: 0.5rem; }
.content-footer { transform: translateY(30px); opacity: 0; transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
.project-tile:hover .content-footer { transform: translateY(0); opacity: 1; }
.tile-desc { font-size: 1rem; color: rgba(255,255,255,0.7); margin-bottom: 2rem; max-width: 80%; }
.view-btn { display: flex; align-items: center; gap: 15px; color: #fff; font-weight: 700; letter-spacing: 1px; }
.arrow { width: 40px; height: 40px; border-radius: 50%; border: 1px solid rgba(255,255,255,0.3); display: flex; align-items: center; justify-content: center; transition: all 0.3s ease; }
.project-tile:hover .arrow { background: #fff; color: #000; }
.progress-bar-container { position: absolute; top: 3rem; right: 3rem; z-index: 6; display: flex; gap: 6px; }
.progress-segment { width: 30px; height: 2px; background: rgba(255,255,255,0.2); border-radius: 10px; overflow: hidden; }
.progress-segment.active { background: var(--accent-primary); }
.gallery-modal { position: fixed; inset: 0; z-index: 999999; background: #080808; display: flex; flex-direction: column; }
.modal-header { padding: 30px 60px; display: flex; justify-content: space-between; align-items: center; z-index: 10; }
.modal-index { color: var(--accent-primary); font-weight: 700; letter-spacing: 3px; font-size: 0.8rem; }
.modal-title { font-size: clamp(1.5rem, 3vw, 2.5rem); font-weight: 800; color: #fff; line-height: 1.1; }
.close-gallery { background: transparent; border: none; color: #fff; display: flex; align-items: center; gap: 20px; cursor: pointer; font-weight: 700; letter-spacing: 2px; }
.close-icon-circle { width: 60px; height: 60px; border: 1px solid rgba(255,255,255,0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 2.5rem; transition: all 0.4s ease; }
.close-gallery:hover .close-icon-circle { background: #fff; color: #000; }
.gallery-body { flex: 1; display: flex; flex-direction: column; position: relative; overflow: hidden; }
.active-media-container { flex: 1; display: flex; align-items: center; justify-content: center; position: relative; padding: 20px 40px; overflow: hidden; }
.active-media { width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; }
.media-content { max-width: 100%; max-height: 100%; object-fit: contain; border-radius: 12px; box-shadow: 0 40px 80px rgba(0,0,0,0.8); }
.nav-arrow { position: absolute; top: 50%; transform: translateY(-50%); background: transparent; border: none; color: #fff; font-size: 5rem; opacity: 0.2; cursor: pointer; z-index: 20; padding: 2rem; transition: all 0.3s; }
.nav-arrow:hover { opacity: 1; transform: translateY(-50%) scale(1.1); }
.prev { left: 40px; }
.next { right: 40px; }
.thumbnails-container { padding: 20px 0 40px; display: flex; justify-content: center; }
.thumbnails-track { display: flex; gap: 12px; background: rgba(255,255,255,0.03); padding: 10px; border-radius: 20px; backdrop-filter: blur(20px); }
.thumb { width: 100px; height: 60px; border-radius: 12px; overflow: hidden; opacity: 0.3; cursor: pointer; transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1); border: 2px solid transparent; position: relative; }
.thumb.active { opacity: 1; transform: scale(1.1) translateY(-5px); border-color: var(--accent-primary); box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
.thumb-media { width: 100%; height: 100%; object-fit: cover; }
.play-hint { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; color: #fff; background: rgba(0,0,0,0.4); font-size: 0.8rem; }
.modal-enter-active, .modal-leave-active { transition: all 0.9s cubic-bezier(0.16, 1, 0.3, 1); }
.modal-enter-from, .modal-leave-to { opacity: 0; transform: translateY(100vh) scale(0.9); }
.media-fade-next-enter-active, .media-fade-next-leave-active, .media-fade-prev-enter-active, .media-fade-prev-leave-active { transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1); }
.media-fade-next-enter-from { opacity: 0; transform: translateX(100px) scale(0.9); }
.media-fade-next-leave-to { opacity: 0; transform: translateX(-100px) scale(0.9); }
.media-fade-prev-enter-from { opacity: 0; transform: translateX(-100px) scale(0.9); }
.media-fade-prev-leave-to { opacity: 0; transform: translateX(100px) scale(0.9); }
@media (max-width: 1200px) { .projects-masonry { grid-template-columns: 1fr 1fr; grid-auto-rows: 400px; } .tile-1, .tile-2, .tile-3, .tile-4, .tile-5 { grid-column: span 1; grid-row: span 1; } .modal-header { padding: 40px; } .active-media-container { padding: 0 5%; } }
@media (max-width: 768px) { .projects-masonry { grid-template-columns: 1fr; } }
</style>
