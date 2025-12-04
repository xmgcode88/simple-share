<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-50 via-white to-blue-50 pt-8 pb-12">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-8">
      <header v-if="false" class="relative overflow-hidden rounded-2xl shadow-xl border border-gray-100 bg-white/5 backdrop-blur-sm">
        <!-- 优化后的动态背景 -->
        <div class="absolute inset-0 bg-gradient-to-br" :class="bannerThemes[activeBannerTheme].backgroundGradient">
          <!-- 简化的装饰元素 -->
          <div class="absolute top-0 right-0 w-64 h-64 bg-white/8 rounded-full blur-2xl"></div>
          <div class="absolute bottom-0 left-0 w-48 h-48 bg-white/5 rounded-full blur-xl"></div>
          
          <!-- 微妙的网格背景 -->
          <div class="absolute inset-0 opacity-5">
            <div class="absolute inset-0" style="background-image: radial-gradient(circle at 1px 1px, rgba(255,255,255,0.4) 1px, transparent 0); background-size: 24px 24px;"></div>
          </div>
        </div>

        <!-- 主题切换标签 - 更紧凑的设计 -->
        <div class="relative flex border-b border-white/15 backdrop-blur-sm">
          <button
            v-for="(theme, index) in bannerThemes"
            :key="index"
            @click="activeBannerTheme = index"
            class="flex-1 px-4 py-3 text-center font-medium transition-all duration-300 relative text-sm hover:bg-white/8"
            :class="activeBannerTheme === index 
              ? 'text-white bg-white/15 shadow-sm backdrop-blur-sm' 
              : 'text-white/75 hover:text-white'"
          >
            <span class="text-base">{{ theme.icon }}</span>
            <span class="ml-2 hidden sm:inline">{{ theme.title }}</span>
            <div v-if="activeBannerTheme === index" class="absolute bottom-0 left-0 right-0 h-0.5 bg-white/60 rounded-full"></div>
          </button>
        </div>

        <!-- 主题内容 - 减少padding -->
        <div class="relative p-6 md:p-8">
          <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-6">
            <!-- 左侧内容 - 优化布局 -->
            <div class="flex-1 space-y-4">
              <div class="space-y-3">
                <!-- 标签区域 - 更简洁 -->
                <div class="flex items-center gap-2 flex-wrap">
                  <span class="px-3 py-1.5 rounded-full text-white text-xs font-medium bg-white/15 backdrop-blur-sm border border-white/25">
                    {{ bannerThemes[activeBannerTheme].icon }} {{ bannerThemes[activeBannerTheme].badge }}
                  </span>
                  <span class="px-3 py-1.5 rounded-full bg-white/15 backdrop-blur-sm border border-white/25 text-white text-xs font-medium">
                    💎 精选推荐
                  </span>
                </div>
                
                <!-- 标题区域 - 更好的层次 -->
                <div class="space-y-2">
                  <h1 class="text-3xl md:text-4xl lg:text-5xl font-bold text-white leading-tight">
                    {{ bannerThemes[activeBannerTheme].title }}
                  </h1>
                  <h2 class="text-xl md:text-2xl lg:text-3xl text-white/90 font-semibold">
                    {{ bannerThemes[activeBannerTheme].subtitle }}
                  </h2>
                </div>
                
                <!-- 描述文本 -->
                <p class="text-base md:text-lg text-white/85 leading-relaxed max-w-2xl">
                  {{ bannerThemes[activeBannerTheme].description }}
                </p>
                
                <!-- 特色标签 - 更紧凑 -->
                <div class="flex flex-wrap gap-2">
                  <div 
                    v-for="(feature, idx) in bannerThemes[activeBannerTheme].features"
                    :key="idx"
                    class="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-white/12 backdrop-blur-sm border border-white/20 text-white text-xs transition-all duration-200 hover:bg-white/20"
                  >
                    <span class="w-1.5 h-1.5 rounded-full bg-white/70"></span>
                    <span>{{ feature }}</span>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- 右侧操作按钮 - 优化尺寸 -->
            <div class="flex flex-col sm:flex-row lg:flex-col gap-3 lg:min-w-[200px]">
              <router-link
                to="/articles"
                class="inline-flex items-center justify-center gap-2 px-6 py-3 rounded-xl text-white font-semibold text-base shadow-lg hover:shadow-xl hover:scale-105 transition-all duration-300 bg-white/20 backdrop-blur-sm border border-white/30 hover:bg-white/25"
              >
                <span>🚀</span>
                <span>立即探索</span>
                <el-icon>
                  <ArrowRight />
                </el-icon>
              </router-link>
              <router-link
                to="/editor"
                class="inline-flex items-center justify-center gap-2 px-6 py-3 rounded-xl border border-white/30 font-medium text-base hover:bg-white/10 transition-all duration-300 text-white backdrop-blur-sm"
              >
                <span>✍️</span>
                <span>分享项目</span>
              </router-link>
            </div>
          </div>
        </div>
        
        <!-- 简化的装饰性浮动元素 -->
        <div class="absolute top-16 right-16 w-2 h-2 bg-white/40 rounded-full animate-pulse"></div>
        <div class="absolute bottom-16 left-16 w-1.5 h-1.5 bg-white/30 rounded-full animate-pulse" style="animation-delay: 1s;"></div>
      </header>

      <div v-if="isAIGoldCategory" class="space-y-6">
        <AIGoldShowcase
          :articles="aiGoldArticles"
          :is-loading="isListLoading"
          :is-loading-more="isLoadingMore"
          :show-infinite-trigger="showInfiniteTrigger"
          @open-article="openArticle"
          @load-more="loadMore"
        />
      </div>

      <div v-else class="grid grid-cols-1 lg:grid-cols-12 gap-8">
        <section class="lg:col-span-8 space-y-6">
          <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4 bg-white/70 backdrop-blur rounded-2xl p-5 shadow-sm">
            <div class="flex items-center gap-3">
              <button
                class="px-4 py-2 rounded-xl text-sm font-medium transition-all"
                :class="activeFilter === 'all' ? 'bg-slate-900 text-white shadow' : 'bg-white/80 text-slate-600 hover:bg-white'"
                @click="activeFilter = 'all'"
              >
                全部内容
              </button>
              <button
                class="px-4 py-2 rounded-xl text-sm font-medium transition-all"
                :class="activeFilter === 'featured' ? 'bg-slate-900 text-white shadow' : 'bg-white/80 text-slate-600 hover:bg-white'"
                @click="activeFilter = 'featured'"
              >
                精选推荐
              </button>
            </div>
          </div>

          <div v-if="isListLoading" class="flex justify-center items-center py-12 bg-white/70 backdrop-blur rounded-2xl shadow-sm">
            <span class="text-sm text-slate-500">正在加载文章...</span>
          </div>
          <div v-else-if="filteredArticles.length" class="space-y-2">
            <article
              v-for="article in filteredArticles"
              :key="article.id"
              @click="openArticle(article)"
              :class="[
                'group bg-white/80 backdrop-blur rounded-3xl overflow-hidden shadow transition-all duration-300 cursor-pointer border hover:border-blue-100 h-[118px]',
                article.isTop ? 'border-amber-300 shadow-lg shadow-amber-100/70 ring-1 ring-amber-100' : '',
                !article.isTop && article.isRecommend ? 'border-blue-200 ring-1 ring-blue-100/60' : ''
              ]"
            >
              <div class="flex h-full">
                <div
                  v-if="article.coverImage"
                  class="relative order-2 h-full w-28 sm:w-36 md:w-48 lg:w-56 overflow-hidden"
                >
                  <img
                    :src="getCoverImageUrl(article.coverImage)"
                    :alt="article.title"
                    class="absolute inset-0 w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                  />
                </div>
                <div class="flex-1 order-1 flex flex-col px-3 py-2 sm:px-3 sm:py-2 md:px-4 md:py-3 gap-2 overflow-hidden">
                  <div class="flex items-center gap-2 text-[11px] sm:text-xs flex-wrap">
                    <span
                      v-if="article.categoryName"
                      class="category-chip"
                    >
                      {{ article.categoryName }}
                    </span>
                    <span
                      v-if="article.isTop"
                      class="article-badge article-badge--top"
                    >
                      置顶
                    </span>
                    <span
                      v-if="article.isRecommend"
                      class="article-badge article-badge--recommend"
                    >
                      推荐
                    </span>
                  </div>
                  <div class="flex flex-col justify-between h-full">
                    <div class="space-y-1 overflow-hidden">
                      <h3 class="text-base sm:text-lg font-semibold text-slate-900 leading-tight line-clamp-1 group-hover:text-blue-600 transition-colors">
                        {{ article.title }}
                      </h3>
                      <p class="text-xs sm:text-sm text-slate-600 leading-relaxed line-clamp-2">
                        {{ article.summary || '这篇内容尚未添加摘要，请进入文章详情查看全部信息。' }}
                      </p>
                    </div>
                    <div class="flex items-center justify-between gap-3 text-[11px] sm:text-xs text-slate-500">
                      <div class="flex flex-wrap items-center gap-x-2 gap-y-1">
                        <span class="inline-flex items-center gap-1">
                          <el-icon class="text-blue-500">
                            <View />
                          </el-icon>
                          {{ article.viewCount || 0 }} 次浏览
                        </span>
                        <span>{{ article.authorName || '匿名作者' }}</span>
                        <span>{{ formatDate(article.publishedAt) }}</span>
                        <span
                          v-if="isVipArticle(article)"
                          class="inline-flex items-center gap-1 text-amber-600"
                        >
                          <el-icon class="text-xs">
                            <Star />
                          </el-icon>
                          {{ getAccessLevelText(article) }}
                        </span>
                      </div>
                      <button
                        class="inline-flex items-center gap-1 px-3 py-1 rounded-lg bg-blue-50 text-blue-600 font-semibold hover:bg-blue-100 transition-all text-xs"
                        @click.stop="openArticle(article)"
                      >
                        阅读
                        <el-icon>
                          <ArrowRight />
                        </el-icon>
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </article>
          <div v-if="showInfiniteTrigger" ref="infiniteScrollTrigger" class="h-2 w-full"></div>
          <div v-if="isLoadingMore" class="py-6 text-center text-sm text-slate-500">正在加载更多内容...</div>
          </div>

          <div v-else class="bg-white/70 backdrop-blur rounded-3xl p-16 text-center text-slate-500 shadow-sm">
            当前筛选下没有文章，尝试选择其他TAB或前往创作中心发布新内容。
          </div>
        </section>

        <aside class="lg:col-span-4 space-y-6">
          <div class="bg-white/80 backdrop-blur rounded-3xl p-6 shadow-lg">
            <div class="flex items-center justify-between mb-4">
              <h2 class="text-lg font-semibold text-slate-900">精选推荐</h2>
              <span class="text-xs text-slate-500">实时</span>
            </div>
            <div class="rounded-2xl bg-purple-50 text-purple-700 p-4">
              <div class="text-xs text-purple-500">精选内容</div>
              <div class="text-2xl font-bold">{{ featuredList.length }}</div>
            </div>
          </div>

          <div v-if="featuredList.length" class="bg-white/80 backdrop-blur rounded-3xl p-6 shadow-lg space-y-4">
            <div class="flex items-center justify-between">
              <h2 class="text-lg font-semibold text-slate-900">精选推荐</h2>
              <router-link to="/articles" class="text-xs text-blue-600 hover:text-blue-700">全部</router-link>
            </div>
            <ul class="space-y-4">
              <li
                v-for="article in featuredList"
                :key="article.id"
                class="group rounded-2xl border border-transparent hover:border-blue-100 transition-all"
              >
                <button
                  type="button"
                  class="w-full text-left p-4 flex flex-col gap-2"
                  @click="openArticle(article)"
                >
                  <div class="flex items-center justify-between gap-3">
                    <span class="text-sm font-semibold text-slate-900 line-clamp-2 group-hover:text-blue-600">
                      {{ article.title }}
                    </span>
                    <div class="flex items-center gap-2">
                      <div v-if="isVipArticle(article)" class="vip-badge vip-badge--small" :title="getAccessLevelText(article)">
                          <el-icon>
                            <Star />
                          </el-icon>
                        </div>
                      <span class="text-[10px] px-2 py-1 rounded-full bg-blue-50 text-blue-600">
                        {{ article.categoryName || '专栏' }}
                      </span>
                    </div>
                  </div>
                  <div class="text-xs text-slate-500">
                    {{ formatDate(article.publishedAt) }} · {{ article.viewCount || 0 }} 次浏览
                  </div>
                </button>
              </li>
            </ul>
          </div>

          <div class="bg-white/80 backdrop-blur rounded-3xl p-6 shadow-lg">
            <h2 class="text-lg font-semibold text-slate-900 mb-3">快速入口</h2>
            <div class="space-y-3 text-sm text-slate-600">
              <router-link to="/articles" class="block px-4 py-3 rounded-2xl bg-blue-50 text-blue-600 font-medium hover:bg-blue-100">
                所有文章列表
              </router-link>
              <router-link to="/editor" class="block px-4 py-3 rounded-2xl bg-purple-50 text-purple-600 font-medium hover:bg-purple-100">
                新建文章
              </router-link>
              <router-link to="/aitech" class="block px-4 py-3 rounded-2xl bg-orange-50 text-orange-600 font-medium hover:bg-orange-100">
                AI 教程
              </router-link>
            </div>
          </div>
        </aside>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onActivated, onBeforeUnmount, onMounted, reactive, ref, watch } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { usePublicCategoryStore } from '@/stores/publicCategory'
import { articleApi } from '@/api'
import { ArticleStatus } from '@/types/article'
import { ArrowRight, View, Star } from '@element-plus/icons-vue'
import type { Category } from '@/types/category'
import { ensureAbsoluteUrl } from '@/utils/url'
import { buildFileUrl } from '@/utils/file'
import AIGoldShowcase from '@/components/AIGoldShowcase.vue'

const route = useRoute()
const router = useRouter()
const categoryStore = usePublicCategoryStore()

const activeBannerTheme = ref(0)

// 三个主题配置
const bannerThemes = ref([])

interface PortalArticle {
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

const parseBooleanFlag = (value: unknown): boolean | undefined => {
  if (value === null || value === undefined) {
    return undefined
  }

  if (typeof value === 'boolean') {
    return value
  }

  if (typeof value === 'number') {
    if (Number.isNaN(value)) {
      return undefined
    }
    if (value === 1) {
      return true
    }
    if (value === 0) {
      return false
    }
    return undefined
  }

  if (typeof value === 'string') {
    const normalized = value.trim().toLowerCase()
    if (!normalized) {
      return undefined
    }

    if (['1', 'true', 'yes', 'y', 'on', '是', '置顶', '推荐'].includes(normalized)) {
      return true
    }
    if (['0', 'false', 'no', 'n', 'off', '否'].includes(normalized)) {
      return false
    }
  }

  return undefined
}

const asArray = (value: unknown): any[] => {
  if (Array.isArray(value)) {
    return value
  }
  if (value && Array.isArray((value as any).items)) {
    return (value as any).items
  }
  if (value && Array.isArray((value as any).list)) {
    return (value as any).list
  }
  if (value && Array.isArray((value as any).rows)) {
    return (value as any).rows
  }
  if (value && Array.isArray((value as any).data)) {
    return (value as any).data
  }
  if (value && Array.isArray((value as any).records)) {
    return (value as any).records
  }
  return []
}

const normalizeArticle = (article: any): PortalArticle | null => {
  if (!article) {
    return null
  }

  const idSource =
    article.id ??
    article.articleId ??
    article.articleID ??
    article.article_id ??
    article.uuid ??
    article.slug

  if (idSource === undefined || idSource === null) {
    return null
  }

  const id = String(idSource)

  const coverCandidate =
    article.coverImage ??
    article.coverUrl ??
    article.cover ??
    article.thumbnail ??
    article.cover_image ??
    article.image ??
    undefined

  const coverImage = ensureAbsoluteUrl(coverCandidate)

  return {
    id,
    title: article.title ?? '未命名文章',
    summary: article.summary ?? article.excerpt ?? '',
    content: article.content ?? '',
    coverImage: coverImage || undefined,
    categoryName: article.categoryName ?? article.category?.name ?? undefined,
    authorName: article.authorName ?? article.author?.nickname ?? article.author?.username ?? undefined,
    viewCount: Number(article.viewCount ?? article.readCount ?? article.views ?? 0),
    publishedAt:
      article.publishedAt ??
      article.publishTime ??
      article.published_at ??
      article.createTime ??
      article.createdAt ??
      undefined,
    accessLevel: article.accessLevel != null
      ? Number(article.accessLevel)
      : (article.access_level != null ? Number(article.access_level) : undefined),
    isTop: parseBooleanFlag(
      article.isTop ?? article.pinned ?? article.top ?? article.isPinned ?? article.sticky
    ) ?? false,
    isRecommend: parseBooleanFlag(
      article.isRecommend ?? article.recommend ?? article.featured ?? article.highlight
    ) ?? false,
    raw: article
  }
}

const uniqueById = (articles: (PortalArticle | null)[]): PortalArticle[] => {
  const seen = new Set<string>()
  const result: PortalArticle[] = []

  articles.forEach(item => {
    if (!item?.id) {
      return
    }
    if (seen.has(item.id)) {
      return
    }
    seen.add(item.id)
    result.push(item)
  })

  return result
}

const getArticlePriorityScore = (article: PortalArticle): number => {
  let score = 0
  if (article.isTop) {
    score += 2
  }
  if (article.isRecommend) {
    score += 1
  }
  return score
}

const getPublishedTimestamp = (article: PortalArticle): number => {
  if (!article.publishedAt) {
    return 0
  }
  const timestamp = new Date(article.publishedAt).getTime()
  return Number.isNaN(timestamp) ? 0 : timestamp
}

const sortArticlesByPriority = (articles: PortalArticle[]): PortalArticle[] => {
  return [...articles].sort((a, b) => {
    const priorityDiff = getArticlePriorityScore(b) - getArticlePriorityScore(a)
    if (priorityDiff !== 0) {
      return priorityDiff
    }

    const timeDiff = getPublishedTimestamp(b) - getPublishedTimestamp(a)
    if (timeDiff !== 0) {
      return timeDiff
    }

    return (b.viewCount ?? 0) - (a.viewCount ?? 0)
  })
}

const updateTopCache = (candidates: PortalArticle[]) => {
  if (!candidates?.length) {
    return
  }

  const merged = sortArticlesByPriority(
    uniqueById([
      ...candidates,
      ...topArticles.value,
    ])
  )

  topArticles.value = merged.slice(0, TOP_PAGE_SIZE)
}

const updateRecommendCache = (candidates: PortalArticle[]) => {
  if (!candidates?.length) {
    return
  }

  const merged = sortArticlesByPriority(
    uniqueById([
      featuredArticle.value,
      ...sidebarFeatured.value,
      ...candidates,
    ])
  )

  featuredArticle.value = merged[0] ?? null
  sidebarFeatured.value = merged.slice(1, 6)
}

const syncPriorityCachesFrom = (articles: PortalArticle[]) => {
  if (!articles?.length) {
    return
  }

  const topCandidates = articles.filter((article) => article?.isTop)
  const recommendCandidates = articles.filter((article) => article?.isRecommend)

  if (topCandidates.length) {
    updateTopCache(topCandidates)
  }

  if (recommendCandidates.length) {
    updateRecommendCache(recommendCandidates)
  }
}

const topArticles = ref<PortalArticle[]>([])
const featuredArticle = ref<PortalArticle | null>(null)
const sidebarFeatured = ref<PortalArticle[]>([])
const latestArticles = ref<PortalArticle[]>([])
const activeFilter = ref<'all' | 'featured'>('all')

const SPECIAL_CATEGORY_ID = '8888'
const CATEGORY_PAGE_SIZE = 12
const LATEST_PAGE_SIZE = 12
const TOP_PAGE_SIZE = 10
const activeCategoryId = ref<string>('all')
const categoryArticles = ref<PortalArticle[]>([])
const isCategoryLoading = ref(false)


const hasCategoriesLoaded = computed(() => categoryStore.categories?.length > 0)

const latestState = reactive({
  page: 1,
  pageSize: LATEST_PAGE_SIZE,
  loading: false,
  hasMore: true
})

const categoryState = reactive({
  page: 1,
  pageSize: CATEGORY_PAGE_SIZE,
  loading: false,
  hasMore: true
})

const isLoadingMore = ref(false)
const infiniteScrollTrigger = ref<HTMLElement | null>(null)

let categoryRequestToken = 0
let latestRequestToken = 0
let intersectionObserver: IntersectionObserver | null = null
let hasInitialized = false
let hasActivatedOnce = false

const formatDate = (date?: string) => {
  if (!date) {
    return '日期未知'
  }
  const time = new Date(date)
  if (Number.isNaN(time.getTime())) {
    return '日期未知'
  }
  return time.toLocaleDateString('zh-CN', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const openArticle = (article: PortalArticle) => {
  const url = router.resolve({ name: 'ArticleReader', params: { id: article.id } }).href
  window.open(url, '_blank', 'noopener')
}

const featuredList = computed<PortalArticle[]>(() => {
  return sortArticlesByPriority(uniqueById([
    featuredArticle.value,
    ...sidebarFeatured.value
  ]))
})

const combinedArticles = computed<PortalArticle[]>(() => {
  if (activeCategoryId.value !== 'all') {
    return sortArticlesByPriority([...categoryArticles.value])
  }

  return sortArticlesByPriority(uniqueById([
    ...topArticles.value,
    featuredArticle.value,
    ...sidebarFeatured.value,
    ...latestArticles.value
  ]))
})

const filteredArticles = computed<PortalArticle[]>(() => {
  if (activeCategoryId.value !== 'all') {
    return sortArticlesByPriority([...categoryArticles.value])
  }

  if (activeFilter.value === 'featured') {
    return sortArticlesByPriority([...featuredList.value])
  }

  return sortArticlesByPriority(uniqueById([
    ...topArticles.value,
    ...featuredList.value,
    ...latestArticles.value
  ]))
})

const isAllCategoryActive = computed(() => activeCategoryId.value === 'all')
const isAIGoldCategory = computed(() => activeCategoryId.value === SPECIAL_CATEGORY_ID || activeCategoryId.value === '8888AI')
const aiGoldArticles = computed<PortalArticle[]>(() => sortArticlesByPriority([...categoryArticles.value]))

const isListLoading = computed(() => {
  if (isAllCategoryActive.value) {
    return latestState.loading && latestArticles.value.length === 0
  }
  return isCategoryLoading.value && categoryArticles.value.length === 0
})

const hasMoreArticles = computed(() => {
  if (activeFilter.value === 'featured' && isAllCategoryActive.value) {
    return false
  }
  return isAllCategoryActive.value ? latestState.hasMore : categoryState.hasMore
})

const canLoadMore = computed(() => {
  if (!hasMoreArticles.value) {
    return false
  }
  if (activeFilter.value === 'featured' && isAllCategoryActive.value) {
    return false
  }
  if (isAllCategoryActive.value) {
    if (latestState.loading) {
      return false
    }
    return latestArticles.value.length > 0
  }
  if (categoryState.loading) {
    return false
  }
  return categoryArticles.value.length > 0
})

const showInfiniteTrigger = computed(() => hasMoreArticles.value && filteredArticles.value.length > 0)

const normalizeCategoryQuery = (value: unknown): string => {
  if (Array.isArray(value) && value.length > 0) {
    return normalizeCategoryQuery(value[0])
  }

  const target = (value !== undefined && value !== null && value !== '') ? String(value) : null
  const hasGold = categoryStore.categories?.some(c => String(c.id) === SPECIAL_CATEGORY_ID)

  // 如果分类已加载
  if (categoryStore.hasCategories) {
    // 如果目标是8888但用户没有该分类，显示全部
    if (target === SPECIAL_CATEGORY_ID && !hasGold) {
      return 'all'
    }
    
    // 如果没有指定分类，且用户有8888分类，默认显示8888，否则显示全部
    if (target === null) {
      return hasGold ? SPECIAL_CATEGORY_ID : 'all'
    }
    
    return target
  }

  // 分类未加载时
  if (target) return target
  
  // 默认先显示all，等分类加载后再根据情况切换
  return 'all'
}

const syncCategoryFromRoute = () => {
  const currentRouteName = route.name
  if (currentRouteName !== 'Home') {
    if (activeCategoryId.value !== 'all') {
      activeCategoryId.value = 'all'
    }
    return
  }

  const nextCategory = normalizeCategoryQuery(route.query.category)
  if (activeCategoryId.value !== nextCategory) {
    activeCategoryId.value = nextCategory
  }
}

const ensureCategories = async (options?: { force?: boolean }) => {
  try {
    await categoryStore.fetchCategories({ force: options?.force })
  } catch (error) {
    console.error('加载分类数据失败:', error)
  }
}



const toNumber = (value: unknown): number | undefined => {
  if (value === undefined || value === null || value === '') {
    return undefined
  }
  const parsed = Number(value)
  return Number.isFinite(parsed) ? parsed : undefined
}

const determineHasMore = (response: any, rawCount: number, page: number, limit: number): boolean => {
  if (!limit || limit <= 0) {
    return false
  }

  if (response && typeof response === 'object') {
    const boolCandidates = [
      (response as any).hasNext,
      (response as any).hasMore,
      (response as any).hasNextPage,
      (response as any).has_more
    ]

    for (const candidate of boolCandidates) {
      if (typeof candidate === 'boolean') {
        return candidate
      }
    }

    const totalCandidates = [
      (response as any).total,
      (response as any).totalCount,
      (response as any).count,
      (response as any).totalRecords,
      (response as any).totalElements
    ]

    for (const candidate of totalCandidates) {
      const total = toNumber(candidate)
      if (total !== undefined) {
        return page * limit < total
      }
    }

    const totalPages = toNumber(
      (response as any).totalPages ??
      (response as any).pages ??
      (response as any).pageCount ??
      (response as any).totalPage
    )

    const pageNum = toNumber(
      (response as any).pageNum ??
      (response as any).page ??
      (response as any).current ??
      (response as any).currentPage
    )

    if (totalPages !== undefined && pageNum !== undefined) {
      return pageNum < totalPages
    }
  }

  return rawCount >= limit
}

const loadTopArticles = async () => {
  try {
    const response = await articleApi.getArticles({
      page: 1,
      limit: TOP_PAGE_SIZE,
      status: ArticleStatus.PUBLISHED,
      pinned: 1,
      isTop: 1,
      top: 1,
      sortBy: 'publishedAt',
      sortOrder: 'desc'
    })

    const normalized = uniqueById(asArray(response).map(normalizeArticle))
    topArticles.value = sortArticlesByPriority(normalized)
    syncPriorityCachesFrom(topArticles.value)
  } catch (error) {
    console.error('加载置顶文章失败:', error)
    topArticles.value = []
  }
}

const loadFeaturedArticles = async () => {
  try {
    const response = await articleApi.getArticles({
      page: 1,
      limit: 12,
      status: ArticleStatus.PUBLISHED,
      featured: true,
      recommend: 1,
      sortBy: 'publishedAt',
      sortOrder: 'desc'
    })

    const normalizedFeatured = uniqueById(
      asArray(response).map(normalizeArticle)
    )
    const orderedFeatured = sortArticlesByPriority(normalizedFeatured)

    featuredArticle.value = orderedFeatured[0] ?? null
    sidebarFeatured.value = orderedFeatured.slice(1, 6)
    syncPriorityCachesFrom(orderedFeatured)
  } catch (error) {
    console.error('加载特色文章失败:', error)
    featuredArticle.value = null
    sidebarFeatured.value = []
  }
}

const loadLatestArticles = async ({ reset = false }: { reset?: boolean } = {}) => {
  if (latestState.loading && !reset) {
    return
  }

  if (reset) {
    latestArticles.value = []
    latestState.page = 1
    latestState.hasMore = true
  } else if (!latestState.hasMore) {
    return
  }

  const requestId = ++latestRequestToken
  latestState.loading = true
  const currentPage = latestState.page

  try {
    const response = await articleApi.getArticles({
      page: currentPage,
      limit: latestState.pageSize,
      status: ArticleStatus.PUBLISHED,
      sortBy: 'publishedAt',
      sortOrder: 'desc'
    })

    if (requestId !== latestRequestToken) {
      return
    }

    const rawItems = asArray(response)
    const normalized = uniqueById(rawItems.map(normalizeArticle))
    const ordered = sortArticlesByPriority(normalized)

    if (reset) {
      latestArticles.value = ordered
    } else {
      latestArticles.value = sortArticlesByPriority(uniqueById([
        ...latestArticles.value,
        ...ordered
      ]))
    }

    syncPriorityCachesFrom(ordered)

    const hasMore = determineHasMore(response, rawItems.length, currentPage, latestState.pageSize)
    latestState.hasMore = hasMore
    if (hasMore) {
      latestState.page = currentPage + 1
    }
  } catch (error) {
    if (reset) {
      latestArticles.value = []
    }
    console.error('加载最新文章失败:', error)
  } finally {
    if (requestId === latestRequestToken) {
      latestState.loading = false
    }
  }
}

const loadCategoryArticles = async ({ reset = false }: { reset?: boolean } = {}) => {
  const categoryId = activeCategoryId.value
  if (categoryId === 'all') {
    return
  }

  if (categoryState.loading && !reset) {
    return
  }

  if (reset) {
    categoryArticles.value = []
    categoryState.page = 1
    categoryState.hasMore = true
    isCategoryLoading.value = true
  } else if (!categoryState.hasMore) {
    return
  }

  const requestId = ++categoryRequestToken
  categoryState.loading = true
  const currentPage = categoryState.page

  const params: Record<string, any> = {
    page: currentPage,
    limit: categoryState.pageSize,
    categoryId,
    status: ArticleStatus.PUBLISHED
  }

  if (activeFilter.value === 'featured') {
    params.featured = true
  }

  try {
    const response = await articleApi.getArticles(params)

    if (requestId !== categoryRequestToken) {
      return
    }

    const rawItems = asArray(response)
    const normalized = uniqueById(rawItems.map(normalizeArticle))
    const ordered = sortArticlesByPriority(normalized)

    if (reset) {
      categoryArticles.value = ordered
    } else {
      categoryArticles.value = sortArticlesByPriority(uniqueById([
        ...categoryArticles.value,
        ...ordered
      ]))
    }

    syncPriorityCachesFrom(ordered)

    const hasMore = determineHasMore(response, rawItems.length, currentPage, categoryState.pageSize)
    categoryState.hasMore = hasMore
    if (hasMore) {
      categoryState.page = currentPage + 1
    }
  } catch (error) {
    if (requestId === categoryRequestToken && reset) {
      categoryArticles.value = []
    }
    console.error('按分类加载文章失败:', error)
  } finally {
    if (requestId === categoryRequestToken) {
      categoryState.loading = false
      if (reset) {
        isCategoryLoading.value = false
      }
    }
  }
}

const loadData = async () => {
  await Promise.all([
    loadTopArticles(),
    loadFeaturedArticles(),
    loadLatestArticles({ reset: true })
  ])
}

const loadMore = async () => {
  if (!canLoadMore.value) {
    return
  }
  if (isLoadingMore.value) {
    return
  }

  isLoadingMore.value = true
  try {
    if (isAllCategoryActive.value) {
      await loadLatestArticles()
    } else {
      await loadCategoryArticles()
    }
  } finally {
    isLoadingMore.value = false
  }
}

const setupIntersectionObserver = (element: Element | null) => {
  if (intersectionObserver) {
    intersectionObserver.disconnect()
    intersectionObserver = null
  }

  if (!element || typeof window === 'undefined' || !('IntersectionObserver' in window)) {
    return
  }

  intersectionObserver = new IntersectionObserver(entries => {
    if (entries.some(entry => entry.isIntersecting)) {
      void loadMore()
    }
  }, {
    root: null,
    rootMargin: '0px 0px 200px 0px',
    threshold: 0
  })

  intersectionObserver.observe(element)
}

watch(
  () => [route.name, route.query.category],
  () => {
    syncCategoryFromRoute()
  }
)

watch(
  () => categoryStore.categories,
  () => {
    syncCategoryFromRoute()
  }
)

watch(activeCategoryId, (categoryId) => {
  if (categoryId === 'all') {
    categoryRequestToken += 1
    categoryArticles.value = []
    categoryState.page = 1
    categoryState.hasMore = true
    categoryState.loading = false
    isCategoryLoading.value = false
    return
  }

  categoryArticles.value = []
  categoryState.page = 1
  categoryState.hasMore = true
  categoryState.loading = false
  isCategoryLoading.value = false
  void loadCategoryArticles({ reset: true })
})

watch(activeFilter, (filter) => {
  if (!isAllCategoryActive.value) {
    void loadCategoryArticles({ reset: true })
    return
  }

  if (filter === 'featured') {
    void loadFeaturedArticles()
  } else if (filter === 'all') {
    if (topArticles.value.length === 0) {
      void loadTopArticles()
    }
    if (latestArticles.value.length === 0) {
      void loadLatestArticles({ reset: true })
    }
  }
})

watch(infiniteScrollTrigger, (element) => {
  setupIntersectionObserver(element)
})

const getAccessLevel = (article: PortalArticle): number => {
  if (!article) {
    return 0
  }

  if (typeof article.accessLevel === 'number' && Number.isFinite(article.accessLevel)) {
    return article.accessLevel
  }

  const rawLevel = article.raw?.accessLevel ?? article.raw?.access_level ?? article.raw?.vipLevel
  const parsed = Number(rawLevel)
  return Number.isFinite(parsed) ? parsed : 0
}

const isVipArticle = (article: PortalArticle): boolean => getAccessLevel(article) > 0

const getAccessLevelText = (article: PortalArticle): string => {
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

// 获取封面图片URL（支持动态域名拼接）
const normalizeUploadPath = (path: string) => {
  if (path.includes('/api/upload/upload/')) {
    return path.replace('/api/upload/upload/', 'upload/')
  }
  if (path.includes('/api/upload/')) {
    return path.replace('/api/upload/', 'upload/')
  }
  return path
}

const isAbsoluteUrl = (value: string) => /^https?:\/\//i.test(value)

const getCoverImageUrl = (coverImage?: string): string => {
  if (!coverImage) return ''

  if (isAbsoluteUrl(coverImage)) {
    try {
      const url = new URL(coverImage)
      const normalizedPath = normalizeUploadPath(
        url.pathname.startsWith('/') ? url.pathname : `/${url.pathname}`
      )
      if (normalizedPath !== url.pathname) {
        url.pathname = normalizedPath.startsWith('/') ? normalizedPath : `/${normalizedPath}`
        return url.toString()
      }
    } catch {
      // ignore parse errors
    }
    return coverImage
  }

  const processedPath = normalizeUploadPath(coverImage)
  const domain = import.meta.env.VITE_FILE_BASE_URL?.replace(/\/$/, '')
  if (domain) {
    const normalized = processedPath.startsWith('/') ? processedPath.slice(1) : processedPath
    return `${domain}/${normalized}`
  }

  return processedPath
}



onMounted(async () => {
  syncCategoryFromRoute()

  try {
    await Promise.all([
      loadData(),
      ensureCategories({ force: true })
    ])
  } catch (error) {
    console.error('初始化首页数据失败:', error)
  }

  hasInitialized = true

  if (activeCategoryId.value !== 'all') {
    await loadCategoryArticles({ reset: true })
  } else if (!hasCategoriesLoaded.value) {
    await ensureCategories({ force: true })
  }
})

onActivated(async () => {
  syncCategoryFromRoute()

  if (!hasInitialized) {
    return
  }

  if (!hasActivatedOnce) {
    hasActivatedOnce = true
    return
  }

  if (!hasCategoriesLoaded.value) {
    await ensureCategories()
  }

  if (latestState.loading) {
    return
  }

  await loadData()

  if (activeCategoryId.value !== 'all') {
    await loadCategoryArticles({ reset: true })
  }
})

onBeforeUnmount(() => {
  if (intersectionObserver) {
    intersectionObserver.disconnect()
    intersectionObserver = null
  }
})
</script>

<style scoped>
.ai-particles {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background-image:
    radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.18) 0, transparent 40%),
    radial-gradient(circle at 80% 30%, rgba(56, 189, 248, 0.25) 0, transparent 45%),
    radial-gradient(circle at 50% 70%, rgba(99, 102, 241, 0.22) 0, transparent 40%);
  opacity: 0.65;
  animation: floatParticles 14s ease-in-out infinite;
}

.ai-glow {
  position: absolute;
  inset: -30%;
  background: radial-gradient(circle at 30% 30%, rgba(56, 189, 248, 0.35), transparent 55%),
    radial-gradient(circle at 70% 60%, rgba(248, 113, 113, 0.3), transparent 50%);
  filter: blur(60px);
  opacity: 0.45;
  animation: pulseGlow 12s ease-in-out infinite;
}

.ai-grid-overlay {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(255, 255, 255, 0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.06) 1px, transparent 1px);
  background-size: 24px 24px;
  mask-image: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.5) 15%, rgba(0, 0, 0, 0.6));
  pointer-events: none;
}

.ai-chip {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 0.5rem 0.875rem;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.12);
  color: #e5edff;
  font-weight: 600;
  border: 1px solid rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(8px);
}

.ai-stat-card {
  position: relative;
  padding: 1rem 1.1rem;
  border-radius: 18px;
  border: 1px solid rgba(255, 255, 255, 0.18);
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.03));
  box-shadow: 0 15px 60px rgba(15, 23, 42, 0.35);
}

.ai-card {
  position: relative;
  overflow: hidden;
  border-radius: 20px;
  padding: 18px;
  border: 1px solid rgba(99, 102, 241, 0.2);
  background: radial-gradient(circle at 20% 20%, rgba(99, 102, 241, 0.14), transparent 40%),
    radial-gradient(circle at 80% 0%, rgba(59, 130, 246, 0.18), transparent 45%),
    linear-gradient(145deg, #0f172a, #111827);
  box-shadow: 0 14px 40px rgba(15, 23, 42, 0.35);
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.2s ease;
}

.ai-card:hover {
  transform: translateY(-4px);
  border-color: rgba(96, 165, 250, 0.5);
  box-shadow: 0 20px 50px rgba(37, 99, 235, 0.28);
}

.ai-card-glow {
  position: absolute;
  inset: -20%;
  background: radial-gradient(circle at 85% 20%, rgba(59, 130, 246, 0.35), transparent 45%),
    radial-gradient(circle at 20% 80%, rgba(244, 114, 182, 0.25), transparent 45%);
  filter: blur(45px);
  opacity: 0.35;
  transition: opacity 0.25s ease;
}

.ai-card:hover .ai-card-glow {
  opacity: 0.55;
}

.ai-card-body {
  position: relative;
  z-index: 1;
  display: grid;
  gap: 0.4rem;
  padding-right: 120px;
}

.ai-card-cover {
  position: absolute;
  right: 16px;
  bottom: 16px;
  width: 110px;
  height: 110px;
  border-radius: 16px;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.18);
  box-shadow: 0 10px 28px rgba(15, 23, 42, 0.5);
  opacity: 0.95;
  z-index: 0;
  transition: transform 0.3s ease;
}

.ai-badge {
  display: inline-flex;
  align-items: center;
  padding: 0.25rem 0.6rem;
  border-radius: 999px;
  font-weight: 700;
  font-size: 11px;
  border: 1px solid transparent;
  background: rgba(255, 255, 255, 0.12);
  color: #e5edff;
}

.ai-badge--amber {
  background: rgba(251, 191, 36, 0.14);
  color: #fcd34d;
  border-color: rgba(251, 191, 36, 0.35);
}

.ai-badge--blue {
  background: rgba(96, 165, 250, 0.16);
  color: #bfdbfe;
  border-color: rgba(96, 165, 250, 0.4);
}

.ai-badge--vip {
  background: linear-gradient(120deg, #f59e0b, #f97316);
  color: #fff7ed;
  border-color: transparent;
}

.ai-dot {
  display: inline-block;
  width: 7px;
  height: 7px;
  border-radius: 999px;
  background: linear-gradient(135deg, #67e8f9, #818cf8);
  box-shadow: 0 0 0 6px rgba(103, 232, 249, 0.08);
}

@keyframes floatParticles {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-12px);
  }
  100% {
    transform: translateY(0);
  }
}

@keyframes pulseGlow {
  0% {
    opacity: 0.35;
  }
  50% {
    opacity: 0.7;
  }
  100% {
    opacity: 0.35;
  }
}

.ai-card:hover .ai-card-cover {
  transform: scale(1.03);
  transition: transform 0.3s ease;
}

@media (max-width: 640px) {
  .ai-card-body {
    padding-right: 0;
  }
  .ai-card-cover {
    position: relative;
    width: 100%;
    height: 160px;
    margin-top: 12px;
  }
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.category-chip {
  @apply px-3 py-1 rounded-full bg-white/90 text-xs font-semibold text-slate-900 shadow-sm;
}

.article-badge {
  @apply inline-flex items-center gap-1 px-2 py-0.5 rounded-full border text-[10px] font-semibold;
}

.article-badge--top {
  @apply bg-amber-50 text-amber-700 border-amber-200;
}

.article-badge--recommend {
  @apply bg-blue-50 text-blue-600 border-blue-200;
}

.vip-badge {
  @apply flex items-center justify-center rounded-full bg-gradient-to-r from-amber-400 to-orange-500 text-white shadow-lg;
  width: 32px;
  height: 32px;
}

.vip-badge :deep(svg) {
  width: 16px;
  height: 16px;
}

.vip-badge--small {
  width: 24px;
  height: 24px;
  @apply shadow;
}

.vip-badge--small :deep(svg) {
  width: 14px;
  height: 14px;
}


</style>
