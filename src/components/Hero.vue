<template>
  <section id="hero" class="hero">
    <!-- Video Background -->
    <div class="hero-bg-wrapper">
      <video 
        autoplay 
        muted 
        loop 
        playsinline 
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
        <p class="hero-slogan fade-up">
          Creamos experiencias digitales de alto impacto para potenciar tu negocio.
        </p>
      </div>


    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const isVisible = ref(false);

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
    typingSpeed = 2000; // Pause at end
  } else if (isDeleting && charIndex === 0) {
    isDeleting = false;
    phraseIndex = (phraseIndex + 1) % phrases.length;
    typingSpeed = 500;
  }

  setTimeout(typeEffect, typingSpeed);
};

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true;
    typeEffect();
  }, 300);
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
  opacity: 0.5; /* Slightly opaque to allow the black background to show through, making it darker */
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* Dark linear gradient to improve text readability and make the video feel integrated */
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
  font-size: clamp(2.5rem, 8vw, 6.5rem); /* Reduced from 8.5rem */
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
  white-space: nowrap; /* Prevents line breaks */
  line-height: 1;
  font-weight: 300; /* More elegant and light */
  letter-spacing: 0.4rem; /* Ultra-minimalist stretched look */
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
  }
  
  .hero-typography {
    margin-bottom: 3rem;
  }
  
  .vertical-text {
    align-items: center;
    font-size: 5rem;
  }
}
</style>
