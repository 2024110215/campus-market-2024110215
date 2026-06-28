<script setup lang="ts">
const groupBuys = [
  {
    id: 1,
    title: '新鲜水果拼团',
    organizer: '小陈',
    price: '¥29.9/箱',
    original: '¥49.9',
    current: 8,
    target: 15,
    deadline: '明天 18:00',
    campus: '东校区',
    tag: '拼单',
  },
  {
    id: 2,
    title: '找健身搭子',
    organizer: '小刘',
    price: '免费',
    original: '',
    current: 2,
    target: 4,
    deadline: '长期有效',
    campus: '西校区',
    tag: '搭子',
  },
  {
    id: 3,
    title: '图书馆自习组队',
    organizer: '小赵',
    price: '免费',
    original: '',
    current: 3,
    target: 6,
    deadline: '本周六',
    campus: '南校区',
    tag: '组队',
  },
  {
    id: 4,
    title: '零食大礼包拼团',
    organizer: '小孙',
    price: '¥39.9/份',
    original: '¥69.9',
    current: 12,
    target: 20,
    deadline: '后天 12:00',
    campus: '东校区',
    tag: '拼单',
  },
]
</script>

<template>
  <section class="group-buy">
    <div class="gb-header">
      <h2>🛍️ 拼单搭子</h2>
      <p>拼单省钱、搭子组队，一起更精彩</p>
    </div>

    <div class="gb-list">
      <div v-for="item in groupBuys" :key="item.id" class="gb-card">
        <div class="gb-top">
          <span class="gb-tag">{{ item.tag }}</span>
          <span class="gb-deadline">⏰ {{ item.deadline }}</span>
        </div>
        <h3 class="gb-title">{{ item.title }}</h3>
        <div class="gb-price-row">
          <span class="gb-price">{{ item.price }}</span>
          <span class="gb-original">{{ item.original }}</span>
        </div>
        <div class="gb-progress">
          <div class="progress-bar">
            <div class="progress-fill" :style="{ width: Math.min((item.current / item.target) * 100, 100) + '%' }"></div>
          </div>
          <span class="progress-text">{{ item.current }}/{{ item.target }}人</span>
        </div>
        <div class="gb-footer">
          <span class="gb-organizer">👤 {{ item.organizer }}</span>
          <span class="gb-campus">📍 {{ item.campus }}</span>
          <button class="gb-join-btn">{{ item.tag === '拼单' ? '参与拼团' : item.tag === '搭子' ? '成为搭子' : '加入组队' }}</button>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.group-buy {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.gb-header h2 {
  margin: 0;
  font-size: 20px;
  color: #37474f;
}

.gb-header p {
  margin: 4px 0 0;
  font-size: 14px;
  color: #90a4ae;
}

.gb-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
}

.gb-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 18px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  transition: all 0.25s ease;
}

.gb-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.gb-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.gb-tag {
  background: rgba(92, 107, 192, 0.12);
  color: #5c6bc0;
  padding: 2px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 600;
}

.gb-deadline {
  font-size: 12px;
  color: #ef6c00;
}

.gb-title {
  margin: 0 0 8px;
  font-size: 16px;
  color: #37474f;
}

.gb-price-row {
  display: flex;
  align-items: baseline;
  gap: 8px;
  margin-bottom: 12px;
}

.gb-price {
  font-size: 20px;
  font-weight: 700;
  color: #ef6c00;
}

.gb-original {
  font-size: 13px;
  color: #b0bec5;
  text-decoration: line-through;
}

.gb-progress {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 12px;
}

.progress-bar {
  flex: 1;
  height: 8px;
  background: #eceff1;
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #26a69a, #66bb6a);
  border-radius: 4px;
  transition: width 0.3s ease;
}

.progress-text {
  font-size: 12px;
  color: #78909c;
  white-space: nowrap;
}

.gb-footer {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 13px;
  color: #90a4ae;
}

.gb-join-btn {
  margin-left: auto;
  padding: 6px 16px;
  border: none;
  border-radius: 16px;
  background: #26a69a;
  color: #fff;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.25s ease;
}

.gb-join-btn:hover {
  background: #00897b;
}
</style>