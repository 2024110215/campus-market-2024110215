<template>
  <section class="page">
    <div class="page-header">
      <h1>失物招领</h1>
      <p>丢失物品或捡到物品，在这里发布信息。</p>
    </div>

    <SearchBar v-model="keyword" placeholder="搜索物品名称..." />

    <LoadingState v-if="loading" text="正在加载失物招领信息..." />

    <ErrorState
      v-else-if="error"
      :text="error"
      :show-retry="true"
      @retry="fetchLostFounds"
    />

    <EmptyState
      v-else-if="filteredItems.length === 0"
      text="暂无失物招领信息"
    />

    <div v-else class="list">
      <ItemCard
        v-for="item in filteredItems"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :tag="item.type === 'lost' ? '寻物' : '招领'"
        :location="item.location"
        :time="item.eventTime"
      >
        <template #footer>
          <span class="contact">联系：{{ item.contact }}</span>

          <button
            class="favorite-btn"
            :class="{ favorited: favoriteStore.isFavorite('lostFound', item.id) }"
            @click="favoriteStore.toggleFavorite({
              id: item.id,
              type: 'lostFound',
              title: item.title,
              description: item.description,
              location: item.location
            })"
          >
            {{ favoriteStore.isFavorite('lostFound', item.id) ? '★ 已收藏' : '☆ 收藏' }}
          </button>
        </template>
      </ItemCard>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref, computed } from 'vue'
import ItemCard from '../components/ItemCard.vue'
import EmptyState from '../components/EmptyState.vue'
import LoadingState from '../components/LoadingState.vue'
import ErrorState from '../components/ErrorState.vue'
import SearchBar from '../components/SearchBar.vue'
import { getLostFounds, type LostFoundItem } from '../api/lostFound'
import { useFavoriteStore } from '../stores/favorite'

const lostFounds = ref<LostFoundItem[]>([])
const loading = ref(true)
const error = ref('')
const keyword = ref('')
const favoriteStore = useFavoriteStore()

const filteredItems = computed(() => {
  if (!keyword.value) return lostFounds.value
  const kw = keyword.value.toLowerCase()
  return lostFounds.value.filter(item =>
    item.title.toLowerCase().includes(kw) ||
    item.itemName?.toLowerCase().includes(kw) ||
    item.description.toLowerCase().includes(kw)
  )
})

async function fetchLostFounds() {
  loading.value = true
  error.value = ''
  try {
    const res = await getLostFounds()
    lostFounds.value = res.data
  } catch {
    error.value = '失物招领信息加载失败，请确认 JSON Server 是否已启动'
  } finally {
    loading.value = false
  }
}

onMounted(fetchLostFounds)
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.page-header {
  padding: 24px;
  border-radius: 16px;
  background: #fff;
}

.page-header h1 {
  margin: 0 0 8px;
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

.favorite-btn {
  margin-left: 12px;
  border: none;
  border-radius: 999px;
  padding: 6px 14px;
  cursor: pointer;
  background: #f3f4f6;
  color: #374151;
  font-size: 13px;
  transition: all 0.2s;
}

.favorite-btn:hover {
  background: #dbeafe;
  color: #2563eb;
}

.favorite-btn.favorited {
  background: #dbeafe;
  color: #2563eb;
}

.contact {
  font-size: 13px;
  color: #2563eb;
  font-weight: 500;
}
</style>
