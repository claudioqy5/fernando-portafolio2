<template>
  <section id="contact" class="cta">
    <div class="container">
      <div class="cta-card glass fade-up">
        <h2>¿Listo para llevar tu proyecto al siguiente nivel?</h2>
        <p>Trabajemos juntos para crear soluciones digitales que impulsen tu negocio.</p>
        <div class="cta-btns">
          <a href="https://wa.me/yourwhatsapp" class="btn btn-primary">
            WhatsApp
          </a>
          <a href="mailto:fernando@example.com" class="btn btn-secondary">
            Enviar Email
          </a>
        </div>
      </div>
    </div>
  </section>

  <footer class="footer">
    <div class="container footer-content">
      <div class="footer-brand">
        <div class="logo">
          <img src="../assets/helify.png" alt="Helify Logo" class="logo-img" />
        </div>
        <p>Soluciones digitales a medida.</p>
      </div>
      
      <div class="footer-links">
        <div class="link-group">
          <h4>Navegación</h4>
          <a href="#hero">Inicio</a>
          <a href="#projects">Proyectos</a>
          <a href="#about">Sobre mí</a>
        </div>
        <div class="link-group">
          <h4>Contacto</h4>
          <a href="#">LinkedIn</a>
          <a href="#">GitHub</a>
          <a href="#">Twitter</a>
        </div>
      </div>
    </div>
    <div class="footer-bottom">
      <p>&copy; 2026 Fernando. Todos los derechos reservados.</p>
    </div>
  </footer>
</template>

<style scoped>
.cta {
  padding: 8rem 10%;
}

.cta-card {
  padding: 5rem 2rem;
  text-align: center;
}

.cta-card h2 {
  font-size: clamp(2rem, 5vw, 3rem);
  margin-bottom: 1.5rem;
}

.cta-card p {
  color: var(--text-secondary);
  font-size: 1.2rem;
  margin-bottom: 3rem;
  max-width: 600px;
  margin-inline: auto;
}

.cta-btns {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  flex-wrap: wrap;
}

.footer {
  background: var(--bg-secondary);
  padding: 5rem 0 2rem;
  border-top: 1px solid var(--glass-border);
}

.footer-content {
  display: flex;
  justify-content: space-between;
  gap: 4rem;
  margin-bottom: 4rem;
}

.footer-brand .logo {
  margin-bottom: 1rem;
}

.logo-img {
  height: 35px;
  width: auto;
  object-fit: contain;
}

.footer-brand p {
  color: var(--text-secondary);
}

.footer-links {
  display: flex;
  gap: 4rem;
}

.link-group h4 {
  margin-bottom: 1.5rem;
  font-size: 1.1rem;
}

.link-group a {
  display: block;
  color: var(--text-secondary);
  margin-bottom: 0.8rem;
  font-size: 0.95rem;
}

.link-group a:hover {
  color: var(--accent-primary);
}

.footer-bottom {
  border-top: 1px solid var(--glass-border);
  padding-top: 2rem;
  text-align: center;
  color: var(--text-secondary);
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .cta {
    padding: 4rem 5%;
  }

  .cta-card {
    padding: 3rem 1.5rem;
  }

  .footer-content {
    flex-direction: column;
    text-align: center;
    gap: 3rem;
  }
  
  .footer-brand {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .footer-links {
    justify-content: center;
    flex-wrap: wrap;
    gap: 2rem;
  }
}
</style>
