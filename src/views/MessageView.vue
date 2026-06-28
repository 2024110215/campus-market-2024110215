<script setup lang="ts">
const conversations = [
  { id: 1, name: '小明', avatar: '🧑', lastMsg: '教材还在吗？可以便宜点吗', time: '刚刚', unread: 2 },
  { id: 2, name: '小红', avatar: '👩', lastMsg: '好的，明天下午东校区见', time: '10分钟前', unread: 0 },
  { id: 3, name: '小华', avatar: '👨', lastMsg: '台灯可以送货到宿舍吗？', time: '1小时前', unread: 1 },
  { id: 4, name: '小李', avatar: '🧑‍🎓', lastMsg: '票已经出了，谢谢！', time: '昨天', unread: 0 },
  { id: 5, name: '小王', avatar: '👩‍🎓', lastMsg: '请问这个还可用吗？', time: '昨天', unread: 0 },
]

const notifications = [
  { id: 1, type: 'system', content: '你的物品「高等数学」已成功发布', time: '2分钟前' },
  { id: 2, type: 'system', content: '小明对你的物品发起了聊天', time: '5分钟前' },
  { id: 3, type: 'system', content: '系统维护通知：今晚22:00-23:00', time: '1小时前' },
]
</script>

<template>
  <section class="message">
    <div class="msg-header">
      <h2>💬 消息中心</h2>
    </div>

    <div class="notification-bar">
      <div v-for="n in notifications" :key="n.id" class="notification-item">
        <span class="notif-dot"></span>
        <span class="notif-content">{{ n.content }}</span>
        <span class="notif-time">{{ n.time }}</span>
      </div>
    </div>

    <div class="section-label">聊天列表</div>
    <div class="conversation-list">
      <div v-for="conv in conversations" :key="conv.id" class="conversation-item">
        <div class="conv-avatar">{{ conv.avatar }}</div>
        <div class="conv-info">
          <div class="conv-top">
            <span class="conv-name">{{ conv.name }}</span>
            <span class="conv-time">{{ conv.time }}</span>
          </div>
          <div class="conv-msg">{{ conv.lastMsg }}</div>
        </div>
        <div v-if="conv.unread > 0" class="conv-badge">{{ conv.unread }}</div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.message {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.msg-header {
  background: linear-gradient(135deg, #5c6bc0, #42a5f5);
  border-radius: 16px;
  padding: 28px 24px;
  color: #fff;
  box-shadow: 0 8px 24px rgba(92, 107, 192, 0.25);
}

.msg-header h2 {
  font-size: 20px;
  font-weight: 700;
}

.notification-bar {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 14px 18px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.notification-item {
  display: flex;
  align-items: center;
  gap: 8px;
}

.notif-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: #26a69a;
  flex-shrink: 0;
}

.notif-content {
  flex: 1;
  font-size: 13px;
  color: #546e7a;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.notif-time {
  font-size: 11px;
  color: #b0bec5;
  flex-shrink: 0;
}

.section-label {
  font-size: 16px;
  font-weight: 600;
  color: #37474f;
  padding-left: 10px;
  border-left: 3px solid #5c6bc0;
}

.conversation-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.conversation-item {
  display: flex;
  align-items: center;
  gap: 14px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(8px);
  border-radius: 14px;
  padding: 14px 18px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  cursor: pointer;
  transition: transform 0.2s;
}

.conversation-item:hover {
  transform: translateX(4px);
}

.conv-avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: linear-gradient(135deg, #e8eaf6, #e3f2fd);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  flex-shrink: 0;
}

.conv-info {
  flex: 1;
  min-width: 0;
}

.conv-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

.conv-name {
  font-size: 14px;
  font-weight: 600;
  color: #37474f;
}

.conv-time {
  font-size: 11px;
  color: #b0bec5;
}

.conv-msg {
  font-size: 13px;
  color: #90a4ae;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.conv-badge {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #ef5350;
  color: #fff;
  font-size: 11px;
  font-weight: 600;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}
</style>