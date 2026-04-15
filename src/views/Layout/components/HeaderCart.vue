<script setup>
import { useCartStore } from '@/stores/cartStore';
const cartStore = useCartStore()
</script>

<template>
  <div class="cart">
    <a class="curr" href="javascript:;">
      <i class="iconfont icon-cart"></i><em>{{ cartStore.cartList.length }}</em>
    </a>
    <div class="layer">
      <div class="list">

        <div class="item" v-for="i in cartStore.cartList" :key="i">
          <RouterLink to="">
            <img :src="i.picture" alt="" />
            <div class="center">
              <p class="name ellipsis-2">
                {{ i.name }}
              </p>
              <p class="attr ellipsis">{{ i.attrsText }}</p>
            </div>
            <div class="right">
              <p class="price">&yen;{{ i.price }}</p>
              <p class="count">x{{ i.count }}</p>
            </div>
          </RouterLink>
          <i class="iconfont icon-close-new" @click="cartStore.delCart(i.skuId)"></i>
        </div>

      </div>
      <div class="foot">
        <div class="total">
          <p>共 {{ cartStore.allCount }} 件商品</p>
          <p>&yen; {{ cartStore.allPrice.toFixed(2) }} </p>
        </div>
        <el-button size="large" type="primary" @click="$router.push('/cartlist')">去购物车结算</el-button>
      </div>
    </div>
</div>
</template>

<style scoped lang="scss">
.cart {
  width: 50px;
  position: relative;
  z-index: 600;

  .curr {
    height: 32px;
    line-height: 32px;
    text-align: center;
    position: relative;
    display: block;

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
      background: $helpColor;
      color: #fff;
      font-size: 12px;
      border-radius: 10px;
      font-family: Arial;
    }
  }

  &:hover {
    .layer {
      opacity: 1;
      transform: none;
    }
  }

  .layer {
    opacity: 0;
    transition: all 0.4s 0.2s;
    transform: translateY(-200px) scale(1, 0);
    width: 400px;
    height: 400px;
    position: absolute;
    top: 50px;
    right: 0;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(124, 92, 252, 0.2);
    background: #1a1a35;
    border-radius: 8px;
    padding-top: 10px;

    &::before {
      content: "";
      position: absolute;
      right: 14px;
      top: -10px;
      width: 20px;
      height: 20px;
      background: #1a1a35;
      transform: scale(0.6, 1) rotate(45deg);
      box-shadow: -3px -3px 5px rgba(0, 0, 0, 0.3);
    }

    .foot {
      position: absolute;
      left: 0;
      bottom: 0;
      height: 70px;
      width: 100%;
      padding: 10px;
      display: flex;
      justify-content: space-between;
      background: rgba(13, 13, 26, 0.8);
      border-top: 1px solid rgba(124, 92, 252, 0.2);
      border-radius: 0 0 8px 8px;
      align-items: center;

      .total {
        padding-left: 10px;
        color: $textSecondary;

        p {
          &:last-child {
            font-size: 18px;
            color: $priceColor;
          }
        }
      }
    }
  }

  .list {
    height: 310px;
    overflow: auto;
    padding: 0 10px;

    &::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }

    &::-webkit-scrollbar-track {
      background: rgba(124, 92, 252, 0.05);
      border-radius: 2px;
    }

    &::-webkit-scrollbar-thumb {
      background: rgba(124, 92, 252, 0.3);
      border-radius: 10px;
    }

    &::-webkit-scrollbar-thumb:hover {
      background: rgba(124, 92, 252, 0.6);
    }

    .item {
      border-bottom: 1px solid rgba(124, 92, 252, 0.1);
      padding: 10px 0;
      position: relative;

      i {
        position: absolute;
        bottom: 38px;
        right: 0;
        opacity: 0;
        color: $textSecondary;
        transition: all 0.5s;
      }

      &:hover {
        i {
          opacity: 1;
          cursor: pointer;
        }
      }

      a {
        display: flex;
        align-items: center;

        img {
          height: 80px;
          width: 80px;
          border-radius: 4px;
        }

        .center {
          padding: 0 10px;
          width: 200px;

          .name {
            font-size: 16px;
            color: $textPrimary;
          }

          .attr {
            color: $textSecondary;
            padding-top: 5px;
          }
        }

        .right {
          width: 100px;
          padding-right: 20px;
          text-align: center;

          .price {
            font-size: 16px;
            color: $priceColor;
          }

          .count {
            color: $textSecondary;
            margin-top: 5px;
            font-size: 16px;
          }
        }
      }
    }
  }
}
</style>
