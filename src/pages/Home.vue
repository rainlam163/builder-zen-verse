<script setup lang="ts">
import { ref, computed } from 'vue'

interface Recipe {
  id: number
  title: string
  cover: string
  likes: number
  time: string
  tags: string[]
}

const allRecipes = ref<Recipe[]>([
  {
    id: 1,
    title: '番茄炒蛋',
    cover: 'https://images.unsplash.com/photo-1504754524776-8f4f37790ca0?q=80&w=1200&auto=format&fit=crop',
    likes: 1258,
    time: '10分钟',
    tags: ['快手', '家常', '素食'],
  },
  {
    id: 2,
    title: '宫保鸡丁',
    cover: 'https://images.unsplash.com/photo-1546549039-49dfcbd3d4e1?q=80&w=1200&auto=format&fit=crop',
    likes: 2016,
    time: '20分钟',
    tags: ['下饭', '鸡肉'],
  },
  {
    id: 3,
    title: '清蒸鲈鱼',
    cover: 'https://images.unsplash.com/photo-1551183053-bf91a1d81141?q=80&w=1200&auto=format&fit=crop',
    likes: 842,
    time: '25分钟',
    tags: ['低脂', '海鲜'],
  },
  {
    id: 4,
    title: '牛肉炒河粉',
    cover: 'https://images.unsplash.com/photo-1544025162-d76694265947?q=80&w=1200&auto=format&fit=crop',
    likes: 1299,
    time: '15分钟',
    tags: ['快手', '牛肉', '面食'],
  },
  {
    id: 5,
    title: '麻婆豆腐',
    cover: 'https://images.unsplash.com/photo-1496116218417-1a781b1c416c?q=80&w=1200&auto=format&fit=crop',
    likes: 1750,
    time: '18分钟',
    tags: ['川菜', '下饭'],
  },
])

const q = ref('')
const results = computed(() => {
  const v = q.value.trim()
  if (!v) return allRecipes.value
  return allRecipes.value.filter((r) => r.title.includes(v) || r.tags.some((t) => t.includes(v)))
})

function like(recipe: Recipe) {
  recipe.likes += 1
}
</script>

<template>
  <section class="pb-24">
    <!-- Hero -->
    <div class="relative overflow-hidden bg-gradient-to-br from-brand-50 to-emerald-100">
      <div class="px-5 py-6 mx-auto max-w-screen-sm">
        <h2 class="text-2xl font-semibold">今日推荐</h2>
        <p class="text-ink-500 text-sm mt-1">新鲜灵感，为你挑选最适合的家常菜</p>
        <div class="mt-4 flex items-center gap-2">
          <div class="relative flex-1">
            <input v-model="q" type="text" placeholder="搜索菜谱或标签，如：快手/鸡肉"
              class="w-full rounded-xl border border-ink-200 bg-white px-4 py-2.5 pr-10 text-sm focus:outline-none focus:ring-2 focus:ring-brand-400" />
            <svg class="absolute right-3 top-1/2 -translate-y-1/2 h-5 w-5 text-ink-400" viewBox="0 0 24 24"><path fill="currentColor" d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5A6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79L20 21.49L21.49 20zM9.5 14A4.5 4.5 0 1 1 14 9.5A4.505 4.505 0 0 1 9.5 14"/></svg>
          </div>
          <button class="px-3 py-2 rounded-lg bg-brand-600 text-white text-sm font-medium shadow-card hover:bg-brand-700">探索</button>
        </div>
      </div>
    </div>

    <!-- Recipe grid -->
    <div class="px-5 mx-auto max-w-screen-sm">
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-5 mt-5">
        <article v-for="r in results" :key="r.id" class="group overflow-hidden rounded-xl bg-white shadow-sm ring-1 ring-ink-100 hover:shadow-lg transition">
          <div class="relative aspect-[4/3] overflow-hidden">
            <img :src="r.cover" :alt="r.title" class="h-full w-full object-cover group-hover:scale-105 transition" />
            <div class="absolute left-3 top-3 inline-flex items-center gap-1 rounded-full bg-white/90 px-2 py-1 text-xs text-ink-700 shadow">
              <span class="inline-block h-2 w-2 rounded-full bg-brand-500"></span>
              今日推荐
            </div>
          </div>
          <div class="p-4">
            <h3 class="font-semibold text-ink-900 line-clamp-1">{{ r.title }}</h3>
            <div class="mt-2 flex flex-wrap items-center gap-2">
              <span v-for="t in r.tags" :key="t" class="text-xs rounded-full bg-ink-100 text-ink-600 px-2 py-0.5">#{{ t }}</span>
            </div>
            <div class="mt-3 flex items-center justify-between text-sm text-ink-600">
              <div class="flex items-center gap-2">
                <svg class="h-4 w-4 text-ink-400" viewBox="0 0 24 24"><path fill="currentColor" d="M12 8a4 4 0 1 0 4 4a4 4 0 0 0-4-4m0-6a10 10 0 0 1 10 10a10 10 0 0 1-10 10A10 10 0 0 1 2 12A10 10 0 0 1 12 2m0 2a8 8 0 1 0 8 8a8 8 0 0 0-8-8"/></svg>
                {{ r.time }}
              </div>
              <button @click="like(r)" class="inline-flex items-center gap-1.5 rounded-full px-2 py-1 hover:bg-ink-100">
                <svg class="h-4 w-4 text-rose-500" viewBox="0 0 24 24"><path fill="currentColor" d="M12.1 8.64L12 8.77l-.1-.13C10.14 6.6 7.1 6.24 5.36 8c-1.54 1.53-1.54 4.04 0 5.57L12 20l6.64-6.43c1.54-1.53 1.54-4.04 0-5.57c-1.74-1.76-4.78-1.4-6.54.64"/></svg>
                <span class="font-medium text-ink-700">{{ r.likes }}</span>
              </button>
            </div>
          </div>
        </article>
      </div>
      <div class="py-10 text-center text-ink-500" v-if="results.length===0">未找到相关菜谱</div>
    </div>
  </section>
</template>

<style scoped>
</style>
