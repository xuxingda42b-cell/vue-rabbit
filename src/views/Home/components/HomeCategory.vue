<script setup>
import { useCategoryStore } from '@/stores/category';

const categoryStore = useCategoryStore()
</script>

<template>
  <div class="home-category">
    <ul class="menu">
      <li v-for="item in categoryStore.categoryList" :key="item.id">
        <RouterLink to="/">{{ item.name }}</RouterLink>
        <RouterLink v-for="i in item.children.slice(0,2)" :key="i.id" to="/">{{ i.name }}</RouterLink>
        <!-- 弹层layer位置 -->
        <div class="layer">
          <h4>分类推荐 <small>根据您的购买或浏览记录推荐</small></h4>
          <ul>
            <li v-for="i in item.goods" :key="i.id">
              <RouterLink to="/">
                <img :src="i.picture" alt="" />
                <div class="info">
                  <p class="name ellipsis-2">
                    {{ i.name }}
                  </p>
                  <p class="desc ellipsis">{{ i.desc }}</p>
                  <p class="price"><i>¥</i>{{ i.price }}</p>
                </div>
              </RouterLink>
            </li>
          </ul>
        </div>
      </li>
    </ul>
  </div>
</template>


<style scoped lang='scss'>
.home-category {
  width: 250px;
  height: 500px;
  background: rgba(13, 13, 26, 0.92);
  border-right: 1px solid rgba(124, 92, 252, 0.25);
  position: relative;
  z-index: 99;

  .menu {
    display: flex;
    flex-direction: column;
    height: 100%;

    li {
      flex: 1;
      padding-left: 16px;
      display: flex;
      align-items: center;
      border-bottom: 1px solid rgba(124, 92, 252, 0.1);
      position: relative;
      transition: background 0.2s;

      &::before {
        content: '';
        position: absolute;
        left: 0;
        top: 20%;
        height: 60%;
        width: 3px;
        background: $xtxColor;
        border-radius: 0 2px 2px 0;
        opacity: 0;
        transition: opacity 0.2s;
      }

      &:hover {
        background: linear-gradient(90deg, rgba(124, 92, 252, 0.25) 0%, rgba(124, 92, 252, 0.05) 100%);

        &::before {
          opacity: 1;
        }
      }

      a {
        margin-right: 4px;
        color: #fff;

        &:first-child {
          font-size: 15px;
          font-weight: 600;
          color: $textPrimary;
        }

        &:not(:first-child) {
          font-size: 12px;
          color: $textSecondary;
        }
      }

      .layer {
        width: 990px;
        height: 500px;
        background: rgba(13, 13, 26, 0.95);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(124, 92, 252, 0.2);
        box-shadow: 4px 4px 20px rgba(0, 0, 0, 0.5);
        position: absolute;
        left: 250px;
        top: 0;
        display: none;
        padding: 0 15px;

        h4 {
          font-size: 20px;
          font-weight: normal;
          line-height: 80px;
          color: $textPrimary;

          small {
            font-size: 16px;
            color: $textSecondary;
          }
        }

        ul {
          display: flex;
          flex-wrap: wrap;

          li {
            width: 310px;
            height: 120px;
            margin-right: 15px;
            margin-bottom: 15px;
            border: 1px solid rgba(124, 92, 252, 0.15);
            border-radius: 4px;
            background: $cardBg;
            transition: border-color 0.2s, background 0.2s;

            &:nth-child(3n) {
              margin-right: 0;
            }

            a {
              display: flex;
              width: 100%;
              height: 100%;
              align-items: center;
              padding: 10px;

              &:hover {
                background: rgba(124, 92, 252, 0.1);
                border-color: rgba(124, 92, 252, 0.4);
              }

              img {
                width: 95px;
                height: 95px;
                border-radius: 4px;
              }

              .info {
                padding-left: 10px;
                line-height: 24px;
                overflow: hidden;

                .name {
                  font-size: 16px;
                  color: $textPrimary;
                }

                .desc {
                  color: $textSecondary;
                }

                .price {
                  font-size: 22px;
                  color: $priceColor;

                  i {
                    font-size: 16px;
                  }
                }
              }
            }
          }
        }
      }

      // 关键样式  hover状态下的layer盒子变成block
      &:hover {
        .layer {
          display: block;
        }
      }
    }
  }
}
</style>
