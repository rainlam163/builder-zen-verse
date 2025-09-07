<script setup lang="ts">
import { ref } from 'vue'

interface Suggestion {
  title: string
  reason: string
}

const mode = ref<'idea' | 'ingredient'>('idea')
const ideaText = ref('今天想吃点清淡的，时间不多')
const ingredientText = ref('鸡胸肉, 西兰花, 鸡蛋')

const suggestions = ref<Suggestion[]>([])

const recipesByTags: Record<string, Suggestion> = {
  快手: { title: '蒜蓉西兰花', reason: '10分钟出锅，清爽少油' },
  清淡: { title: '清蒸鲈鱼', reason: '低脂少盐，鲜味突出' },
  下饭: { title: '宫保鸡丁', reason: '酸甜辣适中，配米饭绝佳' },
  健身: { title: '黑椒鸡胸牛油果沙拉', reason: '高蛋白低碳水' },
}

function generateFromIdea() {
  const text = ideaText.value
  const picks: Suggestion[] = []
  Object.keys(recipesByTags).forEach((k) => {
    if (text.includes(k)) picks.push(recipesByTags[k])
  })
  if (picks.length === 0) {
    picks.push(
      { title: '番茄炒蛋', reason: '材料易得，营养均衡' },
      { title: '清炒时蔬', reason: '应季蔬菜，原汁原味' },
      { title: '香煎三文鱼', reason: '优质蛋白，少油烹饪' },
    )
  }
  suggestions.value = picks
}

function generateFromIngredients() {
  const items = ingredientText.value
  const has = (k: string) => items.includes(k)
  const picks: Suggestion[] = []
  if (has('鸡蛋') && has('番茄')) picks.push({ title: '番茄炒蛋', reason: '番茄鸡蛋黄金搭配' })
  if (has('鸡胸') && has('西兰花')) picks.push({ title: '鸡胸西兰花炒', reason: '高蛋白低脂肪' })
  if (has('米饭') && has('鸡蛋')) picks.push({ title: '蛋炒饭', reason: '快速利用剩饭' })
  if (has('牛肉') && has('河粉')) picks.push({ title: '牛肉炒河粉', reason: '快手又顶饱' })
  if (picks.length === 0) picks.push({ title: '蔬菜鸡蛋汤', reason: '大多数食材都能适配' })
  suggestions.value = picks
}

// initial
generateFromIdea()
</script>

<template>
  <section class="pb-24">
    <div class="px-5 py-5">
      <div class="inline-flex rounded-lg bg-ink-100 p-1 text-sm">
        <button
          @click="mode = 'idea'"
          :class="
            mode === 'idea'
              ? 'bg-white text-ink-900 shadow px-3 py-1.5 rounded-md'
              : 'px-3 py-1.5 text-ink-600'
          "
        >
          我可以做什么菜?
        </button>
        <button
          @click="mode = 'ingredient'"
          :class="
            mode === 'ingredient'
              ? 'bg-white text-ink-900 shadow px-3 py-1.5 rounded-md'
              : 'px-3 py-1.5 text-ink-600'
          "
        >
          我有哪些食材
        </button>
      </div>

      <div class="mt-4 space-y-3">
        <div v-if="mode === 'idea'">
          <label class="block text-sm text-ink-600 mb-1">描述你的需求</label>
          <textarea
            v-model="ideaText"
            rows="3"
            class="w-full resize-none rounded-xl border border-ink-200 bg-white px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-brand-400"
            placeholder="例如：想吃清淡一点，15分钟内搞定"
          ></textarea>
          <div class="mt-3">
            <button
              @click="generateFromIdea"
              class="px-4 py-2.5 rounded-xl bg-brand-600 text-white font-medium shadow-card hover:bg-brand-700"
            >
              生成推荐
            </button>
          </div>
        </div>
        <div v-else>
          <label class="block text-sm text-ink-600 mb-1">输入你现有的食材（用逗号分隔）</label>
          <textarea
            v-model="ingredientText"
            rows="3"
            class="w-full resize-none rounded-xl border border-ink-200 bg-white px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-brand-400"
            placeholder="例如：鸡胸肉, 西兰花, 米���, 鸡蛋"
          ></textarea>
          <div class="mt-3">
            <button
              @click="generateFromIngredients"
              class="px-4 py-2.5 rounded-xl bg-brand-600 text-white font-medium shadow-card hover:bg-brand-700"
            >
              生成推荐
            </button>
          </div>
        </div>
      </div>

      <div class="mt-6">
        <h3 class="text-sm font-semibold text-ink-700">为你推荐</h3>
        <ul class="mt-3 space-y-2">
          <li
            v-for="(s, i) in suggestions"
            :key="i"
            class="flex items-start gap-3 rounded-xl border border-ink-100 bg-white p-3"
          >
            <div
              class="h-8 w-8 grid place-items-center rounded-lg bg-gradient-to-br from-brand-400 to-brand-600 text-white shrink-0"
            >
              <svg viewBox="0 0 24 24" class="h-4 w-4">
                <path
                  fill="currentColor"
                  d="M12 17.27L18.18 21l-1.64-7L22 9.24l-7.19-.61L12 2L9.19 8.63L2 9.24l5.46 4.76L5.82 21z"
                />
              </svg>
            </div>
            <div class="flex-1">
              <p class="font-medium">{{ s.title }}</p>
              <p class="text-xs text-ink-500 mt-0.5">{{ s.reason }}</p>
            </div>
            <button class="text-xs px-2 py-1 rounded-full bg-ink-100 text-ink-700 hover:bg-ink-200">
              查看菜谱
            </button>
          </li>
        </ul>
      </div>

      <div
        class="mt-8 rounded-xl bg-gradient-to-br from-rose-50 to-orange-50 border border-rose-100 p-4"
      >
        <p class="text-sm text-ink-700">
          想要真正的AI对话和联网搜索？你可以连接一个后端服务（如 Supabase/Neon）并接入大模型
          API，我可以继续为你集成。
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
