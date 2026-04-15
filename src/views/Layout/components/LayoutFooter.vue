<template>
  <footer class="app_footer">
    <!-- 品牌信任区 -->
    <div class="trust-bar">
      <div class="container">
        <div class="trust-item">
          <div class="trust-icon">🌌</div>
          <div class="trust-text">
            <strong>宇宙级品质</strong>
            <span>精选全球好物</span>
          </div>
        </div>
        <div class="trust-item">
          <div class="trust-icon">⚡</div>
          <div class="trust-text">
            <strong>极速星际配送</strong>
            <span>48小时直达</span>
          </div>
        </div>
        <div class="trust-item">
          <div class="trust-icon">🛡️</div>
          <div class="trust-text">
            <strong>品质守护</strong>
            <span>7天无忧退换</span>
          </div>
        </div>
        <div class="trust-item">
          <div class="trust-icon">💎</div>
          <div class="trust-text">
            <strong>会员特权</strong>
            <span>专属优惠福利</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 联系信息区 -->
    <div class="contact">
      <div class="container">
        <dl>
          <dt>客户服务</dt>
          <dd><i class="iconfont icon-kefu"></i> 在线客服</dd>
          <dd><i class="iconfont icon-question"></i> 问题反馈</dd>
        </dl>
        <dl>
          <dt>关注我们</dt>
          <dd><i class="iconfont icon-weixin"></i> 公众号</dd>
          <dd><i class="iconfont icon-weibo"></i> 微博</dd>
        </dl>
        <dl>
          <dt>下载 APP</dt>
          <dd class="qrcode"><img src="@/assets/images/qrcode.jpg" /></dd>
          <dd class="download">
            <span>扫码下载</span>
            <span>StellarNook APP</span>
            <a href="javascript:;">立即下载</a>
          </dd>
        </dl>
        <dl>
          <dt>服务热线</dt>
          <dd class="hotline">400-0000-000 <small>周一至周日 8:00-18:00</small></dd>
        </dl>
      </div>
    </div>

    <!-- 底部版权区 -->
    <div class="extra">
      <div class="container">
        <!-- 品牌标语 -->
        <div class="slogan">
          <div class="brand-logo">
            <svg width="36" height="36" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
              <defs>
                <linearGradient id="footerGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" stop-color="#7c5cfc"/>
                  <stop offset="100%" stop-color="#4a9eff"/>
                </linearGradient>
              </defs>
              <polygon
                points="24,6 27.5,17 39,17 30,24.5 33.5,36 24,29 14.5,36 18,24.5 9,17 20.5,17"
                fill="url(#footerGrad)"
              />
            </svg>
            <span class="brand-name">StellarNook Goods</span>
          </div>
          <p class="brand-slogan">探索宇宙级品质 · 精选好物星际直达</p>
        </div>

        <!-- 版权信息 -->
        <div class="copyright">
          <p>
            <a href="javascript:;">关于我们</a>
            <a href="javascript:;">帮助中心</a>
            <a href="javascript:;">售后服务</a>
            <a href="javascript:;">配送与验收</a>
            <a href="javascript:;">商务合作</a>
            <a href="javascript:;">搜索推荐</a>
            <a href="javascript:;">友情链接</a>
          </p>
          <p class="copy-text">Copyright © 2024 StellarNook Goods. All rights reserved.</p>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped lang='scss'>
.app_footer {
  overflow: hidden;
  background-color: $footerBg;

  // 品牌信任区
  .trust-bar {
    background: linear-gradient(135deg, rgba(124, 92, 252, 0.12) 0%, rgba(74, 158, 255, 0.08) 100%);
    border-top: 1px solid rgba(124, 92, 252, 0.2);
    border-bottom: 1px solid rgba(124, 92, 252, 0.1);
    padding: 32px 0;

    .container {
      display: flex;
      justify-content: space-around;
      align-items: center;
    }

    .trust-item {
      display: flex;
      align-items: center;
      gap: 14px;
      cursor: default;
      transition: transform 0.25s;

      &:hover { transform: translateY(-2px); }

      .trust-icon {
        font-size: 32px;
        filter: drop-shadow(0 0 8px rgba(124, 92, 252, 0.5));
      }

      .trust-text {
        display: flex;
        flex-direction: column;
        gap: 2px;

        strong {
          font-size: 15px;
          color: $textPrimary;
          font-weight: 600;
        }

        span {
          font-size: 12px;
          color: $textSecondary;
        }
      }
    }
  }

  // 联系信息区
  .contact {
    background: rgba(255,255,255,0.02);
    border-bottom: 1px solid rgba(255,255,255,0.05);

    .container {
      padding: 50px 0 36px 25px;
      display: flex;
    }

    dl {
      height: 190px;
      text-align: center;
      padding: 0 60px;
      border-right: 1px solid rgba(255,255,255,0.07);
      color: $textSecondary;

      &:first-child { padding-left: 0; }
      &:last-child {
        border-right: none;
        padding-right: 0;
      }
    }

    dt {
      line-height: 1;
      font-size: 16px;
      color: $textPrimary;
      font-weight: 600;
      margin-bottom: 6px;
    }

    dd {
      margin: 30px 12px 0 0;
      float: left;
      width: 88px;
      height: 88px;
      padding-top: 10px;
      border: 1px solid rgba(124, 92, 252, 0.2);
      border-radius: 8px;
      background: rgba(124, 92, 252, 0.05);
      transition: border-color 0.25s, background 0.25s;

      .iconfont {
        font-size: 32px;
        display: block;
        color: $textSecondary;
        transition: color 0.25s;
      }

      &:hover {
        border-color: rgba(124, 92, 252, 0.5);
        background: rgba(124, 92, 252, 0.1);
        .iconfont { color: $xtxColor; }
      }

      &:last-child { margin-right: 0; }
    }

    .qrcode {
      width: 88px;
      height: 88px;
      padding: 6px;
      border: 1px solid rgba(124, 92, 252, 0.2);
      border-radius: 8px;
    }

    .download {
      padding-top: 5px;
      font-size: 13px;
      width: auto;
      height: auto;
      border: none;
      background: transparent;

      span {
        display: block;
        color: $textSecondary;
      }

      a {
        display: block;
        line-height: 1;
        padding: 8px 18px;
        margin-top: 8px;
        color: #fff;
        border-radius: 20px;
        background: $gradientMain;
        font-size: 12px;
        font-weight: 500;
        transition: opacity 0.2s, box-shadow 0.2s;

        &:hover {
          opacity: 0.9;
          box-shadow: 0 4px 15px rgba(124, 92, 252, 0.4);
        }
      }
    }

    .hotline {
      padding-top: 16px;
      font-size: 20px;
      color: $textPrimary;
      font-weight: 600;
      width: auto;
      height: auto;
      border: none;
      background: transparent;

      small {
        display: block;
        font-size: 13px;
        color: $textSecondary;
        margin-top: 6px;
        font-weight: 400;
      }
    }
  }

  // 底部区域
  .extra {
    background: $footerBg;
    border-top: 1px solid rgba(255,255,255,0.04);
  }

  .slogan {
    padding: 40px 0 24px;
    border-bottom: 1px solid rgba(255,255,255,0.05);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;

    .brand-logo {
      display: flex;
      align-items: center;
      gap: 10px;

      .brand-name {
        font-size: 22px;
        font-weight: 700;
        background: linear-gradient(135deg, #7c5cfc 0%, #4a9eff 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        letter-spacing: 0.5px;
      }
    }

    .brand-slogan {
      font-size: 13px;
      color: $textSecondary;
      letter-spacing: 1px;
    }
  }

  .copyright {
    padding: 28px 0 36px;
    text-align: center;
    color: $textSecondary;
    font-size: 13px;

    p {
      line-height: 1;
      margin-bottom: 16px;
    }

    a {
      color: $textSecondary;
      line-height: 1;
      padding: 0 10px;
      border-right: 1px solid rgba(255,255,255,0.12);
      transition: color 0.2s;

      &:last-child { border-right: none; }
      &:hover { color: $xtxColor; }
    }

    .copy-text {
      color: rgba(136, 136, 170, 0.5);
      font-size: 12px;
    }
  }
}
</style>
