<template>
  <div class="error-state">
    <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
      <circle cx="12" cy="12" r="10"/>
      <line x1="12" y1="8" x2="12" y2="12"/>
      <line x1="12" y1="16" x2="12.01" y2="16"/>
    </svg>
    <p>{{ text || '加载失败，请稍后重试' }}</p>
    <button v-if="showRetry" class="retry-btn" @click="$emit('retry')">重试</button>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  text?: string
  showRetry?: boolean
}>()

defineEmits<{
  retry: []
}>()
</script>

<style scoped>
.error-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 48px 24px;
  color: var(--danger, #ef4444);
  gap: 12px;
}

.error-state p {
  color: var(--gray-500, #6b7280);
  font-size: 14px;
}

.retry-btn {
  margin-top: 4px;
  border: none;
  border-radius: 8px;
  padding: 8px 20px;
  background: var(--primary, #2563eb);
  color: #fff;
  font-size: 14px;
  cursor: pointer;
  transition: opacity 0.2s;
}

.retry-btn:hover {
  opacity: 0.9;
}
</style>
