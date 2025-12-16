<template>
  <canvas ref="canvas" class="neural-network"></canvas>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

interface Particle {
  x: number
  y: number
  vx: number
  vy: number
  radius: number
  hue: number
  alpha: number
  life: number
}

const canvas = ref<HTMLCanvasElement | null>(null)
let animationFrameId = 0
let resizeHandler: () => void
let mouseMoveHandler: (e: MouseEvent) => void

onMounted(() => {
  const ctx = canvas.value?.getContext('2d')
  if (!canvas.value || !ctx) return

  const setSize = () => {
    if (!canvas.value) return
    const dpr = window.devicePixelRatio || 1
    canvas.value.width = Math.floor(window.innerWidth * dpr)
    canvas.value.height = Math.floor(window.innerHeight * dpr)
    canvas.value.style.width = `${window.innerWidth}px`
    canvas.value.style.height = `${window.innerHeight}px`
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0)
  }
  setSize()

  const particles: Particle[] = []
  const particleCount = Math.max(80, Math.floor((window.innerWidth * window.innerHeight) / 50000))

  for (let i = 0; i < particleCount; i++) {
    particles.push({
      x: Math.random() * (canvas.value.width / (window.devicePixelRatio || 1)),
      y: Math.random() * (canvas.value.height / (window.devicePixelRatio || 1)),
      vx: (Math.random() - 0.5) * 0.4,
      vy: -0.25 - Math.random() * 0.25,
      radius: 0.8 + Math.random() * 2.5,
      hue: 160 + Math.random() * 50,
      alpha: 0.3 + Math.random() * 0.7,
      life: Math.random()
    })
  }

  let parallaxX = 0
  let parallaxY = 0

  mouseMoveHandler = (e: MouseEvent) => {
    parallaxX = ((e.clientX / window.innerWidth) - 0.5) * 20
    parallaxY = ((e.clientY / window.innerHeight) - 0.5) * 15
  }
  window.addEventListener('mousemove', mouseMoveHandler)

  resizeHandler = () => setSize()
  window.addEventListener('resize', resizeHandler)

  const draw = () => {
    if (!canvas.value) return
    const w = canvas.value.width / (window.devicePixelRatio || 1)
    const h = canvas.value.height / (window.devicePixelRatio || 1)

    // Fondo y gradientes
    const bgGrad = ctx.createRadialGradient(w / 2, h / 3, 0, w / 2, h / 2, Math.max(w, h) * 0.8)
    bgGrad.addColorStop(0, 'rgba(2,12,20,0.98)')
    bgGrad.addColorStop(0.5, 'rgba(1,8,14,0.95)')
    bgGrad.addColorStop(1, 'rgba(0,2,6,0.98)')
    ctx.fillStyle = bgGrad
    ctx.fillRect(0, 0, w, h)

    // Vignette
    ctx.save()
    const vignette = ctx.createRadialGradient(w / 2, h / 2, Math.min(w, h) * 0.3, w / 2, h / 2, Math.max(w, h) * 0.7)
    vignette.addColorStop(0, 'rgba(0,0,0,0)')
    vignette.addColorStop(1, 'rgba(0,0,0,0.5)')
    ctx.fillStyle = vignette
    ctx.fillRect(0, 0, w, h)
    ctx.restore()

    // Parallax
    canvas.value.style.transform = `translate(${parallaxX}px, ${parallaxY}px) scale(1.03)`

    // Grid lines
    ctx.save()
    ctx.globalAlpha = 0.06
    ctx.strokeStyle = 'rgba(0, 255, 170, 0.15)'
    ctx.lineWidth = 0.8
    for (let i = 0; i < 10; i++) {
      const y = h * (i / 10) + (Date.now() / 1000) % 10
      ctx.beginPath()
      ctx.moveTo(0, y)
      ctx.lineTo(w, y + Math.sin(Date.now() / 1500 + i) * 8)
      ctx.stroke()
    }
    ctx.restore()

    // Partículas
    particles.forEach((p, i) => {
      const cx = w * 0.5
      const cy = h * 0.3
      const dx = cx - p.x
      const dy = cy - p.y
      const dist = Math.sqrt(dx * dx + dy * dy) + 0.0001
      const attraction = 0.0009

      p.vx += (dx / dist) * attraction + (Math.random() - 0.5) * 0.003
      p.vy += (dy / dist) * attraction - 0.001 + (Math.random() - 0.5) * 0.003
      p.vx = Math.max(-0.7, Math.min(0.7, p.vx))
      p.vy = Math.max(-1.3, Math.min(0.7, p.vy))

      p.x += p.vx
      p.y += p.vy

      if (p.x < -10) p.x = w + 10
      if (p.x > w + 10) p.x = -10
      if (p.y < -20) p.y = h + 20
      if (p.y > h + 20) p.y = -20

      p.life = (p.life + 0.01) % 1

      const glow = 0.5 + Math.sin(Date.now() / 700 + i) * 0.5
      const pulsate = 0.7 + Math.sin(p.life * Math.PI * 2) * 0.3

      ctx.save()
      ctx.globalCompositeOperation = 'screen'
      ctx.beginPath()
      const finalAlpha = p.alpha * glow * pulsate
      ctx.fillStyle = `rgba(0,${180 + (p.hue - 160) * 1.5},${150 + (p.hue - 160) * 2},${finalAlpha})`
      ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2)
      ctx.fill()
      ctx.restore()
    })

    // Líneas de conexión
    ctx.save()
    ctx.globalCompositeOperation = 'lighter'
    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const p1 = particles[i]
        const p2 = particles[j]
        if (p1 && p2) {
          const dx = p1.x - p2.x
          const dy = p1.y - p2.y

          const d = Math.sqrt(dx * dx + dy * dy)
          if (d < 130) {
            const alpha = 0.15 * (1 - d / 130)
            ctx.strokeStyle = `rgba(0,255,180,${alpha})`
            ctx.lineWidth = 0.8
            ctx.beginPath()
            ctx.moveTo(p1.x, p1.y)
            ctx.lineTo(p2.x, p2.y)
            ctx.stroke()
          }
        }

      }
    }
    ctx.restore()

    animationFrameId = requestAnimationFrame(draw)
  }

  draw()
})

onUnmounted(() => {
  cancelAnimationFrame(animationFrameId)
  window.removeEventListener('resize', resizeHandler)
  window.removeEventListener('mousemove', mouseMoveHandler)
})
</script>

<style scoped lang="scss">
.neural-network {
  position: fixed;
  inset: 0;
  z-index: 0;
  will-change: transform;
  transition: transform 0.6s cubic-bezier(0.2, 0.9, 0.2, 1);
  pointer-events: none;
  filter: saturate(1.1) contrast(1.08) brightness(1.02);
}
</style>
