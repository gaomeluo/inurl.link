<script setup lang="ts">
useHead({
  bodyAttrs: { style: 'background:#03030c;' },
  link: [
    { rel: 'icon', type: 'image/svg+xml', href: '/favicon.svg' },
    { rel: 'alternate icon', href: '/favicon.ico' },
    { rel: 'apple-touch-icon', sizes: '180x180', href: '/apple-touch-icon.png' },
    { rel: 'preconnect', href: 'https://fonts.googleapis.cn' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.cn', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.cn/css2?family=Inter:wght@400;500;600;700;800;900&family=Orbitron:wght@500;700;900&display=swap' },
  ],
  style: [
    {
      key: 'landing-global',
      children: `
html { background:#03030c; scroll-behavior:smooth; overflow-x:hidden; }
body { background:#03030c; overflow-x:hidden; }
::selection { background:rgba(77,168,255,0.32); color:#fff; }
* { scrollbar-width:thin; scrollbar-color:rgba(77,168,255,0.35) transparent; }
::-webkit-scrollbar { width:10px; }
::-webkit-scrollbar:horizontal { width:0; height:0; }
::-webkit-scrollbar-track { background:rgba(255,255,255,0.02); }
::-webkit-scrollbar-thumb {
  background:linear-gradient(180deg,rgba(77,168,255,0.5),rgba(139,108,255,0.5));
  border-radius:10px; border:2px solid transparent; background-clip:padding-box;
}
::-webkit-scrollbar-thumb:hover { background:linear-gradient(180deg,rgba(77,168,255,0.8),rgba(139,108,255,0.8)); background-clip:padding-box; }
`,
    },
  ],
})

const deepField = ref<HTMLCanvasElement | null>(null)

onMounted(() => {
  const canvas = deepField.value
  if (!canvas)
    return
  const ctx = canvas.getContext('2d')
  if (!ctx)
    return

  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  const TAU = Math.PI * 2
  const TINTS = ['255,255,255', '150,195,255', '190,165,255', '130,255,225', '255,196,150']

  interface Star {
    x: number
    y: number
    radius: number
    alpha: number
    speed: number
    phase: number
    depth: number
    tint: string
  }
  interface Meteor {
    x: number
    y: number
    vx: number
    vy: number
    life: number
    maxLife: number
  }
  interface NetNode {
    x: number
    y: number
    vx: number
    vy: number
    r: number
  }

  let width = window.innerWidth
  let height = window.innerHeight
  let stars: Star[] = []
  let nodes: NetNode[] = []
  const meteors: Meteor[] = []

  const parallax = { x: 0, y: 0, tx: 0, ty: 0 }
  const pointer = { x: -9999, y: -9999, active: false }
  let scrollRatio = 0
  let frameId = 0
  let paused = false
  let nextMeteorAt = 1800

  function seed() {
    const target = Math.round((width * height) / 9000)
    const count = Math.max(90, Math.min(260, target))
    stars = Array.from({ length: count }, () => {
      const depth = Math.random()
      return {
        x: Math.random() * width,
        y: Math.random() * height,
        radius: 0.32 + depth * 1.3,
        alpha: 0.18 + Math.random() * 0.68,
        speed: 0.25 + Math.random() * 1.35,
        phase: Math.random() * TAU,
        depth,
        tint: TINTS[(Math.random() * TINTS.length) | 0] as string,
      }
    })

    const nodeCount = width < 760 ? 10 : width < 1280 ? 16 : 22
    const band = Math.min(height, 880)
    nodes = Array.from({ length: nodeCount }, () => ({
      x: Math.random() * width,
      y: Math.random() * band,
      vx: (Math.random() - 0.5) * 0.17,
      vy: (Math.random() - 0.5) * 0.17,
      r: 0.9 + Math.random() * 1.5,
    }))
  }

  function resize() {
    width = window.innerWidth
    height = window.innerHeight
    canvas.width = Math.floor(width * dpr)
    canvas.height = Math.floor(height * dpr)
    canvas.style.width = `${width}px`
    canvas.style.height = `${height}px`
    ctx!.setTransform(dpr, 0, 0, dpr, 0, 0)
    seed()
  }

  function spawnMeteor() {
    const fromLeft = Math.random() > 0.35
    const speed = 5.5 + Math.random() * 6
    meteors.push({
      x: fromLeft ? Math.random() * width * 0.55 : width * (0.45 + Math.random() * 0.55),
      y: -60 - Math.random() * 140,
      vx: (fromLeft ? 1 : -1) * speed * 0.6,
      vy: speed,
      life: 0,
      maxLife: 95 + Math.random() * 45,
    })
  }

  function paintStars(t: number) {
    for (let i = 0; i < stars.length; i++) {
      const s = stars[i]!
      const drift = reduceMotion ? 0 : t * (2 + s.depth * 5)
      const px = ((s.x + drift) % (width + 40)) - 20 + parallax.x * (5 + s.depth * 32)
      const py = s.y + parallax.y * (5 + s.depth * 32)
      const a = reduceMotion
        ? s.alpha
        : s.alpha * (0.42 + 0.58 * Math.sin(t * s.speed + s.phase))

      if (a <= 0.02)
        continue

      if (s.radius > 1.02) {
        ctx!.beginPath()
        ctx!.arc(px, py, s.radius * 3.4, 0, TAU)
        ctx!.fillStyle = `rgba(${s.tint},${a * 0.1})`
        ctx!.fill()
      }
      ctx!.beginPath()
      ctx!.arc(px, py, s.radius, 0, TAU)
      ctx!.fillStyle = `rgba(${s.tint},${a})`
      ctx!.fill()
    }
  }

  function paintNetwork() {
    const layerAlpha = Math.max(0, 1 - scrollRatio * 1.5)
    if (layerAlpha < 0.03)
      return

    const band = Math.min(height, 880)
    const LINK = 172
    const LINK2 = LINK * LINK

    for (let i = 0; i < nodes.length; i++) {
      const n = nodes[i]!
      if (!reduceMotion) {
        n.x += n.vx
        n.y += n.vy
      }
      if (n.x < -30)
        n.x = width + 30
      if (n.x > width + 30)
        n.x = -30
      if (n.y < -30)
        n.y = band + 30
      if (n.y > band + 30)
        n.y = -30
    }

    ctx!.lineWidth = 1
    for (let i = 0; i < nodes.length; i++) {
      const a = nodes[i]!
      for (let j = i + 1; j < nodes.length; j++) {
        const b = nodes[j]!
        const dx = a.x - b.x
        const dy = a.y - b.y
        const d2 = dx * dx + dy * dy
        if (d2 > LINK2)
          continue
        const strength = 1 - Math.sqrt(d2) / LINK
        ctx!.beginPath()
        ctx!.moveTo(a.x, a.y)
        ctx!.lineTo(b.x, b.y)
        ctx!.strokeStyle = `rgba(96,168,255,${strength * 0.2 * layerAlpha})`
        ctx!.stroke()
      }

      if (pointer.active) {
        const mdx = a.x - pointer.x
        const mdy = a.y - pointer.y
        const md2 = mdx * mdx + mdy * mdy
        if (md2 < 240 * 240) {
          const s = 1 - Math.sqrt(md2) / 240
          ctx!.beginPath()
          ctx!.moveTo(a.x, a.y)
          ctx!.lineTo(pointer.x, pointer.y)
          ctx!.strokeStyle = `rgba(0,229,184,${s * 0.4 * layerAlpha})`
          ctx!.stroke()
        }
      }

      ctx!.beginPath()
      ctx!.arc(a.x, a.y, a.r, 0, TAU)
      ctx!.fillStyle = `rgba(150,205,255,${0.55 * layerAlpha})`
      ctx!.fill()
    }
  }

  function paintMeteors() {
    for (let i = meteors.length - 1; i >= 0; i--) {
      const m = meteors[i]!
      m.x += m.vx
      m.y += m.vy
      m.life++

      const ratio = m.life / m.maxLife
      if (ratio >= 1 || m.y > height + 160) {
        meteors.splice(i, 1)
        continue
      }
      const fade = ratio < 0.18 ? ratio / 0.18 : 1 - (ratio - 0.18) / 0.82
      const mag = Math.hypot(m.vx, m.vy) || 1
      const tailX = m.x - (m.vx / mag) * 150
      const tailY = m.y - (m.vy / mag) * 150

      const grad = ctx!.createLinearGradient(m.x, m.y, tailX, tailY)
      grad.addColorStop(0, `rgba(190,225,255,${0.85 * fade})`)
      grad.addColorStop(0.4, `rgba(120,180,255,${0.28 * fade})`)
      grad.addColorStop(1, 'rgba(120,180,255,0)')

      ctx!.strokeStyle = grad
      ctx!.lineWidth = 1.6
      ctx!.lineCap = 'round'
      ctx!.beginPath()
      ctx!.moveTo(m.x, m.y)
      ctx!.lineTo(tailX, tailY)
      ctx!.stroke()

      ctx!.beginPath()
      ctx!.arc(m.x, m.y, 1.8, 0, TAU)
      ctx!.fillStyle = `rgba(230,244,255,${fade})`
      ctx!.fill()
    }
  }

  let lastFrame = 0
  const FPS = 30
  function render(time: number) {
    frameId = requestAnimationFrame(render)
    if (paused)
      return
    if (time - lastFrame < 1000 / FPS)
      return
    lastFrame = time

    const t = time * 0.001
    parallax.x += (parallax.tx - parallax.x) * 0.045
    parallax.y += (parallax.ty - parallax.y) * 0.045

    ctx!.clearRect(0, 0, width, height)
    paintStars(t)
    paintNetwork()

    if (!reduceMotion) {
      if (time > nextMeteorAt) {
        spawnMeteor()
        nextMeteorAt = time + 2800 + Math.random() * 5600
      }
      paintMeteors()
    }
  }

  function onPointerMove(e: PointerEvent) {
    parallax.tx = e.clientX / width - 0.5
    parallax.ty = e.clientY / height - 0.5
    pointer.x = e.clientX
    pointer.y = e.clientY
    pointer.active = true
  }
  function onPointerLeave() {
    pointer.active = false
    parallax.tx = 0
    parallax.ty = 0
  }
  function onScroll() {
    scrollRatio = window.scrollY / Math.max(1, window.innerHeight)
  }
  function onVisibility() {
    paused = document.hidden
  }

  let resizeTimer: ReturnType<typeof setTimeout> | null = null
  function onResize() {
    if (resizeTimer)
      clearTimeout(resizeTimer)
    resizeTimer = setTimeout(resize, 180)
  }

  resize()

  if (reduceMotion) {
    ctx.clearRect(0, 0, width, height)
    paintStars(0)
    paintNetwork()
  }
  else {
    const startCanvas = () => {
      lastFrame = performance.now()
      frameId = requestAnimationFrame(render)
    }
    if ('requestIdleCallback' in window)
      (window as any).requestIdleCallback(startCanvas, { timeout: 600 })
    else
      setTimeout(startCanvas, 250)
  }

  window.addEventListener('pointermove', onPointerMove, { passive: true })
  window.addEventListener('pointerleave', onPointerLeave, { passive: true })
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('resize', onResize)
  document.addEventListener('visibilitychange', onVisibility)

  onBeforeUnmount(() => {
    cancelAnimationFrame(frameId)
    if (resizeTimer)
      clearTimeout(resizeTimer)
    window.removeEventListener('pointermove', onPointerMove)
    window.removeEventListener('pointerleave', onPointerLeave)
    window.removeEventListener('scroll', onScroll)
    window.removeEventListener('resize', onResize)
    document.removeEventListener('visibilitychange', onVisibility)
  })
})
</script>

<template>
  <div class="home-page">
    <div class="bg-stage" aria-hidden="true">
      <canvas ref="deepField" class="deep-field" />
      <div class="nebula-layer" />
      <div class="grid-overlay" />
      <div class="noise-layer" />
      <div class="vignette" />
    </div>
    <div class="home-shell">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.home-page {
  --bg-deep: #03030c;
  --bg-card: rgba(255, 255, 255, 0.028);
  --text-primary: #eef1ff;
  --text-secondary: rgba(238, 241, 255, 0.62);
  --text-tertiary: rgba(238, 241, 255, 0.34);
  --border-subtle: rgba(150, 180, 255, 0.1);
  --border-glow: rgba(100, 140, 255, 0.28);
  --accent-cyan: #4da8ff;
  --accent-purple: #8b6cff;
  --accent-green: #00e5b8;
  --accent-amber: #ff9f43;
  --accent-pink: #ff5e9c;
  --radius-card: 16px;
  --radius-btn: 50px;
  --glow-cyan: 0 0 40px rgba(77, 168, 255, 0.3), 0 0 80px rgba(77, 168, 255, 0.1);
  --glow-purple: 0 0 40px rgba(139, 108, 255, 0.3), 0 0 80px rgba(139, 108, 255, 0.1);
  --glow-green: 0 0 30px rgba(0, 229, 184, 0.3), 0 0 60px rgba(0, 229, 184, 0.1);
  --font-cjk: 'PingFang SC', 'Noto Sans SC', 'Microsoft YaHei', sans-serif;
  --font-en: 'Inter', system-ui, sans-serif;
  --font-tech: 'Orbitron', 'Inter', system-ui, sans-serif;
  --font-mono: ui-monospace, 'SF Mono', 'JetBrains Mono', Menlo, Consolas, monospace;

  position: relative;
  color: var(--text-primary);
  font-family: var(--font-en);
  line-height: 1.6;
  min-height: 100vh;
  overflow-x: clip;
  isolation: isolate;
  background: var(--bg-deep);
}

/* ── Background stage (clips every decorative layer) ── */
.bg-stage {
  position: fixed;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
  z-index: 0;
}

/* ── Canvas deep field ───────────────────────────── */
.deep-field {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

/* ── Nebula clouds ───────────────────────────────── */
.nebula-layer {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  background:
    radial-gradient(ellipse 46% 34% at 50% 4%, rgba(77, 168, 255, 0.15) 0%, transparent 62%),
    radial-gradient(ellipse 38% 30% at 12% 26%, rgba(139, 108, 255, 0.13) 0%, transparent 58%),
    radial-gradient(ellipse 34% 26% at 88% 34%, rgba(0, 229, 184, 0.09) 0%, transparent 55%),
    radial-gradient(ellipse 42% 30% at 24% 74%, rgba(139, 108, 255, 0.08) 0%, transparent 60%),
    radial-gradient(ellipse 36% 28% at 82% 84%, rgba(255, 94, 156, 0.06) 0%, transparent 58%);
  filter: blur(14px);
  animation: nebula-drift 34s ease-in-out infinite alternate;
}

@keyframes nebula-drift {
  0% {
    transform: translate3d(0, 0, 0) scale(1);
    opacity: 0.86;
  }
  50% {
    transform: translate3d(-1.4%, 1.2%, 0) scale(1.05);
    opacity: 1;
  }
  100% {
    transform: translate3d(1.6%, -1%, 0) scale(1.02);
    opacity: 0.9;
  }
}

/* ── Fine tech grid ──────────────────────────────── */
.grid-overlay {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  opacity: 0.5;
  background-image:
    linear-gradient(rgba(120, 170, 255, 0.045) 1px, transparent 1px),
    linear-gradient(90deg, rgba(120, 170, 255, 0.045) 1px, transparent 1px);
  background-size: 68px 68px;
  mask-image: radial-gradient(ellipse 78% 62% at 50% 32%, #000 0%, transparent 78%);
  -webkit-mask-image: radial-gradient(ellipse 78% 62% at 50% 32%, #000 0%, transparent 78%);
}

/* ── Film grain ──────────────────────────────────── */
.noise-layer {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  opacity: 0.03;
  mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='180' height='180'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='180' height='180' filter='url(%23n)'/%3E%3C/svg%3E");
}

/* ── Vignette ────────────────────────────────────── */
.vignette {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  background: radial-gradient(ellipse 96% 76% at 50% 46%, transparent 42%, rgba(0, 0, 4, 0.5) 100%);
}

.home-shell {
  position: relative;
  z-index: 1;
}

@media (prefers-reduced-motion: reduce) {
  .nebula-layer {
    animation: none;
  }
}
</style>
