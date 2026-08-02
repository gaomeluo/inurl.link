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

  /* ── Scroll reveal ─────────────────────────────── */
  const revealTargets = Array.from(document.querySelectorAll<HTMLElement>('[data-reveal]'))
  if ('IntersectionObserver' in window && !reduceMotion) {
    const io = new IntersectionObserver((entries) => {
      for (const entry of entries) {
        if (!entry.isIntersecting)
          continue
        entry.target.classList.add('is-visible')
        io.unobserve(entry.target)
      }
    }, { threshold: 0.12, rootMargin: '0px 0px -8% 0px' })
    revealTargets.forEach(el => io.observe(el))
    disposers.push(() => io.disconnect())
  }
  else {
    revealTargets.forEach(el => el.classList.add('is-visible'))
  }

  /* ── Animated counters ─────────────────────────── */
  const counters = Array.from(document.querySelectorAll<HTMLElement>('[data-count]'))
  const runCounter = (el: HTMLElement) => {
    const raw = el.dataset.count ?? el.textContent ?? ''
    const parsed = /^([\d.]+)(.*)$/.exec(raw.trim())
    if (!parsed)
      return
    const target = Number.parseFloat(parsed[1] as string)
    const suffix = parsed[2] ?? ''
    const decimals = (parsed[1] as string).includes('.') ? 1 : 0
    const duration = 1500
    const start = performance.now()
    const step = (now: number) => {
      const p = Math.min(1, (now - start) / duration)
      const eased = 1 - (1 - p) ** 3
      el.textContent = (target * eased).toFixed(decimals) + suffix
      if (p < 1)
        requestAnimationFrame(step)
      else el.textContent = raw
    }
    requestAnimationFrame(step)
  }
  if ('IntersectionObserver' in window && !reduceMotion) {
    const co = new IntersectionObserver((entries) => {
      for (const entry of entries) {
        if (!entry.isIntersecting)
          continue
        runCounter(entry.target as HTMLElement)
        co.unobserve(entry.target)
      }
    }, { threshold: 0.5 })
    counters.forEach(el => co.observe(el))
    disposers.push(() => co.disconnect())
  }

  /* ── Magnetic 3D tilt + spotlight ──────────────── */
  if (fine && !reduceMotion) {
    const tiltCards = Array.from(document.querySelectorAll<HTMLElement>('[data-tilt]'))
    for (const card of tiltCards) {
      const enter = () => card.classList.add('tilting')
      const move = (e: PointerEvent) => {
        const rect = card.getBoundingClientRect()
        const px = (e.clientX - rect.left) / rect.width
        const py = (e.clientY - rect.top) / rect.height
        card.style.setProperty('--rx', `${(0.5 - py) * 7}deg`)
        card.style.setProperty('--ry', `${(px - 0.5) * 9}deg`)
        card.style.setProperty('--mx', `${px * 100}%`)
        card.style.setProperty('--my', `${py * 100}%`)
      }
      const leave = () => {
        card.classList.remove('tilting')
        card.style.setProperty('--rx', '0deg')
        card.style.setProperty('--ry', '0deg')
      }
      card.addEventListener('pointerenter', enter)
      card.addEventListener('pointermove', move)
      card.addEventListener('pointerleave', leave)
      disposers.push(() => {
        card.removeEventListener('pointerenter', enter)
        card.removeEventListener('pointermove', move)
        card.removeEventListener('pointerleave', leave)
      })
    }

    /* ── Cursor aura ─────────────────────────────── */
    const aura = document.createElement('div')
    aura.className = 'cursor-aura'
    document.body.appendChild(aura)
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

  <!-- ═══════════ HERO ═══════════ -->
  <section class="hero">
    <!-- Deep stage: horizon grid, orbits, energy core -->
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

    <!-- HUD frame -->
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

    <div class="badge" data-reveal>
      <span class="badge-dot" />
      <span class="badge-text">inurl.link · 你的互联网入口</span>
    </div>
    <h1 class="hero-title" data-reveal style="--d:90ms">
      <span class="glitch" data-text="发现 · 连接 · 探索">发现 · 连接 · 探索</span>
    </h1>
    <p class="hero-desc" data-reveal style="--d:180ms">聚合精选工具与服务，打造高效互联网导航体验</p>
  </section>

  <!-- ═══════════ CORE SERVICES ═══════════ -->
  <section class="section">
    <div class="section-header" data-reveal>
      <span class="section-tag cyan"><i class="tag-line" />CORE SERVICES<i class="tag-line" /></span>
      <h2 class="section-title">一站式精选服务</h2>
      <p class="section-sub">云端测速、在线工具、资源搜索，核心功能应有尽有</p>
    </div>
    <div class="service-grid">
      <div class="svc-card svc-card-cyan" data-tilt data-reveal>
        <i class="card-spot" aria-hidden="true" />
        <i class="c-corner tl" aria-hidden="true" /><i class="c-corner br" aria-hidden="true" />
        <div class="svc-icon-area svc-icon-area-cyan">
          <i class="icon-ring" aria-hidden="true" />
          <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#2EA7FF" fill-opacity="0.15" /><path d="M22 10C19.5147 10 17.5 12.0147 17.5 14.5V19.5C17.5 21.9853 19.5147 24 22 24C24.4853 24 26.5 24 26.5 24" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round" /><path d="M22 16L22 13" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round" /><path d="M25 18L28 18" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round" /><path d="M25 22L28 22" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round" /><path d="M22 28L22 30" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round" /><circle cx="32" cy="20" r="2" fill="#2EA7FF" /></svg>
        </div>
        <h3>云服务节点测速</h3>
        <p>实时检测阿里云、腾讯云等主流厂商各节点延迟与速度，助你选择最优线路</p>
        <div class="svc-bottom-links">
          <a href="https://ping.inurl.link/aliyun/" class="svc-sub-link" target="_blank">阿里云</a>
          <a href="https://ping.inurl.link/tengxun/" class="svc-sub-link" target="_blank">腾讯云</a>
          <a href="https://ping.inurl.link/" class="svc-link svc-link-cyan" target="_blank">立即使用 →</a>
        </div>
      </div>
      <a href="https://tools.inurl.link/" class="svc-card svc-card-purple" target="_blank" data-tilt data-reveal style="--d:80ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="c-corner tl" aria-hidden="true" /><i class="c-corner br" aria-hidden="true" />
        <div class="svc-icon-area svc-icon-area-purple">
          <i class="icon-ring" aria-hidden="true" />
          <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#9381FF" fill-opacity="0.15" /><path d="M12 15L18 21L12 27" stroke="#9381FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" /><path d="M32 10V20C32 26.6274 26.6274 32 20 32" stroke="#9381FF" stroke-width="2" stroke-linecap="round" /><circle cx="32" cy="12" r="1.5" fill="#9381FF" /></svg>
        </div>
        <h3>在线工具箱</h3>
        <p>常用开发 & 日常工具集合，即开即用，无需安装</p>
        <span class="svc-link svc-link-purple">立即使用 →</span>
      </a>
      <a href="https://inurl.top/" class="svc-card svc-card-green" target="_blank" data-tilt data-reveal style="--d:160ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="c-corner tl" aria-hidden="true" /><i class="c-corner br" aria-hidden="true" />
        <div class="svc-icon-area svc-icon-area-green">
          <i class="icon-ring" aria-hidden="true" />
          <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#13DDC4" fill-opacity="0.15" /><path d="M14 22H30M22 14V30" stroke="#13DDC4" stroke-width="2.5" stroke-linecap="round" /><circle cx="22" cy="22" r="10" stroke="#13DDC4" stroke-width="1.5" opacity="0.6" /><path d="M31 13L35 9M35 13L31 9" stroke="#13DDC4" stroke-width="1.5" stroke-linecap="round" /><path d="M9 31L13 27M13 31L9 27" stroke="#13DDC4" stroke-width="1.5" stroke-linecap="round" /></svg>
        </div>
        <h3>资源导航</h3>
        <p>聚合 AI 工具、API Token、开发资源与效率神器，一站式探索互联网优质服务</p>
        <span class="svc-link svc-link-green">立即探索 →</span>
      </a>
      <a href="https://blog.gaomeluo.com/" class="svc-card svc-card-white" target="_blank" data-tilt data-reveal style="--d:240ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="c-corner tl" aria-hidden="true" /><i class="c-corner br" aria-hidden="true" />
        <div class="svc-icon-area svc-icon-area-white">
          <i class="icon-ring" aria-hidden="true" />
          <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#2EA7FF" fill-opacity="0.08" /><path d="M14 12H26C27.1046 12 28 12.8954 28 14V30C28 31.1046 27.1046 32 26 32H14C12.8954 32 12 31.1046 12 30V14C12 12.8954 12.8954 12 14 12Z" stroke="#AABBCC" stroke-width="1.5" /><path d="M16 18H24" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round" /><path d="M16 22H24" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round" /><path d="M16 26H20" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round" /></svg>
        </div>
        <h3>技术博客</h3>
        <p>GaoMeluo Blog · inurl.top，技术文章与个人随想</p>
        <span class="svc-link svc-link-white">立即访问 →</span>
      </a>
    </div>
  </section>

  <!-- ═══════════ MORE RESOURCES ═══════════ -->
  <section class="section">
    <div class="section-header" data-reveal>
      <span class="section-tag purple"><i class="tag-line" />MORE RESOURCES<i class="tag-line" /></span>
      <h2 class="section-title">更多精选资源</h2>
      <p class="section-sub">按类别整理，涵盖开发、效率、娱乐、学习多场景工具</p>
    </div>
    <div class="feat-grid">
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal>
        <div class="feat-icon" style="background:#2EA7FF" />
        <div class="feat-text"><h4>JSON 格式化</h4><p>在线 JSON 解析、校验、压缩、转换</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal style="--d:60ms">
        <div class="feat-icon" style="background:#9381FF" />
        <div class="feat-text"><h4>Base64 编解码</h4><p>文本、图片、文件的 Base64 转换</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal style="--d:120ms">
        <div class="feat-icon" style="background:#13DDC4" />
        <div class="feat-text"><h4>UUID 生成器</h4><p>批量生成 UUID v1/v4/v5，校验有效性</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal style="--d:180ms">
        <div class="feat-icon" style="background:#FFB454" />
        <div class="feat-text"><h4>时间戳转换</h4><p>Unix 时间戳与日期互转，支持多时区</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal style="--d:240ms">
        <div class="feat-icon" style="background:#FF4D6D" />
        <div class="feat-text"><h4>正则表达式测试</h4><p>在线测试正则匹配，支持语法高亮</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank" data-reveal style="--d:300ms">
        <div class="feat-icon" style="background:linear-gradient(135deg,#2EA7FF,#13DDC4)" />
        <div class="feat-text"><h4>二维码生成</h4><p>生成或解析二维码，支持自定义颜色 logo</p></div>
        <span class="feat-arrow">→</span>
      </a>
    </div>
  </section>

  <!-- ═══════════ LIVE STATS ═══════════ -->
  <section class="section">
    <div class="section-header" data-reveal>
      <span class="section-tag green"><i class="tag-line" />LIVE STATS<i class="tag-line" /></span>
      <h2 class="section-title">平台实时数据</h2>
      <p class="section-sub">数据每 5 分钟自动更新，反映服务真实运行状态</p>
    </div>
    <div class="stat-grid">
      <div class="stat-card stat-cyan" data-tilt data-reveal>
        <i class="card-spot" aria-hidden="true" />
        <i class="stat-wave" aria-hidden="true" />
        <div class="stat-header-row">
          <span class="stat-label">服务节点数</span>
          <span class="live-dot live-dot-cyan" />
        </div>
        <span class="stat-value" data-count="128">128</span>
        <span class="stat-meta up">↑ 12 本周新增</span>
      </div>
      <div class="stat-card stat-purple" data-tilt data-reveal style="--d:80ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="stat-wave" aria-hidden="true" />
        <div class="stat-header-row">
          <span class="stat-label">在线工具数</span>
          <span class="live-dot live-dot-purple" />
        </div>
        <span class="stat-value" data-count="56">56</span>
        <span class="stat-meta up">↑ 3 本周新增</span>
      </div>
      <div class="stat-card stat-green" data-tilt data-reveal style="--d:160ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="stat-wave" aria-hidden="true" />
        <div class="stat-header-row">
          <span class="stat-label">日访问用户</span>
          <span class="live-dot live-dot-green" />
        </div>
        <span class="stat-value" data-count="8.2K">8.2K</span>
        <span class="stat-meta up">↑ 18% 较上周</span>
      </div>
      <div class="stat-card stat-amber" data-tilt data-reveal style="--d:240ms">
        <i class="card-spot" aria-hidden="true" />
        <i class="stat-wave" aria-hidden="true" />
        <div class="stat-header-row">
          <span class="stat-label">服务可用率</span>
          <span class="live-dot live-dot-amber" />
        </div>
        <span class="stat-value" data-count="99.9%">99.9%</span>
        <span class="stat-meta">SLA 持续保障</span>
      </div>
    </div>
  </section>

  <!-- ═══════════ LATEST POSTS ═══════════ -->
  <section class="section">
    <div class="section-header-row" data-reveal>
      <div class="header-left">
        <span class="section-tag cyan"><i class="tag-line" />LATEST POSTS</span>
        <h2 class="section-title section-title-left">最新文章</h2>
      </div>
      <a href="https://blog.gaomeluo.com/" class="btn-outline" target="_blank">查看全部 →</a>
    </div>
    <div class="article-grid">
      <a href="https://inurl.top/archives/ClashforOpenWRT/" class="art-card" target="_blank" data-tilt data-reveal>
        <i class="card-spot" aria-hidden="true" />
        <div class="art-img art-thumb-router">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a2e" />
            <circle cx="160" cy="90" r="80" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.15" />
            <circle cx="160" cy="90" r="50" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.2" />
            <rect x="120" y="65" width="80" height="50" rx="8" fill="#2EA7FF" opacity="0.3" />
            <rect x="130" y="72" width="60" height="8" rx="2" fill="#2EA7FF" opacity="0.6" />
            <rect x="130" y="84" width="40" height="6" rx="2" fill="#2EA7FF" opacity="0.4" />
            <rect x="130" y="94" width="50" height="6" rx="2" fill="#2EA7FF" opacity="0.4" />
            <path d="M200 70 Q220 50 240 60" stroke="#2EA7FF" stroke-width="2" fill="none" opacity="0.6" />
            <path d="M205 78 Q230 62 250 70" stroke="#9381FF" stroke-width="1.5" fill="none" opacity="0.5" />
            <path d="M210 86 Q235 74 255 80" stroke="#13DDC4" stroke-width="1.5" fill="none" opacity="0.4" />
            <circle cx="155" cy="100" r="3" fill="#13DDC4" opacity="0.9" /><circle cx="165" cy="100" r="3" fill="#13DDC4" opacity="0.5" />
            <text x="160" y="160" text-anchor="middle" fill="#2EA7FF" font-size="11" font-family="sans-serif" opacity="0.6">OpenWRT · Clash</text>
          </svg>
          <i class="art-scan" aria-hidden="true" />
        </div>
        <div class="art-content">
          <span class="art-tag cyan">软路由</span>
          <h4>Clash for OpenWRT 实现全屋设备科学上网</h4>
          <p>从机场选择、软路由推荐到 OpenWRT 刷机、OpenClash 配置，全流程手把手教程</p>
          <div class="art-meta"><span>2023-05-17</span><span>10 分钟</span></div>
        </div>
      </a>
      <a href="https://inurl.top/archives/datizi/" class="art-card" target="_blank" data-tilt data-reveal style="--d:80ms">
        <i class="card-spot" aria-hidden="true" />
        <div class="art-img art-thumb-vpn">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a24" />
            <circle cx="160" cy="90" r="70" fill="none" stroke="#9381FF" stroke-width="1" opacity="0.1" />
            <circle cx="160" cy="90" r="40" fill="none" stroke="#9381FF" stroke-width="1" opacity="0.15" stroke-dasharray="4 4" />
            <path d="M120 110 L135 80 L155 95 L175 65 L195 85 L200 70" stroke="#9381FF" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round" opacity="0.9" />
            <rect x="135" y="105" width="50" height="35" rx="6" fill="#13DDC4" opacity="0.2" stroke="#13DDC4" stroke-width="1.5" />
            <rect x="140" y="112" width="18" height="18" rx="4" fill="#13DDC4" opacity="0.5" />
            <rect x="162" y="112" width="18" height="18" rx="4" fill="#13DDC4" opacity="0.3" />
            <text x="160" y="162" text-anchor="middle" fill="#9381FF" font-size="11" font-family="sans-serif" opacity="0.6">V2ray · VPN</text>
          </svg>
          <i class="art-scan" aria-hidden="true" />
        </div>
        <div class="art-content">
          <span class="art-tag purple">教程</span>
          <h4>如何搭建梯子（VPN）？</h4>
          <p>自己搭建 V2ray 梯子，独享高速线路。从服务器选择到一键脚本部署，一步到位</p>
          <div class="art-meta"><span>2023-08-18</span><span>12 分钟</span></div>
        </div>
      </a>
      <a href="https://blog.gaomeluo.com/archives/wan-OpenClaw/" class="art-card" target="_blank" data-tilt data-reveal style="--d:160ms">
        <i class="card-spot" aria-hidden="true" />
        <div class="art-img art-thumb-claw">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a20" />
            <circle cx="160" cy="75" r="45" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.1" />
            <circle cx="160" cy="75" r="25" fill="#2EA7FF" opacity="0.08" />
            <path d="M160 50 Q175 30 195 35 Q200 40 195 50 Q185 55 170 55" fill="#2EA7FF" opacity="0.3" />
            <path d="M160 50 Q145 30 125 35 Q120 40 125 50 Q135 55 150 55" fill="#2EA7FF" opacity="0.3" />
            <rect x="145" y="55" width="30" height="35" rx="6" fill="#9381FF" opacity="0.25" />
            <circle cx="155" cy="68" r="4" fill="#2EA7FF" opacity="0.8" /><circle cx="165" cy="68" r="4" fill="#2EA7FF" opacity="0.8" />
            <rect x="148" y="76" width="24" height="4" rx="2" fill="#2EA7FF" opacity="0.5" />
            <path d="M155 100 Q140 125 120 130" stroke="#13DDC4" stroke-width="2" fill="none" opacity="0.5" />
            <path d="M165 100 Q180 125 200 130" stroke="#13DDC4" stroke-width="2" fill="none" opacity="0.5" />
            <circle cx="120" cy="130" r="3" fill="#13DDC4" opacity="0.6" /><circle cx="200" cy="130" r="3" fill="#13DDC4" opacity="0.6" />
            <text x="160" y="162" text-anchor="middle" fill="#9381FF" font-size="11" font-family="sans-serif" opacity="0.6">OpenClaw · AI Agent</text>
          </svg>
          <i class="art-scan" aria-hidden="true" />
        </div>
        <div class="art-content">
          <span class="art-tag green">OpenClaw</span>
          <h4>玩转 OpenClaw丨手把手教程合辑</h4>
          <p>从部署到接入微信/QQ/飞书，再到投资 Agent 实战——全场景教程汇总</p>
          <div class="art-meta"><span>2026-06-09</span><span>8 分钟</span></div>
        </div>
      </a>
      <a href="https://blog.gaomeluo.com/archives/mianfeiziti/" class="art-card" target="_blank" data-tilt data-reveal style="--d:240ms">
        <i class="card-spot" aria-hidden="true" />
        <div class="art-img art-thumb-font">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a1e" />
            <circle cx="160" cy="80" r="60" fill="none" stroke="#FFB454" stroke-width="1" opacity="0.1" />
            <circle cx="160" cy="80" r="35" fill="none" stroke="#FFB454" stroke-width="1" opacity="0.15" stroke-dasharray="3 3" />
            <text x="120" y="65" fill="#FFB454" font-size="36" font-weight="700" font-family="serif" opacity="0.8">A</text>
            <text x="150" y="90" fill="#FF4D6D" font-size="28" font-weight="600" font-family="serif" opacity="0.7" transform="rotate(-8,150,90)">a</text>
            <text x="180" y="70" fill="#2EA7FF" font-size="22" font-weight="500" font-family="sans-serif" opacity="0.6" transform="rotate(5,180,70)">字</text>
            <text x="210" y="85" fill="#13DDC4" font-size="18" font-weight="400" font-family="sans-serif" opacity="0.5" transform="rotate(12,210,85)">体</text>
            <text x="130" y="110" fill="#FFFFFF" font-size="14" font-weight="600" font-family="sans-serif" opacity="0.3">Free · 商用 · 可下载</text>
            <text x="160" y="162" text-anchor="middle" fill="#FFB454" font-size="11" font-family="sans-serif" opacity="0.6">Typography Resources</text>
          </svg>
          <i class="art-scan" aria-hidden="true" />
        </div>
        <div class="art-content">
          <span class="art-tag amber">资源</span>
          <h4>几百款免费商用字体下载</h4>
          <p>搜罗了全网可商用的中英文字体，几百款打包下载，设计师和开发者必备</p>
          <div class="art-meta"><span>2025-11-22</span><span>5 分钟</span></div>
        </div>
      </a>
    </div>
  </section>

  <!-- ═══════════ CTA ═══════════ -->
  <section class="section">
    <div class="cta-box" data-reveal>
      <div class="cta-glow" />
      <i class="cta-grid" aria-hidden="true" />
      <i class="c-corner tl" aria-hidden="true" /><i class="c-corner tr" aria-hidden="true" />
      <i class="c-corner bl" aria-hidden="true" /><i class="c-corner br" aria-hidden="true" />
      <h2>推荐你常用的工具与服务</h2>
      <p>帮助更多人发现优质资源，一起构建更好的互联网导航生态</p>
      <div class="cta-buttons">
        <a href="https://inurl.link/dashboard" class="btn-cta-primary" target="_blank">短连接 →</a>
        <a href="https://www.lixiaoxin.com" class="btn-cta-secondary" target="_blank">联系作者</a>
      </div>
    </div>
  </section>

  <!-- ═══════════ FOOTER ═══════════ -->
  <footer class="site-footer">
    <div class="footer-top">
      <div class="footer-brand">
        <div class="logo" style="margin-bottom:4px">
          <span class="logo-mark">
            <svg width="30" height="30" viewBox="0 0 32 32" fill="none">
              <defs><linearGradient id="lgGradF" x1="0" y1="0" x2="32" y2="32"><stop stop-color="#2EA7FF" /><stop offset="1" stop-color="#6366F1" /></linearGradient></defs>
              <circle cx="16" cy="16" r="14" stroke="url(#lgGradF)" stroke-width="2" />
              <circle cx="10" cy="16" r="3" fill="url(#lgGradF)" />
              <circle cx="22" cy="16" r="3" fill="url(#lgGradF)" />
              <path d="M13 16H19" stroke="url(#lgGradF)" stroke-width="2" stroke-linecap="round" />
              <path d="M12.5 12.5L9 16L12.5 19.5" stroke="url(#lgGradF)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none" />
              <path d="M19.5 12.5L23 16L19.5 19.5" stroke="url(#lgGradF)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" fill="none" />
            </svg>
          </span>
          <span class="logo-text">inurl.link</span>
        </div>
        <p>互联网精选导航 · 让每一次访问都更有方向</p>
      </div>
      <div class="footer-col">
        <h4>服务</h4>
        <a href="https://ping.inurl.link/" target="_blank">云服务测速</a>
        <a href="https://tools.inurl.link/" target="_blank">在线工具箱</a>
        <a href="https://blog.gaomeluo.com/" target="_blank">技术博客</a>
      </div>
      <div class="footer-col">
        <h4>资源</h4>
        <a href="https://tools.inurl.link/" target="_blank">开发工具</a>
        <a href="https://tools.inurl.link/" target="_blank">效率工具</a>
        <a href="https://blog.gaomeluo.com/" target="_blank">学习资源</a>
        <a href="#" target="_blank">娱乐资源</a>
      </div>
      <div class="footer-col">
        <h4>关于</h4>
        <a href="https://www.lixiaoxin.com" target="_blank">关于作者</a>
        <a href="https://inurl.link/dashboard" target="_blank">短连接</a>
        <a href="#" target="_blank">友情链接</a>
        <a href="#" target="_blank">隐私协议</a>
      </div>
    </div>
    <div class="footer-bottom">
      <div class="copyright">
        <span class="copy-main">© 2024–2026 inurl.link · 互联网精选导航</span>
        <span class="copy-sub">探索 · 连接 · 发现 — 你的互联网精选入口</span>
      </div>
      <div class="footer-status">
        <span class="status-dot" />
        <span class="status-text">SYSTEM ONLINE</span>
      </div>
    </div>
  </footer>
</template>

<style scoped>
/* ════════════════════════════════════════════
   CUSTOM PROPERTIES (animatable angle)
   ════════════════════════════════════════════ */
@property --bd-angle {
  syntax: '<angle>';
  inherits: false;
  initial-value: 0deg;
}

/* ════════════════════════════════════════════
   KEYFRAMES
   ════════════════════════════════════════════ */
@keyframes pulse {
  0%, 100% { opacity: 1; transform: scale(1); }
  50% { opacity: 0.4; transform: scale(0.85); }
}

@keyframes halo-spin {
  to { transform: rotate(360deg); }
}

@keyframes bd-spin {
  to { --bd-angle: 360deg; }
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

@keyframes wave-slide {
  0% { background-position: 0 0; }
  100% { background-position: 64px 0; }
}

@keyframes art-scan-run {
  0% { transform: translateY(-100%); }
  100% { transform: translateY(400%); }
}

@keyframes rule-flow {
  0%, 100% { opacity: 0.3; transform: scaleX(0.6); }
  50% { opacity: 1; transform: scaleX(1); }
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

/* ════════════════════════════════════════════
   CURSOR AURA (injected into body)
   ════════════════════════════════════════════ */
:global(.cursor-aura) {
  position: fixed;
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
  border-bottom: 1px solid var(--border-subtle);
  transition: border-color 0.3s ease;
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
  background: rgba(4, 4, 12, 0.72);
  backdrop-filter: blur(14px) saturate(140%);
  -webkit-backdrop-filter: blur(14px) saturate(140%);
  transition: background 0.3s ease;
}
.site-header.condensed .header-inner {
  background: rgba(4, 4, 12, 0.92);
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
}

/* Horizon grid floor */
.grid-floor {
  position: absolute;
  left: 50%;
  bottom: -6%;
  width: 300%;
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
  position: absolute;
  left: 8%;
  right: 8%;
  bottom: 33%;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(77, 168, 255, 0.65), rgba(139, 108, 255, 0.5), transparent);
  box-shadow: 0 0 26px rgba(77, 168, 255, 0.5);
}

/* Energy core */
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

/* Orbits */
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
  width: clamp(380px, 56vw, 800px);
  height: clamp(380px, 56vw, 800px);
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

/* Sweep + scanlines */
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

/* HUD frame */
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

/* Hero content */
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
.hero-rule {
  display: flex;
  align-items: center;
  gap: 10px;
}
.hero-rule i {
  display: block;
  width: clamp(40px, 8vw, 90px);
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(77, 168, 255, 0.7));
}
.hero-rule i:last-child {
  background: linear-gradient(90deg, rgba(139, 108, 255, 0.7), transparent);
}
.hero-rule span {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--accent-cyan);
  box-shadow: 0 0 12px var(--accent-cyan);
  animation: rule-flow 3s ease-in-out infinite;
}

/* ════════════════════════════════════════════
   SECTION SHELL
   ════════════════════════════════════════════ */
.section {
  position: relative;
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: clamp(44px, 6vw, 84px) clamp(16px, 5vw, 80px);
  gap: clamp(32px, 4vw, 48px);
  content-visibility: auto;
  contain-intrinsic-size: auto 720px;
}
.section-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}
.section-tag {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 3px;
  text-transform: uppercase;
  font-family: var(--font-tech);
}
.tag-line {
  display: block;
  width: clamp(20px, 4vw, 44px);
  height: 1px;
  background: currentColor;
  opacity: 0.4;
}
.section-tag.cyan { color: var(--accent-cyan); text-shadow: 0 0 12px rgba(77, 168, 255, 0.5); }
.section-tag.purple { color: var(--accent-purple); text-shadow: 0 0 12px rgba(139, 108, 255, 0.5); }
.section-tag.green { color: var(--accent-green); text-shadow: 0 0 12px rgba(0, 229, 184, 0.5); }
.section-title {
  font-size: clamp(26px, 3.6vw, 42px);
  font-weight: 700;
  letter-spacing: -0.5px;
  text-align: center;
  font-family: var(--font-cjk);
  background: linear-gradient(180deg, #ffffff 12%, rgba(200, 220, 255, 0.72) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}
.section-sub {
  font-size: clamp(14px, 1.4vw, 16px);
  color: var(--text-secondary);
  text-align: center;
  max-width: 600px;
  font-family: var(--font-cjk);
  padding: 0 16px;
}
.section-header-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  width: 100%;
  max-width: 1280px;
  flex-wrap: wrap;
  gap: 16px;
}
.header-left {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.section-title-left { text-align: left; }

/* Shared card chrome */
.card-spot {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.35s ease;
  background: radial-gradient(340px circle at var(--mx, 50%) var(--my, 50%), rgba(160, 210, 255, 0.1), transparent 62%);
  z-index: 3;
}
.c-corner {
  position: absolute;
  width: 14px;
  height: 14px;
  border: 1px solid rgba(150, 200, 255, 0.35);
  opacity: 0;
  transition: opacity 0.35s ease;
  pointer-events: none;
  z-index: 3;
}
.c-corner.tl { top: 9px; left: 9px; border-right: 0; border-bottom: 0; }
.c-corner.tr { top: 9px; right: 9px; border-left: 0; border-bottom: 0; }
.c-corner.bl { bottom: 9px; left: 9px; border-right: 0; border-top: 0; }
.c-corner.br { bottom: 9px; right: 9px; border-left: 0; border-top: 0; }

/* ════════════════════════════════════════════
   SERVICE CARDS
   ════════════════════════════════════════════ */
.service-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  width: 100%;
  max-width: 1280px;
}
.svc-card {
  --card-accent: rgba(255, 255, 255, 0.5);
  position: relative;
  overflow: hidden;
  background: var(--bg-card);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-card);
  padding: clamp(20px, 3vw, 28px);
  display: flex;
  flex-direction: column;
  gap: clamp(10px, 2vw, 16px);
  text-decoration: none;
  color: inherit;
  min-height: 280px;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  transform: perspective(1000px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg));
  transform-style: preserve-3d;
  transition:
    transform 0.5s cubic-bezier(0.16, 1, 0.3, 1),
    box-shadow 0.4s ease,
    border-color 0.4s ease;
}
.svc-card.tilting { transition: transform 0.12s linear, box-shadow 0.4s ease, border-color 0.4s ease; }
.svc-card::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1px;
  background: conic-gradient(
    from var(--bd-angle),
    transparent 0deg,
    transparent 232deg,
    var(--card-accent) 306deg,
    transparent 360deg
  );
  mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  mask-composite: exclude;
  -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  opacity: 0;
  transition: opacity 0.45s ease;
  pointer-events: none;
  z-index: 2;
}
.svc-card::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  background: linear-gradient(118deg, transparent 32%, rgba(255, 255, 255, 0.06) 48%, transparent 64%);
  transform: translateX(-130%);
  transition: transform 0.9s cubic-bezier(0.16, 1, 0.3, 1);
  pointer-events: none;
  z-index: 1;
}
.svc-card:hover { transform: perspective(1000px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg)) translateY(-7px); }
.svc-card:hover::before { opacity: 1; animation: bd-spin 3.4s linear infinite; }
.svc-card:hover::after { transform: translateX(130%); }
.svc-card:hover .card-spot { opacity: 1; }
.svc-card:hover .c-corner { opacity: 1; }
.svc-card > *:not(.card-spot):not(.c-corner) { position: relative; z-index: 2; }

.svc-card-cyan {
  --card-accent: var(--accent-cyan);
  background: linear-gradient(150deg, rgba(77, 168, 255, 0.07), var(--bg-card) 62%);
}
.svc-card-cyan:hover {
  border-color: rgba(77, 168, 255, 0.42);
  box-shadow: 0 0 34px rgba(77, 168, 255, 0.2), 0 20px 46px rgba(0, 0, 0, 0.42);
}
.svc-card-purple {
  --card-accent: var(--accent-purple);
  background: linear-gradient(150deg, rgba(139, 108, 255, 0.07), var(--bg-card) 62%);
}
.svc-card-purple:hover {
  border-color: rgba(139, 108, 255, 0.42);
  box-shadow: 0 0 34px rgba(139, 108, 255, 0.2), 0 20px 46px rgba(0, 0, 0, 0.42);
}
.svc-card-green {
  --card-accent: var(--accent-green);
  background: linear-gradient(150deg, rgba(0, 229, 184, 0.07), var(--bg-card) 62%);
}
.svc-card-green:hover {
  border-color: rgba(0, 229, 184, 0.42);
  box-shadow: 0 0 34px rgba(0, 229, 184, 0.2), 0 20px 46px rgba(0, 0, 0, 0.42);
}
.svc-card-white {
  --card-accent: rgba(210, 228, 255, 0.75);
}
.svc-card-white:hover {
  border-color: rgba(255, 255, 255, 0.24);
  box-shadow: 0 0 30px rgba(255, 255, 255, 0.09), 0 20px 46px rgba(0, 0, 0, 0.42);
}

.svc-icon-area {
  position: relative;
  width: 52px;
  height: 52px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
.icon-ring {
  position: absolute;
  inset: -7px;
  border-radius: 50%;
  border: 1px dashed rgba(255, 255, 255, 0.14);
  opacity: 0;
  transition: opacity 0.4s ease;
}
.svc-card:hover .icon-ring {
  opacity: 1;
  animation: halo-spin 9s linear infinite;
}
.svc-icon-area-cyan { background: rgba(77, 168, 255, 0.12); box-shadow: 0 0 24px rgba(77, 168, 255, 0.14) inset; }
.svc-icon-area-purple { background: rgba(139, 108, 255, 0.12); box-shadow: 0 0 24px rgba(139, 108, 255, 0.14) inset; }
.svc-icon-area-green { background: rgba(0, 229, 184, 0.12); box-shadow: 0 0 24px rgba(0, 229, 184, 0.14) inset; }
.svc-icon-area-white { background: rgba(255, 255, 255, 0.06); }
.svc-icon {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  flex-shrink: 0;
}
.svc-card h3 {
  font-size: clamp(16px, 1.6vw, 20px);
  font-weight: 700;
  font-family: var(--font-cjk);
}
.svc-card p {
  font-size: clamp(12px, 1.1vw, 14px);
  color: var(--text-secondary);
  flex: 1;
  font-family: var(--font-cjk);
  line-height: 1.65;
}
.svc-card > :last-child { margin-top: auto; }

.svc-sub-link {
  padding: 5px 12px;
  border-radius: 8px;
  font-size: 12px;
  font-weight: 500;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--border-subtle);
  color: var(--accent-cyan);
  text-decoration: none;
  font-family: var(--font-cjk);
  transition: background 0.2s, border-color 0.2s, box-shadow 0.2s;
}
.svc-sub-link:hover {
  background: rgba(77, 168, 255, 0.16);
  border-color: rgba(77, 168, 255, 0.4);
  box-shadow: 0 0 14px rgba(77, 168, 255, 0.24);
}
.svc-bottom-links {
  display: flex;
  gap: 8px;
  align-items: center;
  flex-wrap: wrap;
}
.svc-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 8px 16px;
  border-radius: var(--radius-btn);
  font-size: 13px;
  font-weight: 600;
  text-decoration: none;
  font-family: var(--font-cjk);
  transition: box-shadow 0.2s, transform 0.2s, background 0.2s;
}
.svc-link:hover { transform: translateX(3px); }
.svc-link-cyan { background: rgba(77, 168, 255, 0.15); color: var(--accent-cyan); }
.svc-link-cyan:hover { box-shadow: 0 0 18px rgba(77, 168, 255, 0.4); }
.svc-link-purple { background: rgba(139, 108, 255, 0.15); color: var(--accent-purple); }
.svc-link-purple:hover { box-shadow: 0 0 18px rgba(139, 108, 255, 0.4); }
.svc-link-green { background: rgba(0, 229, 184, 0.15); color: var(--accent-green); }
.svc-link-green:hover { box-shadow: 0 0 18px rgba(0, 229, 184, 0.4); }
.svc-link-white { background: rgba(255, 255, 255, 0.08); color: var(--text-primary); }
.svc-link-white:hover { box-shadow: 0 0 14px rgba(255, 255, 255, 0.18); }

/* ════════════════════════════════════════════
   FEATURE GRID
   ════════════════════════════════════════════ */
.feat-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
  width: 100%;
  max-width: 1280px;
}
.feat-item {
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  gap: clamp(12px, 2vw, 16px);
  padding: clamp(14px, 2vw, 20px);
  background: rgba(255, 255, 255, 0.025);
  border: 1px solid var(--border-subtle);
  border-radius: 14px;
  text-decoration: none;
  color: inherit;
  transition: background 0.25s, transform 0.25s, border-color 0.25s, box-shadow 0.25s;
}
.feat-item::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 2px;
  background: linear-gradient(180deg, transparent, var(--accent-cyan), transparent);
  transform: scaleY(0);
  transition: transform 0.35s cubic-bezier(0.16, 1, 0.3, 1);
}
.feat-item:hover {
  background: rgba(255, 255, 255, 0.055);
  transform: translateY(-3px);
  border-color: rgba(150, 200, 255, 0.2);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.32);
}
.feat-item:hover::before { transform: scaleY(1); }
.feat-icon {
  width: clamp(36px, 5vw, 44px);
  height: clamp(36px, 5vw, 44px);
  border-radius: 10px;
  flex-shrink: 0;
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.08) inset;
  transition: box-shadow 0.3s, transform 0.3s;
}
.feat-item:hover .feat-icon {
  transform: scale(1.06) rotate(-3deg);
  box-shadow: 0 0 22px rgba(120, 180, 255, 0.45);
}
.feat-text {
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex: 1;
  min-width: 0;
}
.feat-text h4 {
  font-size: clamp(13px, 1.3vw, 15px);
  font-weight: 600;
  font-family: var(--font-cjk);
  transition: color 0.2s;
}
.feat-item:hover .feat-text h4 { color: #bcdcff; }
.feat-text p {
  font-size: clamp(11px, 1vw, 12px);
  color: var(--text-tertiary);
  font-family: var(--font-cjk);
}
.feat-arrow {
  color: rgba(255, 255, 255, 0.3);
  font-size: 16px;
  font-weight: 700;
  flex-shrink: 0;
  transition: color 0.2s, transform 0.25s;
}
.feat-item:hover .feat-arrow {
  color: var(--accent-cyan);
  transform: translateX(4px);
}

/* ════════════════════════════════════════════
   STAT CARDS
   ════════════════════════════════════════════ */
.stat-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 100%;
  max-width: 1280px;
}
.stat-card {
  position: relative;
  overflow: hidden;
  padding: clamp(18px, 2.5vw, 24px);
  background: var(--bg-card);
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-card);
  display: flex;
  flex-direction: column;
  gap: clamp(8px, 1.5vw, 12px);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  transform: perspective(900px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg));
  transition:
    transform 0.5s cubic-bezier(0.16, 1, 0.3, 1),
    border-color 0.35s ease,
    box-shadow 0.35s ease;
}
.stat-card.tilting { transition: transform 0.12s linear, border-color 0.35s ease, box-shadow 0.35s ease; }
.stat-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  opacity: 0;
  transition: opacity 0.3s;
}
.stat-card:hover { border-color: rgba(160, 200, 255, 0.24); }
.stat-card:hover .card-spot { opacity: 1; }
.stat-card > *:not(.card-spot):not(.stat-wave) { position: relative; z-index: 2; }

.stat-wave {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  height: 34px;
  opacity: 0.4;
  pointer-events: none;
  background-image: repeating-linear-gradient(
    90deg,
    currentColor 0 1px,
    transparent 1px 8px
  );
  mask-image: linear-gradient(to top, #000, transparent);
  -webkit-mask-image: linear-gradient(to top, #000, transparent);
  animation: wave-slide 6s linear infinite;
}
.stat-cyan { color: rgba(77, 168, 255, 0.4); }
.stat-purple { color: rgba(139, 108, 255, 0.4); }
.stat-green { color: rgba(0, 229, 184, 0.4); }
.stat-amber { color: rgba(255, 159, 67, 0.4); }

.stat-cyan::after { background: linear-gradient(90deg, transparent, var(--accent-cyan), transparent); }
.stat-cyan:hover::after { opacity: 1; }
.stat-cyan:hover { box-shadow: 0 0 26px rgba(77, 168, 255, 0.16); }
.stat-purple::after { background: linear-gradient(90deg, transparent, var(--accent-purple), transparent); }
.stat-purple:hover::after { opacity: 1; }
.stat-purple:hover { box-shadow: 0 0 26px rgba(139, 108, 255, 0.16); }
.stat-green::after { background: linear-gradient(90deg, transparent, var(--accent-green), transparent); }
.stat-green:hover::after { opacity: 1; }
.stat-green:hover { box-shadow: 0 0 26px rgba(0, 229, 184, 0.16); }
.stat-amber::after { background: linear-gradient(90deg, transparent, var(--accent-amber), transparent); }
.stat-amber:hover::after { opacity: 1; }
.stat-amber:hover { box-shadow: 0 0 26px rgba(255, 159, 67, 0.16); }

.stat-header-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.stat-label {
  font-size: clamp(11px, 1.1vw, 13px);
  font-weight: 500;
  color: var(--text-secondary);
  font-family: var(--font-cjk);
}
.live-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  animation: pulse 2s ease-in-out infinite;
  flex-shrink: 0;
}
.live-dot-cyan { background: var(--accent-cyan); box-shadow: 0 0 10px var(--accent-cyan); }
.live-dot-purple { background: var(--accent-purple); box-shadow: 0 0 10px var(--accent-purple); }
.live-dot-green { background: var(--accent-green); box-shadow: 0 0 10px var(--accent-green); }
.live-dot-amber { background: var(--accent-amber); box-shadow: 0 0 10px var(--accent-amber); }
.stat-value {
  font-size: clamp(28px, 3.5vw, 46px);
  font-weight: 700;
  line-height: 1;
  letter-spacing: -1.5px;
  font-family: var(--font-tech);
  font-variant-numeric: tabular-nums;
}
.stat-meta {
  font-size: clamp(10px, 0.9vw, 12px);
  font-weight: 500;
  font-family: var(--font-cjk);
  color: var(--text-tertiary);
}
.stat-meta.up { color: var(--accent-green); }
.stat-cyan .stat-value { color: var(--accent-cyan); text-shadow: 0 0 26px rgba(77, 168, 255, 0.5); }
.stat-purple .stat-value { color: var(--accent-purple); text-shadow: 0 0 26px rgba(139, 108, 255, 0.5); }
.stat-green .stat-value { color: var(--accent-green); text-shadow: 0 0 26px rgba(0, 229, 184, 0.5); }
.stat-amber .stat-value { color: var(--accent-amber); text-shadow: 0 0 26px rgba(255, 159, 67, 0.5); }

/* ════════════════════════════════════════════
   ARTICLES
   ════════════════════════════════════════════ */
.article-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  width: 100%;
  max-width: 1280px;
}
.art-card {
  position: relative;
  border-radius: var(--radius-card);
  background: var(--bg-card);
  border: 1px solid var(--border-subtle);
  overflow: hidden;
  text-decoration: none;
  color: inherit;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  transform: perspective(1000px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg));
  transition:
    transform 0.5s cubic-bezier(0.16, 1, 0.3, 1),
    box-shadow 0.4s ease,
    border-color 0.4s ease;
}
.art-card.tilting { transition: transform 0.12s linear, box-shadow 0.4s ease, border-color 0.4s ease; }
.art-card:hover {
  transform: perspective(1000px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg)) translateY(-7px);
  border-color: rgba(160, 200, 255, 0.24);
  box-shadow: 0 0 34px rgba(77, 168, 255, 0.13), 0 20px 46px rgba(0, 0, 0, 0.42);
}
.art-card:hover .card-spot { opacity: 1; }
.art-img {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
.art-img::after {
  content: '';
  position: absolute;
  inset: 0;
  background:
    repeating-linear-gradient(0deg, transparent 0 2px, rgba(0, 0, 0, 0.14) 2px 3px),
    linear-gradient(to top, rgba(4, 4, 12, 0.55), transparent 62%);
  transition: opacity 0.35s;
}
.art-card:hover .art-img::after { opacity: 0.72; }
.art-img svg {
  width: 100%;
  height: 100%;
  display: block;
  transition: transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}
.art-card:hover .art-img svg { transform: scale(1.06); }
.art-scan {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  height: 28%;
  z-index: 2;
  background: linear-gradient(180deg, transparent, rgba(120, 200, 255, 0.16), transparent);
  opacity: 0;
  transition: opacity 0.3s;
}
.art-card:hover .art-scan {
  opacity: 1;
  animation: art-scan-run 1.9s linear infinite;
}
.art-content {
  position: relative;
  z-index: 2;
  padding: clamp(14px, 2vw, 20px);
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.art-tag {
  display: inline-block;
  align-self: flex-start;
  padding: 3px 10px;
  border-radius: 6px;
  font-size: 11px;
  font-weight: 600;
  font-family: var(--font-cjk);
  border: 1px solid transparent;
}
.art-tag.cyan { background: rgba(77, 168, 255, 0.16); color: var(--accent-cyan); border-color: rgba(77, 168, 255, 0.28); }
.art-tag.purple { background: rgba(139, 108, 255, 0.16); color: var(--accent-purple); border-color: rgba(139, 108, 255, 0.28); }
.art-tag.green { background: rgba(0, 229, 184, 0.16); color: var(--accent-green); border-color: rgba(0, 229, 184, 0.28); }
.art-tag.amber { background: rgba(255, 159, 67, 0.16); color: var(--accent-amber); border-color: rgba(255, 159, 67, 0.28); }
.art-content h4 {
  font-size: 16px;
  font-weight: 700;
  font-family: var(--font-cjk);
  transition: color 0.2s;
}
.art-card:hover .art-content h4 { color: #9fd4ff; }
.art-content p {
  font-size: 13px;
  color: var(--text-secondary);
  font-family: var(--font-cjk);
}
.art-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 4px;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}
.art-meta span {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.4);
  font-family: var(--font-mono);
  letter-spacing: 0.5px;
}

.btn-outline {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 10px 20px;
  border-radius: var(--radius-btn);
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--border-subtle);
  color: var(--text-primary);
  font-size: 13px;
  font-weight: 500;
  text-decoration: none;
  font-family: var(--font-cjk);
  transition: background 0.2s, border-color 0.2s, box-shadow 0.2s;
}
.btn-outline:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(150, 200, 255, 0.3);
  box-shadow: 0 0 20px rgba(77, 168, 255, 0.18);
}

/* ════════════════════════════════════════════
   CTA
   ════════════════════════════════════════════ */
.cta-box {
  position: relative;
  overflow: hidden;
  width: 100%;
  max-width: 1280px;
  padding: clamp(44px, 6vw, 84px) clamp(24px, 4vw, 60px);
  border-radius: 20px;
  border: 1px solid rgba(150, 190, 255, 0.14);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: clamp(16px, 2vw, 24px);
  background:
    radial-gradient(ellipse 80% 50% at 50% 38%, rgba(77, 168, 255, 0.22) 0%, transparent 62%),
    radial-gradient(ellipse 60% 40% at 26% 74%, rgba(0, 229, 184, 0.13) 0%, transparent 56%),
    radial-gradient(ellipse 52% 36% at 74% 62%, rgba(139, 108, 255, 0.18) 0%, transparent 52%),
    rgba(8, 8, 24, 0.72);
}
.cta-box::before {
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 62%;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--accent-cyan), var(--accent-purple), transparent);
  opacity: 0.7;
}
.cta-grid {
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: 0.35;
  background-image:
    linear-gradient(rgba(120, 170, 255, 0.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(120, 170, 255, 0.08) 1px, transparent 1px);
  background-size: 42px 42px;
  mask-image: radial-gradient(ellipse 70% 60% at 50% 50%, #000, transparent 74%);
  -webkit-mask-image: radial-gradient(ellipse 70% 60% at 50% 50%, #000, transparent 74%);
}
.cta-box .c-corner {
  opacity: 0.6;
  border-color: rgba(150, 200, 255, 0.4);
}
.cta-glow {
  position: absolute;
  top: -50%;
  left: 50%;
  transform: translateX(-50%);
  width: 320px;
  height: 320px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(77, 168, 255, 0.18), transparent 70%);
  pointer-events: none;
}
.cta-box h2 {
  font-size: clamp(24px, 3.5vw, 46px);
  font-weight: 700;
  text-align: center;
  letter-spacing: -1px;
  font-family: var(--font-cjk);
  position: relative;
  z-index: 1;
  text-shadow: 0 0 40px rgba(77, 168, 255, 0.3);
}
.cta-box p {
  font-size: clamp(14px, 1.3vw, 16px);
  color: var(--text-secondary);
  text-align: center;
  max-width: 520px;
  font-family: var(--font-cjk);
  position: relative;
  z-index: 1;
}
.cta-buttons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  justify-content: center;
  position: relative;
  z-index: 1;
}
.btn-cta-primary {
  position: relative;
  overflow: hidden;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 34px;
  border-radius: var(--radius-btn);
  background: linear-gradient(135deg, var(--accent-cyan), #3d8de0);
  color: #fff;
  font-size: 15px;
  font-weight: 600;
  text-decoration: none;
  font-family: var(--font-cjk);
  box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1) inset, 0 0 26px rgba(77, 168, 255, 0.28);
  transition: box-shadow 0.25s, transform 0.2s;
}
.btn-cta-primary::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(115deg, transparent 34%, rgba(255, 255, 255, 0.42) 50%, transparent 66%);
  transform: translateX(-140%);
  transition: transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}
.btn-cta-primary:hover {
  box-shadow: 0 0 36px rgba(77, 168, 255, 0.6), 0 0 74px rgba(77, 168, 255, 0.26);
  transform: translateY(-2px);
}
.btn-cta-primary:hover::after { transform: translateX(140%); }
.btn-cta-secondary {
  display: inline-flex;
  align-items: center;
  padding: 14px 34px;
  border-radius: var(--radius-btn);
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.16);
  color: var(--text-primary);
  font-size: 15px;
  font-weight: 600;
  text-decoration: none;
  font-family: var(--font-cjk);
  transition: background 0.2s, border-color 0.2s, box-shadow 0.2s, transform 0.2s;
}
.btn-cta-secondary:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.32);
  box-shadow: 0 0 22px rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

/* ════════════════════════════════════════════
   FOOTER
   ════════════════════════════════════════════ */
.site-footer {
  position: relative;
  width: 100%;
  max-width: 1440px;
  margin: 0 auto;
  padding: clamp(28px, 3.5vw, 40px) clamp(16px, 5vw, 80px) clamp(10px, 1.5vw, 16px);
  background: rgba(2, 2, 8, 0.86);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: clamp(18px, 2.5vw, 28px);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
}
.site-footer::before {
  display: none;
  content: '';
  position: absolute;
  top: 0;
  left: 12%;
  right: 12%;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(77, 168, 255, 0.45), rgba(139, 108, 255, 0.35), transparent);
}
.footer-top {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 1280px;
  gap: clamp(20px, 3vw, 40px);
  flex-wrap: wrap;
}
.footer-brand {
  display: flex;
  flex-direction: column;
  gap: 12px;
  flex: 2 1 280px;
  min-width: 240px;
}
.footer-brand p {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.5);
  font-family: var(--font-cjk);
}
.footer-col {
  flex: 1 1 140px;
  min-width: 120px;
}
.footer-col h4 {
  font-size: 13px;
  font-weight: 700;
  margin-bottom: 12px;
  font-family: var(--font-cjk);
  color: #cfe0ff;
}
.footer-col a {
  display: block;
  font-size: 13px;
  color: var(--text-secondary);
  text-decoration: none;
  margin-bottom: 8px;
  font-family: var(--font-cjk);
  transition: color 0.15s, transform 0.2s;
}
.footer-col a:hover {
  color: var(--text-primary);
  transform: translateX(3px);
}
.footer-bottom {
  width: 100%;
  max-width: 1280px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 16px;
  flex-wrap: wrap;
  gap: 12px;
}
.copyright {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.copy-main {
  font-size: 12.5px;
  color: rgba(255, 255, 255, 0.6);
  font-family: var(--font-cjk);
}
.copy-sub {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.32);
  font-family: var(--font-cjk);
  letter-spacing: 0.3px;
}
.footer-status {
  display: flex;
  align-items: center;
  gap: 8px;
}
.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--accent-green);
  box-shadow: 0 0 10px var(--accent-green);
  animation: pulse 2s ease-in-out infinite;
}
.status-text {
  font-size: 12px;
  color: var(--text-secondary);
  font-family: var(--font-mono);
  letter-spacing: 0.4px;
}

/* ════════════════════════════════════════════
   RESPONSIVE
   ════════════════════════════════════════════ */
@media (max-width: 1023px) {
  .nav-links,
  .header-inner > .btn-primary { display: none; }
  .hamburger { display: flex; }
  .service-grid { grid-template-columns: repeat(2, 1fr); }
  .feat-grid { grid-template-columns: repeat(2, 1fr); }
  .stat-grid { grid-template-columns: repeat(2, 1fr); }
  .article-grid { grid-template-columns: repeat(2, 1fr); }
  .hud-read.tl,
  .hud-read.tr,
  .hud-read.bl,
  .hud-read.br { font-size: 9px; }
}

@media (max-width: 600px) {
  .header-inner { height: 56px; }
  .mobile-menu { top: 56px; }
  .hero { padding-top: 62px; padding-bottom: 66px; }
  .service-grid { grid-template-columns: 1fr; }
  .feat-grid { grid-template-columns: 1fr; }
  .stat-grid { grid-template-columns: 1fr 1fr; }
  .article-grid { grid-template-columns: 1fr; }
  .cta-buttons { flex-direction: column; align-items: center; width: 100%; }
  .btn-cta-primary,
  .btn-cta-secondary { width: 100%; text-align: center; justify-content: center; }
  .footer-bottom { flex-direction: column; text-align: center; }
  .section-header-row { flex-direction: column; align-items: flex-start; }
  .section-title-left { font-size: 24px; }
  .feat-item { height: auto; min-height: 64px; }
  .orbit-3 { display: none; }
  .hud-read.bl,
  .hud-read.br { display: none; }
  .hud-ticks { display: none; }
}

@media (max-width: 380px) {
  .stat-grid { grid-template-columns: 1fr; }
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
  .stat-wave,
  .logo-halo,
  .glitch::before,
  .glitch::after,
  .art-scan { animation: none !important; }
  .glitch::before,
  .glitch::after { opacity: 0; }
  :global(html.fx-on) [data-reveal] {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
</style>
