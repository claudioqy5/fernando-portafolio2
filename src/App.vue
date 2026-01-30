<template>
  <div class="app-wrapper">
    <Navbar />
    <Hero />
    <About />
    <Projects />
    <Footer />
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import Navbar from './components/Navbar.vue';
import Hero from './components/Hero.vue';
import About from './components/About.vue';
import Projects from './components/Projects.vue';
import Footer from './components/Footer.vue';

onMounted(() => {
  // Global fade-up animation logic
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
/* Global styles in App.vue if needed, but we mostly use style.css */
.app-wrapper {
  overflow-x: hidden;
}
</style>
