<script setup>
import { useScroll } from '@vueuse/core'
import { useCategoryStore } from '@/stores/category';
const { y } = useScroll(window)
const categoryStore = useCategoryStore()
</script>

<template>
  <div class="app-header-sticky" :class="{ show: y > 78 }">
    <div class="container">
      <!-- 吸顶 Logo -->
      <RouterLink class="logo" to="/">
        <svg viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="stickyGrad" x1="0%" y1="0%" x2="100%" y2="100%">
              <stop offset="0%" stop-color="#7c5cfc"/>
              <stop offset="100%" stop-color="#4a9eff"/>
            </linearGradient>
          </defs>
          <polygon
            points="24,6 27.5,17 39,17 30,24.5 33.5,36 24,29 14.5,36 18,24.5 9,17 20.5,17"
            fill="url(#stickyGrad)"
          />
        </svg>
        <span class="logo-label">StellarNook</span>
      </RouterLink>

      <!-- 导航区域 -->
      <ul class="app-header-nav">
        <li class="home">
          <RouterLink to="/">首页</RouterLink>
        </li>
        <li class="home" v-for="item in categoryStore.categoryList" :key="item.id">
          <RouterLink active-class="active" :to="`/category/${item.id}`">{{ item.name }}</RouterLink>
        </li>
      </ul>

      <div class="right">
        <RouterLink to="/">品牌</RouterLink>
        <RouterLink to="/">专题</RouterLink>
      </div>
    </div>
  </div>
</template>


<style scoped lang='scss'>
.app-header-sticky {
  width: 100%;
  height: 68px;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 999;
  background: rgba(13, 13, 26, 0.92);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid rgba(124, 92, 252, 0.25);
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
  transform: translateY(-100%);
  opacity: 0;

  &.show {
    transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
    transform: none;
    opacity: 1;
  }

  .container {
    display: flex;
    align-items: center;
    height: 100%;
  }

  // 吸顶 Logo
  .logo {
    display: flex;
    align-items: center;
    gap: 8px;
    text-decoration: none;
    flex-shrink: 0;
    transition: opacity 0.2s;

    &:hover { opacity: 0.85; }

    svg {
      width: 30px;
      height: 30px;
    }

    .logo-label {
      font-size: 15px;
      font-weight: 700;
      background: linear-gradient(135deg, #7c5cfc 0%, #4a9eff 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      letter-spacing: 0.3px;
    }
  }

  .right {
    width: 180px;
    display: flex;
    text-align: center;
    padding-left: 30px;
    border-left: 1px solid rgba(124, 92, 252, 0.4);
    gap: 20px;

    a {
      font-size: 14px;
      font-weight: 500;
      color: $textSecondary;
      transition: color 0.2s;

      &:hover {
        color: $xtxColor;
      }
    }
  }
}

.app-header-nav {
  flex: 1;
  display: flex;
  padding-left: 30px;
  position: relative;
  z-index: 998;

  li {
    margin-right: 24px;

    a {
      font-size: 14px;
      font-weight: 500;
      line-height: 68px;
      height: 68px;
      display: inline-block;
      color: rgba(232, 232, 245, 0.7);
      position: relative;
      transition: color 0.25s;
      white-space: nowrap;

      &::after {
        content: '';
        position: absolute;
        bottom: 10px;
        left: 0;
        right: 0;
        height: 2px;
        background: $gradientMain;
        border-radius: 2px;
        transform: scaleX(0);
        transition: transform 0.3s ease;
      }

      &:hover {
        color: #fff;
        &::after { transform: scaleX(1); }
      }
    }

    .active {
      color: #fff;
      &::after { transform: scaleX(1); }
    }
  }
}
</style>
