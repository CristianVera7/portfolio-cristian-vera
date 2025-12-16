<template lang="pug">
nav.cyber-menu(:class="{ visible }")
  div.menu-wrapper
    ul
      li(@click="$emit('navigate', 'trajectory')")
        span Trajectory
        div.bar
      li(@click="$emit('navigate', 'projects')")
        span Projects
        div.bar
      li(@click="$emit('navigate', 'contact')")
        span Contact
        div.bar

</template>

<script setup lang="ts">
defineProps<{
  visible: boolean
}>()

defineEmits<{
  (e: 'navigate', section: string): void
}>()
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap");

$primary-color: #00ff88;
$secondary-color: #00d4ff;
$accent-color: #7fd9ff;
$bg-dark: #000205;
$glow-primary: rgba(0, 255, 136, 0.2);

/* ==========================
   CYBER MENU TOP
========================== */
.cyber-menu {
  position: fixed;
  top: 15%;
  left: 50%;
  // width: min(90%, 900px);
  width:100%;
  transform: translateX(-50%) translateY(-40px);
  z-index: 9999;

  font-family: 'Orbitron', sans-serif;
  opacity: 0;
  pointer-events: none;

  transition:
    opacity 0.6s ease,
    transform 0.6s cubic-bezier(0.17, 0.84, 0.44, 1);

  background: linear-gradient(
    to right,
    rgba(0, 2, 5, 0) 0%,
    rgba(0, 255, 136, 0.55) 50%,
    rgba(0, 2, 5, 0) 100%
  );

  backdrop-filter: blur(12px);

  &.visible {
    opacity: 1;
    pointer-events: auto;
    transform: translateX(-50%) translateY(0);
  }

  ul {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 3rem;
    list-style: none;
    padding: 0.8rem 2rem;
    margin: 0;
  }

  li {
    position: relative;
    color: #000;
    font-size: 1.3rem;
    font-weight: 700;
    letter-spacing: 3px;
    text-transform: uppercase;
    cursor: pointer;
    user-select: none;

    transition: transform 0.25s ease, color 0.25s ease;

    &:hover {
      transform: scale(1.08);
      color: $secondary-color;
    }

    span {
      position: relative;
      z-index: 2;
      text-shadow: 0 0 6px rgba(0, 255, 180, 0.4);
    }

    .bar {
      position: absolute;
      left: 0;
      bottom: -3px;
      width: 100%;
      height: 2px;
      background: linear-gradient(
        90deg,
        transparent,
        $secondary-color,
        transparent
      );
      transform: scaleX(0);
      transform-origin: center;
      transition: transform 0.35s ease;
    }

    &:hover .bar {
      transform: scaleX(1);
    }
  }
}


/* -----------------------------
   GLITCH ANIMATION
------------------------------ */
@keyframes cyber-glitch {
  0% {
    text-shadow: 1px 0 $accent-color, -1px 0 $secondary-color;
  }

  50% {
    text-shadow: -1px 0 $accent-color, 1px 0 $secondary-color;
  }

  100% {
    text-shadow: 0 0 $primary-color;
  }
}

/* ==============================
  RESPONSIVE
============================== */
@media (max-width: 768px) {
  .cyber-menu {
    top: 15%;
  }

  .cyber-menu ul {
    gap: 1.2rem;
    padding: 0.6rem 1.2rem;
  }

  .cyber-menu li {
    font-size: 1rem;
    letter-spacing: 2px;
  }
}

@media (max-width: 480px) {
  .cyber-menu ul {
    flex-direction: column;
    gap: 0.8rem;
  }

  .cyber-menu li {
    font-size: 0.95rem;
  }
}


</style>
