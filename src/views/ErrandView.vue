<script setup lang="ts">
import { ref } from 'vue'

const activeTab = ref('all')
const tabs = [
  { key: 'all', label: '全部' },
  { key: 'delivery', label: '代取' },
  { key: 'print', label: '代打印' },
  { key: 'food', label: '代买' },
  { key: 'other', label: '其他' },
]

const errands = [
  { id: 1, title: '代取快递 - 菜鸟驿站', reward: '¥3', location: '东校区菜鸟驿站', deadline: '今天 17:00', publisher: '小明', type: 'delivery', status: 'waiting' },
  { id: 2, title: '代打印论文30页', reward: '¥5', location: '图书馆打印店', deadline: '今天 18:00', publisher: '小红', type: 'print', status: 'waiting' },
  { id: 3, title: '代买一杯奶茶', reward: '¥2', location: '南门茶百道', deadline: '今天 16:00', publisher: '小华', type: 'food', status: 'accepted' },
  { id: 4, title: '代取外卖 - 麦当劳', reward: '¥3', location: '西门外卖柜', deadline: '今天 12:30', publisher: '小李', type: 'delivery', status: 'waiting' },
  { id: 5, title: '帮忙搬书到宿舍', reward: '¥10', location: '教学楼C栋', deadline: '明天 10:00', publisher: '小王', type: 'other', status: 'waiting' },
]

const typeLabels: Record<string, string> = {
  delivery: '📦 代取',
  print: '🖨️ 代打印',
  food: '🧋 代买',
  other: '📋 其他',
}
</script>

<template>
  <section class="errand">
    <div class="errand-header">
      <h2>🏃 校园跑腿</h2>
      <p>互帮互助，轻松搞定校园小事</p>
    </div>

    <div class="tabs">
      <button
        v-for="tab in tabs"
        :key="tab.key"
        :class="['tab-btn', { active: activeTab === tab.key }]"
        @click="activeTab = tab.key"
      >
        {{ tab.label }}
      </button>
    </div>

    <div class="errand-list">
      <div v-for="item in errands" :key="item.id" class="errand-card">
        <div class="errand-top">
          <span class="errand-type">{{ typeLabels[item.type] }}</span>
          <span :class="['errand-status', item.status]">{{ item.status === 'waiting' ? '待接单' : '已接单' }}</span>
        </div>
        <h3 class="errand-title">{{ item.title }}</h3>
        <div class="errand-meta">
          <span>📍 {{ item.location }}</span>
          <span>🕐 {{ item.deadline }}</span>
        </div>
        <div class="errand-footer">
          <span class="errand-reward">酬劳 {{ item.reward }}</span>
          <span class="errand-publisher">👤 {{ item.publisher }}</span>
          <button v-if="item.status === 'waiting'" class="accept-btn">接单</button>
          <button v-else class="accepted-btn" disabled>已接单</button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.errand {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.errand-header h2 {
  margin: 0;
  font-size: 20px;
  color: #37474f;
}

.errand-header p {
  margin: 4px 0 0;
  font-size: 14px;
  color: #90a4ae;
}

.tabs {
  display: flex;
  gap: 8px;
  overflow-x: auto;
}

.tab-btn {
  padding: 8px 18px;
  border: none;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.7);
  color: #78909c;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.25s ease;
  white-space: nowrap;
}

.tab-btn:hover {
  background: rgba(38, 166, 154, 0.08);
  color: #37474f;
}

.tab-btn.active {
  background: #26a69a;
  color: #fff;
  font-weight: 600;
}

.errand-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.errand-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 16px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  transition: all 0.25s ease;
}

.errand-card:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

.errand-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.errand-type {
  font-size: 13px;
  font-weight: 600;
  color: #546e7a;
}

.errand-status {
  font-size: 12px;
  padding: 2px 10px;
  border-radius: 12px;
  font-weight: 500;
}

.errand-status.waiting {
  background: rgba(239, 108, 0, 0.1);
  color: #ef6c00;
}

.errand-status.accepted {
  background: rgba(38, 166, 154, 0.1);
  color: #26a69a;
}

.errand-title {
  margin: 0 0 8px;
  font-size: 16px;
  color: #37474f;
}

.errand-meta {
  display: flex;
  gap: 16px;
  font-size: 13px;
  color: #90a4ae;
  margin-bottom: 12px;
}

.errand-footer {
  display: flex;
  align-items: center;
  gap: 12px;
}

.errand-reward {
  font-size: 16px;
  font-weight: 700;
  color: #ef6c00;
}

.errand-publisher {
  font-size: 13px;
  color: #90a4ae;
}

.accept-btn {
  margin-left: auto;
  padding: 6px 18px;
  border: none;
  border-radius: 16px;
  background: #26a69a;
  color: #fff;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.25s ease;
}

.accept-btn:hover {
  background: #00897b;
}

.accepted-btn {
  margin-left: auto;
  padding: 6px 18px;
  border: none;
  border-radius: 16px;
  background: #eceff1;
  color: #90a4ae;
  font-size: 13px;
  cursor: not-allowed;
}
</style>