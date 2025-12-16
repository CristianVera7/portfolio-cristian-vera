<template lang="pug">
  div(class="robot-interface" ref="mainContainer")
    // Canvas principal de partículas
    NeuralCanvas

    // HUD (barra de estado, scan line, etc.)
    HUD(
      :scanLineVisible="scanLineVisible"
      :scanLinePosition="scanLinePosition"
      :systemInfoVisible="systemInfoVisible"
      :systemStatus="systemStatus"
      :uptime="uptime"
    )

    // Menú lateral
    Menu(:visible="menuVisible" @navigate="scrollToSection")

    // Terminal de arranque
    TerminalWindow(
      :visible="terminalVisible"
      :minimized="terminalMinimized"
      :lines="terminalLines"
      :currentLine="currentLine"
      @minimize="terminalMinimized = !terminalMinimized"
    )

    // Sección de contenido principal
    div(class="content")
      HeroSection(
        :visible="heroVisible"
        :titleVisible="heroTitleVisible"
        :mouthState="mouthState"
        :isBlinking="isBlinking"
        :mouseX="mouseX"
        :mouseY="mouseY"
        :robotVisible="robotVisible"
        :techStack="techStack"
      )
    div.page-content
      TrayectorySection
      ProjectsSection
      ContactSection

    // Footer al final
    FooterSection(:visible="footerVisible" ref="footerSection")
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'

// Componentes
import NeuralCanvas from './components/NeuralCanvas.vue'
import HUD from './components/HUD.vue'
import Menu from './components/Menu.vue'
import TerminalWindow from './components/TerminalWindow.vue'
import HeroSection from './components/sections/HeroSection.vue'
import TrayectorySection from './components/sections/TrayectorySection.vue'
import ProjectsSection from './components/sections/ProjectsSection.vue'
import ContactSection from './components/sections/ContactSection.vue'
import FooterSection from './components/FooterSection.vue'

// ------------------ TYPES ------------------
interface TerminalLine {
  prompt?: string
  text: string
  type?: string
}

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

// ------------------ REFS / STATE ------------------
const mainContainer = ref<HTMLElement | null>(null)
const footerSection = ref<HTMLElement | null>(null)

// Visibility states
const terminalVisible = ref(false)
const terminalMinimized = ref(false)
const scanLineVisible = ref(false)
const menuVisible = ref(false)
const systemInfoVisible = ref(false)
const heroVisible = ref(false)
const robotVisible = ref(false)
const heroTitleVisible = ref(false)
const footerVisible = ref(false)
const mouseX = ref(0)
const mouseY = ref(0)

// Scan line
const scanLinePosition = ref(0)
const scanLineStyle = computed(() => ({ top: `${scanLinePosition.value}px` }))

// Robot states
const isBlinking = ref(false)
const mouthState = ref<'idle' | 'talking'>('idle')

// System info
const systemStatus = ref('BOOTING')
const uptime = ref(0)

// Tech stack organizado
const techStack = ref([
  'JavaScript (ES6+)',
  'TypeScript',
  'Python',
  'Java',

  'SpringBoot',
  'Vue.js',
  'React.js',
  'Node.js',
  'Express',
  'Lit / Web Components',

  'HTML',
  'Pug',
  'CSS',
  'SCSS',

  'Docker',

  'SQL / NoSQL',

  'Unit & Integration Testing',
  'Jest',
  'Jasmine',
  'Mocha',
  'SonarQube',
  'SinonJs',
  'Cypress',
  'Cucumber',
])



// Terminal
const terminalLines = ref<TerminalLine[]>([
  { prompt: '>', text: 'Iniciando sistemas...', type: 'success' },
  { prompt: '>', text: 'Cargando módulo de presentación...', type: 'info' },
  { prompt: '>', text: 'Conectando con núcleo neural...', type: 'info' },
  { prompt: '>', text: 'Renderizando interfaz...', type: 'info' },
  { prompt: '>', text: 'SISTEMA LISTO ✓', type: 'success' }
])
const currentLine = ref<number>(0)

// ------------------ TIMERS / HANDLERS ------------------
let animationFrameId = 0
let uptimeInterval: number | undefined
let blinkInterval: number | undefined
let terminalInterval: number | undefined
let progressInterval: number | undefined

let resizeHandler = () => { }
let mouseMoveHandler = (e: MouseEvent) => {
  mouseX.value = e.clientX
  mouseY.value = e.clientY
}

// ------------------ SCROLL CONTROL ------------------
function lockScroll() {
  document.body.style.overflow = 'hidden'
}

function unlockScroll() {
  document.body.style.overflow = ''
}


// ------------------ BOOT SEQUENCE ------------------
async function startBootSequence() {
  // 1. Mostrar terminal
  await new Promise(resolve => setTimeout(resolve, 300))
  terminalVisible.value = true

  // 2. Terminal lines
  await new Promise(resolve => setTimeout(resolve, 800))

  let idx = 0
  // currentLine.value = -1 // empieza sin mostrar líneas

  terminalInterval = window.setInterval(() => {
    if (idx < terminalLines.value.length) {
      currentLine.value = idx
      idx++
    } else {
      clearInterval(terminalInterval)
    }
  }, 700)


  // 3. Esperar terminal
  await new Promise(resolve => setTimeout(resolve, 700 * terminalLines.value.length + 500))

  // 4. Minimizar terminal y activar sistema
  terminalMinimized.value = true
  systemStatus.value = 'ONLINE'
  await new Promise(resolve => setTimeout(resolve, 800))
  systemInfoVisible.value = true

  // 5. Scan line
  scanLineVisible.value = true
  await animateScanLine()

  // 6. Mostrar hero
  heroVisible.value = true
  robotVisible.value = true

  await new Promise(resolve => setTimeout(resolve, 700))

  heroTitleVisible.value = true
  // activate scroll
  unlockScroll()



  // 7. Robot
  await animateScanLine()
  robotVisible.value = true
  mouthState.value = 'talking'
  menuVisible.value = true

  // 8. Footer
  await animateScanLine()
  footerVisible.value = true
  terminalVisible.value = false

  // 9. Scroll al top
  // await new Promise(resolve => setTimeout(resolve, 2000))
  // window.scrollTo({ top: 0, behavior: 'smooth' })
}

// ------------------ SCROLL MENU ------------------
function scrollToSection(section: string) {
  const element = document.getElementById(section);
  if (!element) return;

  element.scrollIntoView({
    behavior: "smooth",
    block: "start"
  });
}


// ------------------ SCAN LINE ------------------
function animateScanLine(): Promise<void> {
  return new Promise((resolve) => {
    const duration = 2000
    const startPos = scanLinePosition.value
    const endPos = startPos + window.innerHeight * 0.6
    const startTime = Date.now()

    function animate() {
      const elapsed = Date.now() - startTime
      const progress = Math.min(elapsed / duration, 1)
      const eased = progress < 0.5
        ? 2 * progress * progress
        : 1 - Math.pow(-2 * progress + 2, 2) / 2
      scanLinePosition.value = startPos + (endPos - startPos) * eased
      if (progress < 1) requestAnimationFrame(animate)
      else resolve()
    }
    animate()
  })
}

// ------------------ LIFECYCLE ------------------
onMounted(() => {
  lockScroll()
  startBootSequence()

  uptimeInterval = window.setInterval(() => { uptime.value++ }, 1000)

  blinkInterval = window.setInterval(() => {
    isBlinking.value = true
    setTimeout(() => { isBlinking.value = false }, 150)
  }, 3500 + Math.random() * 2500)

  window.addEventListener('mousemove', mouseMoveHandler)
  window.addEventListener('resize', resizeHandler)
})


onUnmounted(() => {
  if (typeof uptimeInterval !== 'undefined') clearInterval(uptimeInterval)
  if (typeof blinkInterval !== 'undefined') clearInterval(blinkInterval)
  if (typeof terminalInterval !== 'undefined') clearInterval(terminalInterval)
  if (typeof progressInterval !== 'undefined') clearInterval(progressInterval)
  if (animationFrameId) cancelAnimationFrame(animationFrameId)
  window.removeEventListener('resize', resizeHandler)
  window.removeEventListener('mousemove', mouseMoveHandler)
})
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Rajdhani:wght@300;500;700;900&family=Orbitron:wght@400;700;900&display=swap");

$primary-color: #00ff88;
$bg-dark: #000205;

/* Base */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}


.content {
  position: relative;
  z-index: 9;
  width: 100%;
}

.page-content {
  position: relative;
  z-index: 5;
  padding: 5rem 2rem 2rem; // espacio arriba, laterales, abajo
  display: flex;           // o grid si quieres gap vertical
  flex-direction: column;
  align-items: center;     // centra todas las secciones horizontalmente
  gap: 6rem;               // espacio vertical entre secciones
}

</style>
