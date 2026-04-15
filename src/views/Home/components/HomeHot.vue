<script setup>
import { onMounted, ref } from 'vue';
import HomePanel from './HomePanel.vue';
import {getHotAPI} from '@/apis/home'

const hotList = ref([])

const getHotList = async()=>{
  const res = await getHotAPI()
  hotList.value = res.data.result
}

onMounted(()=>getHotList())
</script>

<template>
  <HomePanel title="人气推荐" sub-title="人气爆款 不容错过">
    <ul class="goods-list">
    <li v-for="item in hotList" :key="item.id">
      <RouterLink to="/">
        <img v-img-lazy="item.picture" alt="" />
        <p class="name">{{ item.title }}</p>
        <p class="desc">{{ item.alt }}</p>
      </RouterLink>
    </li>
  </ul>
  </HomePanel>
</template>


<style scoped lang='scss'>
.goods-list {
  display: flex;
  justify-content: space-between;
  height: 406px;

  li {
    width: 306px;
    height: 406px;

    background: $cardBg;
    border-radius: 8px;
    border: 1px solid rgba(124, 92, 252, 0.1);
    transition: all .5s;

    &:hover {
      transform: translate3d(0, -3px, 0);
      box-shadow: 0 4px 16px rgba(124, 92, 252, 0.2);
      border-color: rgba(124, 92, 252, 0.3);
    }

    img {
      width: 306px;
      height: 306px;
      border-radius: 8px 8px 0 0;
    }

    p {
      font-size: 22px;
      padding-top: 12px;
      text-align: center;
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
    }

    .name {
      color: $textPrimary;
      font-size: 18px;
    }

    .desc {
      color: $textSecondary;
      font-size: 14px;
      padding-top: 4px;
    }
  }
}
</style>
