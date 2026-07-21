<script setup lang="ts">
// 只替换首页 UI；短链重定向逻辑在 server/middleware/1.redirect.ts，与此文件无关。
definePageMeta({ layout: false })

useSeoMeta({
  title: 'inurl.link · 发现 · 连接 · 探索',
  description: 'inurl.link —— 一站式精选服务：云端测速、在线工具、API 导航与技术博客。发现 · 连接 · 探索。',
})

useHead({
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+SC:wght@400;500;700;900&family=Sora:wght@600;700;800&display=swap' },
  ],
})

onMounted(() => {
  // Navbar scroll state
  const nav = document.getElementById('nav')
  const onScroll = () => nav?.classList.toggle('scrolled', window.scrollY > 20)
  window.addEventListener('scroll', onScroll, { passive: true })
  onScroll()

  // Reveal on scroll
  const io = new IntersectionObserver((entries) => {
    entries.forEach((e) => { if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target) } })
  }, { threshold: 0.12 })
  document.querySelectorAll<HTMLElement>('.reveal').forEach((el) => io.observe(el))

  // Count-up animation
  const fmt = (v: number, dec: number) => dec ? v.toFixed(dec) : Math.round(v).toLocaleString()
  const animate = (el: HTMLElement) => {
    const target = parseFloat(el.dataset.count || '0')
    const suffix = el.dataset.suffix || ''
    const dec = (target % 1 !== 0) ? 1 : 0
    const dur = 1400
    const t0 = performance.now()
    const step = (t: number) => {
      const p = Math.min((t - t0) / dur, 1)
      const eased = 1 - Math.pow(1 - p, 3)
      el.textContent = fmt(target * eased, dec) + suffix
      if (p < 1) requestAnimationFrame(step)
    }
    requestAnimationFrame(step)
  }
  const cio = new IntersectionObserver((entries) => {
    entries.forEach((e) => { if (e.isIntersecting) { animate(e.target as HTMLElement); cio.unobserve(e.target) } })
  }, { threshold: 0.5 })
  document.querySelectorAll<HTMLElement>('[data-count]').forEach((el) => cio.observe(el))

  // Subtle pointer glow on hero cards
  document.querySelectorAll<HTMLElement>('.card').forEach((card) => {
    card.addEventListener('pointermove', (e: PointerEvent) => {
      const r = card.getBoundingClientRect()
      card.style.setProperty('--mx', (e.clientX - r.left) + 'px')
      card.style.setProperty('--my', (e.clientY - r.top) + 'px')
    })
  })

  // Open all external (http/https) links in a new tab
  document.querySelectorAll<HTMLAnchorElement>('a[href^="http"]').forEach((a) => {
    a.target = '_blank'
    a.rel = 'noopener noreferrer'
  })
})
</script>

<template>
  <div class="inurl-landing">
    <div class="bg-fx"></div>
    <div class="grid-overlay"></div>
    <div class="orb a"></div><div class="orb b"></div><div class="orb c"></div>

    <!-- NAV -->
    <header class="nav" id="nav">
      <div class="nav-inner">
        <a href="#top" class="brand">
          <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Cdefs%3E%3ClinearGradient id='g' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' stop-color='%231677ff'/%3E%3Cstop offset='100%25' stop-color='%232979ff'/%3E%3C/linearGradient%3E%3C/defs%3E%3Ccircle cx='16' cy='16' r='14' fill='none' stroke='url(%23g)' stroke-width='2.5'/%3E%3Cpolygon points='13,8 6,17 13,26 8,17' fill='url(%23g)'/%3E%3Cpolygon points='19,8 26,17 19,26 24,17' fill='url(%23g)'/%3E%3C/svg%3E" alt="inurl.link" class="logo-img" />
          <span>inurl<b>.link</b></span>
        </a>
        <nav class="nav-links">
          <a href="https://api.inurl.link/">API Token大全</a>
          <a href="https://tools.inurl.link/">工具箱</a>
          <a href="https://ping.inurl.link/">云测速</a>
          <a href="https://blog.gaomeluo.com/">博客</a>
          <a href="https://www.lixiaoxin.com/">关于</a>
        </nav>
        <a href="https://inurl.link/dashboard" class="btn btn-primary">短连接</a>
      </div>
    </header>

    <!-- HERO -->
    <section class="hero" id="top">
      <div class="wrap">
        <h1 class="hero-title reveal d1">发现 · <span class="grad">连接</span> · 探索</h1>
        <p class="hero-sub reveal d2">云端测速、在线工具、资源搜索与 API 导航，一站式精选服务，助你高效连接每一次探索。</p>
        <div class="badge-row reveal d2" style="margin:0 auto 26px">
          <span class="chip"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6 9 17l-5-5"/></svg>稳定可靠</span>
          <span class="chip"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2 3 14h9l-1 8 10-12h-9z"/></svg>极速体验</span>
          <span class="chip"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 2"/></svg>实时更新</span>
        </div>
        <div class="hero-cta reveal d3">
          <a href="#services" class="btn btn-primary btn-lg">探索核心服务</a>
          <a href="https://api.inurl.link/" class="btn btn-ghost btn-lg">API Token大全</a>
        </div>

        <div class="stat-strip reveal d4">
          <div class="box"><div class="num" data-count="128">0</div><div class="lbl">服务节点数</div></div>
          <div class="box"><div class="num" data-count="56">0</div><div class="lbl">在线工具数</div></div>
          <div class="box"><div class="num" data-count="8.2" data-suffix="K">0</div><div class="lbl">日访问用户</div></div>
          <div class="box"><div class="num" data-count="99.9" data-suffix="%">0</div><div class="lbl">服务可用率</div></div>
        </div>
      </div>
    </section>

    <!-- CORE SERVICES -->
    <section class="wrap" id="services" style="padding:90px 24px">
      <div class="section-head reveal">
        <span class="eyebrow"><span class="dot"></span>CORE SERVICES</span>
        <h2>一站式精选服务</h2>
        <p>云端测速、在线工具、资源搜索，核心功能应有尽有。</p>
      </div>
      <div class="grid cols-4">
        <a class="card reveal d1" href="https://ping.inurl.link/">
          <span class="glow"></span>
          <div class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 12h4l3 8 4-16 3 8h4"/></svg></div>
          <h3>云服务节点测速</h3>
          <p>实时检测阿里云、腾讯云等主流厂商各节点延迟与速度，助你选择最优线路。</p>
          <span class="more">立即使用 →</span>
        </a>
        <a class="card reveal d2" href="https://tools.inurl.link/">
          <span class="glow"></span>
          <div class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14.7 6.3a4 4 0 0 0-5.4 5.4L3 18v3h3l6.3-6.3a4 4 0 0 0 5.4-5.4l-2.6 2.6-2-2 2.6-2.6z"/></svg></div>
          <h3>在线工具箱</h3>
          <p>常用开发 &amp; 日常工具集合，即开即用，无需安装。</p>
          <span class="more">立即使用 →</span>
        </a>
        <a class="card reveal d3" href="https://api.inurl.link/">
          <span class="glow"></span>
          <div class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 7h16M4 12h16M4 17h10"/><circle cx="18" cy="17" r="2"/></svg></div>
          <h3>API Token 大全</h3>
          <p>国内外免费 / 付费 API 一站式收录 · 性价比对比 · 擅长领域导航。</p>
          <span class="more">立即使用 →</span>
        </a>
        <a class="card reveal d4" href="https://blog.gaomeluo.com/">
          <span class="glow"></span>
          <div class="ico"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg></div>
          <h3>技术博客</h3>
          <p>GaoMeluo Blog · inurl.top，技术文章与个人随想。</p>
          <span class="more">立即访问 →</span>
        </a>
      </div>
    </section>

    <!-- BLOG -->
    <section class="wrap" id="blog" style="padding:20px 24px 90px">
      <div class="section-head reveal">
        <span class="eyebrow"><span class="dot"></span>LATEST</span>
        <h2>技术博客 · 最新文章</h2>
        <p>从软路由到 AI Agent，精选实战教程与资源合辑。</p>
      </div>
      <div class="grid cols-4">
        <a class="post reveal d1" href="https://inurl.top/archives/ClashforOpenWRT/">
          <div class="cover cov-net">
            <svg viewBox="0 0 320 150" preserveAspectRatio="xMidYMid slice" aria-hidden="true">
              <g fill="none" stroke="#fff" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
                <rect x="118" y="80" width="84" height="34" rx="9" fill="rgba(255,255,255,.08)"/>
                <circle cx="133" cy="97" r="3" fill="#fff" stroke="none"/>
                <circle cx="146" cy="97" r="3" fill="#fff" stroke="none"/>
                <circle cx="159" cy="97" r="3" fill="#fff" stroke="none"/>
                <path d="M160 80 V64"/><path d="M160 64 l11 -9"/>
                <path d="M160 52 a22 22 0 0 1 30 7" opacity=".7"/>
                <path d="M160 44 a34 34 0 0 1 47 11" opacity=".5"/>
                <circle cx="58" cy="58" r="5" fill="#fff" stroke="none"/>
                <circle cx="264" cy="98" r="5" fill="#fff" stroke="none"/>
                <path d="M63 61 L118 86" opacity=".5"/>
                <path d="M202 92 L259 95" opacity=".5"/>
              </g>
            </svg>
            <span class="tag">软路由</span>
          </div>
          <div class="body"><h4>Clash for OpenWRT 实现全屋设备科学上网</h4>
            <p>从机场选择、软路由推荐到 OpenWRT 刷机、OpenClash 配置，全流程手把手教程。</p>
            <div class="meta"><span>2023-05-17</span><span>10 分钟</span></div></div>
        </a>
        <a class="post reveal d2" href="https://inurl.top/archives/datizi/">
          <div class="cover cov-vpn">
            <svg viewBox="0 0 320 150" preserveAspectRatio="xMidYMid slice" aria-hidden="true">
              <g fill="none" stroke="#fff" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
                <path d="M160 36 l44 17 v27 c0 31 -23 47 -44 55 c-21 -8 -44 -24 -44 -55 V53 z" fill="rgba(255,255,255,.08)"/>
                <rect x="147" y="78" width="26" height="21" rx="4" fill="rgba(255,255,255,.15)"/>
                <path d="M151 78 v-6 a9 9 0 0 1 18 0 v6"/>
                <circle cx="160" cy="88" r="3" fill="#fff" stroke="none"/>
                <path d="M116 48 L58 58" opacity=".5"/>
                <path d="M204 48 L262 58" opacity=".5"/>
                <circle cx="54" cy="58" r="4" fill="#fff" stroke="none"/>
                <circle cx="266" cy="58" r="4" fill="#fff" stroke="none"/>
              </g>
            </svg>
            <span class="tag">教程</span>
          </div>
          <div class="body"><h4>如何搭建梯子（VPN）？</h4>
            <p>自己搭建 V2ray 梯子，独享高速线路。从服务器选择到一键脚本部署，一步到位。</p>
            <div class="meta"><span>2023-08-18</span><span>12 分钟</span></div></div>
        </a>
        <a class="post reveal d3" href="https://blog.gaomeluo.com/archives/wan-OpenClaw/">
          <div class="cover cov-ai">
            <svg viewBox="0 0 320 150" preserveAspectRatio="xMidYMid slice" aria-hidden="true">
              <g fill="none" stroke="#fff" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round">
                <rect x="126" y="52" width="68" height="52" rx="15" fill="rgba(255,255,255,.08)"/>
                <circle cx="146" cy="78" r="6" fill="#fff" stroke="none"/>
                <circle cx="174" cy="78" r="6" fill="#fff" stroke="none"/>
                <path d="M152 94 h16"/>
                <path d="M160 52 V38"/><circle cx="160" cy="35" r="4" fill="#fff" stroke="none"/>
                <circle cx="66" cy="62" r="5" fill="#fff" stroke="none"/>
                <circle cx="252" cy="92" r="5" fill="#fff" stroke="none"/>
                <path d="M71 65 L126 76" opacity=".5"/>
                <path d="M194 88 L247 90" opacity=".5"/>
              </g>
            </svg>
            <span class="tag">OpenClaw</span>
          </div>
          <div class="body"><h4>玩转 OpenClaw丨手把手教程合辑</h4>
            <p>从部署到接入微信/QQ/飞书，再到投资 Agent 实战——全场景教程汇总。</p>
            <div class="meta"><span>2026-06-09</span><span>8 分钟</span></div></div>
        </a>
        <a class="post reveal d4" href="https://blog.gaomeluo.com/archives/mianfeiziti/">
          <div class="cover cov-font">
            <svg viewBox="0 0 320 150" preserveAspectRatio="xMidYMid slice" aria-hidden="true">
              <text x="36" y="106" font-family="Sora, Inter, sans-serif" font-size="86" font-weight="800" fill="#fff" opacity=".96">Aa</text>
              <g stroke="#fff" stroke-width="3" stroke-linecap="round" opacity=".55">
                <path d="M150 58 h112"/>
                <path d="M150 78 h86"/>
                <path d="M150 98 h102"/>
              </g>
            </svg>
            <span class="tag">资源</span>
          </div>
          <div class="body"><h4>几百款免费商用字体下载</h4>
            <p>搜罗了全网可商用的中英文字体，几百款打包下载，设计师和开发者必备。</p>
            <div class="meta"><span>2025-11-22</span><span>5 分钟</span></div></div>
        </a>
      </div>
    </section>

    <!-- LIVE STATS -->
    <section class="wrap" id="stats" style="padding:10px 24px 80px">
      <div class="band reveal">
        <div class="section-head" style="margin-bottom:30px">
          <span class="eyebrow"><span class="dot"></span>LIVE STATS</span>
          <h2>平台实时数据</h2>
          <p>数据每 5 分钟自动更新，反映服务真实运行状态。</p>
        </div>
        <div class="brow">
          <div class="cell"><div class="bnum" data-count="128">0</div><div class="blbl">服务节点数</div><span class="btrend">↑ 12 本周新增</span></div>
          <div class="cell"><div class="bnum" data-count="56">0</div><div class="blbl">在线工具数</div><span class="btrend">↑ 3 本周新增</span></div>
          <div class="cell"><div class="bnum" data-count="8.2" data-suffix="K">0</div><div class="blbl">日访问用户</div><span class="btrend">↑ 18% 较上周</span></div>
          <div class="cell"><div class="bnum" data-count="99.9" data-suffix="%">0</div><div class="blbl">服务可用率</div><span class="btrend">SLA 持续保障</span></div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="wrap cta-sec">
      <div class="cta-box reveal">
        <span class="glow" style="position:absolute;width:300px;height:300px;background:radial-gradient(circle,rgba(124,92,255,.5),transparent 70%);top:-120px;left:50%;transform:translateX(-50%)"></span>
        <h2>准备好开始探索了吗？</h2>
        <p>立即打开 API Token 大全，或访问技术博客，开启你的高效之旅。</p>
        <div class="hero-cta" style="margin-bottom:0">
          <a href="https://api.inurl.link/" class="btn btn-primary btn-lg">API Token大全</a>
          <a href="https://blog.gaomeluo.com/" class="btn btn-ghost btn-lg">阅读博客</a>
        </div>
      </div>
    </section>

    <!-- FOOTER -->
    <footer>
      <div class="wrap">
        <div class="foot-grid">
          <div class="foot-about">
            <a href="#top" class="brand"><img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Cdefs%3E%3ClinearGradient id='g' x1='0%25' y1='0%25' x2='100%25' y2='100%25'%3E%3Cstop offset='0%25' stop-color='%231677ff'/%3E%3Cstop offset='100%25' stop-color='%232979ff'/%3E%3C/linearGradient%3E%3C/defs%3E%3Ccircle cx='16' cy='16' r='14' fill='none' stroke='url(%23g)' stroke-width='2.5'/%3E%3Cpolygon points='13,8 6,17 13,26 8,17' fill='url(%23g)'/%3E%3Cpolygon points='19,8 26,17 19,26 24,17' fill='url(%23g)'/%3E%3C/svg%3E" alt="inurl.link" class="logo-img" /><span>inurl<b>.link</b></span></a>
            <p>发现 · 连接 · 探索。一站式精选服务平台，连接你与高效工具。</p>
          </div>
          <div><h5>核心服务</h5><a href="https://ping.inurl.link/">节点测速</a><a href="https://tools.inurl.link/">在线工具箱</a><a href="https://api.inurl.link/">API 导航</a><a href="https://blog.gaomeluo.com/">技术博客</a></div>
          <div><h5>精选资源</h5><a href="https://tools.inurl.link/">JSON 格式化</a><a href="https://tools.inurl.link/">Base64 编解码</a><a href="https://tools.inurl.link/">UUID 生成</a><a href="https://tools.inurl.link/">时间戳转换</a></div>
          <div><h5>关于</h5><a href="#top">网站首页</a><a href="#stats">实时数据</a><a href="#blog">最新文章</a><a href="https://blog.gaomeluo.com/">联系我们</a></div>
        </div>
        <div class="foot-bottom">
          <span>© 2026 inurl.link · 保留所有权利</span>
          <span>Designed with ⚡ for explorers</span>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>
  :root{
    --bg:#06070f;
    --bg-2:#0a0c1a;
    --surface:rgba(255,255,255,.045);
    --surface-2:rgba(255,255,255,.07);
    --border:rgba(255,255,255,.10);
    --border-strong:rgba(255,255,255,.18);
    --text:#eef1ff;
    --text-dim:#aab0d0;
    --text-faint:#71779c;
    --brand:#7c5cff;
    --brand-2:#22d3ee;
    --brand-3:#ff5c9d;
    --glow:rgba(124,92,255,.45);
    --radius:20px;
    --maxw:1180px;
    --shadow:0 24px 60px -20px rgba(0,0,0,.7);
  }
  html{scroll-behavior:smooth}

  /* ---------- Landing root (scoped to avoid touching /dashboard) ---------- */
  .inurl-landing{
    position:relative;isolation:isolate;min-height:100vh;
    font-family:"Inter","Noto Sans SC",system-ui,-apple-system,Segoe UI,Roboto,sans-serif;
    background:var(--bg);color:var(--text);line-height:1.65;
    overflow-x:hidden;-webkit-font-smoothing:antialiased;
  }
  .inurl-landing *{box-sizing:border-box;margin:0;padding:0}
  .inurl-landing a{color:inherit;text-decoration:none}
  .inurl-landing img{max-width:100%}
  .inurl-landing ::selection{background:rgba(124,92,255,.35);color:#fff}

  /* ---------- Ambient background ---------- */
  .inurl-landing .bg-fx{position:fixed;inset:0;z-index:-2;overflow:hidden;pointer-events:none}
  .inurl-landing .bg-fx::before{
    content:"";position:absolute;inset:0;
    background:
      radial-gradient(900px 600px at 12% -5%, rgba(124,92,255,.22), transparent 60%),
      radial-gradient(800px 600px at 92% 8%, rgba(34,211,238,.18), transparent 55%),
      radial-gradient(900px 700px at 50% 110%, rgba(255,92,157,.14), transparent 60%),
      linear-gradient(180deg,#06070f 0%, #080a16 50%, #06070f 100%);
  }
  .inurl-landing .grid-overlay{
    position:fixed;inset:0;z-index:-1;pointer-events:none;
    background-image:
      linear-gradient(rgba(255,255,255,.035) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,.035) 1px, transparent 1px);
    background-size:54px 54px;
    mask-image:radial-gradient(ellipse 80% 60% at 50% 30%, #000 40%, transparent 100%);
    -webkit-mask-image:radial-gradient(ellipse 80% 60% at 50% 30%, #000 40%, transparent 100%);
  }
  .inurl-landing .orb{
    position:fixed;border-radius:50%;filter:blur(70px);opacity:.55;z-index:-1;pointer-events:none;
    animation:float 16s ease-in-out infinite;
  }
  .inurl-landing .orb.a{width:380px;height:380px;background:radial-gradient(circle,#7c5cff,transparent 70%);top:8%;left:-6%}
  .inurl-landing .orb.b{width:340px;height:340px;background:radial-gradient(circle,#22d3ee,transparent 70%);top:30%;right:-4%;animation-delay:-5s}
  .inurl-landing .orb.c{width:300px;height:300px;background:radial-gradient(circle,#ff5c9d,transparent 70%);bottom:4%;left:30%;animation-delay:-9s}
  @keyframes float{0%,100%{transform:translateY(0) translateX(0)}50%{transform:translateY(-30px) translateX(18px)}}

  /* ---------- Layout ---------- */
  .inurl-landing .wrap{max-width:var(--maxw);margin:0 auto;padding:0 24px}
  .inurl-landing section{position:relative}
  .inurl-landing .eyebrow{
    display:inline-flex;align-items:center;gap:8px;
    font-size:13px;letter-spacing:.14em;text-transform:uppercase;
    color:var(--text-dim);font-weight:600;
    padding:7px 14px;border:1px solid var(--border);border-radius:999px;
    background:var(--surface);backdrop-filter:blur(8px);
  }
  .inurl-landing .eyebrow .dot{width:7px;height:7px;border-radius:50%;background:var(--brand-2);box-shadow:0 0 12px var(--brand-2)}
  .inurl-landing .section-head{text-align:center;max-width:720px;margin:0 auto 56px}
  .inurl-landing .section-head h2{
    font-family:"Sora",sans-serif;font-size:clamp(28px,4.4vw,46px);font-weight:800;
    margin:18px 0 14px;letter-spacing:-.02em;
    background:linear-gradient(180deg,#fff,#c9cdf0);-webkit-background-clip:text;background-clip:text;color:transparent;
  }
  .inurl-landing .section-head p{color:var(--text-dim);font-size:17px}

  /* ---------- Navbar ---------- */
  .inurl-landing header.nav{
    position:fixed;top:0;left:0;right:0;z-index:50;
    transition:all .3s ease;
  }
  .inurl-landing .nav-inner{
    max-width:var(--maxw);margin:14px auto 0;padding:12px 18px;
    display:flex;align-items:center;justify-content:space-between;gap:20px;
    border:1px solid transparent;border-radius:16px;
    transition:all .3s ease;
  }
  .inurl-landing header.nav.scrolled .nav-inner{
    margin-top:10px;background:rgba(10,12,26,.72);
    border-color:var(--border);backdrop-filter:blur(18px) saturate(140%);
    box-shadow:0 12px 40px -20px rgba(0,0,0,.8);
  }
  .inurl-landing .brand{display:flex;align-items:center;gap:11px;font-weight:800;font-size:19px;letter-spacing:-.01em}
  .inurl-landing .brand .logo-img{
    width:34px;height:34px;border-radius:9px;display:block;
    box-shadow:0 8px 22px -8px rgba(22,119,255,.55);
  }
  .inurl-landing .brand b{background:linear-gradient(90deg,#fff,#b9bdf0);-webkit-background-clip:text;background-clip:text;color:transparent}
  .inurl-landing .nav-links{display:flex;align-items:center;gap:6px}
  .inurl-landing .nav-links a{
    font-size:15px;color:var(--text-dim);font-weight:500;padding:9px 14px;border-radius:10px;
    transition:all .2s ease;
  }
  .inurl-landing .nav-links a:hover{color:var(--text);background:var(--surface)}
  .inurl-landing .btn{
    display:inline-flex;align-items:center;gap:9px;font-weight:600;font-size:15px;
    padding:11px 20px;border-radius:12px;cursor:pointer;border:1px solid transparent;
    transition:all .25s ease;white-space:nowrap;
  }
  .inurl-landing .btn-primary{
    color:#fff;background:linear-gradient(135deg,var(--brand),#9b6bff);
    box-shadow:0 12px 30px -10px var(--glow);
  }
  .inurl-landing .btn-primary:hover{transform:translateY(-2px);box-shadow:0 18px 40px -12px var(--glow);filter:brightness(1.07)}
  .inurl-landing .btn-ghost{color:var(--text);border-color:var(--border-strong);background:var(--surface)}
  .inurl-landing .btn-ghost:hover{border-color:var(--brand);color:#fff;background:rgba(124,92,255,.12)}
  .inurl-landing .nav-toggle{display:none;background:none;border:0;color:var(--text);cursor:pointer}

  /* ---------- Hero ---------- */
  .inurl-landing .hero{padding:170px 0 90px;text-align:center;position:relative}
  .inurl-landing .hero .badge-row{display:flex;justify-content:center;flex-wrap:wrap;gap:10px;margin-bottom:26px}
  .inurl-landing .chip{
    display:inline-flex;align-items:center;gap:8px;font-size:13.5px;color:var(--text-dim);
    padding:8px 14px;border:1px solid var(--border);border-radius:999px;background:var(--surface);
    backdrop-filter:blur(6px);
  }
  .inurl-landing .chip svg{width:15px;height:15px;color:var(--brand-2)}
  .inurl-landing h1.hero-title{
    font-family:"Sora",sans-serif;font-weight:800;letter-spacing:-.03em;
    font-size:clamp(40px,7vw,84px);line-height:1.04;margin:0 auto 22px;max-width:14ch;
  }
  .inurl-landing h1.hero-title .grad{
    background:linear-gradient(100deg,var(--brand) 0%,var(--brand-2) 45%,var(--brand-3) 100%);
    -webkit-background-clip:text;background-clip:text;color:transparent;
    background-size:200% auto;animation:shine 6s linear infinite;
  }
  @keyframes shine{to{background-position:200% center}}
  .inurl-landing .hero-sub{font-size:clamp(16px,2vw,20px);color:var(--text-dim);max-width:620px;margin:0 auto 38px}
  .inurl-landing .hero-cta{display:flex;gap:14px;justify-content:center;flex-wrap:wrap;margin-bottom:64px}
  .inurl-landing .btn-lg{padding:15px 28px;font-size:16px;border-radius:14px}

  /* hero stat strip */
  .inurl-landing .stat-strip{
    display:grid;grid-template-columns:repeat(4,1fr);gap:18px;max-width:920px;margin:0 auto;
  }
  .inurl-landing .stat-strip .box{
    background:var(--surface);border:1px solid var(--border);border-radius:16px;
    padding:22px 16px;backdrop-filter:blur(8px);transition:all .3s ease;
  }
  .inurl-landing .stat-strip .box:hover{border-color:var(--border-strong);transform:translateY(-3px)}
  .inurl-landing .stat-strip .num{font-family:"Sora",sans-serif;font-size:clamp(26px,3.4vw,38px);font-weight:800;
    background:linear-gradient(180deg,#fff,#c5c9f2);-webkit-background-clip:text;background-clip:text;color:transparent}
  .inurl-landing .stat-strip .lbl{font-size:13.5px;color:var(--text-faint);margin-top:4px}

  /* ---------- Cards grid ---------- */
  .inurl-landing .grid{display:grid;gap:22px}
  .inurl-landing .grid.cols-4{grid-template-columns:repeat(4,1fr)}
  .inurl-landing .grid.cols-3{grid-template-columns:repeat(3,1fr)}
  .inurl-landing .grid.cols-2{grid-template-columns:repeat(2,1fr)}

  .inurl-landing .card{
    position:relative;background:var(--surface);border:1px solid var(--border);
    border-radius:var(--radius);padding:28px;overflow:hidden;
    transition:transform .35s cubic-bezier(.2,.8,.2,1), border-color .35s, box-shadow .35s;
    backdrop-filter:blur(10px);
  }
  .inurl-landing .card::before{
    content:"";position:absolute;inset:0;border-radius:inherit;padding:1px;
    background:linear-gradient(135deg,transparent,rgba(124,92,255,.5),transparent);
    -webkit-mask:linear-gradient(#000 0 0) content-box,linear-gradient(#000 0 0);
    -webkit-mask-composite:xor;mask-composite:exclude;opacity:0;transition:opacity .35s;
  }
  .inurl-landing .card:hover{transform:translateY(-6px);border-color:transparent;box-shadow:var(--shadow)}
  .inurl-landing .card:hover::before{opacity:1}
  .inurl-landing .card .glow{
    position:absolute;width:240px;height:240px;border-radius:50%;filter:blur(70px);
    background:radial-gradient(circle,rgba(124,92,255,.4),transparent 70%);
    top:-80px;right:-60px;opacity:0;transition:opacity .4s;pointer-events:none;
  }
  .inurl-landing .card:hover .glow{opacity:1}
  .inurl-landing .card .ico{
    width:52px;height:52px;border-radius:14px;display:grid;place-items:center;margin-bottom:18px;
    background:linear-gradient(135deg,rgba(124,92,255,.25),rgba(34,211,238,.18));
    border:1px solid var(--border);color:#fff;
  }
  .inurl-landing .card .ico svg{width:26px;height:26px}
  .inurl-landing .card h3{font-size:19px;font-weight:700;margin-bottom:8px;font-family:"Sora",sans-serif}
  .inurl-landing .card p{color:var(--text-dim);font-size:14.5px}
  .inurl-landing .card .more{
    display:inline-flex;align-items:center;gap:6px;margin-top:18px;font-size:14px;font-weight:600;
    color:var(--brand-2);transition:gap .2s;
  }
  .inurl-landing .card:hover .more{gap:11px}

  /* ---------- Live stats band ---------- */
  .inurl-landing .band{
    margin:30px auto;padding:46px 30px;border-radius:28px;
    background:linear-gradient(135deg,rgba(124,92,255,.12),rgba(34,211,238,.08));
    border:1px solid var(--border-strong);position:relative;overflow:hidden;
  }
  .inurl-landing .band::after{content:"";position:absolute;inset:0;background:radial-gradient(600px 300px at 80% 0,rgba(255,92,157,.16),transparent 60%)}
  .inurl-landing .band .brow{display:grid;grid-template-columns:repeat(4,1fr);gap:24px;position:relative;z-index:1}
  .inurl-landing .band .brow .cell{text-align:center}
  .inurl-landing .band .bnum{font-family:"Sora",sans-serif;font-size:clamp(30px,4vw,48px);font-weight:800;
    background:linear-gradient(180deg,#fff,#cfd3ff);-webkit-background-clip:text;background-clip:text;color:transparent}
  .inurl-landing .band .blbl{color:var(--text-dim);font-size:14.5px;margin-top:6px}
  .inurl-landing .band .btrend{display:inline-block;margin-top:8px;font-size:12.5px;color:#7CFFB2;
    background:rgba(124,255,178,.10);border:1px solid rgba(124,255,178,.25);padding:3px 10px;border-radius:999px}

  /* ---------- Blog ---------- */
  .inurl-landing .post{
    background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);
    overflow:hidden;transition:all .35s;display:flex;flex-direction:column;
  }
  .inurl-landing .post:hover{transform:translateY(-6px);border-color:var(--border-strong);box-shadow:var(--shadow)}
  .inurl-landing .post .cover{height:150px;position:relative;overflow:hidden;background:#0b1020}
  .inurl-landing .post .cover svg{position:absolute;inset:0;width:100%;height:100%;display:block}
  .inurl-landing .post .cover::after{content:"";position:absolute;inset:0;background:radial-gradient(120% 80% at 80% 0%,rgba(255,255,255,.18),transparent 60%)}
  .inurl-landing .post .cover.cov-net{background:linear-gradient(135deg,#7c5cff,#3b6dff)}
  .inurl-landing .post .cover.cov-vpn{background:linear-gradient(135deg,#22d3ee,#0e9f8e)}
  .inurl-landing .post .cover.cov-ai{background:linear-gradient(135deg,#ff5c9d,#7c5cff)}
  .inurl-landing .post .cover.cov-font{background:linear-gradient(135deg,#f59e0b,#ff5c9d)}
  .inurl-landing .post .cover .tag{
    position:absolute;left:16px;top:16px;z-index:2;font-size:12px;font-weight:600;color:#fff;
    background:rgba(0,0,0,.32);backdrop-filter:blur(6px);padding:5px 11px;border-radius:999px;border:1px solid rgba(255,255,255,.25)
  }
  .inurl-landing .post .body{padding:20px 22px 24px;display:flex;flex-direction:column;gap:10px;flex:1}
  .inurl-landing .post .body h4{font-size:16.5px;font-weight:700;line-height:1.45}
  .inurl-landing .post .body p{color:var(--text-dim);font-size:13.5px;flex:1}
  .inurl-landing .post .meta{display:flex;justify-content:space-between;font-size:12.5px;color:var(--text-faint);margin-top:4px}

  /* ---------- CTA ---------- */
  .inurl-landing .cta-sec{padding:90px 0}
  .inurl-landing .cta-box{
    text-align:center;padding:64px 32px;border-radius:30px;position:relative;overflow:hidden;
    background:linear-gradient(135deg,rgba(124,92,255,.18),rgba(34,211,238,.10));
    border:1px solid var(--border-strong);
  }
  .inurl-landing .cta-box h2{font-family:"Sora",sans-serif;font-size:clamp(28px,4vw,44px);font-weight:800;letter-spacing:-.02em;margin-bottom:14px}
  .inurl-landing .cta-box p{color:var(--text-dim);font-size:17px;max-width:520px;margin:0 auto 30px}

  /* ---------- Footer ---------- */
  .inurl-landing footer{border-top:1px solid var(--border);margin-top:40px;padding:54px 0 36px;background:rgba(8,9,20,.5)}
  .inurl-landing .foot-grid{display:grid;grid-template-columns:1.6fr 1fr 1fr 1fr;gap:30px}
  .inurl-landing .foot-grid h5{font-size:14px;color:var(--text);margin-bottom:16px;font-weight:700}
  .inurl-landing .foot-grid a{display:block;color:var(--text-dim);font-size:14px;padding:6px 0;transition:color .2s}
  .inurl-landing .foot-grid a:hover{color:#fff}
  .inurl-landing .foot-about p{color:var(--text-dim);font-size:14px;max-width:320px;margin-top:14px}
  .inurl-landing .foot-bottom{display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px;
    margin-top:40px;padding-top:24px;border-top:1px solid var(--border);color:var(--text-faint);font-size:13.5px}

  /* ---------- Reveal animation ---------- */
  .inurl-landing .reveal{opacity:0;transform:translateY(28px);transition:opacity .7s ease, transform .7s cubic-bezier(.2,.8,.2,1)}
  .inurl-landing .reveal.in{opacity:1;transform:none}
  .inurl-landing .reveal.d1{transition-delay:.08s}.inurl-landing .reveal.d2{transition-delay:.16s}.inurl-landing .reveal.d3{transition-delay:.24s}.inurl-landing .reveal.d4{transition-delay:.32s}

  /* ---------- Responsive ---------- */
  @media(max-width:980px){
    .inurl-landing .grid.cols-4{grid-template-columns:repeat(2,1fr)}
    .inurl-landing .band .brow{grid-template-columns:repeat(2,1fr);gap:34px}
    .inurl-landing .foot-grid{grid-template-columns:1fr 1fr}
  }
  @media(max-width:760px){
    .inurl-landing .nav-links{display:none}
    .inurl-landing .nav-toggle{display:inline-flex}
    .inurl-landing .grid.cols-4,.inurl-landing .grid.cols-3,.inurl-landing .grid.cols-2{grid-template-columns:1fr}
    .inurl-landing .stat-strip{grid-template-columns:repeat(2,1fr)}
    .inurl-landing .hero{padding-top:140px}
    .inurl-landing .foot-grid{grid-template-columns:1fr}
    .inurl-landing .band .brow{grid-template-columns:1fr}
  }
</style>
