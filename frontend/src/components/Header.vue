<template>
  <header class="header-container fixed-header">
    <div class="header-content">
      <div class="header-inner">
        <!-- Logo和网站名称 -->
        <div class="logo-section">
          <router-link to="/" class="logo-link">
            <div class="logo-wrapper">
              <img 
                v-if="tenantStore.tenantInfo?.logo" 
                :src="tenantStore.tenantInfo.logo" 
                :alt="tenantStore.tenantInfo.name"
                class="logo-image"
              />
              <div v-else class="logo-default">
                <span class="logo-text">{{ siteDefaults.initial }}</span>
              </div>
            </div>
            <div class="site-title">
              {{ tenantStore.tenantInfo?.title || siteDefaults.title }}
            </div>
          </router-link>
        </div>
        
        <!-- 导航菜单 -->
        <nav class="nav-menu">
          <router-link 
            to="/" 
            class="nav-item"
            :class="{ 'nav-item-active': $route.name === 'Home' }"
          >
            <el-icon><House /></el-icon>
            <span>首页</span>
          </router-link>
          
          <!-- AI教学 -->
          <router-link
            to="/aitech"
            class="nav-item"
            :class="{ 'nav-item-active': $route.name === 'AiTechList' }"
          >
            <el-icon><GoldMedal /></el-icon>
            <span>AI教学</span>
          </router-link>
          
          <!-- 产品 -->
          <!-- <router-link
            to="/article/99999/reader"
            class="nav-item"
            :class="{ 'nav-item-active': isProductActive }"
          >
            <el-icon><ShoppingBag /></el-icon>
            <span>产品</span>
          </router-link> -->

          <!-- 本站源码 -->
          <router-link
            to="/article/88888/reader"
            class="nav-item"
            :class="{ 'nav-item-active': isSourceCodeActive }"
          >
            <el-icon><Files /></el-icon>
            <span>本站源码</span>
          </router-link>
          <router-link
            to="/article/88889/reader"
            class="nav-item"
            :class="{ 'nav-item-active': isContactActive }"
          >
            <el-icon><Files /></el-icon>
            <span>联系我们</span>
          </router-link>
          
        </nav>
        
        <!-- 搜索框 -->
        <div class="search-section">
          <el-input
            v-model="searchKeyword"
            placeholder="搜索文章..."
            @keyup.enter="handleSearch"
            class="search-input"
            clearable
          >
            <template #prefix>
              <el-icon class="search-icon"><Search /></el-icon>
            </template>
          </el-input>
        </div>
        
        <!-- 用户菜单 -->
        <div class="user-section">
          <!-- 移动端搜索按钮 -->
          <el-button 
            class="mobile-search-btn" 
            text 
            @click="showMobileSearch = true"
          >
            <el-icon><Search /></el-icon>
          </el-button>
          
          <!-- 写文章按钮 -->
          <el-button
            v-if="userStore.hasValidToken()"
            type="primary"
            @click="handleWriteArticle"
            class="write-article-btn"
          >
            <el-icon><EditPen /></el-icon>
            <span class="write-btn-text">写文章</span>
          </el-button>
          
          <!-- 用户头像和菜单 -->
          <div v-if="userStore.hasValidToken()" class="user-menu">
            <el-dropdown trigger="click" placement="bottom-end">
              <div class="user-avatar-wrapper">
                <div class="user-avatar-container">
                  <el-avatar 
                    :src="userStore.userAvatar" 
                    :size="36"
                    class="user-avatar"
                  >
                    {{ userStore.displayName?.charAt(0) }}
                  </el-avatar>
                  <div
                    class="user-badge"
                    :class="{
                      'user-badge-vip': userStore.isVipMember,
                      'user-badge-default': !userStore.isVipMember
                    }"
                  >
                    <el-icon v-if="userStore.isVipMember" class="badge-icon">
                      <GoldMedal />
                    </el-icon>
                    <el-icon v-else class="badge-icon">
                      <UserFilled />
                    </el-icon>
                  </div>
                </div>
                <div class="user-info">
                  <div class="user-name">{{ userStore.displayName }}</div>
                  <div class="user-status">
                    <span class="user-type" :class="{ 'vip-status': userStore.isVipMember }">
                      类型：{{ userTypeLabel }}
                    </span>
                    <span class="user-role">
                      角色：{{ userRoleLabel }}
                    </span>
                  </div>
                </div>
                <el-icon class="dropdown-arrow"><ArrowDown /></el-icon>
              </div>
              
              <template #dropdown>
                <el-dropdown-menu class="user-dropdown">
                  <el-dropdown-item
                    v-if="!userStore.isVipMember"
                    @click="handleMembership"
                    class="dropdown-item membership-item"
                  >
                    <el-icon class="item-icon vip-icon"><GoldMedal /></el-icon>
                    <span>成为会员</span>
                  </el-dropdown-item>
                  <el-dropdown-item @click="$router.push('/profile')" class="dropdown-item">
                    <el-icon class="item-icon"><User /></el-icon>
                    <span>个人资料</span>
                  </el-dropdown-item>
                  <el-dropdown-item @click="$router.push('/profile/articles/create')" class="dropdown-item">
                    <el-icon class="item-icon"><EditPen /></el-icon>
                    <span>写文章</span>
                  </el-dropdown-item>
                  <el-dropdown-item @click="$router.push('/profile/articles')" class="dropdown-item">
                    <el-icon class="item-icon"><Document /></el-icon>
                    <span>我的文章</span>
                  </el-dropdown-item>
                  <el-dropdown-item @click="$router.push('/profile/favorites')" class="dropdown-item">
                    <el-icon class="item-icon"><Star /></el-icon>
                    <span>我的收藏</span>
                  </el-dropdown-item>
                  <el-dropdown-item 
                    v-if="userStore.isAdmin" 
                    divided 
                    @click="$router.push('/admin')"
                    class="dropdown-item"
                  >
                    <el-icon class="item-icon"><Setting /></el-icon>
                    <span>管理后台</span>
                  </el-dropdown-item>
                  <el-dropdown-item divided @click="handleLogout" class="dropdown-item logout-item">
                    <el-icon class="item-icon"><SwitchButton /></el-icon>
                    <span>退出登录</span>
                  </el-dropdown-item>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
          
          <!-- 登录/注册按钮 -->
          <div v-else class="auth-buttons">
            <el-button 
              v-if="enableUserRegistration"
              text 
              @click="$router.push('/auth/login')"
              class="login-btn"
            >
              登录
            </el-button>
            <el-button 
              v-if="enableUserRegistration"
              type="primary" 
              @click="$router.push('/auth/register')"
              class="register-btn"
            >
              注册
            </el-button>
          </div>
          
          <!-- 移动端菜单按钮 -->
          <el-button 
            class="mobile-menu-btn" 
            text 
            @click="showMobileMenu = true"
          >
            <el-icon><Menu /></el-icon>
          </el-button>
        </div>
      </div>
    </div>

    <!-- 移动端搜索抽屉 -->
    <el-drawer
      v-model="showMobileSearch"
      title="搜索文章"
      direction="ttb"
      size="280px"
      class="mobile-search-drawer"
    >
      <div class="mobile-search-content">
        <div class="search-form">
          <el-input
            v-model="searchKeyword"
            placeholder="输入关键词搜索..."
            @keyup.enter="handleMobileSearch"
            size="large"
            class="mobile-search-input"
          >
            <template #prefix>
              <el-icon><Search /></el-icon>
            </template>
          </el-input>
          <el-button 
            type="primary" 
            class="mobile-search-submit" 
            @click="handleMobileSearch"
            :disabled="!searchKeyword.trim()"
          >
            搜索
          </el-button>
        </div>
        
        <!-- 热门搜索 -->
        <div class="hot-searches">
          <div class="hot-title">热门搜索</div>
          <div class="hot-tags">
            <el-tag 
              v-for="tag in hotSearches" 
              :key="tag"
              @click="searchKeyword = tag; handleMobileSearch()"
              class="hot-tag"
            >
              {{ tag }}
            </el-tag>
          </div>
        </div>
      </div>
    </el-drawer>
    
    <!-- 移动端菜单 -->
    <Teleport to="body">
      <transition name="mobile-menu-fade">
        <div
          v-if="showMobileMenu"
          class="mobile-menu-overlay"
          @click.self="closeMobileMenu"
        >
          <div class="mobile-menu-panel">
            <div class="mobile-menu-header">
              <div class="mobile-menu-title">
                <el-icon class="mobile-menu-header-icon"><Menu /></el-icon>
                <span>{{ tenantStore.tenantInfo?.title || siteDefaults.title }}</span>
              </div>
              <button class="mobile-menu-close" type="button" @click="closeMobileMenu">
                <el-icon><Close /></el-icon>
              </button>
            </div>
            <div class="mobile-menu-body">
              <div v-if="userStore.isLoggedIn" class="mobile-user-summary">
                <div class="mobile-user-info">
                  <el-avatar :src="userStore.userAvatar" :size="52" class="mobile-user-avatar">
                    {{ userStore.displayName?.charAt(0) }}
                  </el-avatar>
                  <div class="mobile-user-text">
                    <div class="mobile-user-name">{{ userStore.displayName }}</div>
                    <div class="mobile-user-email">{{ userStore.userInfo?.email }}</div>
                    <div class="mobile-user-meta">
                      <span class="mobile-user-type" :class="{ 'vip-status': userStore.isVipMember }">
                        类型：{{ userTypeLabel }}
                      </span>
                      <span class="mobile-user-role">
                        角色：{{ userRoleLabel }}
                      </span>
                    </div>
                  </div>
                </div>
                <div class="mobile-user-actions">
                  <button type="button" class="mobile-auth-button" @click="navigateAndClose('/profile')">
                    <el-icon><User /></el-icon>
                    <span>个人中心</span>
                  </button>
                  <button type="button" class="mobile-auth-button" @click="handleLogout">
                    <el-icon><SwitchButton /></el-icon>
                    <span>退出登录</span>
                  </button>
                </div>
              </div>
              <div v-else class="mobile-guest-actions">
                <button 
                  v-if="enableUserRegistration"
                  type="button" 
                  class="mobile-auth-button" 
                  @click="navigateAndClose('/auth/login')"
                >
                  登录
                </button>
                <button 
                  v-if="enableUserRegistration"
                  type="button" 
                  class="mobile-auth-button primary" 
                  @click="navigateAndClose('/auth/register')"
                >
                  注册
                </button>
              </div>
              <div class="mobile-menu-section">
                <div class="mobile-menu-section-title">导航</div>
                <div class="mobile-menu-links">
                  <button type="button" class="mobile-menu-link" @click="navigateAndClose('/')">
                    <el-icon><House /></el-icon>
                    <span>首页</span>
                    <el-icon class="mobile-menu-link-arrow"><ArrowRight /></el-icon>
                  </button>
                  <button type="button" class="mobile-menu-link" @click="navigateAndClose('/aitech')">
                    <el-icon><GoldMedal /></el-icon>
                    <span>AI教学</span>
                    <el-icon class="mobile-menu-link-arrow"><ArrowRight /></el-icon>
                  </button>
                  <button type="button" class="mobile-menu-link" @click="navigateAndClose('/article/88888/reader')">
                    <el-icon><Files /></el-icon>
                    <span>本站源码</span>
                    <el-icon class="mobile-menu-link-arrow"><ArrowRight /></el-icon>
                  </button>
                  <button type="button" class="mobile-menu-link" @click="navigateAndClose('/article/88889/reader')">
                    <el-icon><Files /></el-icon>
                    <span>联系我们</span>
                    <el-icon class="mobile-menu-link-arrow"><ArrowRight /></el-icon>
                  </button>
                  <button type="button" class="mobile-menu-link" @click="navigateAndClose('/membership')">
                    <el-icon><Goods /></el-icon>
                    <span>会员服务</span>
                    <el-icon class="mobile-menu-link-arrow"><ArrowRight /></el-icon>
                  </button>
                </div>
              </div>
              <div v-if="categoryBarItems.length" class="mobile-menu-section">
                <div class="mobile-menu-section-title">分类</div>
                <div class="mobile-menu-tags">
                  <button
                    v-for="item in categoryBarItems"
                    :key="item.key"
                    type="button"
                    class="mobile-menu-tag"
                    :class="{ 'is-active': isCategoryActive(item.id) }"
                    @click="handleMobileCategory(item.id)"
                  >
                    <span>{{ item.name }}</span>
                  </button>
                </div>
              </div>
              <div class="mobile-menu-section">
                <div class="mobile-menu-section-title">快捷操作</div>
                <div class="mobile-menu-actions">
                  <button type="button" class="mobile-menu-action" @click="handleMobileWrite">
                    <el-icon><EditPen /></el-icon>
                    <span>写文章</span>
                  </button>
                  <button type="button" class="mobile-menu-action" @click="navigateAndClose('/search')">
                    <el-icon><Search /></el-icon>
                    <span>搜索文章</span>
                  </button>
                  <button
                    v-if="userStore.hasValidToken()"
                    type="button"
                    class="mobile-menu-action"
                    @click="navigateAndClose('/profile/favorites')"
                  >
                    <el-icon><Star /></el-icon>
                    <span>我的收藏</span>
                  </button>
                  <button
                    v-if="userStore.isAdmin"
                    type="button"
                    class="mobile-menu-action"
                    @click="navigateAndClose('/admin')"
                  >
                    <el-icon><Setting /></el-icon>
                    <span>管理后台</span>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </Teleport>

  </header>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch, onBeforeUnmount } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage, ElMessageBox } from 'element-plus'
import {
  ArrowDown,
  ArrowRight,
  Search,
  User,
  UserFilled,
  Document,
  Star,
  Setting,
  SwitchButton,
  Menu,
  Close,
  House,
  Goods,
  EditPen,
  ShoppingBag,
  Files,
  GoldMedal
} from '@element-plus/icons-vue'
import { useTenantStore } from '@/stores/tenant'
import { useUserStore } from '@/stores/user'
import { usePublicCategoryStore } from '@/stores/publicCategory'
import { siteDefaults } from '@/config/site'
import { UserType } from '@/types/user'

const router = useRouter()
const tenantStore = useTenantStore()
const userStore = useUserStore()
const categoryStore = usePublicCategoryStore()

const searchKeyword = ref('')
const showMobileSearch = ref(false)
const showMobileMenu = ref(false)
const hotSearches = ref(['Vue3', 'TypeScript', 'Node.js', 'React', 'Python'])

const closeMobileMenu = () => {
  showMobileMenu.value = false
}

watch(showMobileMenu, value => {
  if (typeof document === 'undefined') return
  document.body.style.overflow = value ? 'hidden' : ''
})

onBeforeUnmount(() => {
  if (typeof document === 'undefined') return
  document.body.style.overflow = ''
})

interface CategoryBarItem {
  id: string | null
  key: string
  name: string
  icon?: string
}

const categories = computed(() =>
  categoryStore.categories.filter(item => item.status === '0')
)

const categoryBarItems = computed<CategoryBarItem[]>(() => {
  if (!categories.value.length) {
    return []
  }

  const mapped = categories.value.map<CategoryBarItem>(category => ({
    id: category.id != null ? String(category.id) : null,
    key: `category-${category.id ?? 'unknown'}`,
    name: category.name,
    icon: category.icon || undefined
  }))

  return [
    {
      id: null,
      key: 'category-all',
      name: '全部'
    },
    ...mapped
  ]
})

const isProductActive = computed(() => {
  return (
    router.currentRoute.value.name === 'ArticleReader' &&
    router.currentRoute.value.params.id === '80'
  )
})

// 环境变量控制用户注册功能
const enableUserRegistration = computed(() => {
  // 优先使用环境变量配置，如果环境变量为 false，则禁用注册
  const envEnabled = import.meta.env.VITE_ENABLE_USER_REGISTRATION === 'true'
  // 如果环境变量启用，再检查租户配置
  return envEnabled && tenantStore.allowRegistration
})

const isSourceCodeActive = computed(() => {
  return (
    router.currentRoute.value.name === 'ArticleReader' &&
    router.currentRoute.value.params.id === '88888'
  )
})

const isContactActive = computed(() => {
  return (
    router.currentRoute.value.name === 'ArticleReader' &&
    router.currentRoute.value.params.id === '88889'
  )
})

const normalizeValue = (value?: string | number | null) => {
  if (value === null || value === undefined) {
    return ''
  }
  return value.toString().trim()
}

const normalizeLower = (value?: string | number | null) => normalizeValue(value).toLowerCase()

const userTypeLabel = computed(() => {
  const userType = userStore.userInfo?.userType
  const normalizedType = normalizeLower(userType)

  const isAdminType =
    normalizedType === normalizeLower(UserType.ADMIN) || normalizedType === 'admin'

  if (isAdminType) {
    return '管理员'
  }

  const isVipType =
    userStore.isVipMember ||
    normalizedType === normalizeLower(UserType.VIP) ||
    normalizedType === 'vip' ||
    normalizedType === 'member'

  if (isVipType) {
    return 'VIP会员'
  }

  return '普通用户'
})

const roleLabelMap: Record<string, string> = {
  admin: '管理员',
  user: '普通用户',
  editor: '编辑',
  author: '作者',
  reviewer: '审核员',
  operator: '运维',
  manager: '管理员',
  owner: '拥有者',
  tenant_admin: '租户管理员',
  guest: '访客',
  member: '会员'
}

const userRoleLabel = computed(() => {
  const roles = userStore.userInfo?.roles
  if (!Array.isArray(roles) || roles.length === 0) {
    return '无角色'
  }

  const mappedRoles = roles
    .map(role => normalizeValue(role))
    .filter(role => role.length > 0)
    .map(role => roleLabelMap[role.toLowerCase()] ?? role)

  const uniqueRoles = Array.from(new Set(mappedRoles))
  if (!uniqueRoles.length) {
    return '无角色'
  }

  return uniqueRoles.join('、')
})

const isCategoryActive = (categoryId: string | number | null) => {
  const currentRoute = router.currentRoute.value

  if (currentRoute.name === 'Home') {
    const rawQuery = currentRoute.query.category
    const queryValue = Array.isArray(rawQuery) ? rawQuery[0] : rawQuery

    if (categoryId === null) {
      return !queryValue || queryValue === ''
    }

    if (queryValue === undefined || queryValue === null) {
      return false
    }

    return String(queryValue) === String(categoryId)
  }

  if (categoryId === null) {
    return false
  }

  return (
    currentRoute.name === 'CategoryArticles' &&
    currentRoute.params.id === String(categoryId)
  )
}

const fetchCategories = async (force = false) => {
  try {
    if (force) {
      await categoryStore.fetchCategories({ force: true })
    } else {
      await categoryStore.fetchCategories()
    }
  } catch (error) {
    console.error('Failed to fetch categories:', error)
  }
}

const handleSearch = () => {
  if (!searchKeyword.value.trim()) {
    ElMessage.warning('请输入搜索关键词')
    return
  }

  router.push({
    name: 'SearchResults',
    query: { keyword: searchKeyword.value.trim() }
  })
}

const handleMobileSearch = () => {
  handleSearch()
  showMobileSearch.value = false
}

const handleCategoryCommand = (categoryId: string | number | null) => {
  const currentRoute = router.currentRoute.value

  if (currentRoute.name === 'Home') {
    const nextQuery = { ...currentRoute.query }

    if (categoryId === null || categoryId === undefined || categoryId === '') {
      delete nextQuery.category
    } else {
      nextQuery.category = String(categoryId)
    }

    router.push({
      name: 'Home',
      query: nextQuery
    })
    closeMobileMenu()
    return
  }

  if (categoryId === null || categoryId === undefined || categoryId === '') {
    router.push({ name: 'Home' })
    closeMobileMenu()
    return
  }

  router.push({
    name: 'CategoryArticles',
    params: { id: String(categoryId) }
  })
  closeMobileMenu()
}

const handleMobileCategory = (categoryId: string | number | null) => {
  handleCategoryCommand(categoryId)
}

const navigateAndClose = (path: string) => {
  router.push(path)
  closeMobileMenu()
}

const handleMembership = () => {
  router.push('/membership')
}

const handleWriteArticle = () => {
  router.push('/profile/articles/create')
}

const handleMobileWrite = () => {
  closeMobileMenu()
  handleWriteArticle()
}

const handleLogout = async () => {
  try {
    await ElMessageBox.confirm('确定要退出登录吗？', '退出确认', {
      type: 'warning'
    })

    await userStore.logout(false)
    closeMobileMenu()
  } catch (error) {
    if (error !== 'cancel') {
      console.error('Logout failed:', error)
      ElMessage.error('退出登录失败')
    }
  }
}

onMounted(() => {
  void fetchCategories(true)
})
</script>



<style scoped>
/* 头部导航样式 */
.header-container {
  position: relative;
  z-index: 100;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
}

.fixed-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}

.header-content {
  width: 100%;
  margin: 0 auto;
  padding: 0 clamp(1rem, 4vw, 3rem);
  box-sizing: border-box;
}

.header-inner {
  display: grid;
  grid-template-columns: auto minmax(0, 1fr) minmax(220px, 300px) auto;
  align-items: center;
  min-height: 64px;
  column-gap: clamp(1rem, 3vw, 2.5rem);
  width: 100%;
}

/* Logo部分 */
.logo-section {
  flex-shrink: 0;
}

.logo-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-decoration: none;
  color: inherit;
}

.logo-wrapper {
  display: flex;
  align-items: center;
}

.logo-image {
  height: 36px;
  width: auto;
  border-radius: 8px;
}

.logo-default {
  width: 36px;
  height: 36px;
  background: #4f46e5;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-text {
  color: white;
  font-weight: 600;
  font-size: 1.1rem;
}

.site-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: #4f46e5;
  letter-spacing: -0.02em;
}

/* 导航菜单 */
.nav-menu {
  display: flex;
  align-items: center;
  gap: clamp(0.5rem, 1vw, 1rem);
  justify-content: flex-start;
  min-width: 0;
  flex-wrap: wrap;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 0.75rem;
  border-radius: 6px;
  text-decoration: none;
  color: #64748b;
  font-weight: 500;
  font-size: 0.95rem;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.nav-item:hover {
  color: #4f46e5;
  background: rgba(79, 70, 229, 0.04);
}

.nav-item-active {
  color: #4f46e5 !important;
  font-weight: 600;
  background: rgba(79, 70, 229, 0.08) !important;
}

/* 搜索部分 */
.search-section {
  flex-shrink: 0;
  width: 100%;
  max-width: 280px;
  min-width: 200px;
  justify-self: stretch;
}

.search-input {
  width: 100%;
}

.search-input :deep(.el-input__wrapper) {
  border-radius: 8px;
  background: #f1f5f9;
  border: 1px solid transparent;
  box-shadow: none !important;
  transition: all 0.2s ease;
  padding: 4px 12px;
}

.search-input :deep(.el-input__wrapper:hover) {
  background: #e2e8f0;
}

.search-input :deep(.el-input__wrapper.is-focus) {
  background: #ffffff;
  border-color: #cbd5e1;
  box-shadow: 0 0 0 2px rgba(79, 70, 229, 0.1) !important;
}

.search-icon {
  color: #94a3b8;
}

/* 用户部分 */
.user-section {
  display: flex;
  align-items: center;
  gap: clamp(0.75rem, 2vw, 1.5rem);
  flex-shrink: 0;
  justify-self: end;
}

.mobile-search-btn,
.mobile-menu-btn {
  display: none;
  padding: 0.5rem;
  border-radius: 8px;
  color: #64748b;
}

.mobile-search-btn:hover,
.mobile-menu-btn:hover {
  background: rgba(79, 70, 229, 0.04);
  color: #4f46e5;
}

.write-article-btn {
  border-radius: 8px;
  padding: 0.5rem 1.25rem;
  font-weight: 500;
  background: #4f46e5;
  border: none;
  box-shadow: 0 1px 2px rgba(79, 70, 229, 0.2);
  transition: all 0.2s ease;
}

.write-article-btn:hover {
  background: #4338ca;
  transform: translateY(-1px);
  box-shadow: 0 4px 6px rgba(79, 70, 229, 0.25);
}

/* 用户菜单 */
.user-menu {
  position: relative;
}

.user-avatar-wrapper {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.5rem;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.user-avatar-wrapper:hover {
  background: rgba(79, 70, 229, 0.04);
}

.user-avatar-container {
  position: relative;
  display: inline-flex;
  flex-shrink: 0;
}

.user-avatar {
  border: 2px solid rgba(0, 0, 0, 0.05);
}

.user-badge {
  position: absolute;
  right: -4px;
  bottom: -4px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  border-radius: 999px;
  border: 2px solid #ffffff;
  box-shadow: 0 2px 5px rgba(15, 23, 42, 0.1);
}

.user-badge-vip {
  background: #f59e0b;
  color: #fff;
}

.user-badge-default {
  background: #e2e8f0;
  color: #475569;
}

.badge-icon {
  width: 14px;
  height: 14px;
}

.user-info {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.user-name {
  font-weight: 600;
  color: #1e293b;
  font-size: 0.875rem;
}

.user-status {
  font-size: 0.75rem;
  color: #64748b;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.user-type,
.user-role {
  display: inline-flex;
  align-items: center;
  gap: 4px;
}

.user-type {
  color: #64748b;
}

.user-role {
  color: #94a3b8;
}

.vip-status {
  color: #f59e0b;
}

.dropdown-arrow {
  color: #94a3b8;
  transition: transform 0.2s ease;
}

.user-avatar-wrapper:hover .dropdown-arrow {
  transform: rotate(180deg);
  color: #4f46e5;
}

/* 下拉菜单 */
.user-dropdown {
  border-radius: 12px;
  border: 1px solid rgba(0, 0, 0, 0.05);
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.1);
  padding: 0.5rem;
}

.dropdown-item {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.6rem 0.75rem;
  border-radius: 6px;
  transition: all 0.2s ease;
  margin-bottom: 2px;
  color: #475569;
}

.dropdown-item:hover {
  background: rgba(79, 70, 229, 0.04);
  color: #4f46e5;
}

.membership-item {
  color: #f59e0b;
  font-weight: 600;
}

.membership-item .vip-icon {
  color: inherit;
}

.logout-item:hover {
  background: #fef2f2;
  color: #ef4444;
}

.item-icon {
  width: 16px;
  height: 16px;
}

/* 认证按钮 */
.auth-buttons {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.login-btn {
  color: #64748b;
  font-weight: 500;
}

.login-btn:hover {
  color: #4f46e5;
}

.register-btn {
  border-radius: 8px;
  padding: 0.5rem 1.25rem;
  font-weight: 500;
  background: #4f46e5;
  color: white;
  border: none;
  box-shadow: 0 1px 2px rgba(79, 70, 229, 0.2);
}

.register-btn:hover {
  background: #4338ca;
  transform: translateY(-1px);
  box-shadow: 0 4px 6px rgba(79, 70, 229, 0.25);
}

/* 移动端样式 */

/* 移动端搜索抽屉 */
.mobile-search-drawer :deep(.el-drawer__header) {
  background: #ffffff;
  color: #1e293b;
  border-radius: 0;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.mobile-search-content {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.mobile-search-input :deep(.el-input__wrapper) {
  border-radius: 8px;
  background: #f1f5f9;
  box-shadow: none !important;
}

.mobile-search-submit {
  width: 100%;
  margin-top: 0.75rem;
}

/* 移动端菜单 */
.mobile-menu-overlay {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100vh;
  background: rgba(15, 23, 42, 0.4);
  backdrop-filter: blur(8px);
  z-index: 2000;
  display: flex;
  justify-content: flex-end;
}

.mobile-menu-panel {
  width: min(320px, 85%);
  max-width: 360px;
  background: #ffffff;
  height: 100vh;
  display: flex;
  flex-direction: column;
  box-shadow: -8px 0 32px rgba(0, 0, 0, 0.1);
  border-top-left-radius: 24px;
  border-bottom-left-radius: 24px;
  padding: 1.5rem;
  padding-bottom: calc(1.5rem + env(safe-area-inset-bottom));
}

.mobile-menu-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid rgba(241, 245, 249, 0.8);
}

.mobile-menu-title {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-weight: 700;
  font-size: 1.25rem;
  color: #1e293b;
  letter-spacing: -0.02em;
}

.mobile-menu-header-icon {
  color: #4f46e5;
  font-size: 1.4rem;
}

.mobile-menu-close {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  border-radius: 12px;
  border: 1px solid rgba(0, 0, 0, 0.05);
  background: rgba(248, 250, 252, 0.8);
  color: #64748b;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.mobile-menu-close:active {
  transform: scale(0.92);
}

.mobile-menu-body {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-right: 4px;
}

.mobile-menu-body::-webkit-scrollbar {
  width: 4px;
}

.mobile-menu-body::-webkit-scrollbar-thumb {
  background: #e2e8f0;
  border-radius: 4px;
}

.mobile-user-summary {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
  padding: 1.25rem;
  border-radius: 20px;
  background: linear-gradient(135deg, #4f46e5, #6366f1);
  color: #ffffff;
  box-shadow: 0 10px 25px -5px rgba(79, 70, 229, 0.3);
  position: relative;
  overflow: hidden;
}

.mobile-user-summary::before {
  content: '';
  position: absolute;
  top: -50%;
  right: -50%;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(255,255,255,0.2) 0%, transparent 70%);
  transform: rotate(45deg);
}

.mobile-user-info {
  display: flex;
  align-items: center;
  gap: 1rem;
  position: relative;
  z-index: 1;
}

.mobile-user-avatar {
  border: 3px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.mobile-user-text {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  flex: 1;
  min-width: 0;
}

.mobile-user-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.25rem;
}

.mobile-user-type,
.mobile-user-role {
  font-size: 0.75rem;
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(4px);
  color: #ffffff;
  font-weight: 500;
}

.mobile-user-name {
  font-weight: 700;
  font-size: 1.15rem;
  letter-spacing: -0.01em;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.mobile-user-email {
  font-size: 0.85rem;
  opacity: 0.8;
}

.mobile-user-actions,
.mobile-guest-actions {
  display: flex;
  gap: 0.75rem;
  position: relative;
  z-index: 1;
}

.mobile-auth-button {
  flex: 1;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.85rem;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.15);
  color: #ffffff;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
  transition: all 0.2s ease;
  font-weight: 600;
  font-size: 0.95rem;
}

.mobile-auth-button.primary {
  background: #ffffff;
  color: #4f46e5;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.mobile-auth-button:active {
  transform: scale(0.96);
}

.mobile-menu-section {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.mobile-menu-section-title {
  font-weight: 700;
  color: #94a3b8;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding-left: 0.5rem;
}

.mobile-menu-links {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.mobile-menu-link {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 1rem 1.25rem;
  background: #f8fafc;
  border: 1px solid transparent;
  border-radius: 16px;
  font-size: 1rem;
  font-weight: 500;
  color: #334155;
  cursor: pointer;
  transition: all 0.2s ease;
}

.mobile-menu-link:hover,
.mobile-menu-link:active {
  background: #ffffff;
  color: #4f46e5;
  border-color: #e2e8f0;
  box-shadow: 0 4px 12px rgba(148, 163, 184, 0.1);
  transform: translateX(4px);
}

.mobile-menu-link-arrow {
  color: #cbd5e1;
  font-size: 1rem;
  transition: transform 0.2s ease;
}

.mobile-menu-link:hover .mobile-menu-link-arrow {
  transform: translateX(2px);
  color: #4f46e5;
}

.mobile-menu-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  padding: 0 0.25rem;
}

.mobile-menu-tag {
  padding: 0.5rem 1rem;
  border-radius: 12px;
  background: #f1f5f9;
  border: 1px solid transparent;
  font-size: 0.9rem;
  font-weight: 500;
  color: #64748b;
  cursor: pointer;
  transition: all 0.2s ease;
}

.mobile-menu-tag:active {
  transform: scale(0.95);
}

.mobile-menu-tag.is-active {
  background: #4f46e5;
  color: #ffffff;
  box-shadow: 0 4px 12px rgba(79, 70, 229, 0.25);
}

.mobile-menu-actions {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
}

.mobile-menu-action {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  padding: 1.25rem;
  border-radius: 16px;
  border: 1px solid #f1f5f9;
  background: #ffffff;
  color: #64748b;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 6px rgba(15, 23, 42, 0.03);
}

.mobile-menu-action:active {
  transform: scale(0.96);
}

.mobile-menu-action:hover {
  border-color: #e2e8f0;
  box-shadow: 0 10px 20px -5px rgba(15, 23, 42, 0.06);
  color: #4f46e5;
  transform: translateY(-2px);
}

.mobile-menu-action .el-icon {
  font-size: 1.5rem;
  margin-bottom: 0.25rem;
}

.mobile-menu-fade-enter-active,
.mobile-menu-fade-leave-active {
  transition: opacity 0.25s ease;
}

.mobile-menu-fade-enter-from,
.mobile-menu-fade-leave-to {
  opacity: 0;
}

.mobile-menu-fade-enter-active .mobile-menu-panel,
.mobile-menu-fade-leave-active .mobile-menu-panel {
  transition: transform 0.3s ease;
}

.mobile-menu-fade-enter-from .mobile-menu-panel,
.mobile-menu-fade-leave-to .mobile-menu-panel {
  transform: translateX(100%);
}

@media (max-width: 1200px) {
  .header-inner {
    grid-template-columns: auto minmax(0, 1fr) minmax(180px, 240px) auto;
  }

  .search-section {
    min-width: 180px;
  }

}

@media (max-width: 768px) {
  .header-inner {
    grid-template-columns: auto auto;
    column-gap: 0.75rem;
  }

  .nav-menu {
    display: none;
  }
  
  .search-section {
    display: none;
  }
  
  .mobile-search-btn,
  .mobile-menu-btn {
    display: flex;
  }
  
  .user-info {
    display: none;
  }
  
  .auth-buttons {
    gap: 0.5rem;
  }
  
  .write-article-btn {
    padding: 0.5rem 1rem;
  }
  
  .write-btn-text {
    display: none;
  }

}

@media (max-width: 480px) {
  .header-content {
    padding: 0 0.65rem;
  }
  
  .header-inner {
    min-height: 60px;
  }
  
  .site-title {
    font-size: 1.25rem;
  }
}
</style>
