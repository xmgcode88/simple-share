<template>
  <div class="ai-gold-showcase">
    <!-- Hero Section with Modern Particle Background -->
    <section class="ai-hero-section">
      <div class="ai-particles-bg"></div>
      <div class="ai-gradient-mesh"></div>
      <div class="ai-floating-elements">
        <div class="floating-element element-1"></div>
        <div class="floating-element element-2"></div>
        <div class="floating-element element-3"></div>
        <div class="floating-element element-4"></div>
      </div>

      <div class="container mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-center min-h-[150px] relative z-10 py-6">
          <!-- Content -->
          <div class="w-full max-w-5xl space-y-4 text-center">
            <div class="inline-flex items-center gap-3 px-5 py-3 rounded-2xl bg-black/30 backdrop-blur-md border border-amber-500/30 transform -rotate-2 hover:rotate-0 transition-transform duration-300 shadow-[0_0_15px_rgba(234,179,8,0.2)]">
              <span class="relative flex h-3 w-3">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-yellow-400 opacity-75"></span>
                <span class="relative inline-flex rounded-full h-3 w-3 bg-yellow-500"></span>
              </span>
              <span class="text-amber-100 font-bold tracking-wider">AI 搞钱 · 路子野</span>
            </div>

            <div class="space-y-6">
              <div class="ai-hero-banner transform hover:scale-105 transition-transform duration-500">
                <div class="banner-content">
                  <div class="banner-title-line">
                    <span class="banner-highlight">不想上班</span>
                    <span class="banner-separator">·</span>
                    <span class="banner-text">只想搞钱</span>
                    <span class="banner-separator">·</span>
                    <span class="banner-highlight">坐等退休</span>
                  </div>
                  <div class="banner-subtitle">
                    搞钱姿势要帅，动作要快
                  </div>
                </div>
                <div class="banner-decoration">
                  <div class="decoration-dots"></div>
                  <div class="decoration-lines"></div>
                </div>
              </div>

              <div class="ai-description">
                <p class="description-main">
                  这里全是 AI 搞钱的野路子，带你弯道超车（小心翻车）
                </p>
                <p class="description-sub">
                  坑都帮你踩过了，再学不会我也没办法了，快上车！
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <section ref="projectsSection" class="ai-projects-section mt-8">
      <div class="container mx-auto px-4 sm:px-6 lg:px-8">

        <div v-if="isLoading" class="loading-container">
          <div class="loading-spinner"></div>
          <p class="loading-text">正在疯狂挖掘 AI 金矿...</p>
        </div>

        <div v-else-if="articles.length" class="projects-grid">
          <article
            v-for="(article, index) in articles"
            :key="article.id"
            :class="['ai-project-card', `card-delay-${index % 4}`]"
            @click="openArticle(article)"
            :style="{ animationDelay: `${index * 100}ms` }"
          >
            <div class="card-content">
              <!-- 封面图片区域 - 主要视觉焦点 -->
              <div class="card-media">
                <div v-if="article.coverImage" class="card-image">
                  <img
                    :src="getCoverImageUrl(article.coverImage)"
                    :alt="article.title"
                    class="cover-img"
                    loading="lazy"
                  />
                  <div class="image-overlay">
                    <div class="view-btn">
                      <el-icon><View /></el-icon>
                      <span>查看项目</span>
                    </div>
                  </div>
                </div>
                <div v-else class="card-placeholder">
                  <div class="placeholder-icon">📁</div>
                  <div class="placeholder-text">项目封面</div>
                </div>

                <!-- 状态徽章 -->
                <div class="card-badges">
                  <span v-if="article.isTop" class="badge badge-top">
                    <el-icon><Star /></el-icon>
                    置顶
                  </span>
                  <span v-if="article.isRecommend" class="badge badge-recommend">
                    <el-icon><Trophy /></el-icon>
                    推荐
                  </span>
                  <span v-if="isVipArticle(article)" class="badge badge-vip">
                    <el-icon><Medal /></el-icon>
                    {{ getAccessLevelText(article) }}
                  </span>
                </div>

                <!-- 金钱装饰元素 -->
                <div class="money-decorations">
                  <div class="floating-coin coin-1">💰</div>
                  <div class="floating-coin coin-2">💵</div>
                  <div class="floating-coin coin-3">🪙</div>
                  <div class="money-sparkles">
                    <div class="sparkle sparkle-1">✨</div>
                    <div class="sparkle sparkle-2">⭐</div>
                    <div class="sparkle sparkle-3">💫</div>
                  </div>
                </div>
              </div>

              <!-- 主要内容区域 -->
              <div class="card-main">
                <!-- 分类标签 -->
                <div class="card-category">
                  <div class="category-dot"></div>
                  <span>{{ article.categoryName || '搞钱项目' }}</span>
                </div>

                <!-- 标题和描述 -->
                <div class="card-content-body">
                  <h3 class="card-title">{{ article.title }}</h3>
                  <p class="card-description">
                    {{ article.summary || '点击查看项目详细信息，了解更多内容。' }}
                  </p>
                </div>

                <!-- 底部操作按钮 -->
                <div class="card-footer">
                  <div class="card-action">
                    <span class="action-icon-start">💎</span>
                    <span>立即赚钱</span>
                    <el-icon class="action-icon"><ArrowRight /></el-icon>
                    <span class="action-icon-end">💰</span>
                  </div>
                </div>
              </div>
            </div>
          </article>
        </div>

        <div v-else class="empty-state">
          <div class="empty-icon">🤖</div>
          <h3 class="empty-title">金矿还是空的</h3>
          <p class="empty-description">
            还没人来挖矿，赶紧来占个坑！
          </p>
          <router-link to="/editor" class="empty-action">
            <span>我是矿主，我要发项目</span>
            <ArrowRight />
          </router-link>
        </div>

        <div v-if="showInfiniteTrigger" ref="infiniteScrollTrigger" class="h-4 w-full"></div>
        <div v-if="isLoadingMore" class="py-8 text-center">
          <div class="inline-flex items-center gap-2 text-sm text-slate-500">
            <div class="loading-spinner-small"></div>
            <span>正在加载更多野路子...</span>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import { ArrowRight, View, Star, Trophy, Medal } from '@element-plus/icons-vue'

interface Article {
  id: string
  title: string
  summary: string
  content: string
  coverImage?: string
  categoryName?: string
  authorName?: string
  viewCount: number
  publishedAt?: string
  accessLevel?: number
  isTop?: boolean
  isRecommend?: boolean
  raw?: any
}

interface Props {
  articles: Article[]
  isLoading: boolean
  isLoadingMore: boolean
  showInfiniteTrigger: boolean
}

const props = defineProps<Props>()

const emit = defineEmits<{
  'open-article': [article: Article]
  'load-more': []
}>()

const projectsSection = ref<HTMLElement | null>(null)
const infiniteScrollTrigger = ref<HTMLElement | null>(null)

// Methods
const openArticle = (article: Article) => {
  emit('open-article', article)
}

const scrollToProjects = () => {
  projectsSection.value?.scrollIntoView({
    behavior: 'smooth',
    block: 'start'
  })
}

const formatDate = (date?: string) => {
  if (!date) return '日期未知'
  const time = new Date(date)
  if (Number.isNaN(time.getTime())) return '日期未知'
  return time.toLocaleDateString('zh-CN', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const getAccessLevel = (article: Article): number => {
  if (typeof article.accessLevel === 'number' && Number.isFinite(article.accessLevel)) {
    return article.accessLevel
  }
  const rawLevel = article.raw?.accessLevel ?? article.raw?.access_level ?? article.raw?.vipLevel
  const parsed = Number(rawLevel)
  return Number.isFinite(parsed) ? parsed : 0
}

const isVipArticle = (article: Article): boolean => getAccessLevel(article) > 0

const getAccessLevelText = (article: Article): string => {
  const level = getAccessLevel(article)
  switch (level) {
    case 1:
      return '会员专享'
    case 2:
      return 'VIP专享'
    default:
      return '会员内容'
  }
}

const formatNumber = (num: number) => {
  if (num >= 10000) {
    return (num / 10000).toFixed(1) + 'w'
  }
  if (num >= 1000) {
    return (num / 1000).toFixed(1) + 'k'
  }
  return num.toString()
}

const isAbsoluteUrl = (url: string) =>
  /^(https?:)?\/\//i.test(url) || url.startsWith('data:')

const getCoverImageUrl = (coverImage?: string): string => {
  if (!coverImage) return ''

  if (isAbsoluteUrl(coverImage)) {
    return coverImage
  }

  let processedPath = coverImage
  if (coverImage.startsWith('/api/upload/upload/')) {
    processedPath = coverImage.replace('/api/upload/upload/', 'upload/')
  } else if (coverImage.startsWith('/api/upload/')) {
    processedPath = coverImage.replace('/api/upload/', 'upload/')
  }

  const domain = import.meta.env?.VITE_FILE_BASE_URL?.replace(/\/$/, '')
  if (domain) {
    const normalizedPath = processedPath.startsWith('/') ? processedPath.slice(1) : processedPath
    return `${domain}/${normalizedPath}`
  }

  return processedPath
}


// Emit load-more when trigger is visible
const setupIntersectionObserver = () => {
  if (!infiniteScrollTrigger.value) return

  const observer = new IntersectionObserver(
    (entries) => {
      if (entries.some(entry => entry.isIntersecting)) {
        emit('load-more')
      }
    },
    {
      root: null,
      rootMargin: '0px 0px 200px 0px',
      threshold: 0
    }
  )

  observer.observe(infiniteScrollTrigger.value)

  return () => observer.disconnect()
}

onMounted(() => {
  const cleanup = setupIntersectionObserver()
  return cleanup
})
</script>

<style scoped>
/* Hero Section Styles */
.ai-gold-showcase {
  @apply min-h-screen;
}

.ai-hero-section {
  @apply relative overflow-hidden bg-gradient-to-br from-neutral-900 via-stone-900 to-zinc-900;
  min-height: 150px;
  padding-top: 2rem;
  border-radius: 2rem;
  box-shadow: 0 20px 50px -12px rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 215, 0, 0.1);
}

.ai-particles-bg {
  position: absolute;
  inset: 0;
  background-image:
    radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.1) 0, transparent 40%),
    radial-gradient(circle at 80% 30%, rgba(234, 179, 8, 0.2) 0, transparent 45%),
    radial-gradient(circle at 50% 70%, rgba(217, 119, 6, 0.15) 0, transparent 40%);
  opacity: 0.8;
  animation: floatParticles 18s ease-in-out infinite;
}

.ai-gradient-mesh {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 150px;
  background:
    radial-gradient(circle at 25% 25%, rgba(250, 204, 21, 0.15) 0, transparent 50%),
    radial-gradient(circle at 75% 75%, rgba(251, 191, 36, 0.15) 0, transparent 45%),
    radial-gradient(circle at 50% 50%, rgba(252, 211, 77, 0.1) 0, transparent 40%);
  filter: blur(80px);
  animation: pulseGlow 15s ease-in-out infinite;
}

.ai-floating-elements {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.floating-element {
  position: absolute;
  border-radius: 50%;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05));
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.element-1 {
  top: 10%;
  left: 15%;
  width: 60px;
  height: 60px;
  animation: floatUp 20s ease-in-out infinite;
}

.element-2 {
  top: 60%;
  right: 20%;
  width: 40px;
  height: 40px;
  animation: floatUp 15s ease-in-out infinite 2s;
}

.element-3 {
  bottom: 20%;
  left: 10%;
  width: 30px;
  height: 30px;
  animation: floatUp 18s ease-in-out infinite 4s;
}

.element-4 {
  top: 30%;
  right: 10%;
  width: 50px;
  height: 50px;
  animation: floatUp 22s ease-in-out infinite 6s;
}


/* Description Styles */
.ai-description {
  @apply space-y-4;
}

.description-main {
  @apply text-xl sm:text-2xl text-amber-50 font-semibold leading-relaxed;
  text-shadow: 0 2px 20px rgba(0, 0, 0, 0.5);
}

.description-sub {
  @apply text-base sm:text-lg text-amber-100/70 leading-relaxed max-w-4xl mx-auto;
  text-shadow: 0 1px 15px rgba(0, 0, 0, 0.5);
}

/* Hero Banner Styles */
.ai-hero-banner {
  @apply relative pt-0 pb-8 px-6 rounded-3xl;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.05), rgba(0, 0, 0, 0.2));
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 215, 0, 0.15);
  overflow: hidden;
}

.banner-content {
  @apply relative z-10 text-center;
}

.banner-title-line {
  @apply flex flex-wrap items-center justify-center gap-2 lg:gap-4 mb-4;
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 800;
  line-height: 1.1;
  letter-spacing: -0.02em;
}

.banner-highlight {
  background: linear-gradient(135deg, #fcd34d 0%, #eab308 50%, #d97706 100%);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  position: relative;
  animation: highlightGlow 3s ease-in-out infinite;
  filter: drop-shadow(0 2px 4px rgba(234, 179, 8, 0.3));
}

.banner-highlight::after {
  content: '';
  position: absolute;
  inset: -4px;
  background: radial-gradient(circle at center, rgba(234, 179, 8, 0.3), transparent 70%);
  border-radius: 50%;
  z-index: -1;
  animation: highlightPulse 3s ease-in-out infinite;
}

.banner-text {
  color: rgba(255, 255, 255, 0.95);
  text-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
}

.banner-separator {
  color: rgba(255, 255, 255, 0.4);
  font-weight: 400;
  animation: separatorPulse 2s ease-in-out infinite;
}

.banner-subtitle {
  font-size: clamp(1.2rem, 3vw, 1.8rem);
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  text-shadow: 0 4px 30px rgba(0, 0, 0, 0.4);
  letter-spacing: 0.02em;
  position: relative;
  display: inline-block;
}

.banner-subtitle::before,
.banner-subtitle::after {
  content: '💰';
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 1.2em;
  animation: coinFloat 4s ease-in-out infinite;
}

.banner-subtitle::before {
  left: -2.5em;
  animation-delay: 0s;
}

.banner-subtitle::after {
  right: -2.5em;
  animation-delay: 1s;
}

.banner-decoration {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.decoration-dots {
  position: absolute;
  top: 10%;
  left: 5%;
  width: 80px;
  height: 80px;
  background-image: radial-gradient(circle, rgba(255, 255, 255, 0.3) 2px, transparent 2px);
  background-size: 16px 16px;
  animation: floatDecoration 15s ease-in-out infinite;
}

.decoration-lines {
  position: absolute;
  bottom: 15%;
  right: 8%;
  width: 100px;
  height: 3px;
  background: linear-gradient(90deg, transparent, rgba(251, 191, 36, 0.6), transparent);
  animation: lineGlow 4s ease-in-out infinite;
}

.decoration-lines::before,
.decoration-lines::after {
  content: '';
  position: absolute;
  width: 60px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
  top: -8px;
  animation: lineGlow 4s ease-in-out infinite;
}

.decoration-lines::before {
  left: -20px;
  animation-delay: 1s;
}

.decoration-lines::after {
  right: -20px;
  animation-delay: 2s;
  top: 8px;
}

/* Banner Animations */
@keyframes highlightGlow {
  0%, 100% {
    filter: brightness(1) drop-shadow(0 0 20px rgba(251, 191, 36, 0.5));
  }
  50% {
    filter: brightness(1.2) drop-shadow(0 0 30px rgba(251, 191, 36, 0.8));
  }
}

@keyframes highlightPulse {
  0%, 100% {
    opacity: 0.3;
    transform: scale(1);
  }
  50% {
    opacity: 0.6;
    transform: scale(1.1);
  }
}

@keyframes separatorPulse {
  0%, 100% {
    opacity: 0.4;
    transform: scale(1);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.2);
  }
}

@keyframes coinFloat {
  0%, 100% {
    transform: translateY(-50%) rotate(0deg);
  }
  25% {
    transform: translateY(-60%) rotate(90deg);
  }
  50% {
    transform: translateY(-50%) rotate(180deg);
  }
  75% {
    transform: translateY(-40%) rotate(270deg);
  }
}

@keyframes floatDecoration {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-15px) rotate(180deg);
  }
}

@keyframes lineGlow {
  0%, 100% {
    opacity: 0.3;
    box-shadow: 0 0 5px rgba(251, 191, 36, 0.3);
  }
  50% {
    opacity: 0.8;
    box-shadow: 0 0 20px rgba(251, 191, 36, 0.6);
  }
}



/* Projects Section */
.ai-projects-section {
  @apply pt-12 pb-8 bg-gradient-to-b from-slate-50 via-white to-blue-50;
}

/* Loading States */
.loading-container {
  @apply flex flex-col items-center justify-center py-20;
}

.loading-spinner {
  @apply w-12 h-12 border-4 border-blue-200 border-t-blue-600 rounded-full animate-spin mb-4;
}

.loading-text {
  @apply text-slate-500 text-lg;
}

.loading-spinner-small {
  @apply w-4 h-4 border-2 border-slate-200 border-t-slate-600 rounded-full animate-spin;
}

/* Projects Grid */
.projects-grid {
  @apply grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-6;
}

/* Modern Project Cards */
.ai-project-card {
  @apply relative rounded-2xl overflow-hidden cursor-pointer transition-all duration-300;
  opacity: 0;
  animation: fadeInUp 0.6s ease-out forwards;
  background: #ffffff;
  border: 1px solid #e5e7eb;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  height: 280px;
  display: flex;
  flex-direction: column;
}

.ai-project-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  border-color: #f59e0b;
}

.card-content {
  @apply flex flex-col h-full;
}

/* Card Media Section */
.card-media {
  @apply relative flex-shrink-0;
  height: 160px;
  overflow: hidden;
}

.card-image {
  @apply relative w-full h-full;
}

.cover-img {
  @apply w-full h-full object-cover;
  transition: transform 0.3s ease;
}

.ai-project-card:hover .cover-img {
  transform: scale(1.05);
}

.image-overlay {
  @apply absolute inset-0 bg-gradient-to-t from-black/60 via-black/20 to-transparent opacity-0 transition-opacity duration-300;
  display: flex;
  align-items: center;
  justify-content: center;
}

.ai-project-card:hover .image-overlay {
  opacity: 1;
}

.view-btn {
  @apply flex items-center gap-2 px-4 py-2 bg-white/90 backdrop-blur-sm rounded-lg text-gray-900 font-medium;
  transform: translateY(10px);
  transition: transform 0.3s ease;
}

.ai-project-card:hover .view-btn {
  transform: translateY(0);
}

.card-placeholder {
  @apply w-full h-full bg-gradient-to-br from-gray-100 to-gray-200 flex flex-col items-center justify-center;
  border-bottom: 1px solid #e5e7eb;
}

.placeholder-icon {
  font-size: 48px;
  opacity: 0.5;
  margin-bottom: 8px;
}

.placeholder-text {
  @apply text-sm text-gray-500;
}

/* Badges */
.card-badges {
  @apply absolute top-3 left-3 flex gap-2 z-10;
}

.badge {
  @apply inline-flex items-center gap-1 px-2 py-1 rounded-full text-xs font-medium;
  backdrop-filter: blur(8px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.badge-top {
  background: rgba(251, 191, 36, 0.9);
  color: white;
}

.badge-recommend {
  background: rgba(59, 130, 246, 0.9);
  color: white;
}

.badge-vip {
  background: linear-gradient(135deg, #f59e0b, #ea580c);
  color: white;
  box-shadow: 0 4px 6px -1px rgba(245, 158, 11, 0.3);
}

/* 金钱装饰元素 */
.money-decorations {
  position: absolute;
  inset: 0;
  pointer-events: none;
  overflow: hidden;
}

.floating-coin {
  position: absolute;
  font-size: 20px;
  animation: float 4s ease-in-out infinite;
  z-index: 5;
  filter: drop-shadow(0 2px 4px rgba(251, 191, 36, 0.3));
}

.coin-1 {
  top: 20%;
  right: 15%;
  animation-delay: 0s;
  animation-duration: 3.5s;
}

.coin-2 {
  top: 60%;
  right: 25%;
  animation-delay: 1.5s;
  animation-duration: 4s;
  font-size: 18px;
}

.coin-3 {
  bottom: 30%;
  right: 10%;
  animation-delay: 2.5s;
  animation-duration: 3s;
  font-size: 16px;
}

.money-sparkles {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.sparkle {
  position: absolute;
  font-size: 14px;
  animation: sparkle 2s ease-in-out infinite;
  z-index: 4;
}

.sparkle-1 {
  top: 30%;
  left: 20%;
  animation-delay: 0.5s;
}

.sparkle-2 {
  top: 50%;
  left: 15%;
  animation-delay: 1.2s;
  font-size: 12px;
}

.sparkle-3 {
  bottom: 40%;
  left: 25%;
  animation-delay: 1.8s;
  font-size: 16px;
}

/* Card Main Content */
.card-main {
  @apply flex-1 flex flex-col p-4;
}

.card-category {
  @apply flex items-center gap-2 text-xs font-medium text-gray-600 mb-2;
}

.category-dot {
  @apply w-2 h-2 rounded-full bg-gradient-to-r from-amber-400 to-orange-500;
}

.card-content-body {
  @apply flex-1 mb-3;
}

.card-title {
  @apply text-base font-semibold text-gray-900 mb-2 line-clamp-2 leading-tight;
}

.card-description {
  @apply text-sm text-gray-600 line-clamp-2 leading-relaxed;
}

/* Card Footer */
.card-footer {
  @apply flex justify-center pt-4 pb-1 border-t border-gray-100 px-4;
}

.card-action {
  @apply w-full flex items-center justify-center gap-2 px-4 py-2.5 bg-gradient-to-r from-slate-900 to-slate-800 hover:from-black hover:to-slate-900 border border-amber-500/30 rounded-xl text-sm font-bold text-amber-400 transition-all duration-300 shadow-md;
  position: relative;
  overflow: hidden;
}

.ai-project-card:hover .card-action {
  @apply border-amber-400 shadow-lg shadow-amber-500/20;
  transform: translateY(-2px);
}

.action-icon {
  @apply w-4 h-4 text-amber-300;
}

.action-icon-start {
  animation: bounce 2s ease-in-out infinite;
  filter: drop-shadow(0 0 5px rgba(251, 191, 36, 0.6));
}

.action-icon-end {
  animation: bounce 2s ease-in-out infinite 0.5s;
  filter: drop-shadow(0 0 5px rgba(251, 191, 36, 0.6));
}


/* Tech Card Animations */
@keyframes techGradient {
  0%, 100% {
    background-position: 0% 50%;
  }
  25% {
    background-position: 100% 0%;
  }
  50% {
    background-position: 100% 100%;
  }
  75% {
    background-position: 0% 100%;
  }
}

@keyframes goldShine {
  0%, 100% {
    background-position: -100% -100%;
  }
  50% {
    background-position: 100% 100%;
  }
}

@keyframes glowPulse {
  0%, 100% {
    opacity: 0.7;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.05);
  }
}

@keyframes gridMove {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(20px, 20px);
  }
}

@keyframes categoryPulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.7;
  }
}

@keyframes coinFloat {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  25% {
    transform: translateY(-4px) rotate(90deg);
  }
  50% {
    transform: translateY(-2px) rotate(180deg);
  }
  75% {
    transform: translateY(-6px) rotate(270deg);
  }
}

@keyframes coinSpin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes coinShine {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes vipGlow {
  0%, 100% {
    box-shadow: 0 3px 12px rgba(217, 119, 6, 0.4);
  }
  50% {
    box-shadow: 0 3px 16px rgba(217, 119, 6, 0.6);
  }
}

/* Empty State */
.empty-state {
  @apply text-center py-20;
}

.empty-icon {
  @apply text-6xl mb-4;
}

.empty-title {
  @apply text-2xl font-semibold text-slate-900 mb-2;
}

.empty-description {
  @apply text-slate-600 mb-6 max-w-md mx-auto;
}

.empty-action {
  @apply inline-flex items-center gap-2 px-6 py-3 rounded-2xl bg-gradient-to-r from-blue-600 to-indigo-600 text-white font-semibold hover:from-blue-700 hover:to-indigo-700 transition-all duration-300;
}

/* Animations */
@keyframes floatParticles {
  0%, 100% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

@keyframes pulseGlow {
  0%, 100% { opacity: 0.3; }
  50% { opacity: 0.7; }
}

@keyframes floatUp {
  0% { transform: translateY(100px) rotate(0deg); opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { transform: translateY(-100px) rotate(360deg); opacity: 0; }
}

@keyframes fadeInUp {
  0% {
    opacity: 0;
    transform: translateY(30px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* 金钱元素动画 */
@keyframes float {
  0%, 100% {
    transform: translateY(0px) rotate(0deg);
    opacity: 0.8;
  }
  25% {
    transform: translateY(-5px) rotate(90deg);
    opacity: 1;
  }
  50% {
    transform: translateY(-8px) rotate(180deg);
    opacity: 0.9;
  }
  75% {
    transform: translateY(-3px) rotate(270deg);
    opacity: 1;
  }
}

@keyframes sparkle {
  0%, 100% {
    transform: scale(1) rotate(0deg);
    opacity: 0;
  }
  25% {
    transform: scale(1.2) rotate(90deg);
    opacity: 1;
  }
  50% {
    transform: scale(1.4) rotate(180deg);
    opacity: 0.8;
  }
  75% {
    transform: scale(1.1) rotate(270deg);
    opacity: 1;
  }
}

@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: translate3d(0,0,0);
  }
  40%, 43% {
    transform: translate3d(0, -8px, 0);
  }
  70% {
    transform: translate3d(0, -4px, 0);
  }
  90% {
    transform: translate3d(0, -2px, 0);
  }
}

/* Card animation delays */
.card-delay-0 { animation-delay: 0ms; }
.card-delay-1 { animation-delay: 100ms; }
.card-delay-2 { animation-delay: 200ms; }
.card-delay-3 { animation-delay: 300ms; }

/* Responsive Design */
@media (max-width: 768px) {
  .ai-hero-section {
    min-height: 500px;
  }

  .projects-grid {
    @apply grid-cols-1 gap-4;
  }

  .ai-project-card {
    @apply rounded-2xl;
  }

  .card-content {
    @apply p-5;
  }

  .card-title {
    @apply text-lg;
  }

  .card-cover {
    @apply h-32;
  }
}

@media (max-width: 640px) {
  .ai-stats-dashboard {
    @apply p-4;
  }

  .ai-stat-card {
    @apply p-4;
  }

  .stat-value {
    @apply text-2xl;
  }
}
</style>
