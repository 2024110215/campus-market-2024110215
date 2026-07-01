<template>
  <section class="page">
    <div class="profile-card">
      <div class="avatar">
        {{ userStore.displayName.slice(0, 1) }}
      </div>

      <div>
        <h1>{{ userStore.displayName }}</h1>
        <p>{{ userStore.userDescription }}</p>
        <p>{{ userStore.currentUser.bio }}</p>
      </div>
    </div>

    <div class="panel">
      <h2>我的收藏</h2>

      <EmptyState
        v-if="favoriteStore.favorites.length === 0"
        text="暂无收藏内容"
      />

      <div v-else class="favorite-list">
        <ItemCard
          v-for="item in favoriteStore.favorites"
          :key="`${item.type}-${item.id}`"
          :title="item.title"
          :description="item.description"
          :tag="getTypeLabel(item.type)"
          :location="item.location"
        >
          <template #footer>
            <button class="remove-btn" @click="favoriteStore.removeFavorite(item.type, item.id)">
              取消收藏
            </button>
          </template>
        </ItemCard>
      </div>
    </div>

    <div class="panel">
      <h2>我的发布</h2>

      <EmptyState
        v-if="myPosts.length === 0"
        text="暂无发布内容"
      />

      <div v-else class="post-list">
        <ItemCard
          v-for="post in myPosts"
          :key="`${post.type}-${post.id}`"
          :title="post.title"
          :description="post.description"
          :tag="getTypeLabel(post.type)"
          :location="post.location"
          :time="post.time"
        />
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import EmptyState from '../components/EmptyState.vue'
import ItemCard from '../components/ItemCard.vue'
import { useFavoriteStore } from '../stores/favorite'
import { useUserStore } from '../stores/user'
import { getTrades, type TradeItem } from '../api/trade'
import { getLostFounds, type LostFoundItem } from '../api/lostFound'
import { getGroupBuys, type GroupBuyItem } from '../api/groupBuy'
import { getErrands, type ErrandItem } from '../api/errand'

const userStore = useUserStore()
const favoriteStore = useFavoriteStore()

interface MyPost {
  id: number
  type: 'trade' | 'lostFound' | 'groupBuy' | 'errand'
  title: string
  description: string
  location: string
  time: string
}

const myPosts = ref<MyPost[]>([])

onMounted(async () => {
  const userName = userStore.displayName

  const [tradesRes, lostFoundsRes, groupBuysRes, errandsRes] = await Promise.all([
    getTrades(),
    getLostFounds(),
    getGroupBuys(),
    getErrands(),
  ])

  const trades = (tradesRes.data as TradeItem[])
    .filter((item) => item.publisher === userName)
    .map((item) => ({
      id: item.id!,
      type: 'trade' as const,
      title: item.title,
      description: item.description,
      location: item.location,
      time: item.publishTime,
    }))

  const lostFounds = (lostFoundsRes.data as LostFoundItem[])
    .filter((item) => item.publisher === userName)
    .map((item) => ({
      id: item.id!,
      type: 'lostFound' as const,
      title: item.title,
      description: item.description,
      location: item.location,
      time: item.eventTime,
    }))

  const groupBuys = (groupBuysRes.data as GroupBuyItem[])
    .filter((item) => item.publisher === userName)
    .map((item) => ({
      id: item.id!,
      type: 'groupBuy' as const,
      title: item.title,
      description: item.description,
      location: item.location,
      time: item.deadline,
    }))

  const errands = (errandsRes.data as ErrandItem[])
    .filter((item) => item.publisher === userName)
    .map((item) => ({
      id: item.id!,
      type: 'errand' as const,
      title: item.title,
      description: item.description,
      location: item.from,
      time: item.deadline,
    }))

  myPosts.value = [...trades, ...lostFounds, ...groupBuys, ...errands]
})

function getTypeLabel(type: string) {
  const map: Record<string, string> = {
    trade: '二手交易',
    lostFound: '失物招领',
    groupBuy: '拼单搭子',
    errand: '跑腿委托',
  }

  return map[type] || '校园信息'
}
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.profile-card,
.panel {
  padding: 24px;
  border-radius: 16px;
  background: #fff;
}

.profile-card {
  display: flex;
  align-items: center;
  gap: 20px;
}

.avatar {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: #eff6ff;
  color: #2563eb;
  font-size: 28px;
  font-weight: 700;
}

.profile-card h1,
.panel h2 {
  margin: 0 0 8px;
}

.profile-card p,
.hint {
  margin: 0;
  color: #6b7280;
  line-height: 1.6;
}

.favorite-list,
.post-list {
  display: grid;
  gap: 16px;
}

.remove-btn {
  border: none;
  border-radius: 999px;
  padding: 6px 12px;
  cursor: pointer;
  background: #f3f4f6;
  color: #374151;
}
</style>