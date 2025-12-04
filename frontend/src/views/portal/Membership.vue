<template>
  <div class="membership-page">
    <!-- 动态背景 -->
    <div class="animated-background">
      <div class="gradient-orb orb-1"></div>
      <div class="gradient-orb orb-2"></div>
      <div class="gradient-orb orb-3"></div>
      <div class="floating-particles">
        <div v-for="i in 20" :key="i" class="particle" :style="getParticleStyle(i)"></div>
      </div>
    </div>

    <!-- 主要内容容器 -->
    <div class="main-container">
      
      <!-- 页面标题区域 -->
      <div class="hero-section">
        <div class="hero-badge">
          <div class="badge-glow"></div>
          <span class="emoji">🎉</span>
          <span>官方福利</span>
        </div>
        
        <h1 class="hero-title">
          <span class="title-line">谈钱</span>
          <span class="title-highlight">伤感情</span>
          <span class="title-line">！</span>
        </h1>
        
        <div class="hero-subtitle-box">
          <p class="hero-subtitle">
            本站主打一个 <span class="highlight-text">交个朋友</span><br>
            <span class="repeat-text">不收米！不收米！不收米！</span>
          </p>
        </div>
      </div>

      <!-- 搞笑方案展示 -->
      <div class="plans-section">
        <div class="plans-container">
          <!-- 白嫖方案 -->
          <div class="plan-card free-plan">
            <div class="popular-badge">
              <span>🔥 99%的人选这个</span>
            </div>
            <div class="plan-header">
              <div class="plan-emoji">😎</div>
              <div class="plan-type">资深白嫖党</div>
              <div class="plan-price">
                <span class="currency">¥</span>
                <span class="amount">0</span>
                <span class="period">/永远</span>
              </div>
            </div>
            <div class="plan-features">
              <div class="feature-item">
                <span class="check">✅</span>
                <span>全站内容随便看</span>
              </div>
              <div class="feature-item">
                <span class="check">✅</span>
                <span>资源源码随便下</span>
              </div>
              <div class="feature-item">
                <span class="check">✅</span>
                <span>站长在线陪唠嗑</span>
              </div>
            </div>
          </div>

          <!-- 下次一定方案 -->
          <div class="plan-card next-time-plan">
            <div class="plan-header">
              <div class="plan-emoji">🤪</div>
              <div class="plan-type">下次一定党</div>
              <div class="plan-price">
                <span class="currency">¥</span>
                <span class="amount">0</span>
                <span class="period">/也能用</span>
              </div>
            </div>
            <div class="plan-features">
              <div class="feature-item">
                <span class="check">✅</span>
                <span>不仅白嫖还想点赞</span>
              </div>
              <div class="feature-item">
                <span class="check">✅</span>
                <span>虽然不付钱但很帅</span>
              </div>
              <div class="feature-item">
                <span class="check">✅</span>
                <span>拥有更强的心理素质</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 二维码开通区域 -->
      <div class="qr-activation-section" id="qr-section">
        <div class="qr-content-wrapper">
          <div class="qr-left">
            <h3 class="qr-title">
              <span class="qr-emoji">👋</span>
              <span>如何获得这泼天的富贵？</span>
            </h3>
            <div class="funny-steps">
              <div class="funny-step">
                <div class="step-emoji">📱</div>
                <div class="step-text">
                  <strong>第一步</strong>
                  <p>掏出你的手机，打开微信扫一扫</p>
                </div>
              </div>
              <div class="funny-step">
                <div class="step-emoji">💬</div>
                <div class="step-text">
                  <strong>第二步</strong>
                  <p>添加站长好友，发送暗号：<span class="code-highlight">"芝麻开门"</span> 或 <span class="code-highlight">"我是秦始皇"</span></p>
                </div>
              </div>
              <div class="funny-step">
                <div class="step-emoji">🚀</div>
                <div class="step-text">
                  <strong>第三步</strong>
                  <p>静待站长通过，立马给你开通 VIP！</p>
                </div>
              </div>
            </div>
            <p class="qr-note">PS: 站长可能在搬砖，看到消息会第一时间通过哦~</p>
          </div>

          <div class="qr-right">
            <div class="qr-code-box">
              <div class="qr-frame">
                <img
                  v-if="ownerQrUrl"
                  :src="ownerQrUrl"
                  alt="联系站长二维码"
                  class="qr-image"
                  @error="handleImageError"
                />
                <div v-else class="qr-placeholder" :class="{ 'qr-placeholder--loading': ownerLoading }">
                  <span v-if="ownerLoading">加载中...</span>
                  <span v-else>二维码去火星了</span>
                </div>
              </div>
              <div class="scan-me-text">👇 扫我 扫我 👇</div>
            </div>
          </div>
        </div>
      </div>

      <!-- 搞笑特权展示 -->
      <div class="benefits-showcase">
        <h2 class="benefits-title">成为会员你能得到什么？</h2>
        <div class="benefits-grid">
          <div class="benefit-card" v-for="(benefit, index) in funnyBenefits" :key="index">
            <div class="benefit-emoji">{{ benefit.emoji }}</div>
            <h3 class="benefit-name">{{ benefit.title }}</h3>
            <p class="benefit-desc">{{ benefit.description }}</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { tenantApi } from '@/api/tenant'
import { ensureAbsoluteUrl } from '@/utils/url'

const ownerQrCode = ref('')
const ownerRemark = ref('')
const ownerLoading = ref(false)

const ownerQrUrl = computed(() => ensureAbsoluteUrl(ownerQrCode.value))

const funnyBenefits = [
  { emoji: '📚', title: '知识自助餐', description: '全站文章随便翻，想看哪篇看哪篇，妈妈再也不用担心我的学习！' },
  { emoji: '💾', title: '硬盘搬运工', description: '看到喜欢的资源？点一下就带走，只要你硬盘够大，本站就是你的后花园。' },
  { emoji: '🤝', title: '各种姿势解锁', description: '解锁所有加密内容，没有任何门槛，主打一个畅通无阻。' },
  { emoji: '👨‍💻', title: '站长陪聊', description: '技术问题？人生困惑？只要站长醒着，不仅能修Bug，还能当树洞。' },
]

const fetchOwnerInfo = async () => {
  try {
    ownerLoading.value = true
    const response = await tenantApi.getPublicOwner()
    const data = (response as any)?.data ?? response
    ownerQrCode.value = typeof data?.qrCode === 'string' ? data.qrCode.trim() : ''
    ownerRemark.value = typeof data?.remark === 'string' ? data.remark.trim() : ''
  } catch (error) {
    console.error('获取站长二维码失败:', error)
    ownerQrCode.value = ''
  } finally {
    ownerLoading.value = false
  }
}

const getParticleStyle = (index: number) => {
  const delay = Math.random() * 20
  const duration = 15 + Math.random() * 10
  const size = 2 + Math.random() * 4
  return {
    left: `${Math.random() * 100}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    width: `${size}px`,
    height: `${size}px`
  }
}

const handleImageError = () => {
  ownerQrCode.value = ''
}

onMounted(() => {
  fetchOwnerInfo()
})
</script>

<style scoped>
/* 基础样式 */
.membership-page {
  position: relative;
  min-height: 100vh;
  background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
  color: white;
  overflow-x: hidden;
  font-family: 'Comic Sans MS', 'Chalkboard SE', 'Inter', sans-serif; /* 尝试用一点轻松的字体 */
}

/* 动态背景 */
.animated-background {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;
  z-index: 1;
}

.gradient-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.4;
  animation: float 20s infinite ease-in-out;
}

.orb-1 {
  width: 400px;
  height: 400px;
  background: linear-gradient(135deg, #ff9a9e, #fecfef);
  top: 10%;
  left: 10%;
}

.orb-2 {
  width: 300px;
  height: 300px;
  background: linear-gradient(135deg, #a18cd1, #fbc2eb);
  top: 50%;
  right: 10%;
  animation-delay: -7s;
}

.orb-3 {
  width: 250px;
  height: 250px;
  background: linear-gradient(135deg, #84fab0, #8fd3f4);
  bottom: 20%;
  left: 30%;
  animation-delay: -14s;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  33% { transform: translateY(-30px) rotate(120deg); }
  66% { transform: translateY(20px) rotate(240deg); }
}

.floating-particles {
  position: absolute;
  width: 100%;
  height: 100%;
}

.particle {
  position: absolute;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  animation: particleFloat linear infinite;
}

@keyframes particleFloat {
  0% { transform: translateY(100vh) rotate(0deg); opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { transform: translateY(-100px) rotate(360deg); opacity: 0; }
}

/* 主容器 */
.main-container {
  position: relative;
  z-index: 2;
  max-width: 1000px;
  margin: 0 auto;
  padding: 4rem 1rem;
}

/* Hero Section */
.hero-section {
  text-align: center;
  margin-bottom: 5rem;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.15);
  padding: 0.5rem 1.5rem;
  border-radius: 2rem;
  margin-bottom: 2rem;
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.badge-glow {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
  animation: shimmer 2s infinite;
}

@keyframes shimmer {
  0% { left: -100%; }
  100% { left: 100%; }
}

.hero-title {
  font-size: 4rem;
  font-weight: 900;
  margin-bottom: 2rem;
  text-shadow: 0 4px 20px rgba(0,0,0,0.3);
}

.title-highlight {
  background: linear-gradient(135deg, #ff9a9e, #fecfef);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  padding: 0 0.5rem;
}

.hero-subtitle-box {
  background: rgba(0, 0, 0, 0.3);
  display: inline-block;
  padding: 1.5rem 3rem;
  border-radius: 1rem;
  backdrop-filter: blur(10px);
  border: 1px dashed rgba(255, 255, 255, 0.3);
}

.hero-subtitle {
  font-size: 1.5rem;
  line-height: 1.6;
  margin: 0;
  color: #e0e0e0;
}

.highlight-text {
  color: #84fab0;
  font-weight: bold;
  font-size: 1.8rem;
}

.repeat-text {
  display: block;
  margin-top: 0.5rem;
  color: #ff6b6b;
  font-weight: 900;
  font-size: 1.6rem;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

/* Plans Section */
.plans-section {
  margin-bottom: 5rem;
}

.plans-container {
  display: flex;
  justify-content: center;
  gap: 3rem;
  flex-wrap: wrap;
}

.plan-card {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(20px);
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: 1.5rem;
  padding: 2.5rem;
  width: 350px;
  transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  position: relative;
}

.plan-card:hover {
  transform: translateY(-10px) rotate(1deg);
  border-color: rgba(255, 255, 255, 0.4);
  box-shadow: 0 20px 40px rgba(0,0,0,0.3);
}

.free-plan {
  background: linear-gradient(135deg, rgba(132, 250, 176, 0.1), rgba(143, 211, 244, 0.1));
  border-color: rgba(132, 250, 176, 0.3);
}

.popular-badge {
  position: absolute;
  top: -1rem;
  left: 50%;
  transform: translateX(-50%);
  background: #ff6b6b;
  padding: 0.5rem 1.5rem;
  border-radius: 2rem;
  font-weight: bold;
  font-size: 0.9rem;
  box-shadow: 0 4px 10px rgba(255, 107, 107, 0.4);
}

.plan-header {
  text-align: center;
  margin-bottom: 2rem;
}

.plan-emoji {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.plan-type {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.plan-price {
  color: #84fab0;
}

.amount {
  font-size: 3.5rem;
  font-weight: 900;
}

.feature-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
  font-size: 1.1rem;
}

/* QR Section */
.qr-activation-section {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 2rem;
  padding: 3rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 5rem;
}

.qr-content-wrapper {
  display: flex;
  align-items: center;
  gap: 4rem;
}

.qr-left {
  flex: 1;
}

.qr-title {
  font-size: 2rem;
  margin-bottom: 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
}

.funny-step {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
  background: rgba(0,0,0,0.2);
  padding: 1.5rem;
  border-radius: 1rem;
  transition: transform 0.3s;
}

.funny-step:hover {
  transform: translateX(10px);
  background: rgba(0,0,0,0.3);
}

.step-emoji {
  font-size: 2.5rem;
}

.step-text strong {
  display: block;
  color: #a18cd1;
  margin-bottom: 0.3rem;
}

.code-highlight {
  background: #ff6b6b;
  padding: 0.2rem 0.5rem;
  border-radius: 0.3rem;
  font-weight: bold;
}

.qr-note {
  margin-top: 1rem;
  font-style: italic;
  opacity: 0.7;
  font-size: 0.9rem;
}

.qr-right {
  flex: 1;
  display: flex;
  justify-content: center;
}

.qr-code-box {
  text-align: center;
}

.qr-frame {
  background: white;
  padding: 1rem;
  border-radius: 1rem;
  transform: rotate(3deg);
  transition: transform 0.3s;
}

.qr-frame:hover {
  transform: rotate(0deg) scale(1.05);
}

.qr-image {
  width: 200px;
  height: 200px;
  display: block;
}

.qr-placeholder {
  width: 200px;
  height: 200px;
  background: #eee;
  color: #333;
  display: flex;
  align-items: center;
  justify-content: center;
}

.scan-me-text {
  margin-top: 1.5rem;
  font-size: 1.2rem;
  font-weight: bold;
  color: #84fab0;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
  40% { transform: translateY(-10px); }
  60% { transform: translateY(-5px); }
}

/* Benefits */
.benefits-title {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 3rem;
}

.benefits-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
}

.benefit-card {
  background: rgba(255, 255, 255, 0.05);
  padding: 2rem;
  border-radius: 1.5rem;
  text-align: center;
  transition: all 0.3s;
}

.benefit-card:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-5px);
}

.benefit-emoji {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.benefit-name {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  color: #fbc2eb;
}

.benefit-desc {
  font-size: 0.95rem;
  line-height: 1.5;
  opacity: 0.8;
}

/* 响应式 */
@media (max-width: 768px) {
  .hero-title {
    font-size: 2.5rem;
  }
  
  .hero-subtitle {
    font-size: 1.2rem;
  }
  
  .plans-container {
    flex-direction: column;
    align-items: center;
  }
  
  .plan-card {
    width: 100%;
  }
  
  .qr-content-wrapper {
    flex-direction: column-reverse;
    gap: 2rem;
  }
  
  .qr-right {
    width: 100%;
  }
  
  .qr-frame {
    transform: rotate(0deg);
  }
}
</style>
