<script setup>
import { useCategory } from './composables/useCategory';
import GoodsItem from '../Home/components/GoodsItem.vue';
import {useBanner} from './composables/useBanner'


//获取分类数据
const {categoryData} = useCategory()

//获取Banner
const {bannerList} = useBanner()
</script>

<template>
  <div class="top-category">
    <div class="container m-top-20">
      <!-- 面包屑 -->
      <div class="bread-container">
        <el-breadcrumb separator=">">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item>{{ categoryData.name }}</el-breadcrumb-item>
        </el-breadcrumb>
      </div>
      <!-- 轮播图 -->
       <div class="home-banner">
        <el-carousel height="500px">
        <el-carousel-item v-for="item in bannerList" :key="item.id">
        <img :src="item.imgUrl" alt="">
        </el-carousel-item>
        </el-carousel>
      </div>
      <div class="sub-list">
      <h3>全部分类</h3>
       <ul>
        <li v-for="i in categoryData.children" :key="i.id">
         <RouterLink :to="`/category/sub/${i.id}`">
              <img :src="i.picture" />
              <p>{{ i.name }}</p>
            </RouterLink>
          </li>
        </ul>
      </div>
      <div class="ref-goods" v-for="item in categoryData.children" :key="item.id">
        <div class="head">
          <h3>- {{ item.name }}-</h3>
        </div>
        <div class="body">
          <GoodsItem v-for="good in item.goods" :goods="good" :key="good.id" />
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped lang="scss">

.top-category {
  h3 {
    font-size: 28px;
    font-weight: 600;
    text-align: center;
    line-height: 100px;
    background: $gradientMain;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .sub-list {
    margin-top: 20px;
    background: $cardBg;
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 8px;

    ul {
      display: flex;
      padding: 0 32px;
      flex-wrap: wrap;

      li {
        width: 168px;
        height: 160px;

        a {
          text-align: center;
          display: block;
          font-size: 16px;
          color: $textPrimary;

          img {
            width: 100px;
            height: 100px;
          }

          p {
            line-height: 40px;
            color: $textSecondary;
          }

          &:hover {
            color: $xtxColor;
            p { color: $xtxColor; }
          }
        }
      }
    }
  }

  .ref-goods {
    background: $cardBg;
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 8px;
    margin-top: 20px;
    position: relative;
    transition: border-color 0.3s;

    &:hover {
      border-color: rgba(124, 92, 252, 0.35);
    }

    .head {
      .xtx-more {
        position: absolute;
        top: 20px;
        right: 20px;
      }

      .tag {
        text-align: center;
        color: $textSecondary;
        font-size: 20px;
        position: relative;
        top: -20px;
      }
    }

    .body {
      display: flex;
      justify-content: space-around;
      padding: 0 40px 30px;
    }
  }

  .bread-container {
    padding: 25px 0;
  }
}
.home-banner {
  width: 1240px;
  height: 500px;
 margin: 0 auto;

  img {
    width: 100%;
    height: 500px;
  }
}
</style>
