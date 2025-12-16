<template lang="pug">
footer.footer-section(:class="{ visible: visible }")
  .footer-container
    .footer-main
      .footer-info
        .brand
          h2 Cristian Vera Hernández
          p.tagline Desarrollador Full Stack

      .footer-social
        a.social-link(
          href="https://github.com/CristianVera7"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="GitHub"
        )
          svg(xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="currentColor")
            path(d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z")

        a.social-link(
          href="https://www.linkedin.com/in/cristian-vera-desarrollador/"
          target="_blank"
          rel="noopener noreferrer"
          aria-label="LinkedIn"
        )
          svg(xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="currentColor")
            path(d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z")

        a.social-link(
          href="mailto:criisv95@gmail.com"
          aria-label="Email"
        )
          svg(xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round")
            rect(width="20" height="16" x="2" y="4" rx="2")
            path(d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7")

    .footer-bottom
      p.copyright © {{ currentYear }} • Todos los derechos reservados
      .status
        span.status-dot
        span.status-text Disponible para cualquier consulta
      p.location
        svg(xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round")
          path(d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z")
          circle(cx="12" cy="10" r="3")
        | Madrid, España • Remoto • Presencial
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  visible: boolean
}>()

const currentYear = computed(() => new Date().getFullYear())
</script>

<style scoped lang="scss">
.footer-section {
  background: linear-gradient(
    to right,
    rgba(0, 2, 5, 0) 0%,
    rgba(0, 255, 136, 0.377) 50%,
    rgba(0, 2, 5, 0) 100%
  );
  backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255, 255, 255, 0.05);
  padding: 3rem 2rem 2rem;
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);

  &.visible {
    opacity: 1;
    transform: translateY(0);
  }
}

.footer-container {
  max-width: 1200px;
  margin: 0 auto;
}

/* MAIN SECTION */
.footer-main {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 2rem;
  margin-bottom: 2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

.footer-info {
  .brand {
    margin-bottom: 1rem;

    h2 {
      font-size: 1.5rem;
      font-weight: 700;
      color: #ffffff;
      margin: 0 0 0.3rem 0;
      letter-spacing: 0.5px;
    }

    .tagline {
      font-size: 0.9rem;
      color: #90a4ae;
      margin: 0;
    }
  }
}

.footer-social {
  display: flex;
  gap: 1rem;
}

.social-link {
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  color: #cfd8dc;
  transition: all 0.3s ease;

  &:hover {
    background: rgba(0, 255, 136, 0.1);
    border-color: rgba(0, 255, 136, 0.3);
    color: #00ff88;
    transform: translateY(-3px);
  }
}

/* BOTTOM SECTION */
.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;

  .copyright {
    font-size: 0.85rem;
    color: #90a4ae;
    margin: 0;
  }

  .status {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.5rem 1rem;
    background: rgba(0, 255, 136, 0.05);
    border: 1px solid rgba(0, 255, 136, 0.2);
    border-radius: 50px;

    .status-dot {
      width: 6px;
      height: 6px;
      background: #00ff88;
      border-radius: 50%;
      animation: pulse 2s ease-in-out infinite;
    }

    .status-text {
      font-size: 0.8rem;
      color: #00ff88;
      font-weight: 500;
    }
  }

  .location {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.95rem;
    color: #90a4ae;
    margin: 0;

    svg {
      color: #00ff88;
    }
  }
}

@keyframes pulse {

  0%,
  100% {
    opacity: 1;
    box-shadow: 0 0 0 0 rgba(0, 255, 136, 0.7);
  }

  50% {
    opacity: 0.6;
    box-shadow: 0 0 0 4px rgba(0, 255, 136, 0);
  }
}

/* RESPONSIVE */

@media (max-width: 1023px) {
  .footer-main {
    flex-direction: column;
    gap: 2rem;
    align-items: center; 
    text-align: center;
  }

  .footer-social {
    width: 100%;
    justify-content: center; 
  }
}

@media (max-width: 767px) {
  .footer-section {
    padding: 2.5rem 1.5rem 1.5rem;
  }

  .footer-main {
    padding-bottom: 1.5rem;
    margin-bottom: 1.5rem;
  }

  .footer-info .brand {
    h2 {
      font-size: 1.3rem;
    }
    .tagline {
      font-size: 0.85rem;
    }
  }

  .footer-bottom {
    flex-direction: column;
    gap: 1.5rem;
    align-items: center; 
    text-align: center;
  }

  .social-link {
    width: 40px;
    height: 40px;

    svg {
      width: 20px;
      height: 20px;
    }
  }

  .footer-bottom .location {
    flex-wrap: wrap;
    justify-content: center;
    gap: 0.5rem;
  }
}

@media (max-width: 479px) {
  .footer-section {
    padding: 2rem 1rem 1.5rem;
  }

  .footer-main {
    gap: 1.5rem;
  }

  .footer-info .brand h2 {
    font-size: 1.2rem;
  }

  .footer-social {
    gap: 0.8rem;
    justify-content: center;
  }

  .social-link {
    width: 38px;
    height: 38px;

    svg {
      width: 18px;
      height: 18px;
    }
  }

  .footer-bottom {
    flex-direction: column;
    gap: 1rem;
    align-items: center;
    text-align: center;
  }
}

@media (min-width: 1400px) {
  .footer-container {
    max-width: 1400px;
  }

  .footer-section {
    padding: 4rem 3rem 2.5rem;
  }

  .footer-info .brand h2 {
    font-size: 1.7rem;
  }
}
</style>