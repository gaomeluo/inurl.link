<script setup lang="ts">
definePageMeta({
  layout: 'landing',
})

// Progressive-enhancement flag: reveal animations only apply when JS is alive,
// so no-JS / crawler visitors always get fully visible content.
useHead({
  title: 'inurl.link · 互联网精选导航 — 发现 · 连接 · 探索',
  meta: [
    { name: 'keywords', content: 'inurl,互联网导航,在线工具,云测速,短链接,API Token,AI工具,开发者资源,效率工具,网址导航' },
    { name: 'description', content: 'inurl.link 是你的互联网入口，聚合云服务节点测速、在线工具箱、AI 资源导航、技术博客与短链接服务，打造高效的一站式互联网导航体验。' },
    { property: 'og:title', content: 'inurl.link · 互联网精选导航' },
    { property: 'og:description', content: '发现、连接、探索 — 聚合精选工具与服务，打造高效互联网导航体验。' },
    { property: 'og:type', content: 'website' },
    { property: 'og:url', content: 'https://inurl.link' },
    { property: 'og:image', content: 'https://inurl.link/banner.png' },
    { name: 'twitter:card', content: 'summary_large_image' },
  ],
  link: [
    { rel: 'canonical', href: 'https://inurl.link' },
  ],
  script: [
    {
      key: 'landing-enhance',
      tagPosition: 'head',
      innerHTML: 'document.documentElement.classList.add("fx-on")',
    },
  ],
})

const mobileMenuOpen = ref(false)
function toggleMenu() {
  mobileMenuOpen.value = !mobileMenuOpen.value
}

const scrolled = ref(false)

// Below-the-fold content is loaded lazily: the homepage paints the hero first,
// then fetches the heavy section chunk only once the visitor scrolls down.
const belowReady = ref(false)

onMounted(() => {
  const root = document.documentElement
  const fine = window.matchMedia('(hover: hover) and (pointer: fine)').matches
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  const disposers: Array<() => void> = []

  /* ── Header condensation on scroll ─────────────── */
  const onScroll = () => {
    scrolled.value = window.scrollY > 24
  }
  onScroll()
  window.addEventListener('scroll', onScroll, { passive: true })
  disposers.push(() => window.removeEventListener('scroll', onScroll))

  /* The first screen now animates in via pure CSS (`data-hero`), so no observer
     is needed here. Below-the-fold reveals are owned by <LandingBelow> itself. */

  /* ── Lazy-load below-the-fold on scroll ──────── */
  const triggerBelow = () => {
    if (window.scrollY > window.innerHeight * 0.35) {
      belowReady.value = true
      window.removeEventListener('scroll', triggerBelow)
    }
  }
  if (reduceMotion) {
    // Reduced motion: show everything immediately, skip the lazy gate.
    belowReady.value = true
  }
  else {
    window.addEventListener('scroll', triggerBelow, { passive: true })
    disposers.push(() => window.removeEventListener('scroll', triggerBelow))
    triggerBelow() // in case the page is reloaded already scrolled down
  }

  /* ── Cursor aura ─────────────────────────────── */
  if (fine && !reduceMotion) {
    const aura = document.createElement('div')
    aura.className = 'cursor-aura'
    const fxStage = document.querySelector('.fx-stage')
    ;(fxStage || document.body).appendChild(aura)
    const pos = { x: window.innerWidth / 2, y: window.innerHeight / 2, tx: 0, ty: 0 }
    pos.tx = pos.x
    pos.ty = pos.y
    let auraFrame = 0
    let auraIdle: ReturnType<typeof setTimeout> | null = null
    const auraMove = (e: PointerEvent) => {
      pos.tx = e.clientX
      pos.ty = e.clientY
      aura.style.opacity = '1'
      if (!auraFrame)
        auraFrame = requestAnimationFrame(auraLoop)
      if (auraIdle)
        clearTimeout(auraIdle)
      auraIdle = setTimeout(() => {
        cancelAnimationFrame(auraFrame)
        auraFrame = 0
      }, 1500)
    }
    const auraOut = () => {
      aura.style.opacity = '0'
    }
    const auraLoop = () => {
      pos.x += (pos.tx - pos.x) * 0.14
      pos.y += (pos.ty - pos.y) * 0.14
      aura.style.transform = `translate3d(${pos.x}px, ${pos.y}px, 0) translate(-50%, -50%)`
      auraFrame = requestAnimationFrame(auraLoop)
    }
    auraFrame = requestAnimationFrame(auraLoop)
    window.addEventListener('pointermove', auraMove, { passive: true })
    window.addEventListener('pointerleave', auraOut, { passive: true })
    disposers.push(() => {
      cancelAnimationFrame(auraFrame)
      if (auraIdle)
        clearTimeout(auraIdle)
      window.removeEventListener('pointermove', auraMove)
      window.removeEventListener('pointerleave', auraOut)
      aura.remove()
    })
  }

  onBeforeUnmount(() => {
    disposers.forEach(fn => fn())
    root.classList.remove('fx-on')
  })
})
</script>

<template>
  <div class="fx-stage" aria-hidden="true" />
  <!-- ═══════════ HEADER ═══════════ -->
  <header class="site-header" :class="{ condensed: scrolled }">
    <div class="header-inner">
      <a href="/" class="logo">
        <span class="logo-mark">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
            <defs><linearGradient id="logoGrad" x1="0" y1="0" x2="32" y2="32"><stop stop-color="#2EA7FF" /><stop offset="1" stop-color="#6366F1" /></linearGradient></defs>
            <circle cx="16" cy="16" r="14" stroke="url(#logoGrad)" stroke-width="2" />
            <circle cx="10" cy="16" r="3" fill="url(#logoGrad)" />
            <circle cx="22" cy="16" r="3" fill="url(#logoGrad)" />
            <path d="M13 16H19" stroke="url(#logoGrad)" stroke-width="2" stroke-linecap="round" />
            <path d="M12.5 12.5L9 16L12.5 19.5" stroke="url(#logoGrad)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none" />
            <path d="M19.5 12.5L23 16L19.5 19.5" stroke="url(#logoGrad)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none" />
          </svg>
          <i class="logo-halo" aria-hidden="true" />
        </span>
        <span class="logo-text">inurl.link</span>
      </a>
      <nav class="nav-links">
        <a href="/" class="active">首页</a>
        <a href="https://tools.inurl.link/" target="_blank">工具箱</a>
        <a href="https://ping.inurl.link/" target="_blank">云测速</a>
        <a href="https://blog.gaomeluo.com/" target="_blank">博客</a>
        <a href="https://www.lixiaoxin.com" target="_blank">关于</a>
      </nav>
      <a href="https://inurl.link/dashboard" class="btn-primary" target="_blank">短连接</a>
      <button class="hamburger" :class="{ active: mobileMenuOpen }" aria-label="菜单" @click="toggleMenu">
        <span /><span /><span />
      </button>
    </div>
    <nav class="mobile-menu" :class="{ active: mobileMenuOpen }">
      <a href="/" :class="{ active: true }" @click="toggleMenu">首页</a>
      <a href="https://tools.inurl.link/" target="_blank" @click="toggleMenu">工具箱</a>
      <a href="https://ping.inurl.link/" target="_blank" @click="toggleMenu">云测速</a>
      <a href="https://blog.gaomeluo.com/" target="_blank" @click="toggleMenu">博客</a>
      <a href="https://www.lixiaoxin.com" target="_blank" @click="toggleMenu">关于</a>
      <a href="https://inurl.link/dashboard" class="btn-primary" target="_blank" @click="toggleMenu">短连接</a>
    </nav>
  </header>

  <!-- ═══════════ HERO (first screen) ═══════════ -->
  <section class="hero">
    <div class="hero-stage" aria-hidden="true">
      <div class="grid-floor" />
      <div class="horizon-line" />
      <div class="core-glow" />
      <div class="orbit orbit-1"><i class="orbit-sat sat-cyan" /></div>
      <div class="orbit orbit-2"><i class="orbit-sat sat-purple" /></div>
      <div class="orbit orbit-3"><i class="orbit-sat sat-green" /></div>
      <div class="scan-sweep" />
      <div class="scan-lines" />
    </div>

    <div class="hud" aria-hidden="true">
      <span class="hud-corner tl" /><span class="hud-corner tr" />
      <span class="hud-corner bl" /><span class="hud-corner br" />
      <span class="hud-read tl">SYS::INURL.LINK</span>
      <span class="hud-read tr">128 NODES</span>
      <span class="hud-read bl">99.9% UPTIME</span>
      <span class="hud-read br">ONLINE</span>
      <span class="hud-ticks left" />
      <span class="hud-ticks right" />
    </div>

    <!-- First screen uses `data-hero` (pure CSS entrance) instead of `data-reveal`
         (JS-driven IntersectionObserver). The homepage is prerendered to static HTML,
         so the hero must paint without waiting for the JS bundle to download. -->
    <div class="badge" data-hero>
      <span class="badge-dot" />
      <span class="badge-text">inurl.link · 你的互联网入口</span>
    </div>
    <h1 class="hero-title" data-hero style="--d:90ms">
      <span class="glitch" data-text="发现 · 连接 · 探索">发现 · 连接 · 探索</span>
    </h1>
    <p class="hero-desc" data-hero style="--d:180ms">聚合精选工具与服务，打造高效互联网导航体验</p>
  </section>

  <!-- ═══════════ BELOW-THE-FOLD (lazy) ═══════════ -->
  <LazyLandingBelow v-if="belowReady" />
  <div v-else class="lazy-skeleton" aria-hidden="true">
    <span class="sk-ring" />
    <span class="sk-text">LOADING MORE · 下滑加载更多内容</span>
  </div>
</template>

<style scoped>
/* ════════════════════════════════════════════
   KEYFRAMES (first screen only)
   ════════════════════════════════════════════ */
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.4; transform: scale(0.85); }
}
@keyframes halo-spin {
  to { transform: rotate(360deg); }
}
@keyframes grid-run {
  to { background-position: 0 64px, 64px 0; }
}
@keyframes core-breathe {
  0%, 100% { opacity: 0.55; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.14); }
}
@keyframes sweep-run {
  0% { transform: translateY(-140%); opacity: 0; }
  12% { opacity: 1; }
  82% { opacity: 1; }
  100% { transform: translateY(300%); opacity: 0; }
}
@keyframes orbit-1-spin {
  from { transform: translate(-50%, -50%) rotateX(74deg) rotateZ(0deg); }
  to { transform: translate(-50%, -50%) rotateX(74deg) rotateZ(360deg); }
}
@keyframes orbit-2-spin {
  from { transform: translate(-50%, -50%) rotateX(66deg) rotateZ(360deg); }
  to { transform: translate(-50%, -50%) rotateX(66deg) rotateZ(0deg); }
}
@keyframes orbit-3-spin {
  from { transform: translate(-50%, -50%) rotateX(80deg) rotateY(12deg) rotateZ(0deg); }
  to { transform: translate(-50%, -50%) rotateX(80deg) rotateY(12deg) rotateZ(360deg); }
}
@keyframes glitch-a {
  0%, 92%, 100% { clip-path: inset(0 0 100% 0); transform: translate(0); opacity: 0; }
  93% { clip-path: inset(8% 0 62% 0); transform: translate(-3px, -1px); opacity: 0.85; }
  95% { clip-path: inset(48% 0 26% 0); transform: translate(3px, 1px); opacity: 0.8; }
  97% { clip-path: inset(72% 0 8% 0); transform: translate(-2px, 0); opacity: 0.7; }
  99% { clip-path: inset(24% 0 54% 0); transform: translate(2px, -1px); opacity: 0.6; }
}
@keyframes glitch-b {
  0%, 90%, 100% { clip-path: inset(0 0 100% 0); transform: translate(0); opacity: 0; }
  91% { clip-path: inset(62% 0 12% 0); transform: translate(3px, 1px); opacity: 0.7; }
  94% { clip-path: inset(16% 0 68% 0); transform: translate(-3px, -1px); opacity: 0.75; }
  96% { clip-path: inset(38% 0 40% 0); transform: translate(2px, 1px); opacity: 0.6; }
  98% { clip-path: inset(80% 0 4% 0); transform: translate(-2px, 0); opacity: 0.5; }
}
@keyframes tick-flow {
  0% { background-position: 0 0; }
  100% { background-position: 0 24px; }
}
@keyframes sk-spin {
  to { transform: rotate(360deg); }
}

/* ════════════════════════════════════════════
   SCROLL REVEAL (progressive enhancement)
   ════════════════════════════════════════════ */
:global(html.fx-on) [data-reveal] {
  opacity: 0;
  transform: translateY(26px);
  transition:
    opacity 0.75s cubic-bezier(0.16, 1, 0.3, 1) var(--d, 0ms),
    transform 0.75s cubic-bezier(0.16, 1, 0.3, 1) var(--d, 0ms);
  will-change: opacity, transform;
}
:global(html.fx-on) [data-reveal].is-visible {
  opacity: 1;
  transform: none;
}

/* ── First-screen entrance: CSS-only, zero JS dependency ──────
   Starts the moment the browser parses the markup, so the prerendered
   hero is fully visible long before the JS bundle finishes loading. */
@keyframes hero-in {
  from { opacity: 0; transform: translateY(26px); }
  to { opacity: 1; transform: none; }
}
[data-hero] {
  opacity: 0;
  animation: hero-in 0.75s cubic-bezier(0.16, 1, 0.3, 1) var(--d, 0ms) both;
  will-change: opacity, transform;
}

/* ════════════════════════════════════════════
   CURSOR AURA (injected into body)
   ════════════════════════════════════════════ */
.fx-stage {
  position: fixed;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
  z-index: 2;
}
:global(.cursor-aura) {
  position: absolute;
  top: 0;
  left: 0;
  width: 460px;
  height: 460px;
  border-radius: 50%;
  pointer-events: none;
  z-index: 2;
  opacity: 0;
  transition: opacity 0.4s ease;
  background: radial-gradient(circle, rgba(77, 168, 255, 0.09) 0%, rgba(139, 108, 255, 0.05) 38%, transparent 68%);
  mix-blend-mode: screen;
}

/* ════════════════════════════════════════════
   HEADER
   ════════════════════════════════════════════ */
.site-header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(4, 4, 12, 0.72);
  backdrop-filter: blur(14px) saturate(140%);
  -webkit-backdrop-filter: blur(14px) saturate(140%);
  border-bottom: 1px solid var(--border-subtle);
  transition: background 0.3s ease, border-color 0.3s ease;
}
.site-header.condensed {
  background: rgba(4, 4, 12, 0.92);
}
.header-inner {
  position: relative;
  max-width: 1440px;
  margin: 0 auto;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 clamp(16px, 4vw, 80px);
}
.header-inner::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent-cyan), var(--accent-purple), transparent);
  opacity: 0.45;
}

.logo {
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  position: relative;
  z-index: 1;
}
.logo-mark {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 30px;
  height: 30px;
}
.logo-halo {
  position: absolute;
  inset: -6px;
  border-radius: 50%;
  border: 1px dashed rgba(77, 168, 255, 0.35);
  animation: halo-spin 14s linear infinite;
  pointer-events: none;
}
.logo-text {
  font-size: clamp(17px, 2vw, 20px);
  font-weight: 700;
  color: var(--text-primary);
  font-family: var(--font-tech);
  letter-spacing: 0.5px;
  text-shadow: 0 0 18px rgba(77, 168, 255, 0.4);
}

.nav-links {
  display: flex;
  gap: clamp(16px, 3vw, 40px);
  align-items: center;
}
.nav-links a {
  font-size: 14px;
  font-weight: 500;
  color: var(--text-secondary);
  text-decoration: none;
  white-space: nowrap;
  transition: color 0.2s, text-shadow 0.2s;
  position: relative;
}
.nav-links a::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 2px;
  border-radius: 1px;
  background: linear-gradient(90deg, var(--accent-cyan), var(--accent-purple));
  box-shadow: 0 0 10px rgba(77, 168, 255, 0.8);
  transition: width 0.25s ease;
}
.nav-links a:hover,
.nav-links a.active {
  color: var(--text-primary);
  text-shadow: 0 0 14px rgba(77, 168, 255, 0.5);
}
.nav-links a:hover::after,
.nav-links a.active::after { width: 100%; }

.btn-primary {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
  padding: 10px 24px;
  border-radius: var(--radius-btn);
  background: linear-gradient(135deg, var(--accent-cyan), #3d8de0);
  color: #fff;
  font-size: 14px;
  font-weight: 600;
  text-decoration: none;
  overflow: hidden;
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.08) inset;
  transition: box-shadow 0.25s, transform 0.2s;
}
.btn-primary::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(115deg, transparent 34%, rgba(255, 255, 255, 0.4) 50%, transparent 66%);
  transform: translateX(-140%);
  transition: transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}
.btn-primary:hover {
  box-shadow: 0 0 30px rgba(77, 168, 255, 0.55), 0 0 62px rgba(77, 168, 255, 0.22);
  transform: translateY(-1px);
}
.btn-primary:hover::after { transform: translateX(140%); }

.hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
  z-index: 110;
}
.hamburger span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--text-primary);
  border-radius: 2px;
  transition: all 0.3s;
}
.hamburger.active span:nth-child(1) { transform: rotate(45deg) translate(5px, 5px); }
.hamburger.active span:nth-child(2) { opacity: 0; }
.hamburger.active span:nth-child(3) { transform: rotate(-45deg) translate(5px, -5px); }

.mobile-menu {
  display: none;
  position: fixed;
  top: 64px;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(4, 4, 12, 0.97);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  flex-direction: column;
  padding: 32px clamp(20px, 5vw, 40px);
  gap: 8px;
  z-index: 99;
}
.mobile-menu.active { display: flex; }
.mobile-menu a {
  display: block;
  padding: 14px 0;
  font-size: 18px;
  font-weight: 500;
  color: var(--text-secondary);
  text-decoration: none;
  border-bottom: 1px solid var(--border-subtle);
  transition: color 0.15s;
}
.mobile-menu a:hover,
.mobile-menu a.active { color: var(--text-primary); }
.mobile-menu .btn-primary {
  margin-top: 16px;
  text-align: center;
  justify-content: center;
}

/* ════════════════════════════════════════════
   HERO
   ════════════════════════════════════════════ */
.hero {
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: clamp(88px, 11vw, 190px) clamp(16px, 5vw, 80px) clamp(90px, 11vw, 175px);
  gap: clamp(20px, 3vw, 32px);
}
.hero > *:not(.hero-stage):not(.hud) {
  position: relative;
  z-index: 2;
}

.hero-stage {
  position: absolute;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  perspective: 900px;
  transform-style: preserve-3d;
  overflow: hidden;
}

.grid-floor {
  position: absolute;
  left: 50%;
  bottom: -6%;
  width: 100%;
  max-width: 1440px;
  height: 66%;
  transform: translateX(-50%) perspective(340px) rotateX(75deg);
  transform-origin: 50% 100%;
  background-image:
    linear-gradient(rgba(77, 168, 255, 0.3) 1px, transparent 1px),
    linear-gradient(90deg, rgba(139, 108, 255, 0.18) 1px, transparent 1px);
  background-size: 64px 64px;
  animation: grid-run 3.2s linear infinite;
  mask-image: linear-gradient(to top, #000 2%, rgba(0, 0, 0, 0.35) 34%, transparent 74%);
  -webkit-mask-image: linear-gradient(to top, #000 2%, rgba(0, 0, 0, 0.35) 34%, transparent 74%);
  opacity: 0.75;
}
.horizon-line {
  display: none;
}

.core-glow {
  position: absolute;
  top: 46%;
  left: 50%;
  width: clamp(220px, 30vw, 380px);
  height: clamp(220px, 30vw, 380px);
  transform: translate(-50%, -50%);
  border-radius: 50%;
  background:
    radial-gradient(circle, rgba(150, 205, 255, 0.2) 0%, rgba(139, 108, 255, 0.1) 42%, transparent 68%);
  filter: blur(4px);
  animation: core-breathe 6s ease-in-out infinite;
}

.orbit {
  position: absolute;
  top: 46%;
  left: 50%;
  border-radius: 50%;
  border: 1px solid rgba(120, 170, 255, 0.16);
  transform-style: preserve-3d;
}
.orbit-1 {
  width: clamp(300px, 44vw, 620px);
  height: clamp(300px, 44vw, 620px);
  border-color: rgba(77, 168, 255, 0.24);
  animation: orbit-1-spin 22s linear infinite;
}
.orbit-2 {
  width: clamp(220px, 32vw, 460px);
  height: clamp(220px, 32vw, 460px);
  border-color: rgba(139, 108, 255, 0.22);
  border-style: dashed;
  animation: orbit-2-spin 17s linear infinite;
}
.orbit-3 {
  width: clamp(280px, 42vw, 560px);
  height: clamp(280px, 42vw, 560px);
  border-color: rgba(0, 229, 184, 0.14);
  animation: orbit-3-spin 30s linear infinite;
}
.orbit-sat {
  position: absolute;
  top: -5px;
  left: 50%;
  width: 9px;
  height: 9px;
  margin-left: -4.5px;
  border-radius: 50%;
}
.sat-cyan {
  background: var(--accent-cyan);
  box-shadow: 0 0 12px var(--accent-cyan), 0 0 28px rgba(77, 168, 255, 0.7);
}
.sat-purple {
  background: var(--accent-purple);
  box-shadow: 0 0 12px var(--accent-purple), 0 0 28px rgba(139, 108, 255, 0.7);
}
.sat-green {
  background: var(--accent-green);
  box-shadow: 0 0 12px var(--accent-green), 0 0 28px rgba(0, 229, 184, 0.6);
}

.scan-sweep {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  height: 36%;
  background: linear-gradient(
    180deg,
    transparent 0%,
    rgba(77, 168, 255, 0.05) 42%,
    rgba(150, 205, 255, 0.11) 50%,
    rgba(77, 168, 255, 0.05) 58%,
    transparent 100%
  );
  animation: sweep-run 9s ease-in-out infinite;
}
.scan-lines {
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(77, 168, 255, 0.016) 2px,
    rgba(77, 168, 255, 0.016) 4px
  );
}

.hud {
  position: absolute;
  inset: clamp(18px, 3.4vw, 46px) clamp(14px, 3.4vw, 60px);
  z-index: 1;
  pointer-events: none;
}
.hud-corner {
  position: absolute;
  width: 26px;
  height: 26px;
  border: 1px solid rgba(77, 168, 255, 0.45);
}
.hud-corner.tl { top: 0; left: 0; border-right: 0; border-bottom: 0; }
.hud-corner.tr { top: 0; right: 0; border-left: 0; border-bottom: 0; }
.hud-corner.bl { bottom: 0; left: 0; border-right: 0; border-top: 0; }
.hud-corner.br { bottom: 0; right: 0; border-left: 0; border-top: 0; }
.hud-read {
  position: absolute;
  font-family: var(--font-mono);
  font-size: 10px;
  letter-spacing: 1.6px;
  color: rgba(140, 190, 255, 0.5);
  white-space: nowrap;
}
.hud-read.tl { top: 4px; left: 36px; }
.hud-read.tr { top: 4px; right: 36px; }
.hud-read.bl { bottom: 4px; left: 36px; }
.hud-read.br { bottom: 4px; right: 36px; }
.hud-ticks {
  position: absolute;
  top: 30%;
  width: 6px;
  height: 40%;
  background-image: repeating-linear-gradient(
    180deg,
    rgba(77, 168, 255, 0.4) 0 1px,
    transparent 1px 12px
  );
  animation: tick-flow 2.6s linear infinite;
}
.hud-ticks.left { left: 0; }
.hud-ticks.right { right: 0; }

.badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 6px 18px;
  border-radius: var(--radius-btn);
  background: linear-gradient(135deg, rgba(0, 229, 184, 0.2), rgba(0, 229, 184, 0.06));
  border: 1px solid rgba(0, 229, 184, 0.32);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  box-shadow: 0 0 26px rgba(0, 229, 184, 0.16);
}
.badge-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent-green);
  box-shadow: 0 0 8px var(--accent-green), 0 0 16px var(--accent-green);
  animation: pulse 2s ease-in-out infinite;
}
.badge-text {
  font-size: clamp(12px, 1.4vw, 13px);
  font-weight: 600;
  color: #fff;
  font-family: var(--font-cjk);
  white-space: nowrap;
}

.hero-title {
  font-size: clamp(32px, 6.2vw, 76px);
  font-weight: 800;
  letter-spacing: -0.03em;
  text-align: center;
  font-family: var(--font-tech);
  line-height: 1.12;
}
.glitch {
  position: relative;
  display: inline-block;
  background: linear-gradient(135deg, #ffffff 0%, #9fd4ff 34%, var(--accent-cyan) 62%, var(--accent-purple) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 0 26px rgba(77, 168, 255, 0.4));
}
.glitch::before,
.glitch::after {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  background: none;
  -webkit-text-fill-color: currentColor;
  pointer-events: none;
}
.glitch::before {
  color: #45e0ff;
  animation: glitch-a 7s infinite steps(1, end);
}
.glitch::after {
  color: #ff5e9c;
  animation: glitch-b 7s infinite steps(1, end);
}

.hero-desc {
  font-size: clamp(14px, 1.4vw, 18px);
  color: var(--text-secondary);
  max-width: 520px;
  text-align: center;
  font-family: var(--font-cjk);
  line-height: 1.7;
}

/* ════════════════════════════════════════════
   LAZY SKELETON (below-the-fold placeholder)
   ════════════════════════════════════════════ */
.lazy-skeleton {
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  min-height: 78vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 18px;
  padding: clamp(22px, 3vw, 42px) clamp(16px, 5vw, 80px);
}
.sk-ring {
  width: 46px;
  height: 46px;
  border-radius: 50%;
  border: 2px solid rgba(77, 168, 255, 0.18);
  border-top-color: var(--accent-cyan);
  animation: sk-spin 0.9s linear infinite;
  box-shadow: 0 0 22px rgba(77, 168, 255, 0.18);
}
.sk-text {
  font-family: var(--font-tech);
  font-size: 11px;
  letter-spacing: 2.5px;
  color: rgba(150, 190, 255, 0.45);
  text-transform: uppercase;
}

/* ════════════════════════════════════════════
   RESPONSIVE
   ════════════════════════════════════════════ */
@media (max-width: 1023px) {
  .nav-links,
  .header-inner > .btn-primary { display: none; }
  .hamburger { display: flex; }
}
@media (max-width: 600px) {
  .header-inner { height: 56px; }
  .mobile-menu { top: 56px; }
  .hero { padding-top: 62px; padding-bottom: 66px; }
  .orbit-3 { display: none; }
  .hud-read.bl,
  .hud-read.br { display: none; }
  .hud-ticks { display: none; }
}
@media (max-width: 380px) {
  .orbit-2 { display: none; }
  .hero-title { font-size: 28px; }
}

/* ════════════════════════════════════════════
   MOTION SAFETY
   ════════════════════════════════════════════ */
@media (prefers-reduced-motion: reduce) {
  .grid-floor,
  .scan-sweep,
  .orbit-1,
  .orbit-2,
  .orbit-3,
  .core-glow,
  .hud-ticks,
  .logo-halo,
  .glitch::before,
  .glitch::after { animation: none !important; }
  .glitch::before,
  .glitch::after { opacity: 0; }
  .sk-ring { animation: none; }
  :global(html.fx-on) [data-reveal] {
    opacity: 1;
    transform: none;
    transition: none;
  }
  /* Never leave the hero stuck at opacity 0 when animations are disabled. */
  [data-hero] {
    opacity: 1;
    transform: none;
    animation: none;
  }
}
</style>
