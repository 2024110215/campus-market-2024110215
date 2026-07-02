<template>
  <div class="empty-state">
    <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.2">
      <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
      <circle cx="12" cy="7" r="4"/>
    </svg>
    <p class="empty-text">{{ text }}</p>
    <p v-if="hint" class="empty-hint">{{ hint }}</p>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  text: string
  hint?: string
}>()
</script>

<style scoped>
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 48px 24px;
  text-align: center;
  background: #fff;
  border-radius: 12px;
  border: 1px dashed var(--gray-200, #e5e7eb);
  color: var(--gray-300, #d1d5db);
  gap: 12px;
}

.empty-text {
  color: var(--gray-400, #9ca3af);
  font-size: 14px;
  margin: 0;
}

.empty-hint {
  color: var(--gray-300, #d1d5db);
  font-size: 13px;
  margin: 0;
}
</style>
