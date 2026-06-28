<script setup lang="ts">
import { ref } from 'vue'

const activeTab = ref('all')
const tabs = [
  { key: 'all', label: '全部' },
  { key: 'book', label: '教材' },
  { key: 'digital', label: '数码' },
  { key: 'life', label: '生活' },
  { key: 'ticket', label: '票券' },
]

const items = [
  { id: 1, title: '高等数学同济第七版', price: '¥15', original: '¥40', tag: '教材', seller: '小明', campus: '东校区' },
  { id: 2, title: '小米充电宝20000mAh', price: '¥45', original: '¥129', tag: '数码', seller: '小红', campus: '西校区' },
  { id: 3, title: '宜家台灯九成新', price: '¥30', original: '¥79', tag: '生活', seller: '小华', campus: '南校区' },
  { id: 4, title: '周杰伦演唱会门票', price: '¥680', original: '¥880', tag: '票券', seller: '小李', campus: '东校区' },
  { id: 5, title: '线性代数教材', price: '¥10', original: '¥35', tag: '教材', seller: '小王', campus: '西校区' },
  { id: 6, title: 'AirPods Pro 二代', price: '¥580', original: '¥1299', tag: '数码', seller: '小张', campus: '南校区' },
]
</script>

<template>
  <section class="trade">
    <div class="trade-header">
      <h2>🛒 二手交易</h2>
      <p>校园闲置好物，低价淘到你想要的</p>
    </div>

    <div class="search-bar">
      <span class="search-icon">🔍</span>
      <input type="text" placeholder="搜索你想要的物品..." />
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

    <div class="item-grid">
      <div v-for="item in items" :key="item.id" class="item-card">
        <div class="card-img">
          <span class="card-tag">{{ item.tag }}</span>
          <div class="card-placeholder">📦</div>
        </div>
        <div class="card-body">
          <div class="card-title">{{ item.title }}</div>
          <div class="card-price">
            <span class="price-now">{{ item.price }}</span>
            <span class="price-original">{{ item.original }}</span>
          </div>
          <div class="card-footer">
            <span class="card-seller">{{ item.seller }}</span>
            <span class="card-campus">{{ item.campus }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.trade {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.trade-header h2 {
  margin: 0;
  font-size: 20px;
  color: #37474f;
}

.trade-header p {
  margin: 4px 0 0;
  font-size: 14px;
  color: #90a4ae;
}

.search-bar {
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 12px;
  padding: 12px 18px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
}

.search-icon {
  font-size: 18px;
}

.search-bar input {
  flex: 1;
  border: none;
  outline: none;
  background: transparent;
  font-size: 15px;
  color: #37474f;
}

.search-bar input::placeholder {
  color: #b0bec5;
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

.item-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
}

.item-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: all 0.25s ease;
}

.item-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.card-img {
  height: 140px;
  background: linear-gradient(135deg, #e0f2f1, #e8f5e9);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.card-placeholder {
  font-size: 40px;
}

.card-tag {
  position: absolute;
  top: 10px;
  left: 10px;
  background: rgba(38, 166, 154, 0.9);
  color: #fff;
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 12px;
}

.card-body {
  padding: 12px 14px;
}

.card-title {
  font-size: 14px;
  font-weight: 600;
  color: #37474f;
  margin-bottom: 6px;
}

.card-price {
  display: flex;
  align-items: baseline;
  gap: 6px;
  margin-bottom: 8px;
}

.price-now {
  font-size: 18px;
  font-weight: 700;
  color: #ef6c00;
}

.price-original {
  font-size: 12px;
  color: #b0bec5;
  text-decoration: line-through;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: #90a4ae;
}
</style>