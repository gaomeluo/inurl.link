<script setup lang="ts">
definePageMeta({
  layout: 'landing',
})
</script>

<template>
  <!-- Nav -->
  <header>
    <div class="nav-inner">
      <a href="/" class="logo">
        <span class="logo-icon">&lt;/&gt;</span>
        <span>inurl<span class="logo-dot">.</span>link</span>
      </a>
      <div class="nav-links">
        <a href="https://github.com/gaomeluo" target="_blank" class="nav-btn">
          <svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
          GitHub
        </a>
        <a href="/dashboard" target="_blank" class="nav-btn-primary">短链接</a>
      </div>
    </div>
  </header>

  <!-- Grid -->
  <div class="container">
    <div class="grid">

      <!-- Card 1: 云服务节点测速 (Featured) — CDN Globe -->
      <div class="card featured">
        <div class="card-thumb">
          <div class="cdn-thumb">
            <div class="cdn-globe">
              <svg viewBox="0 0 500 300" xmlns="http://www.w3.org/2000/svg">
                <defs>
                  <radialGradient id="earthGrad" cx="50%" cy="50%">
                    <stop offset="0%" stop-color="#1e1b4b"/>
                    <stop offset="100%" stop-color="#0a0a2e"/>
                  </radialGradient>
                </defs>

                <!-- Globe base -->
                <ellipse cx="250" cy="150" rx="100" ry="100" fill="url(#earthGrad)" stroke="rgba(99,102,241,.25)" stroke-width="1"/>

                <!-- Latitude lines -->
                <ellipse cx="250" cy="150" rx="100" ry="25" fill="none" stroke="rgba(99,102,241,.08)" stroke-width=".5"/>
                <ellipse cx="250" cy="150" rx="100" ry="55" fill="none" stroke="rgba(99,102,241,.06)" stroke-width=".5"/>
                <ellipse cx="250" cy="150" rx="100" ry="80" fill="none" stroke="rgba(99,102,241,.05)" stroke-width=".5"/>

                <!-- Longitude lines -->
                <ellipse cx="250" cy="150" rx="25" ry="100" fill="none" stroke="rgba(99,102,241,.08)" stroke-width=".5"/>
                <ellipse cx="250" cy="150" rx="55" ry="100" fill="none" stroke="rgba(99,102,241,.06)" stroke-width=".5"/>
                <ellipse cx="250" cy="150" rx="80" ry="100" fill="none" stroke="rgba(99,102,241,.05)" stroke-width=".5"/>

                <!-- Continent shapes -->
                <g fill="rgba(79,70,229,.15)" stroke="rgba(99,102,241,.2)" stroke-width=".5">
                  <path d="M160,80 Q180,65 200,75 Q210,80 205,95 Q200,110 190,115 Q175,120 165,110 Q155,100 160,80Z"/>
                  <path d="M195,165 Q205,160 210,170 Q215,185 210,200 Q205,210 195,210 Q190,200 192,185 Q195,175 195,165Z"/>
                  <path d="M240,70 Q255,60 270,65 Q275,70 270,80 Q265,90 255,95 Q245,95 240,85 Q235,78 240,70Z"/>
                  <path d="M245,110 Q255,105 265,110 Q275,120 270,140 Q265,155 255,160 Q245,158 240,145 Q238,130 245,110Z"/>
                  <path d="M280,60 Q310,45 340,50 Q360,55 370,70 Q375,85 365,95 Q350,100 330,95 Q310,90 290,85 Q280,75 280,60Z"/>
                  <path d="M340,180 Q355,175 360,185 Q362,195 355,200 Q345,200 338,195 Q335,188 340,180Z"/>
                </g>

                <!-- Pulsing location rings -->
                <circle cx="195" cy="118" r="5" class="pulse-ring"/>
                <circle cx="195" cy="118" r="5" class="pulse-ring"/>
                <circle cx="195" cy="118" r="5" class="pulse-ring"/>
                <circle cx="260" cy="95" r="5" class="pulse-ring"/>
                <circle cx="260" cy="95" r="5" class="pulse-ring"/>
                <circle cx="260" cy="95" r="5" class="pulse-ring"/>
                <circle cx="355" cy="145" r="5" class="pulse-ring"/>
                <circle cx="355" cy="145" r="5" class="pulse-ring"/>
                <circle cx="355" cy="145" r="5" class="pulse-ring"/>
                <circle cx="310" cy="80" r="4" class="pulse-ring"/>
                <circle cx="310" cy="80" r="4" class="pulse-ring"/>
                <circle cx="310" cy="80" r="4" class="pulse-ring"/>
                <circle cx="230" cy="200" r="4" class="pulse-ring"/>
                <circle cx="230" cy="200" r="4" class="pulse-ring"/>
                <circle cx="230" cy="200" r="4" class="pulse-ring"/>

                <!-- City dots -->
                <circle cx="195" cy="118" r="3" class="city-node" style="animation-delay:0s"/>
                <circle cx="260" cy="95" r="3" class="city-node" style="animation-delay:.3s"/>
                <circle cx="355" cy="145" r="3" class="city-node" style="animation-delay:.6s"/>
                <circle cx="310" cy="80" r="2.5" class="city-node" style="animation-delay:.2s"/>
                <circle cx="230" cy="200" r="2.5" class="city-node" style="animation-delay:.5s"/>
                <circle cx="170" cy="90" r="2.5" class="city-node" style="animation-delay:.8s"/>
                <circle cx="340" cy="185" r="2.5" class="city-node" style="animation-delay:.4s"/>
                <circle cx="250" cy="140" r="2" class="city-node" style="animation-delay:.1s"/>

                <!-- Connection lines -->
                <line x1="195" y1="118" x2="260" y2="95" class="conn-line"/>
                <line x1="260" y1="95" x2="355" y2="145" class="conn-line" style="animation-delay:-.3s"/>
                <line x1="355" y1="145" x2="310" y2="80" class="conn-line" style="animation-delay:-.5s"/>
                <line x1="310" y1="80" x2="195" y2="118" class="conn-line" style="animation-delay:-.7s"/>
                <line x1="195" y1="118" x2="230" y2="200" class="conn-line" style="animation-delay:-.2s"/>
                <line x1="260" y1="95" x2="170" y2="90" class="conn-line" style="animation-delay:-.4s"/>
                <line x1="355" y1="145" x2="340" y2="185" class="conn-line" style="animation-delay:-.6s"/>
                <line x1="310" y1="80" x2="340" y2="185" class="conn-line" style="animation-delay:-.1s"/>
                <line x1="230" y1="200" x2="340" y2="185" class="conn-line" style="animation-delay:-.8s"/>
                <line x1="170" y1="90" x2="250" y2="140" class="conn-line" style="animation-delay:-.3s"/>

                <!-- Flying data particles -->
                <circle cx="195" cy="118" r="2" class="particle" style="--dx:65px;--dy:-23px"/>
                <circle cx="260" cy="95" r="1.5" class="particle" style="--dx:-65px;--dy:23px"/>
                <circle cx="310" cy="80" r="2" class="particle" style="--dx:45px;--dy:65px"/>
                <circle cx="230" cy="200" r="1.5" class="particle" style="--dx:-35px;--dy:-82px"/>
                <circle cx="355" cy="145" r="2" class="particle" style="--dx:-45px;--dy:-65px"/>

                <!-- Labels -->
                <text x="195" y="108" text-anchor="middle" fill="rgba(199,210,254,.7)" font-size="8" font-family="sans-serif">🇺🇸</text>
                <text x="260" y="85" text-anchor="middle" fill="rgba(199,210,254,.7)" font-size="8" font-family="sans-serif">🇪🇺</text>
                <text x="355" y="136" text-anchor="middle" fill="rgba(199,210,254,.7)" font-size="8" font-family="sans-serif">🇸🇬</text>
                <text x="310" y="72" text-anchor="middle" fill="rgba(199,210,254,.7)" font-size="8" font-family="sans-serif">🇯🇵</text>
                <text x="230" y="192" text-anchor="middle" fill="rgba(199,210,254,.7)" font-size="8" font-family="sans-serif">🇧🇷</text>
              </svg>
            </div>
          </div>
          <div class="thumb-fade"></div>
        </div>
        <div class="card-body">
          <h3>国内外云服务节点测速</h3>
          <p class="desc">实时检测国内外主流云服务厂商各节点的延迟与速度，助你选择最优线路。</p>
          <div class="card-actions">
            <a href="https://ping.gaomeluo.com" target="_blank" class="btn btn-primary">立即使用</a>
            <a href="https://ping.gaomeluo.com/aliyun/" target="_blank" class="btn btn-sm">☁️ 阿里云</a>
            <a href="https://ping.gaomeluo.com/tengxun/" target="_blank" class="btn btn-sm">☁️ 腾讯云</a>
          </div>
        </div>
      </div>

      <!-- Card 2: 在线工具箱 -->
      <div class="card">
        <div class="card-thumb" style="background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 50%, #bae6fd 100%); display: flex; align-items: center; justify-content: center;">
          <span style="font-size: 64px;">🛠️</span>
        </div>
        <div class="card-body">
          <h3>在线工具箱</h3>
          <p class="desc">常用开发 & 日常工具集合，即开即用。</p>
          <div class="card-actions">
            <a href="https://tools.inurl.link/" target="_blank" class="btn btn-primary">立即使用</a>
          </div>
        </div>
      </div>

      <!-- Card 3: BT 种子搜索 -->
      <div class="card">
        <div class="card-thumb" style="background: linear-gradient(135deg, #faf5ff 0%, #f3e8ff 50%, #e9d5ff 100%); display: flex; align-items: center; justify-content: center;">
          <span style="font-size: 64px;">🔍</span>
        </div>
        <div class="card-body">
          <h3>BT 种子搜索</h3>
          <p class="desc">快速搜索 BT 种子资源。</p>
          <div class="card-actions">
            <a href="https://bt.inurl.link/" target="_blank" class="btn btn-primary">立即使用</a>
          </div>
        </div>
      </div>

      <!-- Card 4: GaoMeluo Blog -->
      <div class="card">
        <div class="card-thumb" style="background: linear-gradient(135deg, #fefce8 0%, #fef9c3 50%, #fef08a 100%); display: flex; align-items: center; justify-content: center;">
          <span style="font-size: 64px;">📝</span>
        </div>
        <div class="card-body">
          <h3>GaoMeluo Blog</h3>
          <p class="desc">技术文章 & 个人随想。</p>
          <div class="card-actions">
            <a href="https://blog.gaomeluo.com" target="_blank" class="btn btn-primary">立即访问</a>
          </div>
        </div>
      </div>

      <!-- Card 5: inurl.top -->
      <div class="card">
        <div class="card-thumb" style="background: linear-gradient(135deg, #f0fdf4 0%, #dcfce7 50%, #bbf7d0 100%); display: flex; align-items: center; justify-content: center;">
          <span style="font-size: 64px;">🌐</span>
        </div>
        <div class="card-body">
          <h3>inurl.top</h3>
          <p class="desc">另一个博客站点。</p>
          <div class="card-actions">
            <a href="https://inurl.top/" target="_blank" class="btn btn-primary">立即访问</a>
          </div>
        </div>
      </div>

    </div>
  </div>

  <!-- Footer -->
  <footer>
    <div class="footer-inner">
      <div class="footer-brand">
        <span class="fn">inurl.link</span>
        <p>工具集 & 博客导航</p>
      </div>
      <div class="footer-links">
        <div class="footer-group">
          <h4>工具</h4>
          <a href="https://ping.gaomeluo.com" target="_blank">云服务测速</a>
          <a href="https://tools.inurl.link/" target="_blank">在线工具箱</a>
          <a href="https://bt.inurl.link/" target="_blank">BT 搜索</a>
        </div>
        <div class="footer-group">
          <h4>博客</h4>
          <a href="https://blog.gaomeluo.com" target="_blank">GaoMeluo Blog</a>
          <a href="https://inurl.top/" target="_blank">inurl.top</a>
        </div>
        <div class="footer-group">
          <h4>链接</h4>
          <a href="https://github.com/gaomeluo" target="_blank">GitHub</a>
          <a href="/dashboard" target="_blank">短链接管理</a>
        </div>
      </div>
    </div>
    <div class="copy">&copy; 2026 inurl.link — Web is Cool, Web is Best.</div>
  </footer>
</template>

<style scoped>
*, *::before, *::after {
  box-sizing: border-box;
  border: 0 solid #e5e7eb;
  margin: 0;
  padding: 0;
}
html {
  line-height: 1.5;
  -webkit-text-size-adjust: 100%;
  font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body { background: #fff; color: #09090b; min-height: 100vh; }
a { color: inherit; text-decoration: inherit; }
img, svg { display: block; vertical-align: middle; max-width: 100%; height: auto; }

/* Nav */
header { border-bottom: 1px solid #e5e7eb; }
.nav-inner {
  max-width: 1280px; margin: 0 auto; padding: 0 24px;
  height: 64px; display: flex; align-items: center; justify-content: space-between;
}
.logo { display: flex; align-items: center; gap: 8px; font-size: 20px; font-weight: 800; color: #111827; }
.logo-icon { width: 32px; height: 32px; background: #111827; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: #fff; font-size: 14px; }
.logo-dot { color: #4f46e5; }
.nav-links { display: flex; align-items: center; gap: 10px; }
.nav-btn {
  display: inline-flex; align-items: center; gap: 5px;
  padding: 7px 16px; font-size: 14px; font-weight: 500;
  color: #374151; border-radius: 6px;
  transition: background .15s; cursor: pointer; background: none;
}
.nav-btn:hover { background: #f3f4f6; }
.nav-btn svg { width: 16px; height: 16px; }
.nav-btn-primary {
  display: inline-flex; align-items: center; gap: 5px;
  padding: 8px 20px; font-size: 14px; font-weight: 500;
  color: #fff; background: #111827; border-radius: 9999px;
  transition: background .15s;
}
.nav-btn-primary:hover { background: #1f2937; }

/* Main grid */
.container { max-width: 1280px; margin: 0 auto; padding: 32px 24px 64px; }
.grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; grid-auto-rows: min-content; }

/* Card */
.card {
  position: relative; display: flex; flex-direction: column;
  justify-content: space-between; overflow: hidden; border-radius: 12px;
  background: #fff;
  box-shadow: 0 0 0 1px rgba(0,0,0,.03), 0 2px 4px rgba(0,0,0,.05), 0 12px 24px rgba(0,0,0,.05);
  transition: transform .3s, box-shadow .3s;
}
.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 0 0 1px rgba(0,0,0,.04), 0 4px 8px rgba(0,0,0,.06), 0 20px 40px rgba(0,0,0,.08);
}
.card.featured { grid-column: span 2; }
.card-thumb { position: relative; width: 100%; height: 260px; overflow: hidden; }
.card-thumb img { width: 100%; height: 100%; object-fit: cover; }
.thumb-fade {
  position: absolute; bottom: 0; left: 0; right: 0; height: 50%;
  background: linear-gradient(to top, #fff, #fff 30%, transparent);
  pointer-events: none;
}
.card-body {
  position: relative; z-index: 10;
  padding: 20px 24px 24px; margin-top: -80px;
  transition: transform .3s;
}
.card:hover .card-body { transform: translateY(-8px); }
.card-body h3 { font-size: 20px; font-weight: 600; color: #404040; margin-bottom: 4px; }
.card-body .desc { font-size: 14px; color: #737373; margin-bottom: 12px; line-height: 1.6; }
.card-actions { display: flex; gap: 8px; flex-wrap: wrap; }
.btn {
  display: inline-flex; align-items: center; gap: 4px;
  padding: 6px 14px; font-size: 13px; font-weight: 500;
  border-radius: 6px; border: 1px solid #d4d4d8;
  color: #18181b; background: #fff; transition: all .15s; cursor: pointer;
}
.btn:hover { background: #f4f4f5; border-color: #a1a1aa; }
.btn-primary { background: #111827; color: #fff; border-color: #111827; }
.btn-primary:hover { background: #1f2937; border-color: #1f2937; }
.btn-sm { padding: 4px 10px; font-size: 12px; }

/* CDN Globe Animation */
.cdn-thumb {
  width: 100%; height: 100%;
  background: linear-gradient(135deg, #0a0a2e 0%, #1a1040 30%, #0d1b3e 60%, #0a0a2e 100%);
  position: relative;
}
.cdn-globe {
  position: absolute; inset: 0;
  display: flex; align-items: center; justify-content: center;
}
.cdn-globe svg { width: 90%; height: 90%; }
.pulse-ring {
  fill: none;
  stroke: rgba(99, 102, 241, 0.3);
  stroke-width: 1;
  animation: pulseRing 3s ease-in-out infinite;
}
.pulse-ring:nth-child(2) { animation-delay: 0s; }
.pulse-ring:nth-child(3) { animation-delay: 1s; }
.pulse-ring:nth-child(4) { animation-delay: 2s; }
@keyframes pulseRing {
  0% { r: 8; opacity: 1; }
  100% { r: 140; opacity: 0; }
}
.orbit-dot {
  fill: #818cf8;
  animation: orbit 8s linear infinite;
  filter: drop-shadow(0 0 4px rgba(129,140,248,.8));
}
.orbit-dot:nth-child(5) { animation-delay: 0s; }
.orbit-dot:nth-child(6) { animation-delay: -4s; }
@keyframes orbit {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.particle {
  fill: #a78bfa;
  animation: particleFly 2.5s ease-in-out infinite;
  opacity: 0;
  filter: drop-shadow(0 0 3px rgba(167,139,250,.6));
}
.particle:nth-child(7) { animation-delay: 0s; }
.particle:nth-child(8) { animation-delay: .5s; }
.particle:nth-child(9) { animation-delay: 1s; }
.particle:nth-child(10) { animation-delay: 1.5s; }
.particle:nth-child(11) { animation-delay: 2s; }
@keyframes particleFly {
  0% { opacity: 0; transform: translate(0,0); }
  30% { opacity: 1; }
  70% { opacity: 1; }
  100% { opacity: 0; transform: translate(var(--dx), var(--dy)); }
}
.city-node {
  fill: #6366f1;
  animation: nodeBlink 2s ease-in-out infinite;
  filter: drop-shadow(0 0 3px rgba(99,102,241,.6));
}
@keyframes nodeBlink {
  0%,100% { opacity: .5; }
  50% { opacity: 1; }
}
.conn-line {
  stroke: rgba(99,102,241,.2);
  stroke-width: .5;
  stroke-dasharray: 4 6;
  animation: dashFlow .8s linear infinite;
}
@keyframes dashFlow {
  0% { stroke-dashoffset: 0; }
  100% { stroke-dashoffset: -20; }
}

/* Footer */
footer { border-top: 1px solid #e5e7eb; padding: 40px 24px; }
.footer-inner {
  max-width: 1280px; margin: 0 auto;
  display: flex; flex-wrap: wrap; justify-content: space-between;
  align-items: flex-start; gap: 32px;
}
.footer-brand { font-size: 14px; color: #6b7280; }
.footer-brand .fn { font-weight: 700; color: #111827; }
.footer-links { display: flex; flex-wrap: wrap; gap: 24px; }
.footer-group h4 {
  font-size: 12px; font-weight: 600; text-transform: uppercase;
  letter-spacing: .5px; color: #9ca3af; margin-bottom: 8px;
}
.footer-group a { display: block; font-size: 14px; color: #6b7280; padding: 2px 0; transition: color .15s; }
.footer-group a:hover { color: #111827; }
footer .copy {
  max-width: 1280px; margin: 24px auto 0; padding-top: 16px;
  border-top: 1px solid #f3f4f6; font-size: 13px;
  color: #9ca3af; text-align: center;
}

/* Responsive */
@media (max-width: 900px) {
  .grid { grid-template-columns: repeat(2, 1fr); }
  .card.featured { grid-column: span 2; }
  .card-thumb { height: 200px; }
}
@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
  .card.featured { grid-column: span 1; }
  .card-thumb { height: 180px; }
  .nav-inner { padding: 0 16px; flex-wrap: wrap; height: auto; gap: 10px; padding-top: 12px; padding-bottom: 12px; }
  .container { padding: 24px 16px 48px; }
  .footer-inner { flex-direction: column; gap: 20px; }
}
</style>
