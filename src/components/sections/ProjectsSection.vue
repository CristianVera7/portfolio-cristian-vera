<template lang="pug">
section#projects.section
  h1.section-title  Proyectos destacados:

  .projects-grid
    article.project-card(v-for="project in projects" :key="project.id")
      .image-wrapper
        img(:src="project.image" :alt="project.title")
        .overlay
          span.tag(v-for="tech in project.tech" :key="tech") {{ tech }}

      .content
        h2 {{ project.title }}
        p {{ project.description }}

        .actions
          a.btn(
            v-if="project.demo"
            :href="project.demo"
            target="_blank"
            rel="noopener noreferrer"
          )
            | Demo

          span.btn.disabled(v-else)
            | Próximamente

          a.btn.secondary(
            :href="project.repo"
            target="_blank"
            rel="noopener noreferrer"
          )
            | Código
</template>

<script setup lang="ts">
interface Project {
  id: number
  title: string
  description: string
  image: string
  tech: string[]
  demo: string
  repo: string
}

const projects: Project[] = [
  {
    id: 1,
    title: 'Quepo',
    description:
      'Plataforma colaborativa para compartir rutas y actividades al aire libre, fomentando la movilidad compartida.',
    image: '/quepoWeb.png',
    tech: ['Vue', 'TypeScript', 'Pug', 'SCSS', 'Node.js', 'Express', 'MongoDB'],
    demo: 'http://109.205.183.241:5000/',
    repo: 'https://github.com/CristianVera7/QuepoWeb/tree/master'
  },
  {
    id: 2,
    title: 'Juego de Memoria',
    description:
      'Juego clásico de memoria con cartas. Gira las cartas y encuentra los pares antes de que se acabe el tiempo. Proyecto grupal para practicar React y manejo de estado.',
    image: '/juegoCartas.jpg',
    tech: ['React', 'JavaScript', 'HTML', 'CSS'],
    demo: '',
    repo: 'https://github.com/CristianVera7/Proyecto-Grupo-4fantastic'
  },
  {
    id: 3,
    title: 'RecoveryUp',
    description:
      'Plataforma interactiva para la rehabilitación de pacientes de cáncer de mama. Permite a los usuarios seguir rutinas de forma visual y segura.',
    image: '/recoveryUp.jpg',
    tech: ['React', 'JavaScript', 'HTML', 'CSS', 'Node.js', 'Express', 'MongoDB'],
    demo: '',
    repo: 'https://github.com/CristianVera7/RecoveryUp'
  },
  {
    id: 4,
    title: 'Cyber Portfolio',
    description:
      'Portfolio personal con enfoque visual futurista, animaciones suaves y navegación por scroll.',
    image: '/portfolio.png',
    tech: ['Vue', 'TypeScript', 'Pug', 'SCSS'],
    demo: '',
    repo: ''
  }
]
</script>

<style scoped lang="scss">
.section {
  min-height: 100vh;
  padding: 4rem 2rem;
  scroll-margin-top: 150px;
  text-align: center;
  margin:10rem 0;
}

.section-title {
  text-align: center;
  font-size: 2.8rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0 auto 3rem;
  border-left: 4px solid #00ff88;
  padding-left:1rem;
  display: inline-block;
  width: auto;
}

/* GRID */
.projects-grid {
  max-width: 1400px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 7rem;
  justify-items: center;
}

/* CARD */
.project-card {
  position: relative;
  background: rgba(0, 0, 0, 0.75);
  border-radius: 18px;
  overflow: hidden;
  width: 100%;
  max-width: 450px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  box-shadow:
    0 15px 40px rgba(0, 0, 0, 0.6),
    inset 0 0 0 1px rgba(255, 255, 255, 0.03);
  transition:
    transform 0.4s ease,
    box-shadow 0.4s ease,
    border-color 0.4s ease;

  &:hover {
    transform: translateY(-10px);
    border-color: rgba(0, 255, 136, 0.6);
    box-shadow:
      0 25px 60px rgba(0, 0, 0, 0.8),
      0 0 25px rgba(0, 255, 136, 0.15);

    .image-wrapper img {
      transform: scale(1.1);
    }
  }
}

/* IMAGE */
.image-wrapper {
  position: relative;
  height: 300px;
  overflow: hidden;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.6s ease;
  }

  .overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      to top,
      rgba(0, 0, 0, 0.85),
      rgba(0, 0, 0, 0.2)
    );
    display: flex;
    flex-wrap: wrap;
    align-content: flex-end;
    gap: 0.5rem;
    padding: 1rem;
    opacity: 0;
    transition: opacity 0.4s ease;
  }

  .tag {
    background: rgba(0, 255, 136, 0.15);
    border: 1px solid rgba(0, 255, 136, 0.4);
    color: #00ff88;
    font-size: 0.75rem;
    padding: 0.3rem 0.6rem;
    border-radius: 6px;
    letter-spacing: 1px;
    white-space: nowrap;
  }
}

.project-card:hover .overlay {
  opacity: 1;
}

/* CONTENT */
.content {
  padding: 1.8rem;
  background: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0.02),
    rgba(255, 255, 255, 0.01)
  );

  h2 {
    font-size: 1.5rem;
    margin-bottom: 0.8rem;
    color: #ffffff;
    font-weight: 600;
  }

  p {
    font-size: 1rem;
    line-height: 1.6;
    color: #cfd8dc;
    min-height: 4.8rem;
  }
}

/* ACTIONS */
.actions {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
}

.btn {
  flex: 1;
  text-align: center;
  padding: 0.75rem 1rem;
  border-radius: 8px;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 600;
  background: #00ff88;
  color: #000;
  cursor: pointer;
  transition: all 0.3s ease;
  text-decoration: none;
  display: inline-block;

  &:hover:not(.disabled) {
    background: #00d4ff;
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 255, 136, 0.3);
  }

  &.secondary {
    background: transparent;
    border: 1px solid #00ff88;
    color: #00ff88;

    &:hover {
      background: rgba(0, 255, 136, 0.1);
      border-color: #00d4ff;
      color: #00d4ff;
    }
  }

  &.disabled {
    background: rgba(255, 255, 255, 0.1);
    color: rgba(255, 255, 255, 0.4);
    cursor: not-allowed;
    border: 1px solid rgba(255, 255, 255, 0.2);

    &:hover {
      transform: none;
      box-shadow: none;
    }
  }
}

/* RESPONSIVE */

// Desktop grande (1400px+)
@media (min-width: 1400px) {
  .section {
    padding: 5rem 3rem;
  }

  .section-title {
    font-size: 3.2rem;
    margin-bottom: 4rem;
  }

  .projects-grid {
    max-width: 1600px;
    grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
    gap: 4rem;
  }

  .project-card {
    max-width: 500px;
  }

  .image-wrapper {
    height: 320px;
  }

  .content {
    padding: 2rem;

    h2 {
      font-size: 1.7rem;
    }

    p {
      font-size: 1.05rem;
      min-height: 5rem;
    }
  }
}

// Tablet grande (1024px - 1399px)
@media (max-width: 1399px) and (min-width: 1024px) {
  .section {
    padding: 3.5rem 2rem;
  }

  .section-title {
    font-size: 2.6rem;
    margin-bottom: 2.5rem;
  }

  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 2.5rem;
  }

  .project-card {
    max-width: 100%;
  }
}

// Tablet (768px - 1023px)
@media (max-width: 1023px) and (min-width: 768px) {
  .section {
    padding: 3rem 1.5rem;
  }

  .section-title {
    font-size: 2.3rem;
    margin-bottom: 2rem;
  }

  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 2rem;
    max-width: 900px;
  }

  .project-card {
    max-width: 100%;
  }

  .image-wrapper {
    height: 250px;
  }

  .content {
    padding: 1.5rem;

    h2 {
      font-size: 1.3rem;
    }

    p {
      font-size: 0.95rem;
      min-height: 4.5rem;
    }
  }

  .actions {
    flex-direction: column;
    gap: 0.8rem;

    .btn {
      width: 100%;
    }
  }
}

// Mobile grande (480px - 767px)
@media (max-width: 767px) and (min-width: 480px) {
  .section {
    padding: 2.5rem 1.5rem;
  }

  .section-title {
    font-size: 2rem;
    margin-bottom: 2rem;
  }

  .projects-grid {
    grid-template-columns: 1fr;
    gap: 2rem;
    max-width: 500px;
  }

  .project-card {
    max-width: 100%;
  }

  .image-wrapper {
    height: 240px;
  }

  .content {
    padding: 1.5rem;

    h2 {
      font-size: 1.4rem;
    }

    p {
      font-size: 0.95rem;
      line-height: 1.5;
      min-height: auto;
    }
  }

  .actions {
    margin-top: 1.2rem;
    gap: 0.8rem;
  }

  .btn {
    padding: 0.7rem 1rem;
    font-size: 0.85rem;
  }
}

// Mobile pequeño (menos de 480px)
@media (max-width: 479px) {
  .section {
    padding: 2rem 1rem;
  }

  .section-title {
    font-size: 1.8rem;
    margin-bottom: 1.5rem;
    padding-left: 0.5rem;
  }

  .projects-grid {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .project-card {
    max-width: 100%;
    border-radius: 14px;
  }

  .image-wrapper {
    height: 200px;

    .overlay {
      padding: 0.8rem;
      gap: 0.4rem;
    }

    .tag {
      font-size: 0.7rem;
      padding: 0.25rem 0.5rem;
    }
  }

  .content {
    padding: 1.2rem;

    h2 {
      font-size: 1.3rem;
      margin-bottom: 0.6rem;
    }

    p {
      font-size: 0.9rem;
      line-height: 1.5;
      min-height: auto;
    }
  }

  .actions {
    flex-direction: column;
    gap: 0.7rem;
    margin-top: 1rem;
  }

  .btn {
    width: 100%;
    padding: 0.65rem 0.8rem;
    font-size: 0.8rem;
  }
}

// Mobile muy pequeño (menos de 360px)
@media (max-width: 359px) {
  .section {
    padding: 1.5rem 0.8rem;
  }

  .section-title {
    font-size: 1.6rem;
  }

  .image-wrapper {
    height: 180px;
  }

  .content {
    padding: 1rem;

    h2 {
      font-size: 1.2rem;
    }

    p {
      font-size: 0.85rem;
    }
  }
}
</style>