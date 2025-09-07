<script setup lang="ts">
import { ref, computed } from 'vue'
import Home from './pages/Home.vue'
import AI from './pages/AI.vue'
import Profile from './pages/Profile.vue'

const tabs = [
  { key: 'home', label: '首页', icon: 'home' },
  { key: 'ai', label: 'AI助手', icon: 'spark' },
  { key: 'me', label: '我的', icon: 'user' },
] as const

const current = ref<typeof tabs[number]['key']>('home')

const title = computed(() => {
  switch (current.value) {
    case 'home':
      return '菜谱小厨'
    case 'ai':
      return 'AI智能推荐'
    case 'me':
      return '我的'
  }
})

function isActive(key: typeof tabs[number]['key']) {
  return current.value === key
}
</script>

<template>
  <div class="min-h-screen bg-ink-50 text-ink-900 flex flex-col font-sans">
    <!-- Top App Bar -->
    <header class="sticky top-0 z-20 bg-white/80 backdrop-blur border-b border-ink-100">
      <div class="mx-auto max-w-screen-sm px-5 py-3 flex items-center gap-3">
        <div class="flex items-center gap-2">
          <!-- Logo -->
          <div class="h-9 w-9 grid place-items-center rounded-lg bg-gradient-to-br from-brand-400 to-brand-600 text-white shadow-card">
            <svg viewBox="0 0 24 24" class="h-5 w-5">
              <path fill="currentColor" d="M12 2c2.21 0 4 1.79 4 4v1h1.5a2.5 2.5 0 0 1 2.45 2.01l.96 4.8c.13.65-.37 1.25-1.03 1.25H4.12c-.66 0-1.16-.6-1.03-1.25l.96-4.8A2.5 2.5 0 0 1 6.5 7H8V6c0-2.21 1.79-4 4-4m-2 4v1h4V6c0-1.1-.9-2-2-2s-2 .9-2 2m-6 13a1 1 0 0 1 1-1h14a1 1 0 0 1 1 1v1a1 1 0 0 1-1 1H5a1 1 0 0 1-1-1z"/>
            </svg>
          </div>
          <div>
            <h1 class="text-lg font-semibold leading-tight">{{ title }}</h1>
            <p class="text-xs text-ink-500" v-if="current === 'home'">每日推荐 | 灵感从厨房开始</p>
            <p class="text-xs text-ink-500" v-else-if="current === 'ai'">食材变灵感 · 一键出菜谱</p>
            <p class="text-xs text-ink-500" v-else>欢迎回来</p>
          </div>
        </div>
      </div>
    </header>

    <!-- Content -->
    <main class="flex-1">
      <div class="mx-auto max-w-screen-sm">
        <Home v-if="current === 'home'" />
        <AI v-else-if="current === 'ai'" />
        <Profile v-else />
      </div>
    </main>

    <!-- Bottom Tab Bar -->
    <nav class="sticky bottom-0 z-20 border-t border-ink-100 bg-white/80 backdrop-blur">
      <div class="mx-auto max-w-screen-sm grid grid-cols-3">
        <button
          v-for="t in tabs"
          :key="t.key"
          @click="current = t.key"
          class="py-2.5 flex flex-col items-center justify-center gap-1 text-xs"
          :class="isActive(t.key) ? 'text-brand-600' : 'text-ink-500 hover:text-ink-700'"
        >
          <!-- icons -->
          <svg v-if="t.icon==='home'" viewBox="0 0 24 24" class="h-6 w-6" :class="isActive(t.key) ? '' : 'opacity-70'">
            <path fill="currentColor" d="M12 3.1L1 12h2v9h6v-6h6v6h6v-9h2z"/>
          </svg>
          <svg v-else-if="t.icon==='spark'" viewBox="0 0 24 24" class="h-6 w-6" :class="isActive(t.key) ? '' : 'opacity-70'">
            <path fill="currentColor" d="m11 2l1.5 5H18l-4 3l1.5 5L11 12l-4.5 3L8 10L4 7h5.5z"/>
          </svg>
          <svg v-else viewBox="0 0 24 24" class="h-6 w-6" :class="isActive(t.key) ? '' : 'opacity-70'">
            <path fill="currentColor" d="M12 12a5 5 0 1 0-5-5a5 5 0 0 0 5 5m0 2c-4.42 0-8 1.79-8 4v2h16v-2c0-2.21-3.58-4-8-4"/>
          </svg>
          <span class="font-medium">{{ t.label }}</span>
        </button>
      </div>
    </nav>
  </div>
</template>

<style scoped>
</style>
