<script setup lang="ts">
const user = {
  name: '张同学',
  avatar: '🧑‍🎓',
  campus: '东校区',
  joinDate: '2025年9月',
}

const stats = [
  { label: '已发布', value: '12', icon: '📦' },
  { label: '已售出', value: '8', icon: '💰' },
  { label: '收藏', value: '5', icon: '❤️' },
  { label: '关注', value: '23', icon: '👀' },
]

const menuItems = [
  { icon: '📋', label: '我的发布', desc: '管理发布的物品' },
  { icon: '💰', label: '我的出售', desc: '查看交易记录' },
  { icon: '❤️', label: '我的收藏', desc: '收藏的物品列表' },
  { icon: '🏃', label: '跑腿记录', desc: '接单与发布记录' },
  { icon: '🛍️', label: '团购记录', desc: '参与的拼团' },
  { icon: '📍', label: '收货地址', desc: '管理收货地址' },
  { icon: '⚙️', label: '设置', desc: '账号与隐私设置' },
  { icon: '❓', label: '帮助与反馈', desc: '常见问题与建议' },
]
</script>

<template>
  <section class="user-center">
    <div class="profile-card">
      <div class="profile-avatar">{{ user.avatar }}</div>
      <div class="profile-info">
        <h2 class="profile-name">{{ user.name }}</h2>
        <span class="profile-campus">{{ user.campus }}</span>
        <span class="profile-join">加入于 {{ user.joinDate }}</span>
      </div>
    </div>

    <div class="stats-row">
      <div v-for="stat in stats" :key="stat.label" class="stat-item">
        <span class="stat-icon">{{ stat.icon }}</span>
        <span class="stat-value">{{ stat.value }}</span>
        <span class="stat-label">{{ stat.label }}</span>
      </div>
    </div>

    <div class="menu-list">
      <div v-for="item in menuItems" :key="item.label" class="menu-item">
        <span class="menu-icon">{{ item.icon }}</span>
        <div class="menu-info">
          <span class="menu-label">{{ item.label }}</span>
          <span class="menu-desc">{{ item.desc }}</span>
        </div>
        <span class="menu-arrow">›</span>
      </div>
    </div>
  </section>
</template>

<style scoped>
.user-center {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.profile-card {
  display: flex;
  align-items: center;
  gap: 18px;
  background: linear-gradient(135deg, #ab47bc, #7e57c2);
  border-radius: 16px;
  padding: 28px 24px;
  color: #fff;
  box-shadow: 0 8px 24px rgba(171, 71, 188, 0.25);
}

.profile-avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
}

.profile-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.profile-name {
  margin: 0;
  font-size: 20px;
  font-weight: 700;
}

.profile-campus {
  font-size: 13px;
  opacity: 0.9;
}

.profile-join {
  font-size: 12px;
  opacity: 0.7;
}

.stats-row {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 12px;
}

.stat-item {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 12px;
  padding: 16px 12px;
  text-align: center;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
}

.stat-icon {
  font-size: 22px;
  display: block;
  margin-bottom: 6px;
}

.stat-value {
  font-size: 20px;
  font-weight: 700;
  color: #37474f;
  display: block;
}

.stat-label {
  font-size: 12px;
  color: #90a4ae;
  display: block;
  margin-top: 2px;
}

.menu-list {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
}

.menu-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 16px 20px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.04);
  cursor: pointer;
  transition: background 0.2s ease;
}

.menu-item:last-child {
  border-bottom: none;
}

.menu-item:hover {
  background: rgba(38, 166, 154, 0.04);
}

.menu-icon {
  font-size: 22px;
}

.menu-info {
  flex: 1;
}

.menu-label {
  font-size: 15px;
  font-weight: 600;
  color: #37474f;
  display: block;
}

.menu-desc {
  font-size: 12px;
  color: #b0bec5;
  display: block;
  margin-top: 2px;
}

.menu-arrow {
  font-size: 20px;
  color: #cfd8dc;
}
</style>