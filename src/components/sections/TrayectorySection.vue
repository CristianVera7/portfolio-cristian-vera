<template lang="pug">
section#trajectory.section
  .trajectory-container
    .photo
      img(src="/fotoLinkedin.jpg", alt="Mi foto")
    .text
      h1 Mi Trayectoria profesional:
      p
        | Empecé en el mundo <strong>automovilístico</strong> y, tras varios años de experiencia en el sector, me vi <strong>estancado</strong> y decidí salir de mi <strong>zona de confort</strong>.
        br
        | Con constancia y esfuerzo, me labré un nuevo futuro en el mundo <strong>tecnológico</strong>, encaminándome a convertirme en un profesional del <strong>desarrollo de aplicaciones multiplataforma</strong>.
        br
        | Mi primer contacto con la <strong>programación</strong> fue a través de un <strong>bootcamp</strong>, donde di mis primeros pasos en <strong>JavaScript</strong>, utilizando <strong>React</strong> en el frontend y <strong>Node.js</strong> con <strong>Express</strong> en el backend,
        | apoyándome en <strong>MongoDB</strong> como base de datos y trabajando bajo <strong>metodologías ágiles</strong>. Esta experiencia despertó mi curiosidad y <strong>pasión por el código</strong>, y descubrí que podía pasar horas aprendiendo y resolviendo problemas sin darme cuenta del tiempo.
    .text
      p
        | Motivado por esta pasión, decidí ampliar mis estudios y completé el ciclo superior de <strong>Desarrollo de Aplicaciones Multiplataforma (DAM)</strong> en <strong>Nebrija</strong>,
        | donde adquirí conocimientos sólidos en <strong>Java</strong> y <strong>Python</strong>, profundicé en <strong>programación orientada a objetos</strong>, desarrollo de <strong>interfaces</strong>, acceso a <strong>datos</strong>, programación de <strong>servicios y procesos</strong>, así como en el desarrollo de <strong>aplicaciones móviles nativas con Android</strong> y el diseño de <strong>arquitecturas de software escalables</strong>.
        br
        | Durante mis prácticas en <strong>BornanSport</strong>, tuve la oportunidad de trabajar con <strong>Vue</strong>, utilizando herramientas como <strong>Pug</strong>, <strong>TypeScript</strong> y <strong>SCSS</strong>,
        | consolidando mis habilidades en <strong>frontend</strong> y sintiéndome cómodo en entornos de desarrollo reales. Esta experiencia me llevó a desarrollar mi <strong>TFG</strong>,
        | donde puse en práctica todo lo aprendido y nació <strong>Quepo</strong>, una web que fomenta las <strong>actividades al aire libre</strong>, permitiendo compartir rutas y experiencias con otros usuarios de manera colaborativa.
        br
        | Actualmente sigo aprendiendo y mejorando cada día: realizo <strong>testing</strong> en mis proyectos para garantizar un código <strong>robusto y limpio</strong>, y exploro nuevas herramientas como <strong>Lit</strong> y <strong>Web Components</strong>,
        | enfocándome en la <strong>reutilización de componentes</strong> y la <strong>eficiencia en el desarrollo web</strong>.
        br
        | Mi trayectoria demuestra que con <strong>pasión</strong>, <strong>disciplina</strong> y <strong>aprendizaje continuo</strong> es posible reinventarse profesionalmente. Estoy preparado para afrontar nuevos retos,
        | aportar valor a los proyectos en los que participo y seguir creciendo como <strong>desarrollador full stack</strong>, combinando <strong>creatividad</strong>, <strong>tecnología</strong> y <strong>buenas prácticas</strong> en cada línea de código.
</template>

<script setup lang="ts">
</script>

<style scoped lang="scss">
.section {
  min-height: 100vh;
  padding: 4rem 2rem;
  scroll-margin-top: 200px;
  display: flex;
  align-items: center;

  .trajectory-container {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto auto;
    gap: 2rem 4rem;
    max-width: 1200px;
    margin: 0 auto;
    width: 100%;

    .photo {
      grid-column: 1;
      grid-row: 1;
      margin-top: 70px;
      
      img {
        width: 100%;
        max-width: 250px;
        border-radius: 30%;
        object-fit: cover;
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
        transition: transform 0.3s ease;

        &:hover {
          transform: scale(1.05);
        }
      }
    }

    .text {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;

      &:first-of-type {
        grid-column: 2;
        grid-row: 1;
      }

      &:last-of-type {
        grid-column: 1 / -1;
        grid-row: 2;
      }

      h1 {
        font-size: 2.8rem;
        font-weight: 700;
        color: #ffffff;
        margin-bottom: 1rem;
        border-left: 4px solid #00ff88;
        padding-left: 1rem;
        line-height: 1.2;
      }

      p {
        font-size: 1.1rem;
        line-height: 1.8;
        color: #cfd8dc;
      }

      strong {
        font-weight: 600;
        color: #ffffffcc;
      }
    }
  }
}

// Tablet grande (hasta 1024px)
@media (max-width: 1024px) {
  .section {
    padding: 3rem 1.5rem;

    .trajectory-container {
      grid-template-columns: 220px 1fr;
      gap: 2rem 3rem;

      .photo {
        img {
          max-width: 220px;
        }
      }

      .text {
        h1 {
          font-size: 2.4rem;
        }

        p {
          font-size: 1.05rem;
        }
      }
    }
  }
}

// Tablet (hasta 768px)
@media (max-width: 768px) {
  .section {
    padding: 2.5rem 1.5rem;
    min-height: auto;

    .trajectory-container {
      grid-template-columns: 1fr;
      grid-template-rows: auto auto auto;
      gap: 2.5rem;

      .photo {
        grid-column: 1;
        grid-row: 1;
        justify-self: center;
        margin-top: 0;
        
        img {
          max-width: 200px;
        }
      }

      .text {
        &:first-of-type {
          grid-column: 1;
          grid-row: 2;
        }

        &:last-of-type {
          grid-column: 1;
          grid-row: 3;
        }

        h1 {
          font-size: 2.2rem;
        }

        p {
          font-size: 1rem;
        }
      }
    }
  }
}

// Mobile grande (hasta 480px)
@media (max-width: 480px) {
  .section {
    padding: 2rem 1rem;

    .trajectory-container {
      gap: 2rem;

      .photo {
        img {
          max-width: 180px;
        }
      }

      .text {
        gap: 1.2rem;

        h1 {
          font-size: 1.8rem;
          padding-bottom: 0.4rem;
        }

        p {
          font-size: 0.95rem;
          line-height: 1.7;
        }
      }
    }
  }
}

// Mobile pequeño (hasta 375px)
@media (max-width: 375px) {
  .section {
    padding: 1.5rem 0.8rem;

    .trajectory-container {
      gap: 1.5rem;

      .photo {
        img {
          max-width: 160px;
        }
      }

      .text {
        h1 {
          font-size: 1.6rem;
          border-bottom-width: 3px;
        }

        p {
          font-size: 0.9rem;
          line-height: 1.6;
        }
      }
    }
  }
}

// Pantallas ultra anchas (desde 1400px)
@media (min-width: 1400px) {
  .section {
    .trajectory-container {
      max-width: 1400px;
      grid-template-columns: 280px 1fr;
      gap: 2rem 5rem;

      .photo {
        img {
          max-width: 280px;
        }
      }

      .text {
        h1 {
          font-size: 3rem;
        }

        p {
          font-size: 1.15rem;
          line-height: 1.9;
        }
      }
    }
  }
}
</style>