<script setup lang="ts">
definePageMeta({
  layout: 'landing',
})

const mobileMenuOpen = ref(false)
const toggleMenu = () => { mobileMenuOpen.value = !mobileMenuOpen.value }
</script>

<template>
  <!-- Header -->
  <header class="site-header">
    <div class="header-inner">
      <a href="/" class="logo">
        <svg width="30" height="30" viewBox="0 0 30 30" fill="none">
          <defs><linearGradient id="lgGradH" x1="0" y1="0" x2="30" y2="30"><stop stop-color="#2EA7FF"/><stop offset="1" stop-color="#9381FF"/></linearGradient></defs>
          <circle cx="15" cy="15" r="14" stroke="url(#lgGradH)" stroke-width="2"/>
          <path d="M9 12C9 9.79086 10.7909 8 13 8H17C19.2091 8 21 9.79086 21 12V18C21 20.2091 19.2091 22 17 22H13C10.7909 22 9 20.2091 9 18V12Z" stroke="url(#lgGradH)" stroke-width="2"/>
          <path d="M13 8V22" stroke="url(#lgGradH)" stroke-width="1.5"/>
        </svg>
        <span class="logo-text">inurl.link</span>
      </a>
      <nav class="nav-links">
        <a href="/" class="active">首页</a>
        <a href="https://api.inurl.link/" target="_blank">API Token大全</a>
        <a href="https://tools.inurl.link/" target="_blank">工具箱</a>
        <a href="https://ping.gaomeluo.com/" target="_blank">云测速</a>
        <a href="https://blog.gaomeluo.com/" target="_blank">博客</a>
        <a href="https://www.lixiaoxin.com" target="_blank">关于</a>
      </nav>
      <a href="https://inurl.link/dashboard" class="btn-primary" target="_blank">短连接</a>
      <button class="hamburger" :class="{ active: mobileMenuOpen }" @click="toggleMenu" aria-label="菜单">
        <span></span><span></span><span></span>
      </button>
    </div>
    <nav class="mobile-menu" :class="{ active: mobileMenuOpen }">
      <a href="/" :class="{ active: true }" @click="toggleMenu">首页</a>
      <a href="https://api.inurl.link/" target="_blank" @click="toggleMenu">API Token大全</a>
      <a href="https://tools.inurl.link/" target="_blank" @click="toggleMenu">工具箱</a>
      <a href="https://ping.gaomeluo.com/" target="_blank" @click="toggleMenu">云测速</a>
      <a href="https://blog.gaomeluo.com/" target="_blank" @click="toggleMenu">博客</a>
      <a href="https://www.lixiaoxin.com" target="_blank" @click="toggleMenu">关于</a>
      <a href="https://inurl.link/dashboard" class="btn-primary" target="_blank" @click="toggleMenu">短连接</a>
    </nav>
  </header>

  <!-- Hero -->
  <section class="hero">
    <div class="badge">
      <span class="badge-dot"></span>
      <span class="badge-text">inurl.link · 你的互联网入口</span>
    </div>
    <h1 class="hero-title">发现 · 连接 · 探索</h1>
  </section>

  <!-- 核心服务 -->
  <section class="section">
    <div class="section-header">
      <span class="section-tag cyan">CORE SERVICES</span>
      <h2 class="section-title">一站式精选服务</h2>
      <p class="section-sub">云端测速、在线工具、资源搜索，核心功能应有尽有</p>
    </div>
    <div class="service-grid">
      <div class="svc-card">
        <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#2EA7FF" fill-opacity="0.15"/><path d="M22 10C19.5147 10 17.5 12.0147 17.5 14.5V19.5C17.5 21.9853 19.5147 24 22 24C24.4853 24 26.5 24 26.5 24" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round"/><path d="M22 16L22 13" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round"/><path d="M25 18L28 18" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round"/><path d="M25 22L28 22" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round"/><path d="M22 28L22 30" stroke="#2EA7FF" stroke-width="2" stroke-linecap="round"/><circle cx="32" cy="20" r="2" fill="#2EA7FF"/></svg>
        <h3>云服务节点测速</h3>
        <p>实时检测阿里云、腾讯云等主流厂商各节点延迟与速度，助你选择最优线路</p>
        <div class="svc-bottom-links">
          <a href="https://ping.gaomeluo.com/aliyun/" class="svc-sub-link" target="_blank">阿里云</a>
          <a href="https://ping.gaomeluo.com/tengxun/" class="svc-sub-link" target="_blank">腾讯云</a>
          <a href="https://ping.gaomeluo.com/" class="svc-link svc-link-cyan" target="_blank">立即使用 →</a>
        </div>
      </div>
      <a href="https://tools.inurl.link/" class="svc-card" target="_blank">
        <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#9381FF" fill-opacity="0.15"/><path d="M12 15L18 21L12 27" stroke="#9381FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M32 10V20C32 26.6274 26.6274 32 20 32" stroke="#9381FF" stroke-width="2" stroke-linecap="round"/><circle cx="32" cy="12" r="1.5" fill="#9381FF"/></svg>
        <h3>在线工具箱</h3>
        <p>常用开发 & 日常工具集合，即开即用，无需安装</p>
        <span class="svc-link svc-link-purple">立即使用 →</span>
      </a>
      <a href="https://api.inurl.link/" class="svc-card" target="_blank">
        <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#13DDC4" fill-opacity="0.15"/><circle cx="19" cy="19" r="7" stroke="#13DDC4" stroke-width="2"/><path d="M24 24L30 30" stroke="#13DDC4" stroke-width="2" stroke-linecap="round"/><path d="M14 19H24" stroke="#13DDC4" stroke-width="2" stroke-linecap="round"/></svg>
        <h3>API Token大全</h3>
        <p>国内外免费 / 付费 API 一站式收录 · 性价比对比 · 擅长领域导航</p>
        <span class="svc-link svc-link-green">立即使用 →</span>
      </a>
      <a href="https://blog.gaomeluo.com/" class="svc-card" target="_blank">
        <svg class="svc-icon" viewBox="0 0 44 44" fill="none"><rect width="44" height="44" rx="12" fill="#2EA7FF" fill-opacity="0.08"/><path d="M14 12H26C27.1046 12 28 12.8954 28 14V30C28 31.1046 27.1046 32 26 32H14C12.8954 32 12 31.1046 12 30V14C12 12.8954 12.8954 12 14 12Z" stroke="#AABBCC" stroke-width="1.5"/><path d="M16 18H24" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round"/><path d="M16 22H24" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round"/><path d="M16 26H20" stroke="#AABBCC" stroke-width="1.5" stroke-linecap="round"/></svg>
        <h3>技术博客</h3>
        <p>GaoMeluo Blog · inurl.top，技术文章与个人随想</p>
        <span class="svc-link svc-link-white">立即访问 →</span>
      </a>
    </div>
  </section>

  <!-- 更多资源 -->
  <section class="section">
    <div class="section-header">
      <span class="section-tag purple">MORE RESOURCES</span>
      <h2 class="section-title">更多精选资源</h2>
      <p class="section-sub">按类别整理，涵盖开发、效率、娱乐、学习多场景工具</p>
    </div>
    <div class="feat-grid">
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:#2EA7FF"></div>
        <div class="feat-text"><h4>JSON 格式化</h4><p>在线 JSON 解析、校验、压缩、转换</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:#9381FF"></div>
        <div class="feat-text"><h4>Base64 编解码</h4><p>文本、图片、文件的 Base64 转换</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:#13DDC4"></div>
        <div class="feat-text"><h4>UUID 生成器</h4><p>批量生成 UUID v1/v4/v5，校验有效性</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:#FFB454"></div>
        <div class="feat-text"><h4>时间戳转换</h4><p>Unix 时间戳与日期互转，支持多时区</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:#FF4D6D"></div>
        <div class="feat-text"><h4>正则表达式测试</h4><p>在线测试正则匹配，支持语法高亮</p></div>
        <span class="feat-arrow">→</span>
      </a>
      <a href="https://tools.inurl.link/" class="feat-item" target="_blank">
        <div class="feat-icon" style="background:linear-gradient(135deg,#2EA7FF,#13DDC4)"></div>
        <div class="feat-text"><h4>二维码生成</h4><p>生成或解析二维码，支持自定义颜色 logo</p></div>
        <span class="feat-arrow">→</span>
      </a>
    </div>
  </section>

  <!-- 数据展示 -->
  <section class="section">
    <div class="section-header">
      <span class="section-tag green">LIVE STATS</span>
      <h2 class="section-title">平台实时数据</h2>
      <p class="section-sub">数据每 5 分钟自动更新，反映服务真实运行状态</p>
    </div>
    <div class="stat-grid">
      <div class="stat-card stat-cyan">
        <span class="stat-label">服务节点数</span>
        <span class="stat-value">128</span>
        <span class="stat-meta up">↑ 12 本周新增</span>
      </div>
      <div class="stat-card stat-purple">
        <span class="stat-label">在线工具数</span>
        <span class="stat-value">56</span>
        <span class="stat-meta up">↑ 3 本周新增</span>
      </div>
      <div class="stat-card stat-green">
        <span class="stat-label">日访问用户</span>
        <span class="stat-value">8.2K</span>
        <span class="stat-meta up">↑ 18% 较上周</span>
      </div>
      <div class="stat-card stat-amber">
        <span class="stat-label">服务可用率</span>
        <span class="stat-value">99.9%</span>
        <span class="stat-meta">SLA 持续保障</span>
      </div>
    </div>
  </section>

  <!-- 最新文章 -->
  <section class="section">
    <div class="section-header-row">
      <div class="header-left">
        <span class="section-tag cyan">LATEST POSTS</span>
        <h2 class="section-title section-title-left">最新文章</h2>
      </div>
      <a href="https://blog.gaomeluo.com/" class="btn-outline" target="_blank">查看全部 →</a>
    </div>
    <div class="article-grid">
      <a href="https://inurl.top/archives/ClashforOpenWRT/" class="art-card" target="_blank">
        <div class="art-img art-thumb-router">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a2e"/>
            <circle cx="160" cy="90" r="80" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.15"/>
            <circle cx="160" cy="90" r="50" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.2"/>
            <rect x="120" y="65" width="80" height="50" rx="8" fill="#2EA7FF" opacity="0.3"/>
            <rect x="130" y="72" width="60" height="8" rx="2" fill="#2EA7FF" opacity="0.6"/>
            <rect x="130" y="84" width="40" height="6" rx="2" fill="#2EA7FF" opacity="0.4"/>
            <rect x="130" y="94" width="50" height="6" rx="2" fill="#2EA7FF" opacity="0.4"/>
            <path d="M200 70 Q220 50 240 60" stroke="#2EA7FF" stroke-width="2" fill="none" opacity="0.6"/>
            <path d="M205 78 Q230 62 250 70" stroke="#9381FF" stroke-width="1.5" fill="none" opacity="0.5"/>
            <path d="M210 86 Q235 74 255 80" stroke="#13DDC4" stroke-width="1.5" fill="none" opacity="0.4"/>
            <circle cx="155" cy="100" r="3" fill="#13DDC4" opacity="0.9"/><circle cx="165" cy="100" r="3" fill="#13DDC4" opacity="0.5"/>
            <text x="160" y="160" text-anchor="middle" fill="#2EA7FF" font-size="11" font-family="sans-serif" opacity="0.6">OpenWRT · Clash</text>
          </svg>
        </div>
        <div class="art-content">
          <span class="art-tag cyan">软路由</span>
          <h4>Clash for OpenWRT 实现全屋设备科学上网</h4>
          <p>从机场选择、软路由推荐到 OpenWRT 刷机、OpenClash 配置，全流程手把手教程</p>
          <div class="art-meta"><span>2023-05-17</span><span>10 分钟</span></div>
        </div>
      </a>
      <a href="https://inurl.top/archives/datizi/" class="art-card" target="_blank">
        <div class="art-img art-thumb-vpn">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a24"/>
            <circle cx="160" cy="90" r="70" fill="none" stroke="#9381FF" stroke-width="1" opacity="0.1"/>
            <circle cx="160" cy="90" r="40" fill="none" stroke="#9381FF" stroke-width="1" opacity="0.15" stroke-dasharray="4 4"/>
            <path d="M120 110 L135 80 L155 95 L175 65 L195 85 L200 70" stroke="#9381FF" stroke-width="2.5" fill="none" stroke-linecap="round" stroke-linejoin="round" opacity="0.9"/>
            <rect x="135" y="105" width="50" height="35" rx="6" fill="#13DDC4" opacity="0.2" stroke="#13DDC4" stroke-width="1.5"/>
            <rect x="140" y="112" width="18" height="18" rx="4" fill="#13DDC4" opacity="0.5"/>
            <rect x="162" y="112" width="18" height="18" rx="4" fill="#13DDC4" opacity="0.3"/>
            <text x="160" y="162" text-anchor="middle" fill="#9381FF" font-size="11" font-family="sans-serif" opacity="0.6">V2ray · VPN</text>
          </svg>
        </div>
        <div class="art-content">
          <span class="art-tag purple">教程</span>
          <h4>如何搭建梯子（VPN）？</h4>
          <p>自己搭建 V2ray 梯子，独享高速线路。从服务器选择到一键脚本部署，一步到位</p>
          <div class="art-meta"><span>2023-08-18</span><span>12 分钟</span></div>
        </div>
      </a>
      <a href="https://blog.gaomeluo.com/archives/wan-OpenClaw/" class="art-card" target="_blank">
        <div class="art-img art-thumb-claw">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a20"/>
            <circle cx="160" cy="75" r="45" fill="none" stroke="#2EA7FF" stroke-width="1" opacity="0.1"/>
            <circle cx="160" cy="75" r="25" fill="#2EA7FF" opacity="0.08"/>
            <path d="M160 50 Q175 30 195 35 Q200 40 195 50 Q185 55 170 55" fill="#2EA7FF" opacity="0.3"/>
            <path d="M160 50 Q145 30 125 35 Q120 40 125 50 Q135 55 150 55" fill="#2EA7FF" opacity="0.3"/>
            <rect x="145" y="55" width="30" height="35" rx="6" fill="#9381FF" opacity="0.25"/>
            <circle cx="155" cy="68" r="4" fill="#2EA7FF" opacity="0.8"/><circle cx="165" cy="68" r="4" fill="#2EA7FF" opacity="0.8"/>
            <rect x="148" y="76" width="24" height="4" rx="2" fill="#2EA7FF" opacity="0.5"/>
            <path d="M155 100 Q140 125 120 130" stroke="#13DDC4" stroke-width="2" fill="none" opacity="0.5"/>
            <path d="M165 100 Q180 125 200 130" stroke="#13DDC4" stroke-width="2" fill="none" opacity="0.5"/>
            <circle cx="120" cy="130" r="3" fill="#13DDC4" opacity="0.6"/><circle cx="200" cy="130" r="3" fill="#13DDC4" opacity="0.6"/>
            <text x="160" y="162" text-anchor="middle" fill="#9381FF" font-size="11" font-family="sans-serif" opacity="0.6">OpenClaw · AI Agent</text>
          </svg>
        </div>
        <div class="art-content">
          <span class="art-tag green">OpenClaw</span>
          <h4>玩转 OpenClaw丨手把手教程合辑</h4>
          <p>从部署到接入微信/QQ/飞书，再到投资 Agent 实战——全场景教程汇总</p>
          <div class="art-meta"><span>2026-06-09</span><span>8 分钟</span></div>
        </div>
      </a>
      <a href="https://blog.gaomeluo.com/archives/mianfeiziti/" class="art-card" target="_blank">
        <div class="art-img art-thumb-font">
          <svg viewBox="0 0 320 180" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="320" height="180" fill="#0a0a1e"/>
            <circle cx="160" cy="80" r="60" fill="none" stroke="#FFB454" stroke-width="1" opacity="0.1"/>
            <circle cx="160" cy="80" r="35" fill="none" stroke="#FFB454" stroke-width="1" opacity="0.15" stroke-dasharray="3 3"/>
            <text x="120" y="65" fill="#FFB454" font-size="36" font-weight="700" font-family="serif" opacity="0.8">A</text>
            <text x="150" y="90" fill="#FF4D6D" font-size="28" font-weight="600" font-family="serif" opacity="0.7" transform="rotate(-8,150,90)">a</text>
            <text x="180" y="70" fill="#2EA7FF" font-size="22" font-weight="500" font-family="sans-serif" opacity="0.6" transform="rotate(5,180,70)">字</text>
            <text x="210" y="85" fill="#13DDC4" font-size="18" font-weight="400" font-family="sans-serif" opacity="0.5" transform="rotate(12,210,85)">体</text>
            <text x="130" y="110" fill="#FFFFFF" font-size="14" font-weight="600" font-family="sans-serif" opacity="0.3">Free · 商用 · 可下载</text>
            <text x="160" y="162" text-anchor="middle" fill="#FFB454" font-size="11" font-family="sans-serif" opacity="0.6">Typography Resources</text>
          </svg>
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

  <!-- CTA -->
  <section class="section">
    <div class="cta-box">
      <h2>推荐你常用的工具与服务</h2>
      <p>帮助更多人发现优质资源，一起构建更好的互联网导航生态</p>
      <div class="cta-buttons">
        <a href="https://inurl.link/dashboard" class="btn-cta-primary" target="_blank">短连接 →</a>
        <a href="https://www.lixiaoxin.com" class="btn-cta-secondary" target="_blank">联系作者</a>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="site-footer">
    <div class="footer-top">
      <div class="footer-brand">
        <div class="logo" style="margin-bottom:4px">
          <svg width="30" height="30" viewBox="0 0 30 30" fill="none">
            <defs><linearGradient id="lgGradF" x1="0" y1="0" x2="30" y2="30"><stop stop-color="#2EA7FF"/><stop offset="1" stop-color="#9381FF"/></linearGradient></defs>
            <circle cx="15" cy="15" r="14" stroke="url(#lgGradF)" stroke-width="2"/>
            <path d="M9 12C9 9.79086 10.7909 8 13 8H17C19.2091 8 21 9.79086 21 12V18C21 20.2091 19.2091 22 17 22H13C10.7909 22 9 20.2091 9 18V12Z" stroke="url(#lgGradF)" stroke-width="2"/>
            <path d="M13 8V22" stroke="url(#lgGradF)" stroke-width="1.5"/>
          </svg>
          <span class="logo-text">inurl.link</span>
        </div>
        <p>互联网精选导航 · 让每一次访问都更有方向</p>
      </div>
      <div class="footer-col">
        <h4>服务</h4>
        <a href="https://ping.gaomeluo.com/" target="_blank">云服务测速</a>
        <a href="https://tools.inurl.link/" target="_blank">在线工具箱</a>
        <a href="https://api.inurl.link/" target="_blank">API Token 大全</a>
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
      <span>&copy; 2026 inurl.link · All systems operational</span>
      <div class="footer-status">
        <span class="status-dot"></span>
        <span class="status-text">All systems operational</span>
      </div>
    </div>
  </footer>
</template>

<style scoped>
/* ===== Header ===== */
.site-header { position: sticky; top: 0; z-index: 100; border-bottom: 1px solid var(--border-subtle); }
.header-inner {
  max-width: 1440px; margin: 0 auto; height: 64px; display: flex; align-items: center;
  justify-content: space-between; padding: 0 clamp(16px, 4vw, 80px);
  background: rgba(3,0,14,0.85); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);
}
.logo { display: flex; align-items: center; gap: 10px; text-decoration: none; }
.logo-text { font-size: clamp(17px, 2vw, 20px); font-weight: 700; color: var(--text-primary); }
.nav-links { display: flex; gap: clamp(16px, 3vw, 40px); align-items: center; }
.nav-links a { font-size: 14px; font-weight: 500; color: var(--text-secondary); text-decoration: none; white-space: nowrap; transition: color .15s; }
.nav-links a:hover, .nav-links a.active { color: var(--text-primary); }

.btn-primary {
  display: inline-flex; align-items: center; gap: 8px; flex-shrink: 0;
  padding: 10px 24px; border-radius: var(--radius-btn);
  background: var(--accent-cyan); color: #fff; font-size: 14px; font-weight: 600; text-decoration: none;
  transition: box-shadow .15s;
}
.btn-primary:hover { box-shadow: var(--shadow-glow-cyan); }

/* Hamburger */
.hamburger { display: none; flex-direction: column; gap: 5px; background: none; border: none; cursor: pointer; padding: 4px; z-index: 110; }
.hamburger span { display: block; width: 24px; height: 2px; background: var(--text-primary); border-radius: 2px; transition: all .3s; }
.hamburger.active span:nth-child(1) { transform: rotate(45deg) translate(5px,5px); }
.hamburger.active span:nth-child(2) { opacity: 0; }
.hamburger.active span:nth-child(3) { transform: rotate(-45deg) translate(5px,-5px); }
.mobile-menu {
  display: none; position: fixed; top: 64px; left: 0; right: 0; bottom: 0;
  background: rgba(3,0,14,0.98); backdrop-filter: blur(20px);
  flex-direction: column; padding: 32px clamp(20px, 5vw, 40px); gap: 8px; z-index: 99;
}
.mobile-menu.active { display: flex; }
.mobile-menu a { display: block; padding: 14px 0; font-size: 18px; font-weight: 500; color: var(--text-secondary); text-decoration: none; border-bottom: 1px solid var(--border-subtle); transition: color .15s; }
.mobile-menu a:hover, .mobile-menu a.active { color: var(--text-primary); }
.mobile-menu .btn-primary { margin-top: 16px; text-align: center; justify-content: center; }

/* ===== Hero ===== */
.hero {
  position: relative; overflow: hidden;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  padding: clamp(80px, 10vw, 180px) clamp(16px, 5vw, 80px) clamp(80px, 10vw, 160px);
  gap: clamp(20px, 3vw, 32px);
}
.hero::before {
  content: ''; position: absolute; top: -15%; left: 50%; transform: translateX(-50%);
  width: clamp(500px, 80vw, 1000px); height: clamp(500px, 80vw, 1000px);
  background:
    radial-gradient(ellipse 60% 50% at 50% 45%, rgba(46,167,255,0.25) 0%, transparent 60%),
    radial-gradient(ellipse 40% 35% at 35% 55%, rgba(147,129,255,0.18) 0%, transparent 55%),
    radial-gradient(ellipse 35% 30% at 65% 50%, rgba(19,221,196,0.12) 0%, transparent 50%);
  pointer-events: none; z-index: 0;
}
.hero > * { position: relative; z-index: 1; }
.badge { display: inline-flex; align-items: center; gap: 8px; padding: 6px 18px; border-radius: var(--radius-btn); background: var(--accent-green); }
.badge-dot { width: 8px; height: 8px; border-radius: 50%; background: #fff; flex-shrink: 0; }
.badge-text { font-size: clamp(12px, 1.4vw, 13px); font-weight: 600; color: var(--bg-primary); font-family: var(--font-cjk); white-space: nowrap; }
.hero-title { font-size: clamp(32px, 6vw, 72px); font-weight: 700; letter-spacing: -0.03em; text-align: center; font-family: var(--font-en); line-height: 1.1; }

/* ===== Sections ===== */
.section { width: 100%; max-width: 1440px; margin: 0 auto; display: flex; flex-direction: column; align-items: center; padding: clamp(40px, 6vw, 80px) clamp(16px, 5vw, 80px); gap: clamp(32px, 4vw, 48px); }
.section-header { display: flex; flex-direction: column; align-items: center; gap: 12px; }
.section-tag { font-size: 11px; font-weight: 600; letter-spacing: 3px; text-transform: uppercase; font-family: var(--font-en); }
.section-tag.cyan { color: var(--accent-cyan); }
.section-tag.purple { color: var(--accent-purple); }
.section-tag.green { color: var(--accent-green); }
.section-title { font-size: clamp(26px, 3.5vw, 40px); font-weight: 700; letter-spacing: -0.5px; text-align: center; font-family: var(--font-cjk); }
.section-sub { font-size: clamp(14px, 1.4vw, 16px); color: var(--text-secondary); text-align: center; max-width: 600px; font-family: var(--font-cjk); padding: 0 16px; }
.section-header-row { display: flex; justify-content: space-between; align-items: flex-end; width: 100%; max-width: 1280px; flex-wrap: wrap; gap: 16px; }
.header-left { display: flex; flex-direction: column; gap: 12px; }
.section-title-left { text-align: left; }

/* ===== Service Cards ===== */
.service-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 20px; width: 100%; max-width: 1280px; }
.svc-card {
  background: var(--bg-card); border: 1px solid var(--border-subtle);
  border-radius: var(--radius-card); padding: clamp(20px, 3vw, 28px);
  display: flex; flex-direction: column; gap: clamp(10px, 2vw, 16px);
  text-decoration: none; color: inherit; min-height: 280px;
  transition: transform .2s, box-shadow .2s;
}
.svc-card:hover { transform: translateY(-4px); }
.svc-card:nth-child(1):hover { box-shadow: var(--shadow-glow-cyan); }
.svc-card:nth-child(2):hover { box-shadow: var(--shadow-glow-purple); }
.svc-card:nth-child(3):hover { box-shadow: var(--shadow-glow-green); }
.svc-card:nth-child(4):hover { box-shadow: 0 0 30px rgba(255,255,255,0.15); }
.svc-icon { width: 44px; height: 44px; border-radius: 12px; flex-shrink: 0; }
.svc-card h3 { font-size: clamp(16px, 1.6vw, 20px); font-weight: 700; font-family: var(--font-cjk); }
.svc-card p { font-size: clamp(12px, 1.1vw, 14px); color: var(--text-secondary); flex: 1; font-family: var(--font-cjk); line-height: 1.6; }
.svc-card > :last-child { margin-top: auto; }
.svc-sub-link {
  padding: 5px 12px; border-radius: 8px; font-size: 12px; font-weight: 500;
  background: rgba(255,255,255,0.06); border: 1px solid var(--border-subtle);
  color: var(--accent-cyan); text-decoration: none; font-family: var(--font-cjk);
}
.svc-bottom-links { display: flex; gap: 8px; align-items: center; flex-wrap: wrap; }
.svc-link {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 8px 16px; border-radius: var(--radius-btn);
  font-size: 13px; font-weight: 600; text-decoration: none; font-family: var(--font-cjk);
}
.svc-link-cyan { background: rgba(46,167,255,0.15); color: var(--accent-cyan); }
.svc-link-purple { background: rgba(147,129,255,0.15); color: var(--accent-purple); }
.svc-link-green { background: rgba(19,221,196,0.15); color: var(--accent-green); }
.svc-link-white { background: rgba(255,255,255,0.08); color: var(--text-primary); }

/* ===== Feature Grid ===== */
.feat-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; width: 100%; max-width: 1280px; }
.feat-item {
  display: flex; align-items: center; gap: clamp(12px, 2vw, 16px); padding: clamp(14px, 2vw, 20px);
  background: rgba(255,255,255,0.03); border: 1px solid var(--border-subtle);
  border-radius: 14px; text-decoration: none; color: inherit; transition: background .15s;
}
.feat-item:hover { background: rgba(255,255,255,0.06); }
.feat-icon { width: clamp(36px, 5vw, 44px); height: clamp(36px, 5vw, 44px); border-radius: 10px; flex-shrink: 0; }
.feat-text { display: flex; flex-direction: column; gap: 4px; flex: 1; min-width: 0; }
.feat-text h4 { font-size: clamp(13px, 1.3vw, 15px); font-weight: 600; font-family: var(--font-cjk); }
.feat-text p { font-size: clamp(11px, 1vw, 12px); color: var(--text-tertiary); font-family: var(--font-cjk); }
.feat-arrow { color: rgba(255,255,255,0.3); font-size: 16px; font-weight: 700; flex-shrink: 0; }

/* ===== Stats ===== */
.stat-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; width: 100%; max-width: 1280px; }
.stat-card { padding: clamp(18px, 2.5vw, 24px); background: var(--bg-card); border: 1px solid var(--border-subtle); border-radius: var(--radius-card); display: flex; flex-direction: column; gap: clamp(8px, 1.5vw, 12px); }
.stat-label { font-size: clamp(11px, 1.1vw, 13px); font-weight: 500; color: var(--text-secondary); font-family: var(--font-cjk); }
.stat-value { font-size: clamp(28px, 3.5vw, 44px); font-weight: 700; line-height: 1; letter-spacing: -1.5px; font-family: var(--font-en); }
.stat-meta { font-size: clamp(10px, 0.9vw, 12px); font-weight: 500; font-family: var(--font-cjk); color: var(--text-tertiary); }
.stat-meta.up { color: var(--accent-green); }
.stat-cyan .stat-value { color: var(--accent-cyan); }
.stat-purple .stat-value { color: var(--accent-purple); }
.stat-green .stat-value { color: var(--accent-green); }
.stat-amber .stat-value { color: var(--accent-amber); }

/* ===== Articles ===== */
.article-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 20px; width: 100%; max-width: 1280px; }
.art-card { border-radius: var(--radius-card); background: var(--bg-card); border: 1px solid var(--border-subtle); overflow: hidden; text-decoration: none; color: inherit; transition: transform .2s; }
.art-card:hover { transform: translateY(-4px); }
.art-img { width: 100%; aspect-ratio: 16/9; object-fit: cover; overflow: hidden; display: flex; align-items: center; justify-content: center; }
.art-img svg { width: 100%; height: 100%; display: block; }
.art-content { padding: clamp(14px, 2vw, 20px); display: flex; flex-direction: column; gap: 10px; }
.art-tag { display: inline-block; align-self: flex-start; padding: 3px 10px; border-radius: 6px; font-size: 11px; font-weight: 600; font-family: var(--font-cjk); }
.art-tag.cyan { background: var(--accent-cyan); color: #fff; }
.art-tag.purple { background: var(--accent-purple); color: #fff; }
.art-tag.green { background: var(--accent-green); color: var(--bg-primary); }
.art-tag.amber { background: var(--accent-amber); color: var(--bg-primary); }
.art-content h4 { font-size: 16px; font-weight: 700; font-family: var(--font-cjk); }
.art-content p { font-size: 13px; color: var(--text-secondary); font-family: var(--font-cjk); }
.art-meta { display: flex; justify-content: space-between; align-items: center; }
.art-meta span { font-size: 11px; color: rgba(255,255,255,0.4); font-family: var(--font-en); }
.btn-outline {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 10px 20px; border-radius: var(--radius-btn);
  background: rgba(255,255,255,0.06); border: 1px solid var(--border-subtle);
  color: var(--text-primary); font-size: 13px; font-weight: 500;
  text-decoration: none; font-family: var(--font-cjk); transition: background .15s;
}
.btn-outline:hover { background: rgba(255,255,255,0.12); }

/* ===== CTA ===== */
.cta-box {
  width: 100%; max-width: 1280px;
  padding: clamp(40px, 6vw, 80px) clamp(24px, 4vw, 60px);
  border-radius: 20px; border: 1px solid rgba(255,255,255,0.12);
  display: flex; flex-direction: column; align-items: center; gap: clamp(16px, 2vw, 24px);
  background: radial-gradient(ellipse at center, rgba(46,167,255,0.25) 0%, rgba(19,221,196,0.12) 50%, rgba(147,129,255,0.18) 100%);
}
.cta-box h2 { font-size: clamp(24px, 3.5vw, 44px); font-weight: 700; text-align: center; letter-spacing: -1px; font-family: var(--font-cjk); }
.cta-box p { font-size: clamp(14px, 1.3vw, 16px); color: var(--text-secondary); text-align: center; max-width: 500px; font-family: var(--font-cjk); }
.cta-buttons { display: flex; gap: 12px; flex-wrap: wrap; justify-content: center; }
.btn-cta-primary {
  display: inline-flex; align-items: center; gap: 8px; padding: 14px 32px; border-radius: var(--radius-btn);
  background: var(--accent-cyan); color: #fff; font-size: 15px; font-weight: 600; text-decoration: none; font-family: var(--font-cjk);
}
.btn-cta-primary:hover { box-shadow: var(--shadow-glow-cyan); }
.btn-cta-secondary {
  display: inline-flex; align-items: center; padding: 14px 32px; border-radius: var(--radius-btn);
  background: rgba(255,255,255,0.08); border: 1px solid var(--border-strong);
  color: var(--text-primary); font-size: 15px; font-weight: 600; text-decoration: none; font-family: var(--font-cjk);
}
.btn-cta-secondary:hover { background: rgba(255,255,255,0.12); }

/* ===== Footer ===== */
.site-footer {
  width: 100%; max-width: 1440px; margin: 0 auto;
  padding: clamp(40px, 5vw, 60px) clamp(16px, 5vw, 80px) clamp(24px, 3vw, 40px);
  background: rgba(1,0,4,1); display: flex; flex-direction: column; align-items: center; gap: clamp(28px, 4vw, 48px);
}
.footer-top { display: flex; justify-content: space-between; width: 100%; max-width: 1280px; gap: clamp(20px, 3vw, 40px); flex-wrap: wrap; }
.footer-brand { display: flex; flex-direction: column; gap: 12px; flex: 2 1 280px; min-width: 240px; }
.footer-brand p { font-size: 13px; color: rgba(255,255,255,0.5); font-family: var(--font-cjk); }
.footer-col { flex: 1 1 140px; min-width: 120px; }
.footer-col h4 { font-size: 13px; font-weight: 700; margin-bottom: 12px; font-family: var(--font-cjk); }
.footer-col a { display: block; font-size: 13px; color: var(--text-secondary); text-decoration: none; margin-bottom: 8px; font-family: var(--font-cjk); transition: color .15s; }
.footer-col a:hover { color: var(--text-primary); }
.footer-bottom {
  width: 100%; max-width: 1280px; display: flex; justify-content: space-between;
  align-items: center; padding-top: 20px; flex-wrap: wrap; gap: 12px;
  border-top: 1px solid rgba(255,255,255,0.08);
}
.footer-bottom span { font-size: 12px; color: rgba(255,255,255,0.4); font-family: var(--font-cjk); }
.footer-status { display: flex; align-items: center; gap: 8px; }
.status-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--accent-green); }
.status-text { font-size: 12px; color: var(--text-secondary); font-family: var(--font-en); }

/* ===== Responsive (these class selectors work fine with scoped) ===== */
@media (max-width: 1023px) {
  .nav-links, .header-inner > .btn-primary { display: none; }
  .hamburger { display: flex; }
  .service-grid { grid-template-columns: repeat(2, 1fr); }
  .feat-grid { grid-template-columns: repeat(2, 1fr); }
  .stat-grid { grid-template-columns: repeat(2, 1fr); }
  .article-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 600px) {
  .header-inner { height: 56px; }
  .mobile-menu { top: 56px; }
  .hero { padding-top: 60px; padding-bottom: 60px; }
  .service-grid { grid-template-columns: 1fr; }
  .feat-grid { grid-template-columns: 1fr; }
  .stat-grid { grid-template-columns: 1fr 1fr; }
  .article-grid { grid-template-columns: 1fr; }
  .cta-buttons { flex-direction: column; align-items: center; width: 100%; }
  .btn-cta-primary, .btn-cta-secondary { width: 100%; text-align: center; justify-content: center; }
  .footer-bottom { flex-direction: column; text-align: center; }
  .section-header-row { flex-direction: column; align-items: flex-start; }
  .section-title-left { font-size: 24px; }
  .feat-item { height: auto; min-height: 64px; }
}
@media (max-width: 380px) {
  .stat-grid { grid-template-columns: 1fr; }
}
</style>
