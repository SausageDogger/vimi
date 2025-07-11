<template>
  <div class="min-h-screen p-8 bg-gray-50">
    <!-- 搜索栏 -->
    <div class="mb-8 max-w-3xl mx-auto">
      <el-input 
        v-model="searchKeyword"
        placeholder="搜索感兴趣的面试..."
        size="large"
        @keyup.enter="handleSearch"
      >
        <template #suffix>
          <el-button 
            type="primary" 
            :icon="Search" 
            @click="handleSearch"
            class="search-btn"
          />
        </template>
      </el-input>
    </div>

    <!-- 卡片网格 -->
    <el-row :gutter="20" class="mb-8">
      <el-col 
        v-for="(item, index) in tableData" 
        :key="index"
        :xs="24" :sm="12" :md="8" :lg="6"
        class="mb-6"
      >
        <div 
          class="card-item bg-white rounded-lg shadow-md transition-all duration-300 cursor-pointer"
          @click="$router.push('/candidate/interview-page')"
        >
          <div class="relative">
            <img 
              :src="item.cover" 
              class="w-full h-48 object-cover rounded-t-lg"
              alt="面试封面"
            />
            <div class="absolute bottom-0 left-0 right-0 p-3 bg-gradient-to-t from-black/80">
              <div class="flex justify-between text-white">
                <span>{{ item.position }}</span>
                <span>{{ item.time }}</span>
              </div>
            </div>
          </div>
          <div class="p-4">
            <h3 class="text-lg font-medium mb-2">{{ item.title }}</h3>
            <div class="flex items-center text-gray-600">
              <el-icon class="mr-2"><OfficeBuilding /></el-icon>
              <span>{{ item.employer }}</span>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>

    <!-- 分页 -->
    <el-pagination
      background
      layout="prev, pager, next"
      :total="filteredData.length"
      :page-size="pageSize"
      v-model:current-page="currentPage"
      class="justify-center"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { Search, OfficeBuilding } from '@element-plus/icons-vue'

// 生成更丰富的模拟数据
const generateMockData = () => {
  const positions = ['前端工程师', '全栈开发', '数据科学家', '产品经理', 'UI设计师', '算法工程师']
  const employers = ['腾讯科技', '阿里巴巴', '字节跳动', '华为技术', '百度', '京东集团']
  const covers = Array.from({length: 5}, (_, i) => `/src/assets/interview_${i+1}.jpg`)
  
  return Array.from({length: 50}, (_, i) => ({
    title: `2024秋招-${employers[i%6]}专场`,
    cover: covers[i%5],
    position: positions[i%6],
    employer: employers[i%6],
    time: generateFutureDate(i),
    salary: `${Math.floor(Math.random()*15 + 15)}k-${Math.floor(Math.random()*15 + 20)}k`
  }))
}

// 生成未来30天内的随机日期
const generateFutureDate = (seed: number) => {
  const baseDate = new Date()
  const offset = Math.floor(Math.random() * 30) + seed % 10
  baseDate.setDate(baseDate.getDate() + offset)
  return baseDate.toISOString().split('T')[0]
}

// 数据与状态管理
const searchKeyword = ref('')
const currentPage = ref(1)
const pageSize = ref(12) // 每页显示12条（3行×4个）
const mockData = generateMockData()

// 计算属性
const filteredData = computed(() => {
  const keyword = searchKeyword.value.toLowerCase()
  return mockData.filter(item => 
    item.title.toLowerCase().includes(keyword) ||
    item.position.toLowerCase().includes(keyword) ||
    item.employer.toLowerCase().includes(keyword)
  )
})

const tableData = computed(() => {
  const start = (currentPage.value - 1) * pageSize.value
  return filteredData.value.slice(start, start + pageSize.value)
})

// 事件处理
const handleSearch = () => {
  currentPage.value = 1
}
</script>

<style scoped>
@reference "tailwindcss";
.search-btn {
  @apply h-full ;
}

.card-item {
  &:hover {
    @apply bg-blue-50 transform scale-105 shadow-lg;
    .el-icon {
      @apply text-blue-600;
    }
  }
}


.el-input {
  ::v-deep(.el-input__wrapper) {
    @apply pr-2;
  }
  
  ::v-deep(.search-btn) {
    @apply px-3;
  }
}
</style>