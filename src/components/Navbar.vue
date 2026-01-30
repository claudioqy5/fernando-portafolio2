<template>
  <nav class="navbar" :class="{ 'scrolled': isScrolled }">
    <div class="container nav-content">
      <!-- Logo as in the reference image -->
      <div class="logo" @click="refreshPage">
        <img src="../assets/helify.png" alt="Helify Logo" class="logo-img" />
      </div>
      
      <!-- Centered Navigation -->
      <ul class="nav-links" :class="{ 'active': isMenuOpen }">
        <li><a href="#hero" @click="closeMenu">HOME</a></li>
        <li><a href="#projects" @click="closeMenu">PROJECTS</a></li>
        <li><a href="#about" @click="closeMenu">ABOUT</a></li>
        <li><a href="#process" @click="closeMenu">PROCESS</a></li>
        <!-- Mobile only link for Contact -->
        <li class="mobile-only"><a href="#contact" @click="closeMenu">CONTACT</a></li>
      </ul>

      <!-- Right CTA -->
      <div class="nav-right">
        <a href="#contact" class="nav-cta">CONTACT ME</a>
        <div class="menu-toggle" @click="toggleMenu">
          <div class="bar" :class="{ 'open': isMenuOpen }"></div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const isScrolled = ref(false);
const isMenuOpen = ref(false);

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50;
};

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
  isMenuOpen.value = false;
};

const refreshPage = () => {
  window.location.href = '/';
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 2rem 0;
  z-index: 1000;
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

.navbar.scrolled {
  padding: 1rem 0;
  background: rgba(5, 5, 5, 0.85);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.nav-content {
  display: grid;
  grid-template-columns: 100px 1fr 150px;
  align-items: center;
  max-width: 1600px;
  padding: 0 5%;
}

.logo {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.logo-img {
  height: 40px;
  width: auto;
  object-fit: contain;
}

.nav-links {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 3rem;
  margin: 0;
  padding: 0;
}

.nav-links li a {
  font-size: 0.75rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 2px;
  transition: all 0.3s ease;
  position: relative;
}

.nav-links li a:hover {
  color: #fff;
}

.nav-links li a::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 1px;
  background: var(--accent-primary);
  transition: width 0.3s ease;
}

.nav-links li a:hover::after {
  width: 100%;
}

.nav-right {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.nav-cta {
  font-size: 0.75rem;
  font-weight: 600;
  color: #fff;
  letter-spacing: 1px;
  padding: 0.8rem 1.8rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 99px;
  background: rgba(255, 255, 255, 0.03);
  transition: all 0.3s ease;
}

.nav-cta:hover {
  background: #fff;
  color: #000;
  border-color: #fff;
  transform: translateY(-2px);
}

.menu-toggle {
  display: none;
  cursor: pointer;
  margin-left: 1.5rem;
}

.bar {
  width: 20px;
  height: 1px;
  background: #fff;
  position: relative;
  transition: 0.3s;
}

.bar::before, .bar::after {
  content: '';
  position: absolute;
  width: 20px;
  height: 1px;
  background: #fff;
  transition: 0.3s;
}

.bar::before { top: -6px; }
.bar::after { bottom: -6px; }

.bar.open { background: transparent; }
.bar.open::before { transform: rotate(45deg); top: 0; }
.bar.open::after { transform: rotate(-45deg); bottom: 0; }

.mobile-only {
  display: none;
}

@media (max-width: 1024px) {
  .nav-content {
    grid-template-columns: 1fr 1fr;
  }
  
  .nav-links {
    display: none;
  }
  
  .menu-toggle {
    display: block;
  }
  
  .nav-cta {
    display: none;
  }
  
  .nav-links.active {
    display: flex;
    position: fixed;
    top: 0;
    right: 0;
    width: 100%;
    height: 100vh;
    background: #050505;
    flex-direction: column;
    justify-content: center;
    gap: 3rem;
    z-index: 999;
  }
  
  .nav-links.active li a {
    font-size: 1.5rem;
  }
  
  .mobile-only {
    display: block;
  }
}
</style>
