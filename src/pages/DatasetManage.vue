<script setup>
import { computed, ref, watch } from 'vue'
import fieldNameIcon from '../assets/image/figma-dataset-icon-name.svg'
import fieldSizeIcon from '../assets/image/figma-dataset-icon-size.svg'
import fieldStateIcon from '../assets/image/figma-dataset-icon-state.svg'
import fieldTaskIcon from '../assets/image/figma-dataset-icon-task.svg'
import fieldFileIcon from '../assets/image/figma-dataset-icon-file.svg'
import refreshIcon from '../assets/image/figma-dataset-refresh.svg'
import searchIcon from '../assets/image/figma-dataset-search-icon.svg'
import tagBg from '../assets/image/figma-dataset-tag-bg.svg'
import titleMarker from '../assets/image/figma-dataset-title-marker.svg'

const props = defineProps({
  keyword: String,
  pageJump: String,
})

const emit = defineEmits(['update-keyword', 'update-page-jump', 'select-page'])

function createCards() {
  return Array.from({ length: 5 }, (_, index) => ({
    id: `dataset-card-${index}`,
    key: 'aefd8adc_0_118',
    tag: '块#118',
    summary: '(value=0.425)推理结果：维持“平台部署”类型不变。推理过程：[价值基线]价值基线处于中位区间；[价值稳定性]价值差异极...',
    taskId: 'aefd8adc',
    datasetName: '11dvsv',
    sourceFile: '5151scdsd562sdvcnsjmkj222251514cds_txt.txt',
    dataSize: '111字符',
    state: 'ACTIVE',
  }))
}

const searchDraft = ref(props.keyword || '')
const confirmedKeyword = ref(props.keyword || '')
const jumpDraft = ref(props.pageJump || '')
const activeMode = ref('unsupervised')
const activePageNumber = ref('1')
const selectedIds = ref([])
const editingId = ref('')
const cards = ref(createCards())

const pageNumbers = ['1', '2', '3', '4', '5', '6']
const maxPage = 20
const modes = [
  { key: 'unsupervised', label: '无监督数据集管理' },
  { key: 'supervised', label: '监督数据集管理' },
]
const cardActions = ['编辑', '详情', '删除']

const visibleCards = computed(() => {
  const keyword = confirmedKeyword.value.trim().toLowerCase()
  if (!keyword) {
    return cards.value
  }

  return cards.value.filter((card) =>
    [card.key, card.tag, card.summary, card.taskId, card.datasetName, card.sourceFile, card.dataSize, card.state]
      .some((value) => value.toLowerCase().includes(keyword)),
  )
})

const allSelected = computed(() => visibleCards.value.length > 0 && visibleCards.value.every((card) => selectedIds.value.includes(card.id)))
const totalText = computed(() => (cards.value.length === 5 ? '共 100 项数据' : `共 ${cards.value.length} 项数据`))
const activePageValue = computed(() => Number(activePageNumber.value) || 1)

watch(
  () => props.keyword,
  (value) => {
    searchDraft.value = value || ''
    confirmedKeyword.value = value || ''
  },
)

watch(
  () => props.pageJump,
  (value) => {
    jumpDraft.value = value || ''
  },
)

function confirmSearch() {
  confirmedKeyword.value = searchDraft.value
  activePageNumber.value = '1'
  emit('update-keyword', searchDraft.value)
}

function resetListState() {
  searchDraft.value = ''
  confirmedKeyword.value = ''
  jumpDraft.value = ''
  activePageNumber.value = '1'
  selectedIds.value = []
  editingId.value = ''
  emit('update-keyword', '')
  emit('update-page-jump', '')
}

function refreshList() {
  resetListState()
  cards.value = createCards()
}

function openPretrainGenerator() {
  resetListState()
  emit('select-page', 3)
}

function selectMode(mode) {
  activeMode.value = mode
  selectedIds.value = []
  editingId.value = ''
}

function cardMetrics(card) {
  return [
    { key: 'taskId', label: '任务ID:', value: card.taskId, icon: fieldTaskIcon },
    { key: 'datasetName', label: '数据集名称:', value: card.datasetName, icon: fieldNameIcon, editable: true },
    { key: 'sourceFile', label: '源文件:', value: card.sourceFile, icon: fieldFileIcon, wide: true },
    { key: 'dataSize', label: '数据大小:', value: card.dataSize, icon: fieldSizeIcon },
    { key: 'state', label: '状态:', value: card.state, icon: fieldStateIcon },
  ]
}

function toggleSelection(cardId) {
  selectedIds.value = selectedIds.value.includes(cardId)
    ? selectedIds.value.filter((id) => id !== cardId)
    : [...selectedIds.value, cardId]
}

function toggleAll() {
  selectedIds.value = allSelected.value ? [] : visibleCards.value.map((card) => card.id)
}

function deleteCard(cardId) {
  cards.value = cards.value.filter((card) => card.id !== cardId)
  selectedIds.value = selectedIds.value.filter((id) => id !== cardId)
  if (editingId.value === cardId) {
    editingId.value = ''
  }
}

function deleteSelected() {
  if (!selectedIds.value.length) {
    return
  }
  cards.value = cards.value.filter((card) => !selectedIds.value.includes(card.id))
  selectedIds.value = []
  editingId.value = ''
}

function handleCardAction(action, card) {
  if (action === '删除') {
    deleteCard(card.id)
    return
  }
  if (action === '详情') {
    emit('select-page', 4)
    return
  }
  editingId.value = editingId.value === card.id ? '' : card.id
}

function selectPageNumber(number) {
  activePageNumber.value = number
  jumpDraft.value = ''
  emit('update-page-jump', '')
}

function previousPage() {
  if (activePageValue.value > 1) {
    selectPageNumber(String(activePageValue.value - 1))
  }
}

function nextPage() {
  if (activePageValue.value < maxPage) {
    selectPageNumber(String(activePageValue.value + 1))
  }
}

function commitPageJump() {
  const normalized = jumpDraft.value.trim().replace(/[^\d]/g, '')
  if (!normalized) {
    jumpDraft.value = ''
    emit('update-page-jump', '')
    return
  }

  const page = Math.min(maxPage, Math.max(1, Number(normalized)))
  jumpDraft.value = String(page)
  activePageNumber.value = jumpDraft.value
  emit('update-page-jump', jumpDraft.value)
}
</script>

<template>
  <div class="dataset-manage-page">
    <div class="dataset-heading">
      <img :src="titleMarker" alt="" />
      <span>数据集管理</span>
    </div>

    <div class="dataset-filter-row">
      <button class="dataset-primary-button" type="button" @click="resetListState">微调数据集生成</button>
      <button class="dataset-plain-button" type="button" @click="openPretrainGenerator">预训练数据集生成</button>

      <label class="dataset-search">
        <input
          v-model="searchDraft"
          type="search"
          placeholder="搜索数据集ID、任务ID、数据集名或文件名"
          @keyup.enter="confirmSearch"
        />
        <button type="button" aria-label="搜索" @click="confirmSearch">
          <img :src="searchIcon" alt="" />
        </button>
      </label>

      <button class="dataset-icon-button" type="button" aria-label="刷新" @click="refreshList">
        <img :src="refreshIcon" alt="" />
      </button>

      <div class="dataset-mode-tabs" role="tablist" aria-label="数据集类型">
        <button
          v-for="mode in modes"
          :key="mode.key"
          :class="{ active: activeMode === mode.key }"
          type="button"
          role="tab"
          :aria-selected="activeMode === mode.key"
          @click="selectMode(mode.key)"
        >
          {{ mode.label }}
        </button>
      </div>
    </div>

    <div class="dataset-divider"></div>

    <div class="dataset-bulk-row">
      <button class="dataset-check-button" :class="{ active: allSelected }" type="button" @click="toggleAll">
        <span class="dataset-checkbox" :class="{ checked: allSelected }"></span>
        <span>全选</span>
      </button>
      <button class="dataset-delete-selected" type="button" @click="deleteSelected">删除选中</button>
    </div>

    <section class="dataset-card-grid" aria-label="数据集列表">
      <article
        v-for="card in visibleCards"
        :key="card.id"
        class="dataset-card"
        :class="{ selected: selectedIds.includes(card.id), editing: editingId === card.id }"
      >
        <div class="dataset-card-summary">
          <div class="dataset-card-key">
            <label class="dataset-card-check" @click.stop>
              <input type="checkbox" :checked="selectedIds.includes(card.id)" @change="toggleSelection(card.id)" />
              <span class="dataset-checkbox" :class="{ checked: selectedIds.includes(card.id) }"></span>
            </label>
            <strong>{{ card.key }}</strong>
          </div>
          <span class="dataset-card-tag">
            <img :src="tagBg" alt="" />
            <span>{{ card.tag }}</span>
          </span>
          <p>{{ card.summary }}</p>
        </div>

        <div class="dataset-card-metrics">
          <div v-for="metric in cardMetrics(card)" :key="metric.key" class="dataset-metric" :class="{ wide: metric.wide }">
            <span class="dataset-metric-label">
              <img :src="metric.icon" alt="" />
              {{ metric.label }}
            </span>
            <input
              v-if="metric.editable && editingId === card.id"
              v-model="card.datasetName"
              class="dataset-inline-input"
              type="text"
              @click.stop
            />
            <strong v-else>{{ metric.value }}</strong>
          </div>
        </div>

        <div class="dataset-card-actions">
          <button v-for="action in cardActions" :key="action" type="button" @click="handleCardAction(action, card)">{{ action }}</button>
        </div>
      </article>
    </section>

    <nav class="dataset-pagination" aria-label="分页">
      <span>{{ totalText }}</span>
      <button class="dataset-page-size" type="button" @click="selectPageNumber('1')">10 条/页</button>
      <button class="dataset-page-arrow" type="button" aria-label="上一页" :disabled="activePageValue === 1" @click="previousPage"></button>
      <button
        v-for="number in pageNumbers"
        :key="number"
        class="dataset-page-number"
        :class="{ active: number === activePageNumber }"
        type="button"
        @click="selectPageNumber(number)"
      >
        {{ number }}
      </button>
      <button class="dataset-page-arrow next" type="button" aria-label="下一页" :disabled="activePageValue === maxPage" @click="nextPage"></button>
      <label class="dataset-page-jump">
        <span>跳至</span>
        <input
          v-model="jumpDraft"
          type="text"
          placeholder="请输入"
          inputmode="numeric"
          @blur="commitPageJump"
          @keyup.enter="commitPageJump"
        />
        <span>/20页</span>
      </label>
    </nav>
  </div>
</template>
