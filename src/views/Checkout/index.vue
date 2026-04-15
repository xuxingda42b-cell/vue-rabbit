<script setup>
import { getCheckInfoAPI,createOrderAPI } from '@/apis/checkout';
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';
import { useCartStore } from '@/stores/cartStore';
const cartStore = useCartStore()
const router = useRouter()
//获取结算信息
const checkInfo = ref({})  // 订单对象
const curAddress = ref({})
const getCheckInfo = async()=>{
  const res = await getCheckInfoAPI()
  checkInfo.value = res.data.result
  //适配默认地址
  //从地址列表中筛选出来 isDefault === 0 那一项
  const item = checkInfo.value.userAddresses.find((item)=>item.isDefault === 0)
  curAddress.value = item
}

onMounted(()=>getCheckInfo())

//控制弹框打开
const showDialog = ref(false)

//切换地址
const activeAddress = ref({})
const switchAddress = (item)=>{
  activeAddress.value = item
}
const confirm = ()=>{
 curAddress.value = activeAddress.value
 showDialog.value = false
 activeAddress.value = {}
}
const cancel = ()=>{
  showDialog.value = false
  activeAddress.value = {}
}

//创建订单
const createOrder = async()=>{
  const res = await createOrderAPI({
    deliveryTimeType:1,
    payType:1,
    payChannel:1,
    buyerMessage:'',
    goods:checkInfo.value.goods.map((item)=>{
      return {
        skuId:item.skuId,
        count:item.count
      }
    }),
    addressId:curAddress.value.id
  })
  const orderId = res.data.result.id
  router.push({
    path:'/pay',
    query:{
      id:orderId
    }
  })
  //更新购物车
  cartStore.updateNewList()
}


</script>

<template>
  <div class="xtx-pay-checkout-page">
    <div class="container">
      <div class="wrapper">
        <!-- 收货地址 -->
        <h3 class="box-title">收货地址</h3>
        <div class="box-body">
          <div class="address">
            <div class="text">
              <div class="none" v-if="!curAddress">您需要先添加收货地址才可提交订单。</div>
              <ul v-else>
                <li><span>收<i />货<i />人：</span>{{ curAddress.receiver }}</li>
                <li><span>联系方式：</span>{{ curAddress.contact }}</li>
                <li><span>收货地址：</span>{{ curAddress.fullLocation }} {{ curAddress.address }}</li>
              </ul>
            </div>
            <div class="action">
              <el-button size="large" @click="showDialog = true">切换地址</el-button>
              <el-button size="large" @click="addFlag = true">添加地址</el-button>
            </div>
          </div>
        </div>
        <!-- 商品信息 -->
        <h3 class="box-title">商品信息</h3>
        <div class="box-body">
          <table class="goods">
            <thead>
              <tr>
                <th width="520">商品信息</th>
                <th width="170">单价</th>
                <th width="170">数量</th>
                <th width="170">小计</th>
                <th width="170">实付</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="i in checkInfo.goods" :key="i.id">
                <td>
                  <a href="javascript:;" class="info">
                    <img :src="i.picture" alt="">
                    <div class="right">
                      <p>{{ i.name }}</p>
                      <p>{{ i.attrsText }}</p>
                    </div>
                  </a>
                </td>
                <td>&yen;{{ i.price }}</td>
                <td>{{ i.price }}</td>
                <td>&yen;{{ i.totalPrice }}</td>
                <td>&yen;{{ i.totalPayPrice }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <!-- 配送时间 -->
        <h3 class="box-title">配送时间</h3>
        <div class="box-body">
          <a class="my-btn active" href="javascript:;">不限送货时间：周一至周日</a>
          <a class="my-btn" href="javascript:;">工作日送货：周一至周五</a>
          <a class="my-btn" href="javascript:;">双休日、假日送货：周六至周日</a>
        </div>
        <!-- 支付方式 -->
        <h3 class="box-title">支付方式</h3>
        <div class="box-body">
          <a class="my-btn active" href="javascript:;">在线支付</a>
          <a class="my-btn" href="javascript:;">货到付款</a>
          <span style="color:#999">货到付款需付5元手续费</span>
        </div>
        <!-- 金额明细 -->
        <h3 class="box-title">金额明细</h3>
        <div class="box-body">
          <div class="total">
            <dl>
              <dt>商品件数：</dt>
              <dd>{{ checkInfo.summary?.goodsCount }}件</dd>
            </dl>
            <dl>
              <dt>商品总价：</dt>
              <dd>¥{{ checkInfo.summary?.totalPrice.toFixed(2) }}</dd>
            </dl>
            <dl>
              <dt>运<i></i>费：</dt>
              <dd>¥{{ checkInfo.summary?.postFee.toFixed(2) }}</dd>
            </dl>
            <dl>
              <dt>应付总额：</dt>
              <dd class="price">{{ checkInfo.summary?.totalPayPrice.toFixed(2) }}</dd>
            </dl>
          </div>
        </div>
        <!-- 提交订单 -->
        <div class="submit">
          <el-button @click="createOrder" type="primary" size="large" >提交订单</el-button>
        </div>
      </div>
    </div>
  </div>
  <!-- 切换地址 -->
   <el-dialog v-model="showDialog" title="切换收货地址" width="30%" center>
  <div class="addressWrapper">
    <div class="text item" :class="{active:activeAddress.id === item.id}" @click="switchAddress(item)" v-for="item in checkInfo.userAddresses"  :key="item.id">
      <ul>
      <li><span>收<i />货<i />人：</span>{{ item.receiver }} </li>
      <li><span>联系方式：</span>{{ item.contact }}</li>
      <li><span>收货地址：</span>{{ item.fullLocation + item.address }}</li>
      </ul>
    </div>
  </div>
  <template #footer>
    <span class="dialog-footer">
      <el-button @click="cancel">取消</el-button>
      <el-button type="primary" @click="confirm">确定</el-button>
    </span>
  </template>
</el-dialog>
  <!-- 添加地址 -->
</template>

<style scoped lang="scss">
.xtx-pay-checkout-page {
  margin-top: 20px;

  .wrapper {
    background: $cardBg;
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 8px;
    padding: 0 20px;

    .box-title {
      font-size: 16px;
      font-weight: 600;
      padding-left: 10px;
      line-height: 70px;
      border-bottom: 1px solid rgba(124, 92, 252, 0.12);
      color: $textPrimary;
      background: $gradientMain;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .box-body {
      padding: 20px 0;
    }
  }
}

.address {
  border: 1px solid rgba(124, 92, 252, 0.2);
  border-radius: 6px;
  display: flex;
  align-items: center;

  .text {
    flex: 1;
    min-height: 90px;
    display: flex;
    align-items: center;

    .none {
      line-height: 90px;
      color: $textSecondary;
      text-align: center;
      width: 100%;
    }

    >ul {
      flex: 1;
      padding: 20px;

      li {
        line-height: 30px;
        color: $textPrimary;

        span {
          color: $textSecondary;
          margin-right: 5px;

          >i {
            width: 0.5em;
            display: inline-block;
          }
        }
      }
    }

    >a {
      color: $xtxColor;
      width: 160px;
      text-align: center;
      height: 90px;
      line-height: 90px;
      border-right: 1px solid rgba(124, 92, 252, 0.15);
    }
  }

  .action {
    width: 420px;
    text-align: center;

    .btn {
      width: 140px;
      height: 46px;
      line-height: 44px;
      font-size: 14px;

      &:first-child { margin-right: 10px; }
    }
  }
}

.goods {
  width: 100%;
  border-collapse: collapse;
  border-spacing: 0;
  color: $textPrimary;

  .info {
    display: flex;
    text-align: left;

    img {
      width: 70px;
      height: 70px;
      margin-right: 20px;
      border-radius: 4px;
    }

    .right {
      line-height: 24px;

      p {
        &:last-child { color: $textSecondary; }
      }
    }
  }

  tr {
    th {
      background: rgba(124, 92, 252, 0.08);
      font-weight: 500;
      color: $textSecondary;
    }

    td, th {
      text-align: center;
      padding: 20px;
      border-bottom: 1px solid rgba(124, 92, 252, 0.1);

      &:first-child { border-left: 1px solid rgba(124, 92, 252, 0.1); }
      &:last-child  { border-right: 1px solid rgba(124, 92, 252, 0.1); }
    }
  }
}

.my-btn {
  width: 228px;
  height: 50px;
  border: 1px solid rgba(124, 92, 252, 0.2);
  text-align: center;
  line-height: 48px;
  margin-right: 25px;
  color: $textSecondary;
  display: inline-block;
  border-radius: 4px;
  transition: border-color 0.2s, color 0.2s, background 0.2s;

  &.active,
  &:hover {
    border-color: $xtxColor;
    color: $xtxColor;
    background: rgba(124, 92, 252, 0.08);
  }
}

.total {
  dl {
    display: flex;
    justify-content: flex-end;
    line-height: 50px;
    color: $textPrimary;

    dt {
      i {
        display: inline-block;
        width: 2em;
      }
    }

    dd {
      width: 240px;
      text-align: right;
      padding-right: 70px;

      &.price {
        font-size: 20px;
        color: $priceColor;
      }
    }
  }
}

.submit {
  text-align: right;
  padding: 60px;
  border-top: 1px solid rgba(124, 92, 252, 0.12);
}

.addressWrapper {
  max-height: 500px;
  overflow-y: auto;
}

.text {
  flex: 1;
  min-height: 90px;
  display: flex;
  align-items: center;

  &.item {
    border: 1px solid rgba(124, 92, 252, 0.15);
    border-radius: 6px;
    margin-bottom: 10px;
    cursor: pointer;
    transition: border-color 0.2s, background 0.2s;
    color: $textPrimary;

    &.active,
    &:hover {
      border-color: $xtxColor;
      background: rgba(124, 92, 252, 0.1);
    }

    >ul {
      padding: 10px;
      font-size: 14px;
      line-height: 30px;
    }
  }
}
</style>

