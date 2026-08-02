<script setup lang="ts">
useHead({
  bodyAttrs: { style: 'background:#06060f;' },
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=Orbitron:wght@500;700;900&display=swap' },
  ],
})
</script>

<template>
  <div class="home-page">
    <slot />
  </div>
</template>

<style scoped>
.home-page {
  --bg-deep: #06060f;
  --bg-card: rgba(255,255,255,0.03);
  --text-primary: #f0f0ff;
  --text-secondary: rgba(240,240,255,0.6);
  --text-tertiary: rgba(240,240,255,0.35);
  --border-subtle: rgba(255,255,255,0.08);
  --border-glow: rgba(100,140,255,0.25);
  --accent-cyan: #4da8ff;
  --accent-purple: #8b6cff;
  --accent-green: #00e5b8;
  --accent-amber: #ff9f43;
  --accent-pink: #ff5e9c;
  --radius-card: 16px;
  --radius-btn: 50px;
  --glow-cyan: 0 0 40px rgba(77,168,255,0.3),0 0 80px rgba(77,168,255,0.1);
  --glow-purple: 0 0 40px rgba(139,108,255,0.3),0 0 80px rgba(139,108,255,0.1);
  --glow-green: 0 0 30px rgba(0,229,184,0.3),0 0 60px rgba(0,229,184,0.1);
  --font-cjk: 'PingFang SC','Noto Sans SC','Microsoft YaHei',sans-serif;
  --font-en: 'Inter',system-ui,sans-serif;
  --font-tech: 'Orbitron','Inter',system-ui,sans-serif;

  color: var(--text-primary);
  font-family: var(--font-en);
  line-height: 1.6;
  min-height: 100vh;
  overflow-x: hidden;
  background:
    radial-gradient(ellipse 80% 60% at 50% 0%, rgba(77,168,255,0.08) 0%, transparent 60%),
    radial-gradient(ellipse 60% 50% at 20% 80%, rgba(139,108,255,0.06) 0%, transparent 50%),
    radial-gradient(ellipse 50% 40% at 80% 60%, rgba(0,229,184,0.04) 0%, transparent 50%),
    var(--bg-deep);
}
</style>
