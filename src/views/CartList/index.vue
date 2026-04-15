<script setup>
import { useCartStore } from '@/stores/cartStore';
import { useRouter } from 'vue-router';
import { ElMessage } from 'element-plus';

const cartStore = useCartStore()
const router = useRouter()

// 单选回调
const singleCheck = (i, selected) => {
  cartStore.singleCheck(i.skuId, selected)
}

// 全选回调
const allCheck = (selected) => {
  cartStore.allCheck(selected)
}

// 结算前校验
const toCheckout = () => {
  if (cartStore.selectedCount === 0) {
    ElMessage.warning('请先勾选要结算的商品')
    return
  }
  router.push('/checkout')
}
</script>

<template>
  <div class="xtx-cart-page">
    <div class="container m-top-20">
      <div class="cart">
        <table>
          <thead>
            <tr>
              <th width="120">
                <el-checkbox :model-value="cartStore.isAll" @change="allCheck"/>
              </th>
              <th width="400">商品信息</th>
              <th width="220">单价</th>
              <th width="180">数量</th>
              <th width="180">小计</th>
              <th width="140">操作</th>
            </tr>
          </thead>
          <!-- 商品列表 -->
          <tbody>
            <tr v-for="i in cartStore.cartList" :key="i.id">
              <td>
                <!-- 单选框 -->
                <el-checkbox :model-value="i.selected" @change="(selected)=>singleCheck(i,selected)"/>
              </td>
              <td>
                <div class="goods">
                  <RouterLink to="/"><img :src="i.picture" alt="" /></RouterLink>
                  <div>
                    <p class="name ellipsis">
                      {{ i.name }}
                    </p>
                  </div>
                </div>
              </td>
              <td class="tc">
                <p>&yen;{{ i.price }}</p>
              </td>
              <td class="tc">
                <el-input-number v-model="i.count" />
              </td>
              <td class="tc">
                <p class="f16 red">&yen;{{ (i.price * i.count).toFixed(2) }}</p>
              </td>
              <td class="tc">
                <p>
                  <el-popconfirm title="确认删除吗?" confirm-button-text="确认" cancel-button-text="取消" @confirm="delCart(i)">
                    <template #reference>
                      <a href="javascript:;">删除</a>
                    </template>
                  </el-popconfirm>
                </p>
              </td>
            </tr>
            <tr v-if="cartStore.cartList.length === 0">
              <td colspan="6">
                <div class="cart-none">
                  <el-empty description="购物车列表为空">
                    <el-button type="primary">随便逛逛</el-button>
                  </el-empty>
                </div>
              </td>
            </tr>
          </tbody>

        </table>
      </div>
      <!-- 操作栏 -->
      <div class="action">
        <div class="batch">
          共 {{ cartStore.allCount }} 件商品，已选择 {{ cartStore.selectedCount }} 件，商品合计：
          <span class="red">¥ {{ cartStore.selectedPrice.toFixed(2) }} </span>
        </div>
        <div class="total">
          <el-button size="large" type="primary" @click="toCheckout">下单结算</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.xtx-cart-page {
  margin-top: 20px;

  .cart {
    background: $cardBg;
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 8px;
    overflow: hidden;
    color: $textPrimary;

    table {
      width: 100%;
      border-spacing: 0;
      border-collapse: collapse;
      line-height: 24px;

      th,
      td {
        padding: 10px;
        border-bottom: 1px solid rgba(124, 92, 252, 0.1);

        &:first-child {
          text-align: left;
          padding-left: 30px;
          color: $textSecondary;
        }
      }

      th {
        font-size: 16px;
        font-weight: 500;
        line-height: 50px;
        color: $textSecondary;
        background: rgba(124, 92, 252, 0.06);
      }
    }
  }

  .cart-none {
    text-align: center;
    padding: 120px 0;
    background: $cardBg;

    p {
      color: $textSecondary;
      padding: 20px 0;
    }
  }

  .tc {
    text-align: center;

    a {
      color: $xtxColor;
      transition: color 0.2s;
      &:hover { color: lighten(#7c5cfc, 15%); }
    }

    .xtx-numbox {
      margin: 0 auto;
      width: 120px;
    }
  }

  .red { color: $priceColor; }
  .green { color: $xtxColor; }
  .f16 { font-size: 16px; }

  .goods {
    display: flex;
    align-items: center;

    img {
      width: 100px;
      height: 100px;
      border-radius: 6px;
    }

    >div {
      width: 280px;
      font-size: 16px;
      padding-left: 10px;
      color: $textPrimary;

      .attr {
        font-size: 14px;
        color: $textSecondary;
      }
    }
  }

  .action {
    display: flex;
    background: $cardBg;
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 8px;
    margin-top: 16px;
    height: 80px;
    align-items: center;
    font-size: 16px;
    justify-content: space-between;
    padding: 0 30px;
    color: $textPrimary;

    .batch {
      a { margin-left: 20px; }
    }

    .red {
      font-size: 18px;
      margin-right: 20px;
      font-weight: bold;
    }
  }

  .tit {
    color: $textSecondary;
    font-size: 16px;
    font-weight: normal;
    line-height: 50px;
  }
}
</style>

