<template>
  <nav class="navbar" :class="{ 'scrolled': isScrolled }">
    <div class="container nav-content">
      <!-- Logo as in the reference image -->
      <div class="logo" @click="refreshPage">
        <img src="../assets/helify.png" alt="Helify Logo" class="logo-img" />
      </div>
      
      <!-- Centered Navigation -->
      <ul class="nav-links" :class="{ 'active': isMenuOpen }">
        <li><a href="#hero" @click="closeMenu">Inicio</a></li>
        <li><a href="#projects" @click="closeMenu">¿Que hacemos?</a></li>
        <li><a href="#about" @click="closeMenu">Servicios</a></li>
        <li><a href="#process" @click="closeMenu">¿Quiénes somos?</a></li>
        <li><a href="#contact" @click="closeMenu">Contacto</a></li>
        <!-- Mobile only link for Contact -->
        <li class="mobile-only"><a href="#contact" @click="closeMenu">CONTACT</a></li>
      </ul>

      <!-- Right CTA -->
      <div class="nav-right">
        <a href="https://wa.me/51962956919?text=¡Hola Helify! 👋 Me gustaría recibir más información sobre sus servicios digitales. ¿Podrían ayudarme?" target="_blank" class="nav-cta">
          <span>ESCRÍBENOS</span>
          <svg class="wsp-icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12.031 6.172c-3.181 0-5.767 2.586-5.767 5.767 0 1.267.405 2.436 1.091 3.388L6.5 18l2.76-.846a5.727 5.727 0 002.771.71c3.181 0 5.767-2.586 5.767-5.767 0-3.181-2.586-5.767-5.767-5.767zm3.332 7.914c-.145.41-.741.769-1.026.804-.282.034-.564.045-.899-.101-.212-.093-.849-.336-1.637-1.036-.612-.544-1.026-1.216-1.146-1.42s-.13-.341-.13-.51c0-.17.09-.255.121-.301.034-.045.074-.074.111-.11s.074-.056.111-.093c.037-.037.048-.062.074-.105.026-.045.013-.086-.007-.127s-.278-.669-.381-.918c-.1-.24-.2-.206-.278-.21h-.237c-.086 0-.224.034-.341.157s-.448.437-.448 1.063c0 .626.456 1.232.52 1.319.063.086.897 1.37 2.173 1.921s1.332.613 2.103.541c.771-.072 1.705-.694 1.946-1.362.241-.669.241-1.241.17-1.362-.072-.121-.264-.194-.555-.339z"/>
          </svg>
        </a>
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
  background: rgba(10, 10, 10, 0.95);
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
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.6);
  text-transform: uppercase;
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
  font-family: var(--font-heading);
  font-size: 0.7rem;
  font-weight: 700;
  color: #fff;
  letter-spacing: 2px;
  padding: 0.7rem 1.6rem;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 99px;
  background: rgba(255, 255, 255, 0.05);
  transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
  display: flex;
  align-items: center;
  gap: 10px;
}

.wsp-icon {
  width: 18px;
  height: 18px;
  color: #25D366; /* WhatsApp Green */
  transition: transform 0.3s ease;
}

.nav-cta:hover {
  background: #fff;
  color: #000;
  border-color: #fff;
  transform: translateY(-3px) scale(1.02);
  box-shadow: 0 10px 30px rgba(255, 255, 255, 0.2);
}

.nav-cta:hover .wsp-icon {
  transform: scale(1.1) rotate(10deg);
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
