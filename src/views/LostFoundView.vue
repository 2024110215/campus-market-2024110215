<script setup lang="ts">
import { ref } from 'vue'

const activeTab = ref('lost')
const tabs = [
  { key: 'lost', label: '寻物启事' },
  { key: 'found', label: '失物招领' },
]

const lostItems = [
  { id: 1, title: '丢失蓝色水杯', location: '教学楼A栋3楼', time: '今天 14:30', desc: '膳魔师蓝色保温杯，杯盖有贴纸', contact: '微信: zhang123' },
  { id: 2, title: '丢失校园卡', location: '食堂二楼', time: '今天 12:00', desc: '姓名：李明，学号尾号 2024', contact: '电话: 138****5678' },
  { id: 3, title: '丢失黑色雨伞', location: '图书馆一楼', time: '昨天 18:00', desc: '天堂牌黑色长柄伞，手柄有磨损', contact: '微信: wang_umbrella' },
]

const foundItems = [
  { id: 4, title: '捡到一串钥匙', location: '操场看台', time: '今天 15:00', desc: '三把钥匙加一个汽车遥控器', contact: '微信: finder001' },
  { id: 5, title: '捡到AirPods耳机盒', location: '教学楼B栋', time: '今天 10:30', desc: '白色AirPods充电盒，无耳机', contact: '电话: 139****8765' },
  { id: 6, title: '捡到一本高数课本', location: '自习室201', time: '昨天 21:00', desc: '高等数学同济版，内夹笔记', contact: '微信: book_finder' },
]
</script>

<template>
  <section class="lost-found">
    <div class="lf-header">
      <h2>🔍 失物招领</h2>
      <p>丢失物品或捡到物品，在这里发布信息</p>
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

    <div class="item-list">
      <template v-if="activeTab === 'lost'">
        <div v-for="item in lostItems" :key="item.id" class="lf-card lost">
          <div class="lf-badge">寻物</div>
          <div class="lf-body">
            <h3 class="lf-title">{{ item.title }}</h3>
            <p class="lf-desc">{{ item.desc }}</p>
            <div class="lf-meta">
              <span>📍 {{ item.location }}</span>
              <span>🕐 {{ item.time }}</span>
            </div>
            <div class="lf-contact">联系：{{ item.contact }}</div>
          </div>
        </div>
      </template>
      <template v-else>
        <div v-for="item in foundItems" :key="item.id" class="lf-card found">
          <div class="lf-badge">招领</div>
          <div class="lf-body">
            <h3 class="lf-title">{{ item.title }}</h3>
            <p class="lf-desc">{{ item.desc }}</p>
            <div class="lf-meta">
              <span>📍 {{ item.location }}</span>
              <span>🕐 {{ item.time }}</span>
            </div>
            <div class="lf-contact">联系：{{ item.contact }}</div>
          </div>
        </div>
      </template>
    </div>
  </section>
</template>

<style scoped>
.lost-found {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.lf-header h2 {
  margin: 0;
  font-size: 20px;
  color: #37474f;
}

.lf-header p {
  margin: 4px 0 0;
  font-size: 14px;
  color: #90a4ae;
}

.tabs {
  display: flex;
  gap: 8px;
}

.tab-btn {
  padding: 8px 20px;
  border: none;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.7);
  color: #78909c;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.25s ease;
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

.item-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.lf-card {
  display: flex;
  gap: 14px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 16px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  border-left: 4px solid;
}

.lf-card.lost {
  border-left-color: #ef6c00;
}

.lf-card.found {
  border-left-color: #26a69a;
}

.lf-badge {
  writing-mode: vertical-lr;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 8px;
  padding: 8px 4px;
  font-size: 13px;
  font-weight: 600;
  color: #546e7a;
  text-align: center;
  letter-spacing: 2px;
}

.lf-body {
  flex: 1;
}

.lf-title {
  margin: 0 0 6px;
  font-size: 16px;
  color: #37474f;
}

.lf-desc {
  margin: 0 0 8px;
  font-size: 14px;
  color: #78909c;
}

.lf-meta {
  display: flex;
  gap: 16px;
  font-size: 13px;
  color: #90a4ae;
  margin-bottom: 8px;
}

.lf-contact {
  font-size: 13px;
  color: #26a69a;
  font-weight: 500;
}
</style>