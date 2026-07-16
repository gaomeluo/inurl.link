<script setup lang="ts">
// 仅在首页激活时设置 body 背景色；离开首页时 Nuxt 自动清理
useHead({
  bodyAttrs: { style: 'background: #030014;' },
  link: [
    { rel: 'preconnect', href: 'https://fonts.googleapis.com' },
    { rel: 'preconnect', href: 'https://fonts.gstatic.com', crossorigin: '' },
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap' },
  ],
})
</script>

<template>
  <div class="home-page">
    <slot />
  </div>
</template>

<style scoped>
/* 变量定义在 .home-page 上，自动向下级组件（index.vue）继承 */
.home-page {
  --bg-primary: #030014;
  --bg-card: rgba(255,255,255,0.04);
  --text-primary: #FFFFFF;
  --text-secondary: rgba(255,255,255,0.65);
  --text-tertiary: rgba(255,255,255,0.45);
  --border-subtle: rgba(255,255,255,0.10);
  --border-strong: rgba(255,255,255,0.18);
  --accent-cyan: #2EA7FF;
  --accent-purple: #9381FF;
  --accent-green: #13DDC4;
  --accent-amber: #FFB454;
  --accent-red: #FF4D6D;
  --radius-card: 20px;
  --radius-btn: 99px;
  --shadow-glow-cyan: 0 0 30px rgba(46,167,255,0.2);
  --shadow-glow-purple: 0 0 30px rgba(147,129,255,0.2);
  --shadow-glow-green: 0 0 30px rgba(19,221,196,0.2);
  --font-cjk: 'PingFang SC', 'Noto Sans SC', 'Microsoft YaHei', sans-serif;
  --font-en: 'Inter', system-ui, sans-serif;

  color: var(--text-primary);
  font-family: var(--font-en);
  line-height: 1.6;
  min-height: 100vh;
  overflow-x: hidden;
}
</style>
