<template>
  <section class="page">
    <div class="page-header">
      <h1>拼单搭子</h1>
      <p>拼单省钱、搭子组队，一起更精彩。</p>
    </div>

    <EmptyState
      v-if="groupBuys.length === 0"
      text="暂无拼单搭子信息"
    />
    <div v-else class="list">
      <ItemCard
        v-for="item in groupBuys"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :tag="item.type"
        :location="item.location"
        :time="item.deadline"
      >
        <template #footer>
          <div class="gb-footer">
            <span class="progress">{{ item.currentCount }}/{{ item.targetCount }}人</span>
            <span class="publisher">{{ item.publisher }}</span>
          </div>

          <button class="favorite-btn" @click="favoriteStore.toggleFavorite({
            id: item.id,
            type: 'groupBuy',
            title: item.title,
            description: item.description,
            location: item.location
          })">
            {{ favoriteStore.isFavorite('groupBuy', item.id) ? '已收藏' : '收藏' }}
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
import { getGroupBuys, type GroupBuyItem } from '../api/groupBuy'
import { useFavoriteStore } from '../stores/favorite'

const groupBuys = ref<GroupBuyItem[]>([])
const favoriteStore = useFavoriteStore()

onMounted(async () => {
  const res = await getGroupBuys()
  groupBuys.value = res.data
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

.gb-footer {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 13px;
}

.progress {
  color: #2563eb;
  font-weight: 600;
}

.publisher {
  color: #6b7280;
}
</style>