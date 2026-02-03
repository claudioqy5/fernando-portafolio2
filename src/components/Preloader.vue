<template>
  <Transition name="fade-preloader">
    <div v-if="show" class="preloader">
      <div class="preloader-content">
        <span class="welcome-text">Bienvenido a</span>
        <h1 class="brand-text">
          <span 
            v-for="(letter, index) in brandName" 
            :key="index"
            class="letter"
            :style="{ animationDelay: `${index * 0.1}s` }"
          >
            {{ letter }}
          </span>
        </h1>
        <div class="loader-container">
          <div class="loader-bar"></div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  show: Boolean
});

const brandName = "Helify".split("");
</script>

<style scoped>
.preloader {
  position: fixed;
  inset: 0;
  background: #000;
  z-index: 9999999;
  display: flex;
  align-items: center;
  justify-content: center;
}

.preloader-content {
  text-align: center;
}

.welcome-text {
  display: block;
  font-family: var(--font-main);
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.4);
  letter-spacing: 4px;
  text-transform: uppercase;
  margin-bottom: 20px;
  opacity: 0;
  animation: fadeInDown 1s forwards cubic-bezier(0.16, 1, 0.3, 1);
}

.brand-text {
  display: flex;
  gap: 5px;
  font-family: var(--font-heading);
  font-size: clamp(3rem, 10vw, 6rem);
  font-weight: 800;
  color: #fff;
  overflow: hidden;
}

.letter {
  display: inline-block;
  transform: translateY(-150%);
  opacity: 0;
  animation: letterFall 1.2s forwards cubic-bezier(0.16, 1, 0.3, 1);
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes letterFall {
  0% {
    transform: translateY(-150%);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

.loader-container {
  width: 200px;
  height: 2px;
  background: rgba(255, 255, 255, 0.1);
  margin: 40px auto 0;
  border-radius: 10px;
  overflow: hidden;
  position: relative;
}

.loader-bar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 0;
  background: #fff;
  border-radius: 10px;
  animation: loadProgress 2.5s forwards cubic-bezier(0.16, 1, 0.3, 1);
  box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
}

@keyframes loadProgress {
  0% { width: 0; }
  100% { width: 100%; }
}

/* Preloader Exit Transition */
.fade-preloader-leave-active {
  transition: opacity 1s cubic-bezier(0.16, 1, 0.3, 1), transform 1s cubic-bezier(0.16, 1, 0.3, 1);
}

.fade-preloader-leave-to {
  opacity: 0;
  transform: scale(1.1);
}
</style>
