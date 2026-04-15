<script setup>
//定义props
defineProps({
  //主标题
  title:{
    type: String
  },
  //副标题
  subTitle:{
    type:String
  }
})
</script>


<template>
  <div class="home-panel">
    <div class="container">
      <div class="head">
         <!-- 主标题和副标题 -->
        <h3>
          {{ title }}<small>{{ subTitle }}</small>
        </h3>
      </div>
      <!-- 主体内容区域 -->
      <div>
        <slot />
      </div>
    </div>
  </div>
</template>

<style scoped lang='scss'>
.home-panel {
  background-color: $headerBg;

  .head {
    padding: 40px 0;
    display: flex;
    align-items: flex-end;

    h3 {
      flex: 1;
      font-size: 32px;
      font-weight: 700;
      margin-left: 6px;
      height: 35px;
      line-height: 35px;
      background: $gradientMain;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;

      small {
        font-size: 16px;
        color: $textSecondary;
        margin-left: 20px;
        background: none;
        -webkit-text-fill-color: $textSecondary;
      }
    }
  }
}
</style>
