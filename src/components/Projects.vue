<template>
  <div id="projects" class="projects-wrapper">
    <!-- Static Title Section -->
    <div class="title-section">
      <div class="container">
        <div class="text-reveal fade-up header-flex">
          <h2 class="section-title">CASOS DE ÉXITO</h2>
          <div class="subtitle-box">
             <div class="auth-badge">
               <span class="pulse-dot"></span>
               <span>Autorizado por clientes</span>
             </div>
             <p class="section-subtitle">
               Explora una selección de los proyectos digitales más destacados que hemos construido, compartidos públicamente con la autorización exclusiva de nuestros clientes.
             </p>
          </div>
        </div>
      </div>
    </div>

    <!-- 3D Carousel Section with Swiper -->
    <section class="projects-carousel-3d-section">
      <div class="container-wide">
        <Swiper
          :effect="'coverflow'"
          :grabCursor="true"
          :centeredSlides="true"
          :slidesPerView="'auto'"
          :loop="true"
          :speed="5000"
          :mousewheel="{ forceToAxis: true }"
          :coverflowEffect="{
            rotate: 25,
            stretch: 0,
            depth: 250,
            modifier: 1,
            slideShadows: true,
          }"
          :autoplay="{
            delay: 0,
            disableOnInteraction: false,
            pauseOnMouseEnter: true,
          }"
          :modules="modules"
          class="projects-swiper"
        >
          <SwiperSlide 
            v-for="(service, index) in services" 
            :key="index"
            class="swiper-card"
            @click="openGallery(index)"
          >
            <div class="card-media-wrapper">
              <video 
                v-if="service.assets[0].type === 'video'"
                autoplay muted loop playsinline class="card-media"
              >
                <source :src="service.assets[0].src" type="video/mp4">
              </video>
              <img 
                v-else 
                :src="service.assets[0].src" 
                class="card-media" 
                loading="lazy"
              />
            </div>
            
            <div class="card-content-overlay">
              <h3 class="card-title">{{ service.title }}</h3>
              <p class="card-desc">{{ service.description }}</p>
              <div class="view-btn">
                <span>Ver Galería</span>
                <div class="arrow">→</div>
              </div>
            </div>
          </SwiperSlide>
        </Swiper>
      </div>
    </section>

    <!-- Fullscreen Gallery Modal (Teleported to body to avoid transform issues) -->
    <Teleport to="body">
      <Transition name="modal">
        <div v-if="selectedIndex !== null && services[selectedIndex]" class="gallery-modal" @click.self="resetSelection">
          <div class="modal-header">
            <div class="modal-info">
              <span class="modal-index">PROYECTO 0{{ selectedIndex + 1 }}</span>
              <h2 class="modal-title">{{ services[selectedIndex].title }}</h2>
            </div>
            <button class="close-gallery" @click="resetSelection">
              <span>CERRAR</span>
              <div class="close-icon-circle">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                  <line x1="18" y1="6" x2="6" y2="18"></line>
                  <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
              </div>
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
          <button class="nav-arrow prev" @click="prevAsset">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="15 18 9 12 15 6"></polyline>
            </svg>
          </button>
          <button class="nav-arrow next" @click="nextAsset">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="9 18 15 12 9 6"></polyline>
            </svg>
          </button>
        </div>
      </Transition>
    </Teleport>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import { EffectCoverflow, Autoplay, Mousewheel } from 'swiper/modules';
import 'swiper/css';
import 'swiper/css/effect-coverflow';

const modules = [EffectCoverflow, Autoplay, Mousewheel];

// Assets Imports
import libFamurp1 from '../assets/materiales (fotos y videos)/Biblioteca FAMURP/bibliotecafamurp.png';

import libVirtual1 from '../assets/materiales (fotos y videos)/Biblioteca Virtual y Especializada/bvedashboard.png';
import libVirtual2 from '../assets/materiales (fotos y videos)/Biblioteca Virtual y Especializada/bvemapa.png';
import libVirtual3 from '../assets/materiales (fotos y videos)/Biblioteca Virtual y Especializada/loginbve.png';

import colx1 from '../assets/materiales (fotos y videos)/Colegio-X/dashboard.JPG';
import colx2 from '../assets/materiales (fotos y videos)/Colegio-X/cursos.JPG';
import colx3 from '../assets/materiales (fotos y videos)/Colegio-X/gestion de usuarios.JPG';
import colx4 from '../assets/materiales (fotos y videos)/Colegio-X/login.JPG';

import elegance1 from '../assets/materiales (fotos y videos)/Elegance Eventos/principal.JPG';
import elegance2 from '../assets/materiales (fotos y videos)/Elegance Eventos/descripcion.JPG';

import ericka1 from '../assets/materiales (fotos y videos)/ericka/efecto.mp4';
import ericka2 from '../assets/materiales (fotos y videos)/ericka/principal.JPG';

import nexoshop1 from '../assets/materiales (fotos y videos)/NexoShop/principal.JPG';
import nexoshop2 from '../assets/materiales (fotos y videos)/NexoShop/categorias.JPG';
import nexoshop3 from '../assets/materiales (fotos y videos)/NexoShop/productos.JPG';

import ninosano1 from '../assets/materiales (fotos y videos)/NiñoSano/dashboard.JPG';
import ninosano2 from '../assets/materiales (fotos y videos)/NiñoSano/gestion de usuarios.JPG';
import ninosano3 from '../assets/materiales (fotos y videos)/NiñoSano/login.JPG';

import patitas1 from '../assets/materiales (fotos y videos)/Patitas spa/patitasSpa.JPG';

import portafolio1 from '../assets/materiales (fotos y videos)/Portafolio digital/Claudio Portafolio digital.mp4';
import portafolio2 from '../assets/materiales (fotos y videos)/Portafolio digital/carlos Portafolio digital.mp4';

import refcon1 from '../assets/materiales (fotos y videos)/REFCON mensajeria/dashboard.png';
import refcon2 from '../assets/materiales (fotos y videos)/REFCON mensajeria/refconlogin.png';

import ventassaas1 from '../assets/materiales (fotos y videos)/VentasSaas/dashboardventassaas.png';
import ventassaas2 from '../assets/materiales (fotos y videos)/VentasSaas/carritocompras.png';
import ventassaas3 from '../assets/materiales (fotos y videos)/VentasSaas/login.png';

import vidaanimal1 from '../assets/materiales (fotos y videos)/Vida Animal/loginvida.png';

const isVisible = ref(false);
const selectedIndex = ref(null);
const currentAssetIndex = ref(0);

const transitionName = ref('media-fade-next');

const services = [
  {
    title: "Biblioteca FAMURP",
    description: "Plataforma de gestión bibliotecaria y catálogo digital para estudiantes.",
    assets: [ { src: libFamurp1, type: 'image' } ]
  },
  {
    title: "Biblioteca Virtual",
    description: "Sistema avanzado de biblioteca digital con visualización de mapas y recursos.",
    assets: [ { src: libVirtual1, type: 'image' }, { src: libVirtual2, type: 'image' }, { src: libVirtual3, type: 'image' } ]
  },
  {
    title: "Colegio-X",
    description: "Intranet educativa completa con gestión de cursos, usuarios y dashboard interactivo.",
    assets: [ { src: colx1, type: 'image' }, { src: colx2, type: 'image' }, { src: colx3, type: 'image' }, { src: colx4, type: 'image' } ]
  },
  {
    title: "Elegance Eventos",
    description: "Página web corporativa enfocada en la elegancia y presentación de servicios.",
    assets: [ { src: elegance1, type: 'image' }, { src: elegance2, type: 'image' } ]
  },
  {
    title: "Landing Page Ericka",
    description: "Landing page de alto impacto con videos de fondo y diseño moderno.",
    assets: [ { src: ericka1, type: 'video' }, { src: ericka2, type: 'image' } ]
  },
  {
    title: "NexoShop",
    description: "Plataforma de comercio electrónico optimizada para conversión y categorías de productos.",
    assets: [ { src: nexoshop1, type: 'image' }, { src: nexoshop2, type: 'image' }, { src: nexoshop3, type: 'image' } ]
  },
  {
    title: "NiñoSano",
    description: "Aplicación web de gestión de salud con dashboards de seguimiento para pacientes.",
    assets: [ { src: ninosano1, type: 'image' }, { src: ninosano2, type: 'image' }, { src: ninosano3, type: 'image' } ]
  },
  {
    title: "Patitas SPA",
    description: "Sitio web para negocio de cuidado de mascotas con un diseño amigable y atractivo.",
    assets: [ { src: patitas1, type: 'image' } ]
  },
  {
    title: "Portafolios Digitales",
    description: "Vitrinas interactivas para marca personal de alto nivel.",
    assets: [ { src: portafolio1, type: 'video' }, { src: portafolio2, type: 'video' } ]
  },
  {
    title: "REFCON Mensajería",
    description: "Sistema de gestión logística y mensajería con paneles de control en tiempo real.",
    assets: [ { src: refcon1, type: 'image' }, { src: refcon2, type: 'image' } ]
  },
  {
    title: "VentasSaaS",
    description: "Software as a Service para la gestión integral de ventas y carritos de compras.",
    assets: [ { src: ventassaas1, type: 'image' }, { src: ventassaas2, type: 'image' }, { src: ventassaas3, type: 'image' } ]
  },
  {
    title: "Vida Animal",
    description: "Plataforma web enfocada en el cuidado y bienestar animal.",
    assets: [ { src: vidaanimal1, type: 'image' } ]
  }
];



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
  // Carousel mounted
});

onUnmounted(() => {
  // Cleanup
});
</script>

<style scoped>
/* CSS styles truncated for brevity as they are presumed unchanged from previous successful intent */
.projects-wrapper { background-color: #050505; min-height: 100vh; padding-bottom: 15vh; }
.title-section { padding: 140px 0 40px; }
.header-flex { display: flex; flex-direction: column; gap: 2rem; }
@media (min-width: 1024px) {
  .header-flex { flex-direction: row; align-items: flex-end; justify-content: space-between; gap: 4rem; }
  .section-title { margin-bottom: 0; }
  .subtitle-box { margin-bottom: 15px; }
}
.section-title { font-family: var(--font-heading); font-size: clamp(3rem, 10vw, 7.5rem); font-weight: 800; color: #fff; letter-spacing: -4px; line-height: 0.85; margin: 0; }
.subtitle-box { max-width: 500px; display: flex; flex-direction: column; gap: 15px; border-left: 2px solid var(--accent-primary, #10b981); padding-left: 20px; }
.auth-badge { display: inline-flex; align-items: center; gap: 10px; background: rgba(255,255,255,0.05); padding: 8px 16px; border-radius: 30px; border: 1px solid rgba(255,255,255,0.1); width: fit-content; }
.auth-badge span:last-child { font-size: 0.75rem; font-weight: 700; color: #fff; letter-spacing: 1px; text-transform: uppercase; }
.pulse-dot { width: 8px; height: 8px; background: #10b981; border-radius: 50%; box-shadow: 0 0 10px #10b981; animation: pulse 2s infinite; }
@keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(16, 185, 129, 0.7); } 70% { box-shadow: 0 0 0 10px rgba(16, 185, 129, 0); } 100% { box-shadow: 0 0 0 0 rgba(16, 185, 129, 0); } }
.section-subtitle { font-size: clamp(1rem, 1.5vw, 1.1rem); color: rgba(255,255,255,0.7); line-height: 1.6; font-weight: 300; margin: 0; letter-spacing: 0; }
.projects-carousel-3d-section { padding: 80px 0 120px; overflow: hidden; background: #050505; }
.projects-swiper { width: 100%; padding-top: 50px; padding-bottom: 50px; }
.projects-swiper :deep(.swiper-wrapper) { transition-timing-function: linear !important; }
.swiper-card { width: 700px; height: 450px; border-radius: 24px; overflow: hidden; background: #111; box-shadow: 0 20px 50px rgba(0,0,0,0.8); cursor: pointer; position: relative; }

.card-media-wrapper { width: 100%; height: 100%; position: absolute; inset: 0; }
.card-media { width: 100%; height: 100%; object-fit: cover; opacity: 0.9; transition: opacity 0.3s, transform 0.5s; }
.swiper-slide-active .card-media { opacity: 1; }
.swiper-card:hover .card-media { transform: scale(1.05); }

.card-content-overlay { position: absolute; inset: 0; background: linear-gradient(to top, rgba(0,0,0,0.95) 0%, rgba(0,0,0,0.6) 50%, rgba(0,0,0,0.1) 100%); display: flex; flex-direction: column; justify-content: flex-end; padding: 40px; opacity: 0; transition: opacity 0.4s; }
.swiper-slide-active:hover .card-content-overlay { opacity: 1; }

.card-title { color: #fff; font-family: var(--font-heading); font-size: 2.5rem; font-weight: 800; margin-bottom: 10px; transform: translateY(20px); transition: transform 0.4s; }
.card-desc { color: rgba(255,255,255,0.7); font-size: 1.1rem; line-height: 1.5; margin-bottom: 20px; transform: translateY(20px); transition: transform 0.4s 0.1s; }
.view-btn { display: flex; align-items: center; gap: 15px; color: var(--accent-primary, #fff); font-weight: 700; letter-spacing: 1px; transform: translateY(20px); transition: transform 0.4s 0.2s; }
.arrow { width: 40px; height: 40px; border-radius: 50%; border: 1px solid rgba(255,255,255,0.3); display: flex; align-items: center; justify-content: center; transition: all 0.3s ease; }
.swiper-card:hover .arrow { background: #fff; color: #000; }
.swiper-card:hover .card-title, .swiper-card:hover .card-desc, .swiper-card:hover .view-btn { transform: translateY(0); }
.gallery-modal { position: fixed; inset: 0; z-index: 999999; background: rgba(10, 10, 10, 0.85); backdrop-filter: blur(20px); display: flex; flex-direction: column; }
.modal-header { padding: 40px 60px; display: flex; justify-content: space-between; align-items: center; z-index: 10; border-bottom: 1px solid rgba(255,255,255,0.05); }
.modal-index { color: var(--accent-primary, #10b981); font-weight: 700; letter-spacing: 4px; font-size: 0.75rem; text-transform: uppercase; margin-bottom: 5px; display: block; }
.modal-title { font-size: clamp(1.5rem, 3vw, 2.5rem); font-weight: 800; color: #fff; line-height: 1.1; margin: 0; }
.close-gallery { background: transparent; border: none; color: #fff; display: flex; align-items: center; gap: 15px; cursor: pointer; font-weight: 600; letter-spacing: 2px; transition: color 0.3s; }
.close-icon-circle { width: 50px; height: 50px; border: 1px solid rgba(255,255,255,0.2); border-radius: 50%; display: flex; align-items: center; justify-content: center; transition: all 0.4s ease; background: rgba(255,255,255,0.03); }
.close-gallery:hover { color: var(--accent-primary, #10b981); }
.close-gallery:hover .close-icon-circle { background: #fff; color: #000; border-color: #fff; transform: rotate(90deg); }
.gallery-body { flex: 1; display: flex; flex-direction: column; position: relative; overflow: hidden; }
.active-media-container { flex: 1; display: flex; align-items: center; justify-content: center; position: relative; padding: 20px 40px; overflow: hidden; }
.active-media { width: 100%; height: 100%; display: flex; align-items: center; justify-content: center; }
.media-content { max-width: 100%; max-height: 100%; object-fit: contain; border-radius: 12px; box-shadow: 0 40px 80px rgba(0,0,0,0.8); }
.nav-arrow { position: absolute; top: 50%; transform: translateY(-50%); background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.1); border-radius: 50%; width: 70px; height: 70px; color: #fff; display: flex; align-items: center; justify-content: center; cursor: pointer; z-index: 20; transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1); backdrop-filter: blur(10px); }
.nav-arrow:hover { background: #fff; color: #000; transform: translateY(-50%) scale(1.1); box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
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
@media (max-width: 1200px) { .swiper-card { width: 550px; height: 350px; } .modal-header { padding: 40px; } .active-media-container { padding: 0 5%; } }
@media (max-width: 768px) { .swiper-card { width: 350px; height: 450px; } }
@media (max-width: 480px) { .swiper-card { width: 280px; height: 400px; } }
</style>
