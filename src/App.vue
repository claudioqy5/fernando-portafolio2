<template>
  <Navbar />
  
  <div class="app-wrapper" :class="{ 'is-loaded': isLoaded }">
    <Hero />
    <Projects />
    <About />
    <WhoWeAre />
    <Footer />
  </div>

  <!-- Global Floating WA Button -->
  <a href="https://wa.me/51962956919?text=¡Hola Helify! 👋 Me gustaría recibir más información sobre sus servicios digitales. ¿Podrían ayudarme?" target="_blank" class="global-wa-btn" title="Contactar por WhatsApp">
    <img src="./assets/logowsp.png" alt="WhatsApp" class="wa-icon" />
  </a>

  <!-- Global Service Modal -->
  <Transition name="fade">
    <div v-if="showModal" class="modal-overlay" @click.self="showModal = false">
      <div class="modal-content fade-up-modal">
        <button class="close-btn" @click="showModal = false">✕</button>
        
        <div class="modal-header">
          <h3>NUEVO PROYECTO</h3>
          <p>Cuéntanos qué tienes en mente y hagámoslo realidad.</p>
        </div>

        <form @submit.prevent="sendServiceRequest" class="modal-form">
          <div class="form-row">
            <div class="form-group">
              <label>NOMBRE</label>
              <input type="text" v-model="serviceForm.name" required placeholder="Tu nombre" />
            </div>
            <div class="form-group">
              <label>TELÉFONO</label>
              <input type="tel" v-model="serviceForm.phone" required placeholder="+51 ..." />
            </div>
          </div>

          <div class="form-group">
            <label>SERVICIO INTERESADO</label>
            <select v-model="serviceForm.serviceType" required>
              <option value="" disabled>Selecciona un servicio</option>
              <option value="Desarrollo Web">Desarrollo Web / E-commerce</option>
              <option value="Apps Móviles">Apps Móviles</option>
              <option value="Diseño UI/UX">Diseño UI/UX</option>
              <option value="Apps Institucionales">Apps Institucionales</option>
              <option value="Landing Pages">Landing Pages</option>
              <option value="Portafolio digital">Portafolio digital</option>
              <option value="Consultoría">Consultoría Técnica</option>
              <option value="Otro">Otro</option>
            </select>
          </div>

          <div class="form-group">
            <label>DETALLES DEL PROYECTO</label>
            <textarea v-model="serviceForm.details" required placeholder="Describe brevemente tu idea..." rows="4"></textarea>
          </div>

          <button type="submit" class="modal-submit">
            ENVIAR POR WHATSAPP
            <span class="arrow">→</span>
          </button>
        </form>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { onMounted, ref, provide } from 'vue';
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import WhoWeAre from './components/WhoWeAre.vue';
import Footer from './components/Footer.vue';

const isLoaded = ref(false);
const showModal = ref(false);

const serviceForm = ref({
  name: '',
  phone: '',
  serviceType: '',
  details: ''
});

const openModal = () => {
  showModal.value = true;
};

// Provide the function globally
provide('openServiceModal', openModal);

const sendServiceRequest = () => {
  const message = `¡Hola Helify! 👋 Solicito información para un nuevo proyecto:
  
📌 *Nombre:* ${serviceForm.value.name}
📌 *Teléfono:* ${serviceForm.value.phone}
📌 *Servicio:* ${serviceForm.value.serviceType}
📌 *Detalles:* ${serviceForm.value.details}`;

  const whatsappUrl = `https://wa.me/51962956919?text=${encodeURIComponent(message)}`;
  window.open(whatsappUrl, '_blank');
  showModal.value = false;
  
  // Reset form
  serviceForm.value = { name: '', phone: '', serviceType: '', details: '' };
};

onMounted(() => {
  setTimeout(() => {
    isLoaded.value = true;
  }, 100);

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { 
    threshold: 0.1,
    rootMargin: '0px 0px -50px 0px'
  });

  document.querySelectorAll('.fade-up').forEach(el => observer.observe(el));
});
</script>

<style>
/* Reset and Globals */
:root {
  --accent-primary: #fff;
  --font-heading: 'Plus Jakarta Sans', sans-serif;
  --font-main: 'Inter', sans-serif;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: #000;
  color: #fff;
  font-family: var(--font-main);
}

.app-wrapper {
  overflow-x: hidden;
  opacity: 0;
  transform: translateY(10px);
  transition: opacity 1.2s cubic-bezier(0.16, 1, 0.3, 1), 
              transform 1.2s cubic-bezier(0.16, 1, 0.3, 1);
}

.app-wrapper.is-loaded {
  opacity: 1;
  transform: translateY(0);
}

/* Global Floating WhatsApp Button */
.global-wa-btn {
  position: fixed;
  bottom: 30px;
  right: 30px;
  width: 65px;
  height: 65px;
  z-index: 99999;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  filter: drop-shadow(0 10px 15px rgba(0,0,0,0.3));
  animation: float-global-wa 3s ease-in-out infinite;
}

.global-wa-btn:hover {
  transform: scale(1.1) translateY(-5px);
  filter: drop-shadow(0 15px 25px rgba(37, 211, 102, 0.4));
}

.wa-icon {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

@keyframes float-global-wa {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

/* Global Service Modal Styling */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(15px);
  z-index: 999999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.modal-content {
  background: #0a0a0a;
  border: 1px solid rgba(255, 255, 255, 0.1);
  width: 100%;
  max-width: 600px;
  padding: 60px;
  border-radius: 40px;
  position: relative;
  box-shadow: 0 30px 60px rgba(0,0,0,0.5);
}

.close-btn {
  position: absolute;
  top: 30px;
  right: 30px;
  background: transparent;
  border: none;
  color: #fff;
  font-size: 1.5rem;
  cursor: pointer;
  opacity: 0.5;
  transition: opacity 0.3s;
}

.close-btn:hover { opacity: 1; }

.modal-header {
  margin-bottom: 40px;
}

.modal-header h3 {
  font-family: var(--font-heading);
  font-size: 2rem;
  margin-bottom: 10px;
  letter-spacing: -1px;
}

.modal-header p {
  color: #888;
  font-size: 0.9rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 25px;
}

.form-group label {
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 2px;
  color: #555;
}

.form-group input, 
.form-group select, 
.form-group textarea {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  padding: 15px 20px;
  border-radius: 15px;
  color: #fff;
  font-family: var(--font-main);
  transition: all 0.3s;
  appearance: none;
}

.form-group select option {
  background-color: #111;
  color: #fff;
  padding: 10px;
}

.form-submit,
.modal-submit {
  width: 100%;
  padding: 20px;
  border-radius: 99px;
  border: none;
  background: #fff;
  color: #000;
  font-family: var(--font-heading);
  font-weight: 700;
  font-size: 0.9rem;
  letter-spacing: 2px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 15px;
  transition: all 0.3s;
}

.modal-submit:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(255, 255, 255, 0.2);
}

/* Animations */
.fade-up {
  opacity: 0;
  transform: translateY(30px);
  transition: all 1s cubic-bezier(0.16, 1, 0.3, 1);
}

.fade-up.visible {
  opacity: 1;
  transform: translateY(0);
}

.fade-enter-active, .fade-leave-active { transition: opacity 0.5s ease; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

.fade-up-modal {
  animation: modalIn 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes modalIn {
  from { opacity: 0; transform: translateY(40px) scale(0.95); }
  to { opacity: 1; transform: translateY(0) scale(1); }
}

@media (max-width: 600px) {
  .form-row { grid-template-columns: 1fr; }
  .modal-content { padding: 40px 20px; }
}
</style>
