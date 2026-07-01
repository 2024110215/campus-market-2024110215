<template>
  <section class="page">
    <div class="page-header">
      <h1>跑腿委托</h1>
      <p>互帮互助，轻松搞定校园小事。</p>
    </div>

    <EmptyState
      v-if="errands.length === 0"
      text="暂无跑腿委托信息"
    />
    <div v-else class="list">
      <ItemCard
        v-for="item in errands"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :tag="item.taskType"
        :location="item.from"
        :time="item.deadline"
      >
        <template #footer>
          <div class="errand-footer">
            <strong>￥{{ item.reward }}</strong>
            <span class="status" :class="item.status">{{ item.status === 'open' ? '待接单' : '已接单' }}</span>
          </div>

          <button class="favorite-btn" @click="favoriteStore.toggleFavorite({
            id: item.id,
            type: 'errand',
            title: item.title,
            description: item.description,
            location: item.from
          })">
            {{ favoriteStore.isFavorite('errand', item.id) ? '已收藏' : '收藏' }}
          </button>
        </template>
      </ItemCard>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import ItemCard from '../components/ItemCard.vue'
import EmptyState from '../components/EmptyState.vue'
import { getErrands, type ErrandItem } from '../api/errand'
import { useFavoriteStore } from '../stores/favorite'

const errands = ref<ErrandItem[]>([])
const favoriteStore = useFavoriteStore()

onMounted(async () => {
  const res = await getErrands()
  errands.value = res.data
})
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.page-header {
  padding: 24px;
  border-radius: 16px;
  background: #fff;
}

.page-header h1 {
  margin: 0 0 8px;
}

.favorite-btn {
  margin-left: 12px;
  border: none;
  border-radius: 999px;
  padding: 6px 12px;
  cursor: pointer;
  background: #f3f4f6;
  color: #374151;
}

.page-header p {
  margin: 0;
  color: #6b7280;
}

.list {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.errand-footer {
  display: flex;
  align-items: center;
  gap: 12px;
}

.status {
  font-size: 12px;
  padding: 2px 8px;
  border-radius: 999px;
  font-weight: 500;
}

.status.open {
  background: #fef3c7;
  color: #d97706;
}

.status.accepted {
  background: #d1fae5;
  color: #059669;
}
</style>