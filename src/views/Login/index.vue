<script setup>
import { ref } from 'vue';
import { ElMessage } from 'element-plus'
import 'element-plus/theme-chalk/el-message.css'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/stores/user'

const userStore = useUserStore()

// 星空粒子样式生成
const starStyle = (n) => {
  const size = (((n * 7) % 3) + 1) + 'px'
  const x = ((n * 137) % 100) + '%'
  const y = ((n * 97) % 100) + '%'
  const dur = (((n * 13) % 4) + 2) + 's'
  const delay = '-' + (((n * 23) % 5)) + 's'
  return {
    width: size, height: size,
    left: x, top: y,
    '--dur': dur, '--delay': delay,
    opacity: ((n * 17) % 6) / 10 + 0.1
  }
}

const form = ref({
  account:'xiaotuxian001',
  password:'123456',
  agree:true
})

const rules = {
  account:[{ required:true, message:'用户名不能为空', trigger:'blur' }],
  password:[
    { required:true, message:'密码不能为空', trigger:'blur' },
    { min:6, max:14, message:'密码长度为6-14个字符', trigger:'blur' }
  ],
  agree:[{
    validator:(rule,value,callback)=>{
      if(value){ callback() } else { callback(new Error('请勾选协议')) }
    }
  }]
}

const formRef = ref(null)
const router = useRouter()
const doLogin = async()=>{
  const {account,password} = form.value
  formRef.value.validate(async (valid) =>{
    if(valid){
      await userStore.getUserInfo({account,password})
      ElMessage({type:'success',message:'登录成功'})
      router.replace({path:'/'})
    }
  })
}
</script>



<template>
  <div class="login-page">
    <!-- 星空背景 -->
    <div class="stars-bg">
      <div class="star" v-for="n in 60" :key="n" :style="starStyle(n)"></div>
    </div>

    <header class="login-header">
      <div class="container">
        <RouterLink to="/" class="logo-link">
          <svg class="logo-icon" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="loginLogoGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                <stop offset="0%" stop-color="#7c5cfc"/>
                <stop offset="100%" stop-color="#4a9eff"/>
              </linearGradient>
            </defs>
            <circle cx="24" cy="24" r="22" fill="url(#loginLogoGrad)" opacity="0.15"/>
            <polygon
              points="24,6 27.5,17 39,17 30,24.5 33.5,36 24,29 14.5,36 18,24.5 9,17 20.5,17"
              fill="url(#loginLogoGrad)"
            />
            <circle cx="38" cy="10" r="1.5" fill="#4a9eff" opacity="0.8"/>
            <circle cx="10" cy="38" r="1" fill="#7c5cfc" opacity="0.6"/>
          </svg>
          <div class="logo-text">
            <span class="logo-main">StellarNook</span>
            <span class="logo-sub">星隅好物</span>
          </div>
        </RouterLink>
        <RouterLink class="entry" to="/">
          进入网站首页
          <i class="iconfont icon-angle-right"></i>
          <i class="iconfont icon-angle-right"></i>
        </RouterLink>
      </div>
    </header>

    <section class="login-section">
      <div class="wrapper">
        <div class="wrapper-glow"></div>
        <nav>
          <a href="javascript:;">账户登录</a>
        </nav>
        <div class="account-box">
          <div class="form">
            <el-form ref="formRef" :model="form" :rules="rules" label-position="right" label-width="60px" status-icon>
              <el-form-item prop="account" label="账户">
                <el-input v-model="form.account"/>
              </el-form-item>
              <el-form-item prop="password" label="密码">
                <el-input v-model="form.password" type="password"/>
              </el-form-item>
              <el-form-item prop="agree" label-width="22px">
                <el-checkbox size="large" v-model="form.agree">
                  我已同意隐私条款和服务条款
                </el-checkbox>
              </el-form-item>
              <el-button size="large" type="primary" class="subBtn" @click="doLogin">点击登录</el-button>
            </el-form>
          </div>
        </div>
      </div>
    </section>

    <footer class="login-footer">
      <div class="container">
        <p>
          <a href="javascript:;">关于我们</a>
          <a href="javascript:;">帮助中心</a>
          <a href="javascript:;">售后服务</a>
          <a href="javascript:;">配送与验收</a>
          <a href="javascript:;">商务合作</a>
          <a href="javascript:;">搜索推荐</a>
          <a href="javascript:;">友情链接</a>
        </p>
        <p>CopyRight &copy; 星隅好物 StellarNook</p>
      </div>
    </footer>
  </div>
</template>

<style scoped lang='scss'>
.login-page {
  min-height: 100vh;
  background: #0d0d1a;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

// 星空背景粒子
.stars-bg {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;

  .star {
    position: absolute;
    border-radius: 50%;
    background: #fff;
    animation: twinkle var(--dur, 3s) var(--delay, 0s) infinite alternate ease-in-out;
  }
}

@keyframes twinkle {
  from { opacity: 0.1; transform: scale(0.8); }
  to   { opacity: 0.9; transform: scale(1.2); }
}

// 顶部 header
.login-header {
  background: rgba(13, 13, 26, 0.9);
  border-bottom: 1px solid rgba(124, 92, 252, 0.2);
  backdrop-filter: blur(10px);
  position: relative;
  z-index: 10;

  .container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 80px;
  }

  .logo-link {
    display: flex;
    align-items: center;
    gap: 12px;
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
    color: #f5a623;
    letter-spacing: 2px;
    margin-top: 4px;
  }

  .entry {
    font-size: 15px;
    color: rgba(232, 232, 245, 0.7);
    transition: color 0.2s;

    &:hover { color: #7c5cfc; }

    i {
      font-size: 13px;
      color: #7c5cfc;
      letter-spacing: -5px;
    }
  }
}

// 登录主体
.login-section {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  z-index: 1;
  padding: 60px 0;

  .wrapper {
    width: 420px;
    background: rgba(26, 26, 53, 0.85);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(124, 92, 252, 0.3);
    border-radius: 16px;
    box-shadow: 0 8px 40px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(124, 92, 252, 0.1) inset;
    position: relative;
    overflow: hidden;

    // 顶部光晕装饰
    &::before {
      content: '';
      position: absolute;
      top: -60px;
      left: 50%;
      transform: translateX(-50%);
      width: 200px;
      height: 120px;
      background: radial-gradient(ellipse, rgba(124, 92, 252, 0.25) 0%, transparent 70%);
      pointer-events: none;
    }

    nav {
      font-size: 14px;
      height: 60px;
      margin-bottom: 10px;
      border-bottom: 1px solid rgba(124, 92, 252, 0.15);
      display: flex;
      padding: 0 40px;
      align-items: center;

      a {
        flex: 1;
        line-height: 1;
        display: inline-block;
        font-size: 18px;
        font-weight: 600;
        text-align: center;
        color: $textPrimary;
        position: relative;

        &::after {
          content: '';
          position: absolute;
          bottom: -20px;
          left: 50%;
          transform: translateX(-50%);
          width: 40px;
          height: 2px;
          background: $gradientMain;
          border-radius: 2px;
        }
      }
    }
  }
}

.account-box {
  .form {
    padding: 20px 30px 30px;
  }
}

.subBtn {
  width: 100%;
  height: 44px;
  font-size: 16px;
  letter-spacing: 2px;
  background: $gradientMain;
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(124, 92, 252, 0.4);
  transition: box-shadow 0.3s, transform 0.2s;

  &:hover {
    box-shadow: 0 6px 20px rgba(124, 92, 252, 0.6);
    transform: translateY(-1px);
  }

  &:active {
    transform: translateY(0);
  }
}

// 底部
.login-footer {
  padding: 24px 0 32px;
  background: rgba(10, 10, 24, 0.9);
  border-top: 1px solid rgba(124, 92, 252, 0.1);
  position: relative;
  z-index: 1;

  p {
    text-align: center;
    color: $textSecondary;
    padding-top: 12px;
    font-size: 13px;

    a {
      line-height: 1;
      padding: 0 10px;
      color: $textSecondary;
      display: inline-block;
      transition: color 0.2s;

      &:hover { color: #7c5cfc; }

      ~ a {
        border-left: 1px solid rgba(124, 92, 252, 0.2);
      }
    }
  }
}
</style>

