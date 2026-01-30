<template>
  <section id="projects" class="projects">
    <div class="container">
      <h2 class="section-title fade-up">Proyectos Destacados</h2>
      
      <div class="filter-tabs fade-up">
        <button 
          v-for="cat in categories" 
          :key="cat.id" 
          @click="activeCategory = cat.id"
          :class="{ 'active': activeCategory === cat.id }"
        >
          {{ cat.name }}
        </button>
      </div>

      <div class="projects-grid">
        <div 
          v-for="project in filteredProjects" 
          :key="project.id" 
          class="project-card glass fade-up"
        >
          <div class="project-img">
            <div class="img-placeholder">{{ project.type }}</div>
            <div class="project-overlay">
              <a :href="project.link" class="btn btn-primary">Ver Demo</a>
            </div>
          </div>
          <div class="project-info">
            <span class="p-category">{{ project.type }}</span>
            <h3>{{ project.title }}</h3>
            <p>{{ project.description }}</p>
            <div class="p-tags">
              <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const activeCategory = ref('all');

const categories = [
  { id: 'all', name: 'Todos' },
  { id: 'landing', name: 'Landing Pages' },
  { id: 'webapp', name: 'Aplicaciones Web' },
  { id: 'automation', name: 'Automatizaciones' },
  { id: 'ecommerce', name: 'eCommerce' }
];

const projects = [
  {
    id: 1,
    title: 'Landing Page Pharma',
    type: 'Landing Pages',
    category: 'landing',
    description: 'Propuesta de alto impacto enfocada en conversión para industria médica.',
    tags: ['Vue.js', 'GSAP', 'CSS3'],
    link: '#'
  },
  {
    id: 2,
    title: 'SaaS Dashboard',
    type: 'Aplicaciones Web',
    category: 'webapp',
    description: 'Sistema administrativo con visualización de datos en tiempo real.',
    tags: ['React', 'Node.js', 'Chart.js'],
    link: '#'
  },
  {
    id: 3,
    title: 'Bot de Reservas WhatsApp',
    type: 'Automatizaciones',
    category: 'automation',
    description: 'Automatización completa de flujo de citas para clínicas locales.',
    tags: ['n8n', 'Node.js', 'API'],
    link: '#'
  },
  {
    id: 4,
    title: 'Fashion Store',
    type: 'eCommerce',
    category: 'ecommerce',
    description: 'Tienda online optimizada para dispositivos móviles y SEO.',
    tags: ['Shopify', 'Liquid', 'JS'],
    link: '#'
  }
];

const filteredProjects = computed(() => {
  if (activeCategory.value === 'all') return projects;
  return projects.filter(p => p.category === activeCategory.value);
});

onMounted(() => {
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) entry.target.classList.add('visible');
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.fade-up').forEach(el => observer.observe(el));
});
</script>

<style scoped>
.filter-tabs {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 4rem;
  flex-wrap: wrap;
}

.filter-tabs button {
  padding: 0.6rem 1.5rem;
  background: transparent;
  color: var(--text-secondary);
  border: 1px solid var(--glass-border);
  border-radius: 50px;
  font-size: 0.9rem;
}

.filter-tabs button.active {
  background: var(--accent-primary);
  color: white;
  border-color: var(--accent-primary);
}

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 2.5rem;
}

.project-card {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.4s ease;
}

.project-card:hover {
  transform: translateY(-10px);
}

.project-img {
  height: 220px;
  background: var(--bg-secondary);
  position: relative;
  overflow: hidden;
}

.img-placeholder {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.9rem;
  color: var(--text-secondary);
  background: linear-gradient(45deg, #1a1a1e, #25252b);
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: rgba(10, 10, 12, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: 0.3s;
}

.project-card:hover .project-overlay {
  opacity: 1;
}

.project-info {
  padding: 2rem;
  flex-grow: 1;
}

.p-category {
  color: var(--accent-primary);
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  display: block;
  margin-bottom: 0.5rem;
}

h3 {
  margin-bottom: 1rem;
}

p {
  color: var(--text-secondary);
  font-size: 0.95rem;
  margin-bottom: 1.5rem;
}

.p-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.p-tags span {
  font-size: 0.75rem;
  padding: 0.3rem 0.8rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 4px;
}

@media (max-width: 480px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}
</style>
