<template lang="pug">
div(class="terminal-window" :class="{ visible: visible, minimized: minimized }")
  div(class="terminal-header")
    div(class="terminal-buttons")
      span(class="btn close")
      span(class="btn minimize" @click="$emit('minimize')")
      span(class="btn maximize")
    span(class="terminal-title") ASSISTANT.PROTOCOL.EXE

  div(class="terminal-body")
    div(
      class="terminal-line"
      v-for="(line, index) in lines"
      :key="index"
      v-show="index <= currentLine"
    )
      span(class="prompt" v-if="line.prompt") {{ line.prompt }}
      span(class="text" :class="line.type") {{ line.text }}
      span(class="cursor" v-if="index === currentLine")
</template>

<script setup lang="ts">
interface TerminalLine {
  prompt?: string
  text: string
  type?: string
}

defineProps<{
  visible: boolean
  minimized: boolean
  lines: TerminalLine[]
  currentLine: number
}>()

defineEmits(["minimize"])
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

.terminal-window {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.9);
  background: linear-gradient(180deg, $terminal-bg, rgba(6, 7, 9, 0.85));
  border: 1.5px solid $primary-color;
  border-radius: 12px;
  box-shadow: 0 8px 70px $shadow-dark, inset 0 2px 15px rgba($primary-color, 0.03);
  overflow: hidden;
  backdrop-filter: blur(8px) saturate(1.2);
  width: min(90vw, 28rem);
  max-width: 28rem;
  z-index: 1000;
  opacity: 0;
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);

  &.visible {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }

  &.minimized {
    top: 17%;
    left: 2%;
    transform: translate(0, 0) scale(0.85);
    width: min(90vw, 22rem);
    max-width: 22rem;
  }
}

.terminal-header {
  background: rgba(0, 0, 0, 0.2);
  padding: clamp(0.45rem, 1.5vw, 0.6rem) clamp(0.9rem, 2vw, 1.1rem);
  display: flex;
  align-items: center;
  gap: 1rem;
  border-bottom: 1px solid $primary-color;
}

.terminal-buttons {
  display: flex;
  gap: 0.6rem;
}

.btn {
  width: clamp(11px, 2vw, 13px);
  height: clamp(11px, 2vw, 13px);
  border-radius: 50%;
  border: 1px solid rgba($primary-color, 0.1);
  transition: all 0.3s ease;

  &.close {
    background: #ff5c5c;
    box-shadow: 0 0 8px rgba(255, 90, 90, 0.1);
  }

  &.minimize {
    background: #ffd36b;
    box-shadow: 0 0 8px rgba(255, 211, 107, 0.08);
  }

  &.maximize {
    background: #69ffb3;
    box-shadow: 0 0 10px rgba(105, 255, 179, 0.12);
  }
}

.terminal-title {
  font-size: clamp(0.75rem, 1.8vw, 0.88rem);
  color: #9fffd2;
  letter-spacing: 0.04em;
}

.terminal-body {
  padding: clamp(1.1rem, 3vw, 1.5rem);
  color: $text-light;
}

.terminal-line {
  margin-bottom: 0.6rem;
  opacity: 0;
  animation: fade-in 0.35s ease forwards;
  font-size: clamp(0.78rem, 1.8vw, 0.92rem);
}

.prompt {
  color: $primary-color;
  margin-right: 0.6rem;
}

.text {
  &.success {
    color: #a8ffcc;
  }

  &.info {
    color: #8fd7ff;
  }

  &.error {
    color: #ff8b8b;
  }
}

.cursor {
  display: inline-block;
  width: 9px;
  height: 15px;
  background: $primary-color;
  margin-left: 6px;
  animation: cursor-blink 900ms infinite;
  border-radius: 2px;
}

@keyframes cursor-blink {

  0%,
  50% {
    opacity: 1;
  }

  51%,
  100% {
    opacity: 0;
  }
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(4px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}


/* ==============================
   RESPONSIVE
============================== */
@media (max-width: 1000px) {
  .terminal-window {
    width: min(80vw, 26rem);
    max-width: 26rem;

    &.minimized {
      top: 25%;
      left: 33%;
      width: min(85vw, 20rem);
    }
  }

  .terminal-body {
    padding: clamp(0.8rem, 2.5vw, 1.2rem);
    font-size: clamp(0.7rem, 1.5vw, 0.85rem);
  }

  .terminal-header {
    padding: clamp(0.4rem, 1.2vw, 0.6rem) clamp(0.7rem, 2vw, 1rem);
  }

  .terminal-title {
    font-size: clamp(0.7rem, 1.5vw, 0.85rem);
  }
}

@media (max-width: 768px) {
  .terminal-window {
    width: 90vw;
    &.minimized {
      width: 80vw;
      transform: translate(0, 0) scale(0.8);
    }
  }
}

@media (max-width: 480px) {
  .terminal-window {
    width: 95vw;
    &.minimized {
      width: 90vw;
      transform: translate(0, 0) scale(0.75);
      top: 10%;
    }
  }
}

</style>
