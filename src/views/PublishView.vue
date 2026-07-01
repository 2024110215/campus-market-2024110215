<template>
  <section class="page">
    <div class="page-header">
      <h1>发布信息</h1>
      <p>选择发布类型，填写必要信息，让校园需求更快被看见。</p>
    </div>

    <form class="publish-form" @submit.prevent="handleSubmit">
      <FormField label="发布类型" required>
        <select v-model="publishType">
          <option value="trade">二手交易</option>
          <option value="lostFound">失物招领</option>
          <option value="groupBuy">拼单搭子</option>
          <option value="errand">跑腿委托</option>
        </select>
      </FormField>

      <FormField label="标题" required :error="baseErrors.title">
        <input v-model.trim="baseForm.title" type="text" placeholder="请输入标题" />
      </FormField>

      <FormField label="地点" required :error="baseErrors.location">
        <input v-model.trim="baseForm.location" type="text" placeholder="请输入地点" />
      </FormField>

      <FormField label="描述" required :error="baseErrors.description">
        <textarea v-model.trim="baseForm.description" rows="5" placeholder="请简要描述具体情况"></textarea>
      </FormField>

      <template v-if="publishType === 'trade'">
        <FormField label="商品分类" required :error="tradeErrors.category">
          <input v-model.trim="tradeForm.category" type="text" placeholder="如：数码配件、教材资料、生活用品" />
        </FormField>

        <FormField label="价格" required :error="tradeErrors.price">
          <input v-model.number="tradeForm.price" type="number" min="0" placeholder="请输入价格" />
        </FormField>

        <FormField label="成色" required :error="tradeErrors.condition">
          <select v-model="tradeForm.condition">
            <option value="">请选择成色</option>
            <option value="全新">全新</option>
            <option value="九成新">九成新</option>
            <option value="八成新">八成新</option>
            <option value="正常使用痕迹">正常使用痕迹</option>
          </select>
        </FormField>
      </template>

      <template v-if="publishType === 'lostFound'">
        <FormField label="信息类型" required>
          <select v-model="lostFoundForm.type">
            <option value="lost">寻物</option>
            <option value="found">招领</option>
          </select>
        </FormField>

        <FormField label="物品名称" required :error="lostFoundErrors.itemName">
          <input v-model.trim="lostFoundForm.itemName" type="text" placeholder="请输入物品名称" />
        </FormField>

        <FormField label="发生时间" required :error="lostFoundErrors.eventTime">
          <input v-model="lostFoundForm.eventTime" type="datetime-local" />
        </FormField>

        <FormField label="联系方式" required :error="lostFoundErrors.contact">
          <input v-model.trim="lostFoundForm.contact" type="text" placeholder="如：微信、手机号" />
        </FormField>
      </template>

      <template v-if="publishType === 'groupBuy'">
        <FormField label="拼单类型" required :error="groupBuyErrors.type">
          <input v-model.trim="groupBuyForm.type" type="text" placeholder="如：拼餐、资料团购、运动搭子" />
        </FormField>

        <FormField label="目标人数" required :error="groupBuyErrors.targetCount">
          <input v-model.number="groupBuyForm.targetCount" type="number" min="2" placeholder="请输入目标人数" />
        </FormField>

        <FormField label="截止时间" required :error="groupBuyErrors.deadline">
          <input v-model="groupBuyForm.deadline" type="datetime-local" />
        </FormField>
      </template>

      <template v-if="publishType === 'errand'">
        <FormField label="任务类型" required :error="errandErrors.taskType">
          <input v-model.trim="errandForm.taskType" type="text" placeholder="如：取快递、代买、代送" />
        </FormField>

        <FormField label="酬劳" required :error="errandErrors.reward">
          <input v-model.number="errandForm.reward" type="number" min="0" placeholder="请输入酬劳" />
        </FormField>

        <FormField label="取件地点" required :error="errandErrors.from">
          <input v-model.trim="errandForm.from" type="text" placeholder="请输入取件地点" />
        </FormField>

        <FormField label="送达地点" required :error="errandErrors.to">
          <input v-model.trim="errandForm.to" type="text" placeholder="请输入送达地点" />
        </FormField>

        <FormField label="截止时间" required :error="errandErrors.deadline">
          <input v-model="errandForm.deadline" type="datetime-local" />
        </FormField>
      </template>

      <div class="actions">
        <button type="button" class="secondary" @click="resetForm">重置</button>
        <button type="submit" class="primary" :disabled="submitting">
          {{ submitting ? '提交中...' : '发布' }}
        </button>
      </div>
    </form>
  </section>
</template>

<script setup lang="ts">
import { ref, reactive, watch } from 'vue'
import { useRouter } from 'vue-router'
import FormField from '../components/FormField.vue'
import { createTrade, type TradeItem } from '../api/trade'
import { createLostFound, type LostFoundItem } from '../api/lostFound'
import { createGroupBuy, type GroupBuyItem } from '../api/groupBuy'
import { createErrand, type ErrandItem } from '../api/errand'
import { useUserStore } from '../stores/user'

const router = useRouter()
const userStore = useUserStore()

const publishType = ref<'trade' | 'lostFound' | 'groupBuy' | 'errand'>('trade')
const submitting = ref(false)

const baseForm = reactive({
  title: '',
  location: '',
  description: '',
})

const baseErrors = reactive({
  title: '',
  location: '',
  description: '',
})

const tradeForm = reactive({
  category: '',
  price: '' as number | '',
  condition: '',
})

const tradeErrors = reactive({
  category: '',
  price: '',
  condition: '',
})

const lostFoundForm = reactive({
  type: 'lost' as 'lost' | 'found',
  itemName: '',
  eventTime: '',
  contact: '',
})

const lostFoundErrors = reactive({
  itemName: '',
  eventTime: '',
  contact: '',
})

const groupBuyForm = reactive({
  type: '',
  targetCount: '' as number | '',
  deadline: '',
})

const groupBuyErrors = reactive({
  type: '',
  targetCount: '',
  deadline: '',
})

const errandForm = reactive({
  taskType: '',
  reward: '' as number | '',
  from: '',
  to: '',
  deadline: '',
})

const errandErrors = reactive({
  taskType: '',
  reward: '',
  from: '',
  to: '',
  deadline: '',
})

function clearErrors(obj: Record<string, string>) {
  Object.keys(obj).forEach((key) => { obj[key] = '' })
}

const typeFormMap: Record<typeof publishType.value, { form: Record<string, unknown>; errors: Record<string, string>; defaults: Record<string, unknown> }> = {
  trade: { form: tradeForm, errors: tradeErrors, defaults: { category: '', price: '', condition: '' } },
  lostFound: { form: lostFoundForm, errors: lostFoundErrors, defaults: { type: 'lost', itemName: '', eventTime: '', contact: '' } },
  groupBuy: { form: groupBuyForm, errors: groupBuyErrors, defaults: { type: '', targetCount: '', deadline: '' } },
  errand: { form: errandForm, errors: errandErrors, defaults: { taskType: '', reward: '', from: '', to: '', deadline: '' } },
}

watch(publishType, () => {
  clearErrors(baseErrors)
  Object.keys(typeFormMap).forEach((key) => {
    const { form, errors, defaults } = typeFormMap[key as keyof typeof typeFormMap]
    Object.assign(form, defaults)
    clearErrors(errors)
  })
})

function validateBase(): boolean {
  baseErrors.title = baseForm.title ? '' : '请输入标题'
  if (baseForm.title && baseForm.title.length > 30) baseErrors.title = '标题不能超过 30 个字符'
  baseErrors.location = baseForm.location ? '' : '请输入地点'
  baseErrors.description = baseForm.description ? '' : '请输入描述'
  if (baseForm.description && baseForm.description.length > 500) baseErrors.description = '描述不能超过 500 个字符'
  return !baseErrors.title && !baseErrors.location && !baseErrors.description
}

function validateTrade(): boolean {
  tradeErrors.category = tradeForm.category ? '' : '请输入商品分类'
  tradeErrors.price = tradeForm.price !== '' && tradeForm.price > 0 ? '' : '请输入有效价格'
  tradeErrors.condition = tradeForm.condition ? '' : '请选择成色'
  return !tradeErrors.category && !tradeErrors.price && !tradeErrors.condition
}

function validateLostFound(): boolean {
  lostFoundErrors.itemName = lostFoundForm.itemName ? '' : '请输入物品名称'
  lostFoundErrors.eventTime = lostFoundForm.eventTime ? '' : '请选择发生时间'
  lostFoundErrors.contact = lostFoundForm.contact ? '' : '请输入联系方式'
  return !lostFoundErrors.itemName && !lostFoundErrors.eventTime && !lostFoundErrors.contact
}

function validateGroupBuy(): boolean {
  groupBuyErrors.type = groupBuyForm.type ? '' : '请输入拼单类型'
  groupBuyErrors.targetCount = groupBuyForm.targetCount !== '' && groupBuyForm.targetCount >= 2 ? '' : '请输入有效目标人数（至少 2 人）'
  groupBuyErrors.deadline = groupBuyForm.deadline ? '' : '请选择截止时间'
  return !groupBuyErrors.type && !groupBuyErrors.targetCount && !groupBuyErrors.deadline
}

function validateErrand(): boolean {
  errandErrors.taskType = errandForm.taskType ? '' : '请输入任务类型'
  errandErrors.reward = errandForm.reward !== '' && errandForm.reward > 0 ? '' : '请输入有效酬劳'
  errandErrors.from = errandForm.from ? '' : '请输入取件地点'
  errandErrors.to = errandForm.to ? '' : '请输入送达地点'
  errandErrors.deadline = errandForm.deadline ? '' : '请选择截止时间'
  return !errandErrors.taskType && !errandErrors.reward && !errandErrors.from && !errandErrors.to && !errandErrors.deadline
}

watch(baseForm, () => {
  if (baseErrors.title && baseForm.title) baseErrors.title = ''
  if (baseErrors.title && baseForm.title && baseForm.title.length > 30) baseErrors.title = '标题不能超过 30 个字符'
  if (baseErrors.location && baseForm.location) baseErrors.location = ''
  if (baseErrors.description && baseForm.description) baseErrors.description = ''
  if (baseErrors.description && baseForm.description && baseForm.description.length > 500) baseErrors.description = '描述不能超过 500 个字符'
})

watch(tradeForm, () => {
  if (tradeErrors.category && tradeForm.category) tradeErrors.category = ''
  if (tradeErrors.price && tradeForm.price !== '' && tradeForm.price > 0) tradeErrors.price = ''
  if (tradeErrors.condition && tradeForm.condition) tradeErrors.condition = ''
})

watch(lostFoundForm, () => {
  if (lostFoundErrors.itemName && lostFoundForm.itemName) lostFoundErrors.itemName = ''
  if (lostFoundErrors.eventTime && lostFoundForm.eventTime) lostFoundErrors.eventTime = ''
  if (lostFoundErrors.contact && lostFoundForm.contact) lostFoundErrors.contact = ''
})

watch(groupBuyForm, () => {
  if (groupBuyErrors.type && groupBuyForm.type) groupBuyErrors.type = ''
  if (groupBuyErrors.targetCount && groupBuyForm.targetCount !== '' && groupBuyForm.targetCount >= 2) groupBuyErrors.targetCount = ''
  if (groupBuyErrors.deadline && groupBuyForm.deadline) groupBuyErrors.deadline = ''
})

watch(errandForm, () => {
  if (errandErrors.taskType && errandForm.taskType) errandErrors.taskType = ''
  if (errandErrors.reward && errandForm.reward !== '' && errandForm.reward > 0) errandErrors.reward = ''
  if (errandErrors.from && errandForm.from) errandErrors.from = ''
  if (errandErrors.to && errandForm.to) errandErrors.to = ''
  if (errandErrors.deadline && errandForm.deadline) errandErrors.deadline = ''
})

function validate(): boolean {
  const baseValid = validateBase()
  const typeValidators: Record<typeof publishType.value, () => boolean> = {
    trade: validateTrade,
    lostFound: validateLostFound,
    groupBuy: validateGroupBuy,
    errand: validateErrand,
  }
  const typeValid = typeValidators[publishType.value]()
  return baseValid && typeValid
}

function resetForm() {
  publishType.value = 'trade'
  baseForm.title = ''
  baseForm.location = ''
  baseForm.description = ''
  clearErrors(baseErrors)

  Object.keys(typeFormMap).forEach((key) => {
    const { form, errors, defaults } = typeFormMap[key as keyof typeof typeFormMap]
    Object.assign(form, defaults)
    clearErrors(errors)
  })
}

const typeRouteMap: Record<typeof publishType.value, string> = {
  trade: '/trade',
  lostFound: '/lost-found',
  groupBuy: '/group-buy',
  errand: '/errand',
}

function getSubmitData(): TradeItem | LostFoundItem | GroupBuyItem | ErrandItem {
  const now = new Date().toISOString().slice(0, 16).replace('T', ' ')
  const base = { title: baseForm.title, location: baseForm.location, description: baseForm.description, status: 'open', publisher: userStore.displayName, publishTime: now }

  if (publishType.value === 'trade') {
    return { ...base, category: tradeForm.category, price: tradeForm.price, condition: tradeForm.condition, image: '' } as TradeItem
  }
  if (publishType.value === 'lostFound') {
    return { ...base, type: lostFoundForm.type, itemName: lostFoundForm.itemName, eventTime: lostFoundForm.eventTime, contact: lostFoundForm.contact } as LostFoundItem
  }
  if (publishType.value === 'groupBuy') {
    return { ...base, type: groupBuyForm.type, targetCount: groupBuyForm.targetCount, currentCount: 0, deadline: groupBuyForm.deadline } as GroupBuyItem
  }
  return { ...base, taskType: errandForm.taskType, reward: errandForm.reward, from: errandForm.from, to: errandForm.to, deadline: errandForm.deadline } as ErrandItem
}

async function handleSubmit() {
  if (!validate()) return
  submitting.value = true
  try {
    const data = getSubmitData()
    const creators: Record<typeof publishType.value, (data: unknown) => Promise<unknown>> = {
      trade: (d) => createTrade(d as TradeItem),
      lostFound: (d) => createLostFound(d as LostFoundItem),
      groupBuy: (d) => createGroupBuy(d as GroupBuyItem),
      errand: (d) => createErrand(d as ErrandItem),
    }
    await creators[publishType.value](data)
    alert('发布成功！')
    resetForm()
    router.push(typeRouteMap[publishType.value])
  } catch {
    alert('发布失败，请检查 Mock 服务是否已启动。')
  } finally {
    submitting.value = false
  }
}
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

.page-header p {
  margin: 0;
  color: #6b7280;
}

.publish-form {
  display: grid;
  gap: 18px;
  padding: 24px;
  border-radius: 16px;
  background: #fff;
}

input,
select,
textarea {
  width: 100%;
  box-sizing: border-box;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  padding: 10px 12px;
  font-size: 14px;
}

textarea {
  resize: vertical;
}

.actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
}

button {
  border: none;
  border-radius: 8px;
  padding: 10px 18px;
  cursor: pointer;
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.7;
}

.primary {
  background: #2563eb;
  color: #fff;
}

.secondary {
  background: #f3f4f6;
  color: #374151;
}
</style>