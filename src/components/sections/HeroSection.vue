<template lang="pug">
  div(class="hero-section" :class="{ visible: visible }" ref="heroSection")
    div(class="hero-title")
      RobotHead(
        :mouthState="mouthState"
        :isBlinking="isBlinking"
        :mouseX="mouseX"
        :mouseY="mouseY"
        :robotVisible="robotVisible"
      )
      div(class="glitch-wrapper")
        div(class="title-main" data-text="CRISTIAN VERA") CRISTIAN VERA
      div(class="subtitle-dev")
        span(class="bracket") [
        span(class="dev-text") FULL STACK DEVELOPER
        span(class="bracket") ]
      div(class="tech-stack")
        span(
          class="tech-item"
          v-for="(tech, i) in techStack"
          :key="i"
          :style="{ animationDelay: (i * 0.1) + 's' }"
        ) {{ tech }}
</template>

<script setup lang="ts">
import { ref } from 'vue'
import RobotHead from '../RobotHead.vue'

interface Props {
  visible: boolean
  mouthState: 'idle' | 'talking'
  isBlinking: boolean
  mouseX: number
  mouseY: number
  robotVisible: boolean
  techStack: string[]
}

const props = defineProps<Props>()
const heroSection = ref<HTMLElement | null>(null)
</script>

<style scoped lang="scss">
$primary-color: #00ff88;
$secondary-color: #00d4ff;
$accent-color: #7fd9ff;
$bg-dark: #000205;
$text-light: #bfffe6;
$terminal-bg: rgba(8, 10, 12, 0.92);
$shadow-dark: rgba(0, 0, 0, 0.8);
$glow-primary: rgba(0, 255, 136, 0.15);

/* ========================================
   HERO SECTION
======================================== */
.hero-section {
  margin-top: 100px;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: clamp(2rem, 5vw, 4rem) clamp(1.5rem, 3vw, 2.5rem);
  opacity: 0;
  transform: translateY(50px);
  transition: all 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
  z-index: 1;

  &.visible {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-title {
  text-align: center;
  width: 100%;
  max-width: 1200px;
}

.glitch-wrapper {
  position: relative;
  margin-bottom: 1.5rem;
}

.title-main {
  font-family: "Orbitron", sans-serif;
  font-size: clamp(2.5rem, 8vw,3.8rem);
  font-weight: 900;
  color: $primary-color;
  text-shadow:
    0 0 20px rgba($primary-color, 0.8),
    0 0 40px rgba($primary-color, 0.5),
    0 0 60px rgba($primary-color, 0.3);
  letter-spacing: 0.1em;
  position: relative;
  animation: glow-pulse 2s ease-in-out infinite alternate;

  &::before,
  &::after {
    content: attr(data-text);
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    opacity: 0.8;
  }

  &::before {
    color: #00ffff;
    animation: glitch-1 2.5s infinite;
    clip-path: polygon(0 0, 100% 0, 100% 45%, 0 45%);
  }

  &::after {
    color: #000000;
    animation: glitch-2 3s infinite;
    clip-path: polygon(0 55%, 100% 55%, 100% 100%, 0 100%);
  }
}

@keyframes glow-pulse {
  from {
    filter: brightness(1);
  }

  to {
    filter: brightness(1.2);
  }
}

@keyframes glitch-1 {

  0%,
  90%,
  100% {
    transform: translate(0);
  }

  92% {
    transform: translate(-2px, 2px);
  }

  94% {
    transform: translate(2px, -2px);
  }

  96% {
    transform: translate(-2px, 0);
  }
}

@keyframes glitch-2 {

  0%,
  85%,
  100% {
    transform: translate(0);
  }

  87% {
    transform: translate(2px, -2px);
  }

  89% {
    transform: translate(-2px, 2px);
  }

  91% {
    transform: translate(2px, 0);
  }
}

.subtitle-dev {
  font-family: "Rajdhani", sans-serif;
  font-size: clamp(1.1rem, 3vw, 1.7rem);
  color: $secondary-color;
  letter-spacing: clamp(0.12em, 0.3vw, 0.25em);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;

  .bracket {
    color: $primary-color;
    font-size: 1.4em;
    font-weight: bold;
    text-shadow: 0 0 15px rgba($primary-color, 0.8);
    animation: bracket-pulse 2s ease-in-out infinite;
  }

  .dev-text {
    background: linear-gradient(90deg, #00ff88, #00ffff, #00ff88);
    background-size: 200% auto;
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: gradient-flow 3s linear infinite;
    font-weight: 700;
  }
}

@keyframes bracket-pulse {

  0%,
  100% {
    opacity: 1;
    transform: scale(1);
  }

  50% {
    opacity: 0.7;
    transform: scale(1.1);
  }
}

@keyframes gradient-flow {
  0% {
    background-position: 0% center;
  }

  100% {
    background-position: 200% center;
  }
}

.tech-stack {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
  margin-top: 1.5rem;
}

.tech-item {
  font-family: "Share Tech Mono", monospace;
  font-size: clamp(0.75rem, 1.6vw, 0.9rem);
  color: $accent-color;
  padding: 0.5rem 1.1rem;
  border: 1px solid rgba($primary-color, 0.4);
  border-radius: 8px;
  background: rgba(0, 255, 136, 0.05);
  box-shadow: 0 0 15px rgba($primary-color, 0.1);
  opacity: 0;
  animation: tech-fade-in 0.6s ease forwards;
  transition: all 0.3s ease;

  &:hover {
    background: rgba(0, 255, 136, 0.15);
    box-shadow: 0 0 25px rgba($primary-color, 0.3);
    transform: translateY(-3px);
  }
}

@keyframes tech-fade-in {
  from {
    opacity: 0;
    transform: translateY(10px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* ==============================
  RESPONSIVE
============================== */
@media (max-width: 1629px) {
  .hero-section {
    margin-top: 10rem;
  }

}

@media (max-width: 1000px) {
  .hero-section {
    padding: clamp(2rem, 4vw, 3rem);
    margin-top: 15rem;
  }

  .title-main {
    font-size: clamp(2rem, 6vw, 4.5rem);
  }

  .subtitle-dev {
    font-size: clamp(1rem, 2.5vw, 1.5rem);
    gap: 0.5rem;
  }

  .tech-item {
    font-size: clamp(0.65rem, 1.5vw, 0.85rem);
    padding: 0.4rem 0.9rem;
  }
}

@media (max-width: 768px) {
  .hero-section {
    flex-direction: column;
    padding: clamp(1.5rem, 4vw, 2.5rem);
  }

  .title-main {
    font-size: clamp(1.8rem, 7vw, 3.8rem);
  }

  .subtitle-dev {
    font-size: clamp(0.95rem, 3vw, 1.2rem);
  }

  .tech-item {
    font-size: clamp(0.6rem, 2vw, 0.8rem);
    padding: 0.35rem 0.8rem;
  }
}

@media (max-width: 480px) {
  .hero-section {
    padding: clamp(1rem, 4vw, 2rem);
  }

  .title-main {
    font-size: clamp(1.5rem, 8vw, 3rem);
  }

  .subtitle-dev {
    flex-direction: row;      
    flex-wrap: nowrap;        
    white-space: nowrap;      
    gap: 0.4rem;              
    font-size: clamp(0.85rem, 3.5vw, 1rem);
  }

  .tech-stack {
    gap: 0.5rem;
  }

  .tech-item {
    font-size: clamp(0.55rem, 2.5vw, 0.7rem);
    padding: 0.3rem 0.7rem;
  }
}
</style>
