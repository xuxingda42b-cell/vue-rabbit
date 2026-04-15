<script setup>
import { useCategoryStore } from '@/stores/category';
import HeaderCart from './HeaderCart.vue';
const categoryStore = useCategoryStore()
</script>

<template>
  <header class='app-header'>
    <div class="star-bg"></div>
    <div class="container">
      <!-- SVG Logo -->
      <h1 class="logo">
        <RouterLink to="/" class="logo-link">
          <svg class="logo-icon" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="logoGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                <stop offset="0%" stop-color="#7c5cfc"/>
                <stop offset="100%" stop-color="#4a9eff"/>
              </linearGradient>
              <filter id="glow">
                <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
                <feMerge><feMergeNode in="coloredBlur"/><feMergeNode in="SourceGraphic"/></feMerge>
              </filter>
            </defs>
            <!-- 星形背景圆 -->
            <circle cx="24" cy="24" r="22" fill="url(#logoGrad)" opacity="0.15"/>
            <!-- 五角星 -->
            <polygon
              points="24,6 27.5,17 39,17 30,24.5 33.5,36 24,29 14.5,36 18,24.5 9,17 20.5,17"
              fill="url(#logoGrad)"
              filter="url(#glow)"
            />
            <!-- 小星点装饰 -->
            <circle cx="38" cy="10" r="1.5" fill="#4a9eff" opacity="0.8"/>
            <circle cx="10" cy="38" r="1" fill="#7c5cfc" opacity="0.6"/>
            <circle cx="42" cy="30" r="1" fill="#f5a623" opacity="0.7"/>
          </svg>
          <span class="logo-text">
            <span class="logo-main">StellarNook</span>
            <span class="logo-sub">星隅好物</span>
          </span>
        </RouterLink>
      </h1>

      <!-- 导航菜单 -->
      <ul class="app-header-nav">
        <li class="home">
          <RouterLink to="/">首页</RouterLink>
        </li>
        <li class="home" v-for="item in categoryStore.categoryList" :key="item.id">
          <RouterLink active-class="active" :to="`/category/${item.id}`">{{ item.name }}</RouterLink>
        </li>
      </ul>

      <!-- 搜索框 -->
      <div class="search">
        <i class="iconfont icon-search"></i>
        <input type="text" placeholder="探索星际好物...">
      </div>

      <!-- 购物车 -->
      <HeaderCart />
    </div>
  </header>
</template>


<style scoped lang='scss'>
.app-header {
  background: $headerBg;
  border-bottom: 1px solid rgba(124, 92, 252, 0.2);
  position: relative;
  z-index: 200;

  // 星空粒子背景
  .star-bg {
    position: absolute;
    inset: 0;
    background-image:
      radial-gradient(1px 1px at 10% 30%, rgba(255,255,255,0.4) 0%, transparent 100%),
      radial-gradient(1px 1px at 25% 70%, rgba(124,92,252,0.5) 0%, transparent 100%),
      radial-gradient(1.5px 1.5px at 50% 20%, rgba(74,158,255,0.4) 0%, transparent 100%),
      radial-gradient(1px 1px at 75% 60%, rgba(255,255,255,0.3) 0%, transparent 100%),
      radial-gradient(1px 1px at 90% 40%, rgba(245,166,35,0.4) 0%, transparent 100%),
      radial-gradient(1px 1px at 60% 80%, rgba(255,255,255,0.3) 0%, transparent 100%);
    pointer-events: none;
  }

  .container {
    display: flex;
    align-items: center;
    position: relative;
    z-index: 1;
  }

  // Logo 区域
  .logo {
    width: 220px;
    flex-shrink: 0;

    .logo-link {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 20px 0;
      text-decoration: none;

      &:hover .logo-icon {
        transform: rotate(20deg) scale(1.1);
        filter: drop-shadow(0 0 8px rgba(124, 92, 252, 0.8));
      }
    }

    .logo-icon {
      width: 44px;
      height: 44px;
      transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1), filter 0.3s;
      flex-shrink: 0;
    }

    .logo-text {
      display: flex;
      flex-direction: column;
      line-height: 1;
    }

    .logo-main {
      font-size: 18px;
      font-weight: 700;
      background: linear-gradient(135deg, #7c5cfc 0%, #4a9eff 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      letter-spacing: 0.5px;
    }

    .logo-sub {
      font-size: 11px;
      font-weight: 600;
      color: $stellarGold;
      letter-spacing: 2px;
      margin-top: 4px;
      opacity: 0.9;
    }
  }

  // 导航菜单
  .app-header-nav {
    flex: 1;
    display: flex;
    padding-left: 30px;
    position: relative;
    z-index: 998;

    li {
      margin-right: 28px;

      a {
        font-size: 15px;
        font-weight: 500;
        line-height: 80px;
        height: 80px;
        display: inline-block;
        color: rgba(232, 232, 245, 0.75);
        position: relative;
        transition: color 0.25s;
        white-space: nowrap;

        &::after {
          content: '';
          position: absolute;
          bottom: 16px;
          left: 0;
          right: 0;
          height: 2px;
          background: $gradientMain;
          border-radius: 2px;
          transform: scaleX(0);
          transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
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

  // 搜索框
  .search {
    width: 200px;
    height: 36px;
    position: relative;
    border: 1px solid rgba(124, 92, 252, 0.3);
    border-radius: 20px;
    line-height: 36px;
    background: rgba(124, 92, 252, 0.08);
    transition: border-color 0.25s, box-shadow 0.25s;

    &:focus-within {
      border-color: rgba(124, 92, 252, 0.7);
      box-shadow: 0 0 0 3px rgba(124, 92, 252, 0.15);
    }

    .icon-search {
      font-size: 16px;
      margin-left: 12px;
      color: $textSecondary;
    }

    input {
      width: 148px;
      padding-left: 6px;
      color: $textPrimary;
      background: transparent;
      font-size: 13px;

      &::placeholder {
        color: $textSecondary;
      }
    }
  }

  // 购物车继承样式
  .cart {
    width: 60px;
    margin-left: 16px;

    .curr {
      height: 36px;
      line-height: 36px;
      text-align: center;
      position: relative;
      display: block;
      color: $textSecondary;
      transition: color 0.25s;

      &:hover { color: $xtxColor; }

      .icon-cart {
        font-size: 22px;
      }

      em {
        font-style: normal;
        position: absolute;
        right: 0;
        top: 0;
        padding: 1px 6px;
        line-height: 1;
        background: $gradientMain;
        color: #fff;
        font-size: 11px;
        border-radius: 10px;
        font-family: 'Inter', Arial, sans-serif;
      }
    }
  }
}
</style>
