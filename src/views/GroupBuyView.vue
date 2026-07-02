<template>
  <section class="page">
    <div class="page-header">
      <h1>拼单搭子</h1>
      <p>拼单省钱、搭子组队，一起更精彩。</p>
    </div>

    <SearchBar v-model="keyword" placeholder="搜索拼单类型..." />

    <LoadingState v-if="loading" text="正在加载拼单信息..." />

    <ErrorState
      v-else-if="error"
      :text="error"
      :show-retry="true"
      @retry="fetchGroupBuys"
    />

    <EmptyState
      v-else-if="filteredItems.length === 0"
      text="暂无拼单搭子信息"
    />

    <div v-else class="list">
      <ItemCard
        v-for="item in filteredItems"
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

          <button
            class="favorite-btn"
            :class="{ favorited: favoriteStore.isFavorite('groupBuy', item.id) }"
            @click="favoriteStore.toggleFavorite({
              id: item.id,
              type: 'groupBuy',
              title: item.title,
              description: item.description,
              location: item.location
            })"
          >
            {{ favoriteStore.isFavorite('groupBuy', item.id) ? '★ 已收藏' : '☆ 收藏' }}
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
import { getGroupBuys, type GroupBuyItem } from '../api/groupBuy'
import { useFavoriteStore } from '../stores/favorite'

const groupBuys = ref<GroupBuyItem[]>([])
const loading = ref(true)
const error = ref('')
const keyword = ref('')
const favoriteStore = useFavoriteStore()

const filteredItems = computed(() => {
  if (!keyword.value) return groupBuys.value
  const kw = keyword.value.toLowerCase()
  return groupBuys.value.filter(item =>
    item.title.toLowerCase().includes(kw) ||
    item.type.toLowerCase().includes(kw) ||
    item.description.toLowerCase().includes(kw)
  )
})

async function fetchGroupBuys() {
  loading.value = true
  error.value = ''
  try {
    const res = await getGroupBuys()
    groupBuys.value = res.data
  } catch {
    error.value = '拼单信息加载失败，请确认 JSON Server 是否已启动'
  } finally {
    loading.value = false
  }
}

onMounted(fetchGroupBuys)
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
