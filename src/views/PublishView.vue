<script setup lang="ts">
import { ref } from 'vue'

const form = ref({
  title: '',
  category: 'trade',
  price: '',
  originalPrice: '',
  description: '',
  campus: 'east',
})

const categories = [
  { key: 'trade', label: '🛒 二手交易' },
  { key: 'lost-found', label: '🔍 失物招领' },
  { key: 'group-buy', label: '🛍️ 拼单搭子' },
  { key: 'errand', label: '🏃 跑腿委托' },
]

const campuses = [
  { key: 'east', label: '东校区' },
  { key: 'west', label: '西校区' },
  { key: 'south', label: '南校区' },
  { key: 'north', label: '北校区' },
]
</script>

<template>
  <section class="publish">
    <div class="publish-header">
      <h2>✏️ 发布信息</h2>
      <p>选择类型，发布你的二手、失物、拼单或跑腿信息</p>
    </div>

    <div class="form-card">
      <div class="form-group">
        <label>信息标题</label>
        <input v-model="form.title" type="text" placeholder="请输入标题" />
      </div>

      <div class="form-group">
        <label>发布类型</label>
        <div class="option-grid">
          <button
            v-for="cat in categories"
            :key="cat.key"
            :class="['option-btn', { active: form.category === cat.key }]"
            @click="form.category = cat.key"
          >
            {{ cat.label }}
          </button>
        </div>
      </div>

      <div class="form-row">
        <div class="form-group flex-1">
          <label>出售价格</label>
          <input v-model="form.price" type="text" placeholder="¥" />
        </div>
        <div class="form-group flex-1">
          <label>原价</label>
          <input v-model="form.originalPrice" type="text" placeholder="¥" />
        </div>
      </div>

      <div class="form-group">
        <label>所在校区</label>
        <div class="option-grid">
          <button
            v-for="camp in campuses"
            :key="camp.key"
            :class="['option-btn', { active: form.campus === camp.key }]"
            @click="form.campus = camp.key"
          >
            {{ camp.label }}
          </button>
        </div>
      </div>

      <div class="form-group">
        <label>详细描述</label>
        <textarea v-model="form.description" placeholder="请详细描述信息内容..." rows="4"></textarea>
      </div>

      <div class="form-group">
        <label>上传图片</label>
        <div class="upload-area">
          <span class="upload-icon">📷</span>
          <span class="upload-text">点击上传图片</span>
        </div>
      </div>

      <button class="submit-btn">发布信息</button>
    </div>
  </section>
</template>

<style scoped>
.publish {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.publish-header {
  background: linear-gradient(135deg, #ff7043, #ffca28);
  border-radius: 16px;
  padding: 28px 24px;
  color: #fff;
  box-shadow: 0 8px 24px rgba(255, 112, 67, 0.25);
}

.publish-header h2 {
  font-size: 20px;
  margin-bottom: 6px;
  font-weight: 700;
}

.publish-header p {
  font-size: 14px;
  opacity: 0.9;
}

.form-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 2px 16px rgba(0, 0, 0, 0.06);
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  font-size: 14px;
  font-weight: 600;
  color: #37474f;
}

.form-group input,
.form-group textarea {
  border: 1.5px solid #e0e0e0;
  border-radius: 10px;
  padding: 10px 14px;
  font-size: 14px;
  outline: none;
  transition: border-color 0.2s;
  background: rgba(255, 255, 255, 0.7);
  color: #37474f;
  resize: none;
}

.form-group input:focus,
.form-group textarea:focus {
  border-color: #26a69a;
  box-shadow: 0 0 0 3px rgba(38, 166, 154, 0.1);
}

.form-row {
  display: flex;
  gap: 14px;
}

.flex-1 {
  flex: 1;
}

.option-grid {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.option-btn {
  padding: 8px 16px;
  border: 1.5px solid #e0e0e0;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.7);
  font-size: 13px;
  cursor: pointer;
  transition: all 0.2s;
  color: #78909c;
}

.option-btn.active {
  border-color: #26a69a;
  background: rgba(38, 166, 154, 0.1);
  color: #26a69a;
  font-weight: 600;
}

.option-btn:hover:not(.active) {
  border-color: #b0bec5;
}

.upload-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border: 2px dashed #cfd8dc;
  border-radius: 12px;
  padding: 28px;
  cursor: pointer;
  transition: border-color 0.2s;
}

.upload-area:hover {
  border-color: #26a69a;
}

.upload-icon {
  font-size: 32px;
}

.upload-text {
  font-size: 13px;
  color: #90a4ae;
}

.submit-btn {
  padding: 14px;
  border: none;
  border-radius: 12px;
  background: linear-gradient(135deg, #26a69a, #66bb6a);
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  box-shadow: 0 4px 16px rgba(38, 166, 154, 0.3);
}

.submit-btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(38, 166, 154, 0.4);
}

.submit-btn:active {
  transform: translateY(0);
}
</style>