<script setup lang="ts">
const quickActions = [
  { icon: '🛒', label: '二手交易', desc: '闲置好物低价淘', route: 'trade' },
  { icon: '🔍', label: '失物招领', desc: '丢失找回捡到归还', route: 'lostFound' },
  { icon: '🛍️', label: '拼单搭子', desc: '一起拼团更优惠', route: 'groupBuy' },
  { icon: '🏃', label: '跑腿委托', desc: '互帮互助校园事', route: 'errand' },
]

const recentItems = [
  { id: 1, title: '高等数学同济第七版', price: '¥15', tag: '教材', time: '3分钟前' },
  { id: 2, title: '小米充电宝20000mAh', price: '¥45', tag: '数码', time: '10分钟前' },
  { id: 3, title: '宜家台灯九成新', price: '¥30', tag: '生活', time: '30分钟前' },
  { id: 4, title: '周杰伦演唱会门票', price: '¥680', tag: '票券', time: '1小时前' },
]
</script>

<template>
  <section class="home">
    <div class="hero-card">
      <h2>👋 欢迎来到校园轻集市</h2>
      <p>在这里轻松买卖校园闲置物品，让资源循环起来！</p>
    </div>

    <div class="section-title">快捷分类</div>
    <div class="quick-actions">
      <RouterLink v-for="action in quickActions" :key="action.label" :to="{ name: action.route }" class="action-card">
        <span class="action-icon">{{ action.icon }}</span>
        <span class="action-label">{{ action.label }}</span>
        <span class="action-desc">{{ action.desc }}</span>
      </RouterLink>
    </div>

    <div class="section-title">最新发布</div>
    <div class="recent-list">
      <div v-for="item in recentItems" :key="item.id" class="recent-item">
        <div class="item-info">
          <span class="item-tag">{{ item.tag }}</span>
          <span class="item-title">{{ item.title }}</span>
        </div>
        <div class="item-meta">
          <span class="item-price">{{ item.price }}</span>
          <span class="item-time">{{ item.time }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.hero-card {
  background: linear-gradient(135deg, #26a69a, #66bb6a);
  border-radius: 16px;
  padding: 32px 28px;
  color: #fff;
  box-shadow: 0 8px 24px rgba(38, 166, 154, 0.25);
}

.hero-card h2 {
  font-size: 22px;
  margin-bottom: 8px;
  font-weight: 700;
}

.hero-card p {
  font-size: 14px;
  opacity: 0.9;
  line-height: 1.6;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
  color: #37474f;
  padding-left: 10px;
  border-left: 3px solid #26a69a;
}

.quick-actions {
  display: flex;
  justify-content: space-between;
  gap: 12px;
}

.action-card {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 18px 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  text-decoration: none;
  color: inherit;
}

.action-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.action-icon {
  font-size: 28px;
}

.action-label {
  font-size: 14px;
  font-weight: 600;
  color: #37474f;
}

.action-desc {
  font-size: 11px;
  color: #90a4ae;
}

.recent-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.recent-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(8px);
  border-radius: 12px;
  padding: 14px 18px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  transition: transform 0.2s;
}

.recent-item:hover {
  transform: translateX(4px);
}

.item-info {
  display: flex;
  align-items: center;
  gap: 10px;
}

.item-tag {
  background: rgba(38, 166, 154, 0.12);
  color: #26a69a;
  font-size: 11px;
  font-weight: 600;
  padding: 2px 8px;
  border-radius: 6px;
}

.item-title {
  font-size: 14px;
  color: #37474f;
}

.item-meta {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 2px;
}

.item-price {
  font-size: 15px;
  font-weight: 700;
  color: #ef6c00;
}

.item-time {
  font-size: 11px;
  color: #b0bec5;
}
</style>