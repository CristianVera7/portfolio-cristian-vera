<template lang="pug">
div(class="hud-overlay")
  div.hud-top-bar
  div(class="hud-corner tl")
  div(class="hud-corner tr")
  div(class="hud-corner bl")
  div(class="hud-corner br")

  div(class="scan-line" v-if="scanLineVisible" :style="scanLineStyle")

  div(class="system-info top-left" :class="{ visible: systemInfoVisible }")
    span SYSTEM_ID: CV_2024_v3.7
    span STATUS: {{ systemStatus }}
    span UPTIME: {{ uptime }}s

  div(class="system-info top-right" :class="{ visible: systemInfoVisible }")
    span NEURAL_CORE: ACTIVE
    span AI_MODE: PRESENTATION
    span PROTOCOL: INITIALIZED
</template>

<script setup lang="ts">
import { computed, defineProps } from 'vue'

interface Props {
  scanLineVisible: boolean
  scanLinePosition: number
  systemInfoVisible: boolean
  systemStatus: string
  uptime: number
}

const props = defineProps<Props>()

const scanLineStyle = computed(() => ({
  top: `${props.scanLinePosition}px`
}))
</script>

<style scoped lang="scss">
$primary-color: #00ff88;
$glow-primary: rgba(0, 255, 136, 0.25);

/* ========================================
   HUD OVERLAY
======================================== */
.hud-overlay {
  position: fixed;
  inset: 0;
  z-index: 999;
  pointer-events: none;
}

/* ========================================
   HUD CORNERS – CLEAN NEON 
======================================== */
/* Barra negra superior */
.hud-top-bar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: clamp(200px, 5vw, 80px); // Ajustable según el tamaño de tus HUD corners
  background-color: #000000e8;
  z-index: 1000; // Debe estar debajo de los HUD corners (z-index:6) pero encima del contenido
  pointer-events: none; // No bloquea clicks
}

.hud-corner {
  position: fixed;
  width: clamp(45px, 8vw, 70px);
  height: clamp(45px, 8vw, 70px);
  opacity: 0.7;
  pointer-events: none;

  &::before,
  &::after {
    content: "";
    position: absolute;
    background: $primary-color;
    box-shadow: 0 0 12px rgba($primary-color, 0.6);
    animation: hud-pulse 3.5s ease-in-out infinite;
  }
}

/* ───────── TOP LEFT ───────── */
.hud-corner.tl {
  top: clamp(15px, 2vw, 20px);
  left: clamp(15px, 2vw, 20px);
  z-index: 1000;

  &::before {
    width: 2.5px;
    height: 100%;
    left: 0;
    top: 0;
  }

  &::after {
    height: 2.5px;
    width: 100%;
    left: 0;
    top: 0;
  }
}

/* ───────── TOP RIGHT ───────── */
.hud-corner.tr {
  top: clamp(15px, 2vw, 20px);
  right: clamp(15px, 2vw, 20px);
  z-index: 1000;

  &::before {
    width: 2.5px;
    height: 100%;
    right: 0;
    top: 0;
  }

  &::after {
    height: 2.5px;
    width: 100%;
    right: 0;
    top: 0;
  }
}

/* ───────── BOTTOM LEFT ───────── */
.hud-corner.bl {
  bottom: clamp(15px, 2vw, 20px);
  left: clamp(15px, 2vw, 20px);

  &::before {
    width: 2.5px;
    height: 100%;
    left: 0;
    bottom: 0;
  }

  &::after {
    height: 2.5px;
    width: 100%;
    left: 0;
    bottom: 0;
  }
}

/* ───────── BOTTOM RIGHT ───────── */
.hud-corner.br {
  bottom: clamp(15px, 2vw, 20px);
  right: clamp(15px, 2vw, 20px);

  &::before {
    width: 2.5px;
    height: 100%;
    right: 0;
    bottom: 0;
  }

  &::after {
    height: 2.5px;
    width: 100%;
    right: 0;
    bottom: 0;
  }
}

/* ========================================
   PULSE 
======================================== */
@keyframes hud-pulse {

  0%,
  100% {
    opacity: 0.45;
  }

  50% {
    opacity: 0.8;
  }
}

/* ========================================
   SCAN LINE
======================================== */
.scan-line {
  position: fixed;
  left: 0;
  width: 100%;
  height: 3px;
  background: linear-gradient(90deg,
      transparent,
      rgba($primary-color, 1),
      transparent);
  filter: blur(2px);
  box-shadow:
    0 0 20px rgba($primary-color, 0.8),
    0 0 40px rgba($primary-color, 0.4);
  z-index: 100;
  pointer-events: none;
  transition: top 0.3s ease-out;
}

/* ========================================
   SYSTEM INFO
======================================== */
.system-info {
  position: fixed;
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  font-size: clamp(0.65rem, 1.5vw, 0.8rem);
  color: #8fffd3;
  opacity: 0;
  z-index: 1000;
  text-shadow: 0 0 12px rgba($primary-color, 0.15);
  transition: opacity 0.8s ease;

  &.visible {
    opacity: 0.9;
  }

  &.top-left {
    top: clamp(30px, 5vw, 38px);
    left: clamp(45px, 15vw, 45px);
  }

  &.top-right {
    top: clamp(30px, 5vw, 38px);
    right: clamp(45px, 15vw, 45px);
    align-items: flex-end;
  }
}
</style>
