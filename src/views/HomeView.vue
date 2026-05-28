<template>
  <div class="home">
    <header class="hero">
      <h1>✈️ AI 旅游规划师</h1>
      <p>输入你的想法，AI 为你生成专属行程</p>
    </header>

    <div class="container">
      <div class="form-card">
        <div class="form-group">
          <label>目的地</label>
          <input v-model="form.destination" placeholder="例如：大理、京都、巴厘岛" />
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>天数</label>
            <input v-model.number="form.days" type="number" min="1" max="30" placeholder="3" />
          </div>
          <div class="form-group">
            <label>预算（元）</label>
            <input v-model.number="form.budget" type="number" min="0" placeholder="3000" />
          </div>
        </div>
        <div class="form-group">
          <label>偏好（选填）</label>
          <textarea v-model="form.preferences" placeholder="例如：喜欢安静，不喜欢爬山，偏爱美食..." rows="3" />
        </div>
        <button @click="generatePlan" :disabled="loading" class="btn-generate">
          {{ loading ? '⏳ AI 规划中...' : '🚀 生成行程' }}
        </button>
      </div>

      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>AI 正在为你精心规划行程，请稍候...</p>
      </div>

      <div v-if="result" class="result-card">
        <h2>📍 {{ result.destination }} · {{ result.days }}天行程</h2>
        <div class="plan-content" v-html="renderedPlan"></div>
      </div>

      <div v-if="error" class="error">{{ error }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import axios from 'axios'
import { marked } from 'marked'

const form = ref({
  destination: '',
  days: 3,
  budget: 3000,
  preferences: ''
})

const loading = ref(false)
const result = ref<any>(null)
const error = ref('')

const renderedPlan = computed(() => {
  if (!result.value?.planContent) return ''
  return marked(result.value.planContent)
})

async function generatePlan() {
  if (!form.value.destination) {
    error.value = '请输入目的地'
    return
  }
  loading.value = true
  error.value = ''
  result.value = null

  try {
    const response = await axios.post('http://localhost:8080/api/travel/plans', form.value)
    result.value = response.data
  } catch (e: any) {
    error.value = '生成失败，请重试'
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.home {
  min-height: 100vh;
}

.hero {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
  padding: 60px 20px;
}

.hero h1 {
  font-size: 2.5rem;
  margin-bottom: 10px;
}

.hero p {
  font-size: 1.1rem;
  opacity: 0.9;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 30px 20px;
}

.form-card {
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
  margin-bottom: 24px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  font-weight: 600;
  margin-bottom: 8px;
  color: #333;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 12px 16px;
  border: 2px solid #e8e8e8;
  border-radius: 10px;
  font-size: 1rem;
  transition: border-color 0.2s;
  outline: none;
}

.form-group input:focus,
.form-group textarea:focus {
  border-color: #667eea;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.btn-generate {
  width: 100%;
  padding: 14px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn-generate:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.btn-generate:hover:not(:disabled) {
  opacity: 0.9;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #666;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f0f0f0;
  border-top-color: #667eea;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
  margin: 0 auto 16px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.result-card {
  background: white;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.08);
}

.result-card h2 {
  font-size: 1.5rem;
  margin-bottom: 20px;
  color: #333;
  padding-bottom: 16px;
  border-bottom: 2px solid #f0f0f0;
}

.plan-content {
  line-height: 1.8;
  color: #444;
}

.plan-content h3 {
  margin: 20px 0 10px;
  color: #333;
}

.plan-content ul, .plan-content ol {
  padding-left: 20px;
  margin: 10px 0;
}

.plan-content table {
  width: 100%;
  border-collapse: collapse;
  margin: 16px 0;
}

.plan-content th, .plan-content td {
  padding: 10px;
  border: 1px solid #e8e8e8;
  text-align: left;
}

.plan-content th {
  background: #f8f8f8;
  font-weight: 600;
}

.error {
  background: #fff2f0;
  border: 1px solid #ffccc7;
  color: #ff4d4f;
  padding: 12px 16px;
  border-radius: 10px;
  text-align: center;
}
</style>
