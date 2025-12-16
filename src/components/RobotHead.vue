<template lang="pug">
div(class="robot-section" :class="{ visible: robotVisible }")
  div(class="robot-container")
    div(class="robot-head" :class="{ 'speaking': mouthState === 'talking' }")
      div(class="robot-face")
        div(class="eye-container")
          div(class="eye left" :class="{ blink: isBlinking }")
            div(class="pupil" :style="pupilLeftStyle")
            div(class="glare")
            div(class="iris-ring")
          div(class="eye right" :class="{ blink: isBlinking }")
            div(class="pupil" :style="pupilRightStyle")
            div(class="glare")
            div(class="iris-ring")
        div(class="mouth" :class="mouthState")
          div(class="mouth-light")
          div(class="mouth-grid")
        div(class="antenna")
          div(class="antenna-tip")
            div(class="pulse-ring")
      div(class="head-details")
        div(class="ear left")
        div(class="ear right")
        div(class="head-shine")
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, defineProps } from 'vue'

const props = defineProps<{
  mouthState: 'idle' | 'talking'
  isBlinking: boolean
  robotVisible: boolean
}>()

// --- Posición del ratón ---
const mouseX = ref(window.innerWidth / 2)
const mouseY = ref(window.innerHeight / 2)

const updateMouse = (event: MouseEvent) => {
  mouseX.value = event.clientX
  mouseY.value = event.clientY
}

onMounted(() => window.addEventListener('mousemove', updateMouse))
onUnmounted(() => window.removeEventListener('mousemove', updateMouse))

// --- Estilos de las pupilas ---
const pupilLeftStyle = computed(() => {
  const tx = ((mouseX.value / window.innerWidth) - 0.5) * 10
  const ty = ((mouseY.value / window.innerHeight) - 0.5) * 8
  return { transform: `translate(calc(-50% + ${tx}px), ${ty}px)` }
})

const pupilRightStyle = computed(() => {
  const tx = ((mouseX.value / window.innerWidth) - 0.5) * 10
  const ty = ((mouseY.value / window.innerHeight) - 0.5) * 8
  return { transform: `translate(calc(-50% + ${tx}px), ${ty}px)` }
})
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@300;500;700;900&family=Orbitron:wght@400;700;900&display=swap");

$primary-color: #00ff88;
$shadow-dark: rgba(0, 0, 0, 0.8);

/* ========================================
   ROBOT SECTION
======================================== */
.robot-section {
  height: 20vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: clamp(2rem, 5vw, 4rem) clamp(1.5rem, 3vw, 2.5rem);
  opacity: 0;
  transform: translateY(50px);
  transition: all 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);

  &.visible {
    opacity: 1;
    transform: translateY(0);
  }
}

.robot-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 700px;
}

/* ================== ROBOT HEAD ================== */
.robot-head {
  position: relative;
  width: clamp(180px, 38vw, 240px);
  height: clamp(160px, 34vw, 220px);
  background: linear-gradient(180deg, #0f1117, #0b0d12);
  border-radius: 50% 50% 44% 44%;
  border: 2.5px solid $primary-color;
  box-shadow:
    0 12px 70px $shadow-dark,
    inset 0 -4px 35px rgba($primary-color, 0.03),
    0 0 50px rgba($primary-color, 0.1);
  transition: all 0.4s ease;
  animation: float 6s ease-in-out infinite;
  margin-bottom: 2rem;

  &.speaking {
    box-shadow:
      0 18px 90px rgba($primary-color, 0.15),
      inset 0 -6px 45px rgba($primary-color, 0.08),
      0 0 70px rgba($primary-color, 0.2);
  }
}

@keyframes float {

  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-12px);
  }
}

.head-shine {
  position: absolute;
  top: 10%;
  left: 20%;
  width: 60%;
  height: 30%;
  background: radial-gradient(ellipse at center, rgba(255, 255, 255, 0.08) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
}

.robot-face {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 82%;
}

/* ================== EYES ================== */
.eye-container {
  display: flex;
  justify-content: space-between;
  gap: clamp(12px, 2.8vw, 16px);
  margin-bottom: clamp(24px, 5vw, 34px);
}

.eye {
  width: clamp(50px, 11vw, 65px);
  height: clamp(56px, 12vw, 74px);
  background: linear-gradient(180deg, #000, #06060a);
  border-radius: 50% / 60% 60% 40% 40%;
  border: 2px solid $primary-color;
  position: relative;
  overflow: hidden;
  box-shadow: 0 0 30px $shadow-dark, inset 0 0 45px rgba($primary-color, 0.03);
  transition: all 0.15s ease;

  &.blink {
    height: 8px;
    transform: translateY(8px);
  }
}

.iris-ring {
  position: absolute;
  bottom: clamp(8px, 2vw, 12px);
  left: 50%;
  transform: translateX(-50%);
  width: clamp(28px, 6vw, 36px);
  height: clamp(28px, 6vw, 36px);
  border: 2px solid rgba($primary-color, 0.3);
  border-radius: 50%;
  animation: iris-pulse 3s ease-in-out infinite;
}

@keyframes iris-pulse {

  0%,
  100% {
    transform: translateX(-50%) scale(1);
    opacity: 0.3;
  }

  50% {
    transform: translateX(-50%) scale(1.15);
    opacity: 0.6;
  }
}

.pupil {
  position: absolute;
  bottom: clamp(12px, 3vw, 16px);
  left: 50%;
  width: clamp(22px, 5vw, 28px);
  height: clamp(22px, 5vw, 28px);
  background: radial-gradient(circle at 35% 35%, #d0ffed, $primary-color 50%, #00764f 100%);
  border-radius: 50%;
  box-shadow: 0 0 30px rgba($primary-color, 1), 0 3px 10px $shadow-dark, inset 0 2px 8px rgba(255, 255, 255, 0.2);
  transition: transform 0.15s cubic-bezier(0.2, 0.9, 0.2, 1);
  z-index: 5;
}

.glare {
  position: absolute;
  top: clamp(8px, 2vw, 12px);
  left: clamp(14px, 3vw, 18px);
  width: clamp(8px, 2vw, 11px);
  height: clamp(8px, 2vw, 11px);
  background: rgba(255, 255, 255, 1);
  border-radius: 50%;
  filter: blur(0.5px);
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.8);
}

/* ================== MOUTH ================== */
.mouth {
  width: clamp(85px, 18vw, 110px);
  height: clamp(18px, 4vw, 24px);
  margin: 0 auto;
  background: linear-gradient(180deg, #000, #050606);
  border: 2px solid $primary-color;
  border-radius: 0 0 50px 50px;
  box-shadow: inset 0 -8px 25px rgba($primary-color, 0.04);
  overflow: hidden;
  position: relative;

  &.talking .mouth-light {
    animation: voice-wave 0.5s ease-in-out infinite;
    opacity: 0.6;
  }
}

.mouth-light {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100%;
  background: linear-gradient(to top, rgba($primary-color, 0.7), transparent);
  opacity: 0.2;
}

.mouth-grid {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100%;
  background-image: repeating-linear-gradient(0deg, rgba($primary-color, 0.1) 0px, transparent 1px, transparent 3px, rgba($primary-color, 0.1) 4px);
  opacity: 0.3;
}

@keyframes voice-wave {

  0%,
  100% {
    transform: scaleY(1);
    opacity: 0.6;
  }

  50% {
    transform: scaleY(1.4);
    opacity: 0.8;
  }
}

/* ================== ANTENNA ================== */
.antenna {
  position: absolute;
  top: clamp(-70px, -14vw, -88px);
  left: 50%;
  transform: translateX(-50%);
  width: clamp(4px, 1vw, 5px);
  height: clamp(40px, 9vw, 52px);
  background: linear-gradient(to top, $primary-color, #00c48a);
  border-radius: 3px;
  box-shadow: 0 0 10px rgba($primary-color, 0.5);
}

.antenna-tip {
  position: absolute;
  top: clamp(-14px, -3vw, -18px);
  left: 50%;
  transform: translateX(-50%);
  width: clamp(16px, 3.5vw, 20px);
  height: clamp(16px, 3.5vw, 20px);
  background: radial-gradient(circle at 30% 20%, #fff 0%, #c8ffea 10%, $primary-color 50%, #00764f 100%);
  border-radius: 50%;
  box-shadow: 0 0 30px rgba($primary-color, 1), 0 0 50px rgba($primary-color, 0.6);
  animation: antenna-pulse 3s ease-in-out infinite;
}

.pulse-ring {
  position: absolute;
  inset: -10px;
  border: 2px solid $primary-color;
  border-radius: 50%;
  opacity: 0;
  animation: pulse-ring 3s ease-in-out infinite;
}

@keyframes antenna-pulse {

  0%,
  100% {
    transform: translateX(-50%) scale(1);
  }

  50% {
    transform: translateX(-50%) scale(1.25);
    box-shadow: 0 0 40px rgba($primary-color, 1.2), 0 0 70px rgba($primary-color, 0.8);
  }
}

@keyframes pulse-ring {
  0% {
    opacity: 0;
    transform: scale(0.8);
  }

  50% {
    opacity: 0.6;
    transform: scale(1.2);
  }

  100% {
    opacity: 0;
    transform: scale(1.5);
  }
}

/* ================== HEAD DETAILS ================== */
.head-details {
  position: absolute;
  inset: 0;
}

.circuit {
  position: absolute;
  width: clamp(28px, 6vw, 38px);
  height: 2.5px;
  background: linear-gradient(90deg, $primary-color, #00c48a);
  top: 42%;
  box-shadow: 0 0 15px rgba($primary-color, 0.12);
}

.circuit.left {
  left: clamp(-36px, -7vw, -46px);
  transform: rotate(-30deg);
}

.circuit.right {
  right: clamp(-36px, -7vw, -46px);
  transform: rotate(30deg);
}

.circuit::before,
.circuit::after {
  content: '';
  position: absolute;
  width: clamp(8px, 2vw, 10px);
  height: clamp(8px, 2vw, 10px);
  background: $primary-color;
  border-radius: 50%;
  box-shadow: 0 0 15px rgba($primary-color, 0.12);
  top: -4px;
}

.circuit::before {
  left: 0;
}

.circuit::after {
  right: 0;
}

.ear {
  position: absolute;
  width: clamp(24px, 5.5vw, 34px);
  height: clamp(42px, 9vw, 54px);
  background: linear-gradient(180deg, #0f1117, #0a0c10);
  border: 2px solid $primary-color;
  top: 36%;
  box-shadow: 0 0 20px rgba($primary-color, 0.06);
}

.ear.left {
  left: clamp(-30px, -6vw, -40px);
  border-radius: 18px 0 0 18px;
}

.ear.right {
  right: clamp(-30px, -6vw, -40px);
  border-radius: 0 18px 18px 0;
}
</style>
