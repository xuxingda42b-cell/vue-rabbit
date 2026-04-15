<script setup>
import { getOrderAPI } from '@/apis/pay';
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import { useCountDown } from '@/composables/useCountDown';
const {formatTime,start} = useCountDown()
//获取订单数据
const route = useRoute()
const payInfo = ref({})
const getPayInfo=async ()=>{
  const res = await getOrderAPI(route.query.id)
  payInfo.value = res.data.result
  //初始化倒计时秒数
  start(res.data.result.countdown)
}
onMounted(()=>getPayInfo())

//跳转支付
//携带订单id以及回调地址跳转到支付地址（get）
// 支付地址
const baseURL = 'http://pcapi-xiaotuxian-front-devtest.itheima.net/'
const backURL = 'http://Localhost:5173/paycallback'
const redirectUrl = encodeURIComponent(backURL)
const payUrl = `${baseURL}pay/aliPay?orderId=${route.query.id}&redirect=${redirectUrl}`
</script>


<template>
  <div class="xtx-pay-page">
    <div class="container">
      <!-- 付款信息 -->
      <div class="pay-info">
        <span class="icon iconfont icon-queren2"></span>
        <div class="tip">
          <p>订单提交成功！请尽快完成支付。</p>
          <p>支付还剩 <span>{{ formatTime }}</span>, 超时后将取消订单</p>
        </div>
        <div class="amount">
          <span>应付总额：</span>
          <span>¥{{ payInfo.payMoney?.toFixed(2) }}</span>
        </div>
      </div>
      <!-- 付款方式 -->
      <div class="pay-type">
        <p class="head">选择以下支付方式付款</p>
        <div class="item">
          <p>支付平台</p>
          <a class="btn wx" href="javascript:;"></a>
          <a class="btn alipay" :href="payUrl"></a>
        </div>
        <div class="item">
          <p>支付方式</p>
          <a class="btn" href="javascript:;">招商银行</a>
          <a class="btn" href="javascript:;">工商银行</a>
          <a class="btn" href="javascript:;">建设银行</a>
          <a class="btn" href="javascript:;">农业银行</a>
          <a class="btn" href="javascript:;">交通银行</a>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.xtx-pay-page {
  margin-top: 20px;
}

.pay-info {
  background: $cardBg;
  border: 1px solid rgba(124, 92, 252, 0.15);
  border-radius: 8px;
  display: flex;
  align-items: center;
  height: 240px;
  padding: 0 80px;

  .icon {
    font-size: 80px;
    color: #1dc779;
    filter: drop-shadow(0 0 12px rgba(29, 199, 121, 0.5));
  }

  .tip {
    padding-left: 10px;
    flex: 1;

    p {
      &:first-child {
        font-size: 20px;
        margin-bottom: 5px;
        color: $textPrimary;
      }

      &:last-child {
        color: $textSecondary;
        font-size: 16px;

        span {
          color: $priceColor;
          font-weight: 600;
        }
      }
    }
  }

  .amount {
    span {
      &:first-child {
        font-size: 16px;
        color: $textSecondary;
      }

      &:last-child {
        color: $priceColor;
        font-size: 24px;
        font-weight: 700;
        text-shadow: 0 0 10px rgba(255, 80, 80, 0.4);
      }
    }
  }
}

.pay-type {
  margin-top: 20px;
  background: $cardBg;
  border: 1px solid rgba(124, 92, 252, 0.15);
  border-radius: 8px;
  padding-bottom: 70px;

  p {
    line-height: 70px;
    height: 70px;
    padding-left: 30px;
    font-size: 16px;
    color: $textPrimary;

    &.head {
      border-bottom: 1px solid rgba(124, 92, 252, 0.12);
      background: $gradientMain;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      font-weight: 600;
    }
  }

  .btn {
    width: 150px;
    height: 50px;
    border: 1px solid rgba(124, 92, 252, 0.2);
    border-radius: 6px;
    text-align: center;
    line-height: 48px;
    margin-left: 30px;
    color: $textSecondary;
    display: inline-block;
    transition: border-color 0.2s, background 0.2s, color 0.2s;

    &.active,
    &:hover {
      border-color: $xtxColor;
      background: rgba(124, 92, 252, 0.08);
      color: $xtxColor;
    }

    &.alipay {
      background: url(https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/7b6b02396368c9314528c0bbd85a2e06.png) no-repeat center / contain;
      filter: brightness(0.9);
    }

    &.wx {
      background: url(https://cdn.cnbj1.fds.api.mi-img.com/mi-mall/c66f98cff8649bd5ba722c2e8067c6ca.jpg) no-repeat center / contain;
      filter: brightness(0.9);
    }
  }
}
</style>
