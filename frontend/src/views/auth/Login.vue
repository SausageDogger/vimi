<template>
  <div class="min-h-screen bg-gradient-to-b from-white to-blue-100 flex items-center justify-center p-4">
    <div class="w-full max-w-5xl bg-white rounded-xl shadow-2xl overflow-hidden flex">
      <!-- 左侧蓝色背景 -->
      <div class="w-1/3 bg-gradient-to-b from-blue-500 to-blue-600 p-10 flex flex-col justify-center text-white">
        <h1 class="text-4xl font-bold mb-6">Vimi-AI面试系统</h1>
        <p class="text-xl mb-8">智能面试平台，助力求职与招聘</p>
        <div class="flex items-center space-x-4">
          <el-icon :size="30"><Monitor /></el-icon>
          <span class="text-lg">高效便捷的面试体验</span>
        </div>
      </div>

      <!-- 右侧操作部分 -->
      <div class="w-2/3 p-12">
        <el-tabs v-model="activeTab" class="mb-8" type="card">
          <el-tab-pane label="应聘者登录" name="candidate">
            <el-form :model="loginForm" :rules="rules" ref="loginForm" class="mt-6">
              <el-form-item prop="username" class="mb-6">
                <el-input 
                  v-model="loginForm.username" 
                  placeholder="请输入账号" 
                  size="large"
                  :prefix-icon="User"
                  class="text-lg"
                />
              </el-form-item>
              <el-form-item prop="password" class="mb-6">
                <el-input 
                  v-model="loginForm.password" 
                  type="password" 
                  placeholder="请输入密码" 
                  size="large"
                  :prefix-icon="Lock"
                  show-password
                  class="text-lg"
                />
              </el-form-item>
              <el-form-item prop="captcha" class="mb-6">
                <div class="flex items-center space-x-4">
                  <el-input 
                    v-model="loginForm.captcha" 
                    placeholder="请输入验证码" 
                    size="large"
                    class="text-lg flex-1"
                  />
                  <div class="w-32 h-12 bg-gray-200 rounded flex items-center justify-center cursor-pointer">
                    <span class="text-xl font-mono">A7B9</span>
                  </div>
                </div>
              </el-form-item>
              <el-form-item class="mt-10">
                <el-button 
                  type="primary" 
                  size="large" 
                  class="w-full text-lg" 
                  @click="handleLogin"
                >
                  登录
                </el-button>
              </el-form-item>
            </el-form>
          </el-tab-pane>
          <el-tab-pane label="面试官登录" name="interviewer">
            <!-- 面试官登录表单内容与应聘者类似 -->
            <el-form :model="loginForm" :rules="rules" ref="loginFormRef" class="mt-6">
              <el-form-item prop="username" class="mb-6">
                <el-input 
                  v-model="loginForm.username" 
                  placeholder="请输入账号" 
                  size="large"
                  :prefix-icon="User"
                  class="text-lg"
                />
              </el-form-item>
              <el-form-item prop="password" class="mb-6">
                <el-input 
                  v-model="loginForm.password" 
                  type="password" 
                  placeholder="请输入密码" 
                  size="large"
                  :prefix-icon="Lock"
                  show-password
                  class="text-lg"
                />
              </el-form-item>
              <el-form-item prop="captcha" class="mb-6">
                <div class="flex items-center space-x-4">
                  <el-input 
                    v-model="loginForm.captcha" 
                    placeholder="请输入验证码" 
                    size="large"
                    class="text-lg flex-1"
                  />
                  <div class="w-32 h-12 bg-gray-200 rounded flex items-center justify-center cursor-pointer">
                    <span class="text-xl font-mono">C3D5</span>
                  </div>
                </div>
              </el-form-item>
              <el-form-item class="mt-10">
                <el-button 
                  type="primary" 
                  size="large" 
                  class="w-full text-lg" 
                  @click="handleLogin"
                >
                  登录
                </el-button>
              </el-form-item>
            </el-form>
          </el-tab-pane>
        </el-tabs>

        <div class="text-center mt-8">
          <span class="text-gray-600">还没有账号？</span>
          <router-link to="/auth/register" class="text-blue-600 font-medium ml-2">立即注册</router-link>
        </div>
      </div>
    </div>

    <!-- 页脚 -->
    <footer class="absolute bottom-0 w-full py-4 bg-white text-center text-gray-600">
      未来科技AI. 保留所有权利. | 由AI驱动的智能求职平台
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref,reactive } from 'vue'
import { User, Lock, Monitor } from '@element-plus/icons-vue'
import { useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'

const router = useRouter()
const activeTab = ref('candidate')

const loginForm = reactive({
  username: '',
  password: '',
  captcha: ''
})

const rules = {
  username: [
    { required: true, message: '请输入账号', trigger: 'blur' },
    { min: 4, max: 16, message: '长度在4到16个字符', trigger: 'blur' }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 6, max: 20, message: '长度在6到20个字符', trigger: 'blur' }
  ],
//   captcha: [
//     { required: true, message: '请输入验证码', trigger: 'blur' },
//     { pattern: /^[A-Za-z0-9]{4}$/, message: '验证码格式不正确', trigger: 'blur' }
//   ]
}

const handleLogin = () => {
//   if (loginForm.value.captcha !== 'A7B9' && loginForm.value.captcha !== 'C3D5') {
//     ElMessage.error('验证码错误')
//     return
//   }

  if (activeTab.value === 'candidate') {
    router.push('/candidate/dashboard')
  } else {
    router.push('/interviewer/dashboard')
  }
}
</script>

<style scoped>
.el-tabs__item {
  font-size: 18px;
  padding: 0 20px;
  height: 50px;
}

.el-input {
  font-size: 18px;
}

.el-button {
  height: 50px;
  font-size: 18px;
}
</style>