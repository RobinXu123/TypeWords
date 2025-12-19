<script setup lang="ts">
import { computed, ref } from 'vue'
import BasePage from '@/components/BasePage.vue'
import BaseButton from '@/components/BaseButton.vue'

// 资源分类
const categories = ref([
  {
    id: 'new-concept',
    name: '新概念英语',
    icon: '📚',
    description: '经典英语教材，适合系统学习',
    resources: [
      {
        name: '新概念英语第一册',
        description: '适合英语初学者',
        difficulty: '入门',
        link: 'https://pan.quark.cn/s/92a317cf1a16',
      },
      {
        name: '新概念英语第二册',
        description: '基础英语学习，巩固语法和词汇',
        difficulty: '基础',
        link: 'https://pan.quark.cn/s/1ee9c8a7e8e2',
      },
      {
        name: '新概念英语第三册',
        description: '提高英语水平，增强阅读能力',
        difficulty: '进阶',
        link: 'https://pan.quark.cn/s/b35c2859812a',
      },
      {
        name: '新概念英语第四册',
        description: '高级英语学习，提升综合能力',
        difficulty: '高级',
        link: 'https://pan.quark.cn/s/a56713cafbc5',
      },
      {
        name: '新概念英青少年版',
        description: '儿童读物',
        difficulty: '7岁至14岁',
        link: 'https://pan.quark.cn/s/9de8d7967de2',
      },
      {
        name: '新概念英语1-4 教材高清PDF',
        description: '仅 1-4 册的教材高清PDF',
        difficulty: '',
        link: 'https://pan.quark.cn/s/ec49145d6b00',
      },
      {
        name: '新概念讲解视频',
        description: '包含了 N 家机构/个人的讲解视频',
        difficulty: '',
        link: 'https://pan.quark.cn/s/09e98acd55b4',
      },
    ],
  },
  {
    id: 'exam',
    name: '英语相关电视/电影',
    icon: '🎯',
    description: '雅思、托福等考试备考资料',
    resources: [
      {
        name: '老友记',
        description: '',
        difficulty: '经典',
        link: 'https://pan.quark.cn/s/674834e7a5b1',
      },
      {
        name: '生活大爆炸',
        description: '',
        difficulty: '经典',
        link: 'https://pan.quark.cn/s/0539c10704ba',
      },
      {
        name: '是大臣/是首相',
        description: '',
        difficulty: '经典',
        link: 'https://pan.quark.cn/s/316323ce51d5',
      },
    ],
  },
  {
    id: 'grammar',
    name: '语法学习',
    icon: '📝',
    description: '系统性学习英语语法',
    resources: [
      {
        name: '剑桥中级英语语法',
        description: '清晰讲解语法点，配有大量练习',
        difficulty: '中级',
        link: 'https://pan.baidu.com/s/xxx',
      },
      {
        name: 'English Grammar in Use',
        description: '经典语法教材，适合自学',
        difficulty: '中级',
        link: 'https://pan.baidu.com/s/xxx',
      },
      {
        name: "Murphy's English Grammar",
        description: '系统讲解英语语法，适合各类学习者',
        difficulty: '全级别',
        link: 'https://pan.baidu.com/s/xxx',
      },
    ],
  },
  {
    id: 'listening',
    name: '听力训练',
    icon: '🎧',
    description: '提升英语听力水平',
    resources: [
      {
        name: 'VOA慢速英语合集',
        description: '新闻类听力材料，语速适中，内容丰富',
        difficulty: '中级',
        link: 'https://pan.baidu.com/s/xxx',
      },
      {
        name: 'BBC Learning English',
        description: 'BBC官方英语学习资源，涵盖多方面内容',
        difficulty: '中高级',
        link: 'https://pan.baidu.com/s/xxx',
      },
      {
        name: 'TED演讲精选',
        description: '高质量演讲，锻炼听力同时开拓视野',
        difficulty: '中高级',
        link: 'https://pan.baidu.com/s/xxx',
      },
      {
        name: '哈弗演讲',
        description: '高质量演讲，锻炼听力同时开拓视野',
        difficulty: '中高级',
        link: 'https://pan.quark.cn/s/f2bfa8a50d25',
      },
    ],
  },
])

// 当前选中的分类
const selectedCategory = ref('all')

// 筛选后的资源
const filteredResources = computed(() => {
  if (selectedCategory.value === 'all') {
    return categories.value
  }
  return categories.value.filter(cat => cat.id === selectedCategory.value)
})

// 跳转到网盘链接
const openLink = (url: string) => {
  window.open(url, '_blank')
}

// 根据难度获取对应的样式类
const getDifficultyClass = (difficulty: string) => {
  switch (difficulty) {
    case '入门':
      return 'bg-green-500'
    case '基础':
      return 'bg-blue-500'
    case '中级':
      return 'bg-purple-500'
    case '进阶':
      return 'bg-amber-500'
    case '高级':
      return 'bg-red-500'
    case '全级别':
      return 'bg-gray-500'
    default:
      return 'bg-blue-500'
  }
}
</script>

<template>
  <BasePage>
    <div class="flex flex-col items-center justify-center px-4 py-8 max-w-7xl mx-auto">
      <!-- 页面标题 -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold mb-4">📚 英语学习资源分享</h1>
        <p class="text-lg text-gray-600 dark:text-gray-300 max-w-3xl mx-auto">
          以下是我整理的个人收藏的优质英语学习资源，希望对大家有所帮助！
        </p>
      </div>

      <!-- 分类筛选 -->
      <div
        class="flex flex-wrap justify-center gap-2 mb-8 p-4 bg-white dark:bg-gray-800 rounded-lg shadow-md w-full"
      >
        <BaseButton
          :type="selectedCategory === 'all' ? 'primary' : 'info'"
          @click="selectedCategory = 'all'"
        >
          全部资源
        </BaseButton>
        <BaseButton
          v-for="category in categories"
          :key="category.id"
          :type="selectedCategory === category.id ? 'primary' : 'info'"
          @click="selectedCategory = category.id"
        >
          {{ category.icon }} {{ category.name }}
        </BaseButton>
      </div>

      <!-- 资源列表 -->
      <div class="w-full">
        <div v-for="category in filteredResources" :key="category.id" class="mb-12">
          <div class="text-center mb-6">
            <h2 class="text-2xl font-bold mb-2">{{ category.icon }} {{ category.name }}</h2>
            <p class="text-gray-600 dark:text-gray-300">{{ category.description }}</p>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
            <div
              v-for="resource in category.resources"
              :key="resource.name"
              class="card-white mb-0 hover:shadow-xl transition-all duration-300 hover:-translate-y-1 flex flex-col justify-between"
            >
              <div class="">
                <div class="text-xl font-semibold mb-2 text-gray-800 dark:text-gray-100">
                  {{ resource.name }}
                </div>
                <p>
                  <span
                    v-if="resource.difficulty"
                    class="mr-2 inline-block px-3 py-1 rounded-full text-xs font-medium text-white"
                    :class="getDifficultyClass(resource.difficulty)"
                  >
                    {{ resource.difficulty }}
                  </span>
                  <span class=" text-gray-600 dark:text-gray-300 mb-4">{{
                    resource.description
                  }}</span>
                </p>
              </div>
              <div class="flex flex-col gap-3">
                <BaseButton type="primary" @click="openLink(resource.link)"> 打开链接 </BaseButton>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 页面底部 -->
      <div class="mt-12 pt-8 border-t border-gray-200 dark:border-gray-700">
        <div class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6">
          <h3 class="text-xl font-bold mb-4">💡 温馨提示</h3>
          <ul class="list-disc list-inside space-y-2 text-gray-600 dark:text-gray-300">
            <li>所有资源均来自互联网收集，仅供学习交流使用</li>
            <li>如果链接失效，请及时告知，我会尽快更新</li>
          </ul>
        </div>
      </div>
    </div>
  </BasePage>
</template>
