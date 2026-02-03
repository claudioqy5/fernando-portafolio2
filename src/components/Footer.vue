<template>
  <section id="contact" class="contact-section">
    <!-- New Effect: Vertical Kinetic Typography (Side) -->
    <div class="vertical-marquee">
      <div class="marquee-track">
        <span v-for="n in 6" :key="'v-'+n">CONTÁCTANOS — HELIFY — DISEÑO — INGENIERÍA — </span>
      </div>
    </div>

    <div class="container relative-content">
      <div class="contact-grid">
        <!-- Left Side: Info & Headline -->
        <div class="info-side fade-up">
          <h2 class="massive-title">
            HABLEMOS<br/>
            DE TU<br/>
            IDEA
          </h2>
          <div class="contact-details">
            <div class="detail-item">
              <span class="label">WHATSAPP</span>
              <a href="https://wa.me/51962956919" target="_blank" class="link">962 956 919</a>
            </div>
            <div class="detail-item">
              <span class="label">EMAIL</span>
              <a href="mailto:claudioquello5@gmail.com" class="link">claudioquello5@gmail.com</a>
            </div>
          </div>
        </div>

        <!-- Right Side: Contact Form -->
        <div class="form-side fade-up">
          <form @submit.prevent="handleSubmit" class="contact-form">
            <div class="form-group">
              <label>NOMBRE</label>
              <input type="text" v-model="form.name" required placeholder="Tu nombre" />
            </div>
            <div class="row-group">
              <div class="form-group">
                <label>EMAIL</label>
                <input type="email" v-model="form.email" required placeholder="tu@email.com" />
              </div>
              <div class="form-group">
                <label>TELÉFONO</label>
                <input type="tel" v-model="form.phone" required placeholder="+51 ..." />
              </div>
            </div>
            <div class="form-group">
              <label>PROYECTO</label>
              <textarea v-model="form.message" required placeholder="Cuéntanos tu idea..." rows="4"></textarea>
            </div>
            <button type="submit" class="submit-btn" :disabled="isSending">
              <span v-if="!isSending">ENVIAR MENSAJE</span>
              <span v-else>ENVIANDO...</span>
              <div class="btn-arrow">→</div>
            </button>
            <p v-if="sentStatus" class="status-msg">{{ sentStatus }}</p>
          </form>
        </div>
      </div>

      <!-- Footer Brand Info -->
      <footer class="footer-wrap fade-up">
        <div class="footer-line"></div>
        <div class="footer-meta">
          <div class="brand">
            <span class="brand-name">HELIFY</span>
            <span class="tagline">Boutique de Ingeniería Digital</span>
          </div>
          <div class="bottom-links">
            <a href="#hero">INICIO</a>
            <a href="#projects">PROYECTOS</a>
            <a href="#about">SERVICIOS</a>
          </div>
          <div class="legal">
            <span>© 2026 HELIFY</span>
          </div>
        </div>
      </footer>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const isSending = ref(false);
const sentStatus = ref('');
const form = ref({
  name: '',
  email: '',
  phone: '',
  message: ''
});

const handleSubmit = async () => {
  isSending.value = true;
  sentStatus.value = '';

  try {
    // 1. Send Email via FormSubmit AJAX (Prevents redirect)
    const response = await fetch("https://formsubmit.co/ajax/claudioquello5@gmail.com", {
      method: "POST",
      headers: { 
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify({
        Nombre: form.value.name,
        Email: form.value.email,
        Telefono: form.value.phone,
        Proyecto: form.value.message,
        _subject: `Nuevo Proyecto de ${form.value.name}`
      })
    });

    // 2. Open WhatsApp Message
    const waMessage = `¡Hola Helify! Mi nombre es ${form.value.name}. 
Email: ${form.value.email}
Tel: ${form.value.phone}
Proyecto: ${form.value.message}`;
    
    const waUrl = `https://wa.me/51962956919?text=${encodeURIComponent(waMessage)}`;
    window.open(waUrl, '_blank');

    if (response.ok) {
      sentStatus.value = '¡Excelente! El correo ha sido enviado y se ha abierto el chat de WhatsApp.';
      form.value = { name: '', email: '', phone: '', message: '' };
    }
  } catch (error) {
    console.error('Error:', error);
    sentStatus.value = 'Error al enviar por correo, pero el chat de WhatsApp se ha abierto.';
  } finally {
    isSending.value = false;
  }
};

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
.contact-section {
  background-color: #050505;
  color: #ffffff;
  padding: 160px 0 60px;
  min-height: 100vh;
  position: relative;
  overflow: hidden;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 5%;
}

.relative-content {
  position: relative;
  z-index: 10;
}

/* New Vertical Effect */
.vertical-marquee {
  position: absolute;
  top: 0;
  right: 5%;
  height: 100%;
  width: 100px;
  z-index: 1;
  pointer-events: none;
  opacity: 0.05;
  writing-mode: vertical-rl;
}

.marquee-track {
  display: flex;
  flex-direction: column;
  animation: scroll-vertical 40s linear infinite;
}

.marquee-track span {
  font-family: var(--font-heading);
  font-size: 6rem;
  font-weight: 900;
  white-space: nowrap;
  padding: 50px 0;
  color: #ffffff;
}

@keyframes scroll-vertical {
  from { transform: translateY(0); }
  to { transform: translateY(-50%); }
}

/* Contact Grid */
.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 100px;
  align-items: flex-start;
  justify-content: center;
}

.massive-title {
  font-family: var(--font-heading);
  font-size: clamp(4rem, 12vw, 10rem);
  font-weight: 300;
  line-height: 0.85;
  letter-spacing: -0.06em;
  color: #b0b1a2;
  margin: 0 0 60px 0;
}

.contact-details {
  display: flex;
  flex-direction: column;
  gap: 40px;
}

.detail-item {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.label {
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 3px;
  color: rgba(255, 255, 255, 0.4);
}

.link {
  font-family: var(--font-heading);
  font-size: 1.5rem;
  font-weight: 500;
  color: #fff;
  text-decoration: none;
  transition: color 0.3s;
}

.link:hover {
  color: var(--accent-primary);
}

/* Form Styles */
.contact-form {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 60px;
  border-radius: 30px;
  backdrop-filter: blur(10px);
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 30px;
  flex: 1;
}

.row-group {
  display: flex;
  gap: 30px;
}

@media (max-width: 600px) {
  .row-group {
    flex-direction: column;
    gap: 0;
  }
}

.form-group label {
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 2px;
  color: rgba(255, 255, 255, 0.4);
}

.form-group input, 
.form-group textarea {
  background: transparent;
  border: none;
  border-bottom: 2px solid rgba(255, 255, 255, 0.1);
  padding: 15px 0;
  color: #fff;
  font-size: 1.1rem;
  transition: border-color 0.3s;
  font-family: var(--font-main);
}

.form-group input:focus, 
.form-group textarea:focus {
  outline: none;
  border-color: #fff;
}

.submit-btn {
  width: 100%;
  background: #ffffff;
  color: #000;
  padding: 20px 40px;
  border-radius: 99px;
  font-family: var(--font-heading);
  font-weight: 700;
  font-size: 0.9rem;
  letter-spacing: 2px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  margin-top: 20px;
}

.submit-btn:hover {
  transform: scale(1.02);
  background: var(--accent-primary);
  color: #fff;
}

@keyframes float-wa {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}


.btn-arrow {
  font-size: 1.5rem;
  font-weight: 300;
}

.status-msg {
  margin-top: 20px;
  color: #4BB543;
  font-size: 0.9rem;
  text-align: center;
}

/* Footer Wrap */
.footer-wrap {
  margin-top: 120px;
}

.footer-line {
  width: 100%;
  height: 1px;
  background: rgba(255, 255, 255, 0.1);
  margin-bottom: 40px;
}

.footer-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.brand {
  display: flex;
  flex-direction: column;
}

.brand-name {
  font-family: var(--font-heading);
  font-size: 1.5rem;
  font-weight: 800;
  letter-spacing: 2px;
}

.tagline {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.4);
}

.bottom-links {
  display: flex;
  gap: 30px;
}

.bottom-links a {
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 2px;
  color: rgba(255, 255, 255, 0.5);
  transition: color 0.3s;
}

.bottom-links a:hover {
  color: #fff;
}

.legal {
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: rgba(255, 255, 255, 0.3);
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

@media (max-width: 1024px) {
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 60px;
  }
}

@media (max-width: 768px) {
  .contact-section { padding: 100px 0 40px; }
  .contact-form { padding: 40px 20px; }
  .massive-title { font-size: 4rem; }
  .footer-meta { flex-direction: column; gap: 40px; text-align: center; }
  .bottom-links { flex-direction: column; gap: 15px; }
}
</style>
