<template>
  <section id="hero" class="hero">
    <!-- Video Background -->
    <div class="hero-bg-wrapper">
      <video 
        ref="videoPlayer"
        autoplay 
        muted 
        loop 
        playsinline 
        webkit-playsinline
        class="hero-bg-video"
      >
        <source src="../assets/fondopantalla.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
    </div>
    
    <div class="hero-overlay"></div>

    <div class="hero-container">
      <!-- Left: Large Vertical Typography -->
      <div class="hero-typography">
        <h1 class="vertical-text">
          <span class="word">TÚ</span>
          <span class="word">LO IMAGINAS,</span>
          <span class="word">NOSOTROS</span>
          <span class="word highlight typing-text">{{ currentPhrase }}<span class="cursor">|</span></span>
        </h1>
        <p class="hero-slogan">
          Creamos experiencias digitales de alto impacto para potenciar tu negocio.
        </p>
        <!-- Service Request Button -->
        <button @click="openModal" class="hero-cta">
          <span>SOLICITAR SERVICIO</span>
          <div class="cta-line"></div>
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, inject } from 'vue';

const openModal = inject('openServiceModal');
const videoPlayer = ref(null);

const phrases = ["LO CREAMOS.", "LO DISEÑAMOS.", "LO ESCALAMOS.", "LO IMPULSAMOS."];
const currentPhrase = ref("");

let phraseIndex = 0;
let charIndex = 0;
let isDeleting = false;
let typingSpeed = 100;

const typeEffect = () => {
  const fullText = phrases[phraseIndex];
  
  if (isDeleting) {
    currentPhrase.value = fullText.substring(0, charIndex - 1);
    charIndex--;
    typingSpeed = 50;
  } else {
    currentPhrase.value = fullText.substring(0, charIndex + 1);
    charIndex++;
    typingSpeed = 100;
  }

  if (!isDeleting && charIndex === fullText.length) {
    isDeleting = true;
    typingSpeed = 2000;
  } else if (isDeleting && charIndex === 0) {
    isDeleting = false;
    phraseIndex = (phraseIndex + 1) % phrases.length;
    typingSpeed = 500;
  }

  setTimeout(typeEffect, typingSpeed);
};

onMounted(() => {
  typeEffect();
  
  // Force play for mobile devices / Low power mode
  if (videoPlayer.value) {
    videoPlayer.value.play().catch(err => {
      console.log("Autoplay blocked, user interaction might be needed or battery saver is on.", err);
    });
  }
});
</script>

<style scoped>
.typing-text {
  min-height: 1.2em;
  display: inline-block;
}

.cursor {
  font-weight: 200;
  animation: blink 1s infinite;
  margin-left: 2px;
  color: #fff;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0; }
}

.hero {
  height: 100vh;
  width: 100%;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  background-color: #050505;
}

.hero-bg-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.hero-bg-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.5;
  pointer-events: none;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to bottom,
    rgba(5, 5, 5, 0.9) 0%,
    rgba(5, 5, 5, 0.4) 50%,
    rgba(5, 5, 5, 0.9) 100%
  );
  z-index: 1;
}

.hero-container {
  position: relative;
  z-index: 2;
  width: 100%;
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 5%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.hero-typography {
  flex: 1;
}

.vertical-text {
  display: flex;
  flex-direction: column;
  font-size: clamp(2.5rem, 8vw, 6.5rem);
  line-height: 0.95;
  font-weight: 800;
  color: #fff;
  text-transform: uppercase;
  font-family: var(--font-heading);
  margin-bottom: 2rem;
}

.hero-slogan {
  font-size: clamp(0.8rem, 1.5vw, 1.1rem);
  color: rgba(255, 255, 255, 0.5);
  max-width: fit-content;
  white-space: nowrap;
  line-height: 1;
  font-weight: 300;
  letter-spacing: 0.4rem;
  text-transform: uppercase;
  font-family: var(--font-main);
  opacity: 0;
  transform: translateY(10px);
  animation: sloganFadeIn 1s forwards cubic-bezier(0.2, 1, 0.3, 1);
  animation-delay: 1.5s;
}

@keyframes sloganFadeIn {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-cta {
  background: transparent;
  border: none;
  color: #fff;
  font-family: var(--font-heading);
  font-size: 0.9rem;
  font-weight: 700;
  letter-spacing: 3px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 0;
  margin-top: 50px;
  transition: all 0.3s ease;
  width: fit-content;
  opacity: 0;
  animation: sloganFadeIn 1s forwards cubic-bezier(0.2, 1, 0.3, 1);
  animation-delay: 1.8s;
}

.cta-line {
  width: 40px;
  height: 2px;
  background-color: #fff;
  transition: width 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.hero-cta:hover .cta-line {
  width: 100%;
}

.hero-cta:hover {
  letter-spacing: 5px;
}

.word {
  display: block;
  opacity: 0;
  transform: translateX(-50px);
  animation: slideIn 1s forwards cubic-bezier(0.2, 1, 0.3, 1);
}

.word:nth-child(2) { animation-delay: 0.2s; }
.word:nth-child(3) { animation-delay: 0.4s; }
.word:nth-child(4) { animation-delay: 0.6s; }

@keyframes slideIn {
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.highlight {
  color: transparent;
  -webkit-text-stroke: 1px rgba(255,255,255,0.8);
}

@media (max-width: 1024px) {
  .hero-container {
    flex-direction: column;
    text-align: center;
    padding-top: 120px;
    justify-content: center;
  }
  
  .hero-typography {
    margin-bottom: 2rem;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .vertical-text {
    align-items: center;
    font-size: clamp(2rem, 12vw, 4rem);
    text-align: center;
  }

  .hero-slogan {
    white-space: normal;
    text-align: center;
    letter-spacing: 0.2rem;
    font-size: 0.9rem;
    width: 100%;
    max-width: 320px;
    margin: 0 auto;
  }

  .hero-cta {
    margin: 40px auto 0;
    align-items: center;
  }

  .typing-text {
    text-align: center;
    display: block;
  }
}

@media (max-width: 480px) {
  .vertical-text {
    font-size: clamp(1.8rem, 15vw, 3rem);
  }
  
  .hero-slogan {
    letter-spacing: 0.1rem;
    font-size: 0.8rem;
  }
}
</style>
