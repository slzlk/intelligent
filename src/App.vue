<script setup>
import { computed, ref } from 'vue'
import bottomOrb from './assets/image/bottom-orb.png'
import filePreview from './assets/image/file-preview.png'
import navDataset from './assets/image/nav-dataset.png'
import navModeling from './assets/image/nav-modeling.png'
import navScenario from './assets/image/nav-scenario.png'
import navSituation from './assets/image/nav-situation.png'
import navTraining from './assets/image/nav-training.png'
import selectArrow from './assets/image/select-arrow.png'
import DatasetConfig from './pages/DatasetConfig.vue'
import DatasetEmpty from './pages/DatasetEmpty.vue'
import DatasetManage from './pages/DatasetManage.vue'
import DatasetPreview from './pages/DatasetPreview.vue'
import DatasetTask from './pages/DatasetTask.vue'
import FineTuneBase from './pages/FineTuneBase.vue'
import FineTuneExpand from './pages/FineTuneExpand.vue'

const pages = [
  { id: 'fineTuneBase', code: '4.1', title: '大模型训练微调', type: 'fineTuneBase' },
  { id: 'fineTuneExpand', code: '4.2', title: '大模型训练微调', type: 'fineTuneExpand' },
  { id: 'datasetManage', code: '5.1', title: '训练数据集生成_数据集管理', type: 'datasetManage' },
  { id: 'datasetEmpty', code: '5.2', title: '训练数据集生成_预训练数据集生成_空状态', type: 'datasetEmpty' },
  { id: 'datasetPreview', code: '5.3', title: '训练数据集生成_预训练数据集生成_文件预览', type: 'datasetPreview' },
  { id: 'datasetConfig', code: '5.4', title: '训练数据集生成_预训练数据集生成_配置', type: 'datasetConfig' },
  { id: 'datasetTask', code: '5.5', title: '训练数据集生成_预训练数据生成服务', type: 'datasetTask' },
]

const activeIndex = ref(0)
const activePage = computed(() => pages[activeIndex.value])
const activeTitle = computed(() => `${activePage.value.code} ${activePage.value.title}`)
const pageComponents = {
  fineTuneBase: FineTuneBase,
  fineTuneExpand: FineTuneExpand,
  datasetManage: DatasetManage,
  datasetEmpty: DatasetEmpty,
  datasetPreview: DatasetPreview,
  datasetConfig: DatasetConfig,
  datasetTask: DatasetTask,
}
const activeComponent = computed(() => pageComponents[activePage.value.type])

const topTabs = ['《防务快报》', '数据配置', '训练参数', '服务发布']
const footerItems = [
  { label: '目标建模', icon: navModeling },
  { label: '想定编辑', icon: navScenario },
  { label: '模型训练', icon: navTraining },
  { label: '数据生成', icon: navDataset },
  { label: '二维态势', icon: navSituation },
]

const openDropdown = ref('')
const activeTab = ref(0)
const openPanels = ref(['训练数据集配置'])
const selectedDataset = ref('作战构想训练集')
const keyword = ref('')
const showTaskModal = ref(true)
const enabledRules = ref(['去重过滤', '敏感词清洗'])

const baseFields = ref([
  { key: 'modelName', label: '模型名称', value: 'XXX大模型', options: ['XXX大模型', '防务推演大模型', '目标识别大模型'] },
  { key: 'trainMode', label: '训练方式', value: '微调训练', options: ['微调训练', '全量训练', '增量训练'] },
  { key: 'dataset', label: '训练数据集', value: '请选择内容', options: ['作战构想训练集', '防务快报语料', '态势推演样本'] },
  { key: 'baseModel', label: '基础模型', value: '请选择内容', options: ['BMZY-7B', 'BMZY-13B', 'BMZY-32B'] },
  { key: 'taskType', label: '任务类型', value: '请选择内容', options: ['指令微调', '领域适配', '能力增强'] },
  { key: 'runtime', label: '执行环境', value: '请选择内容', options: ['GPU集群A', 'GPU集群B', '本地调试环境'] },
  { key: 'sampleRate', label: '样本比例', value: '60', options: ['40', '60', '80', '100'] },
  { key: 'epochs', label: '训练轮次', value: '8', options: ['3', '5', '8', '10'] },
  { key: 'learningRate', label: '学习率', value: '0.0003', options: ['0.0001', '0.0003', '0.0005'] },
  { key: 'savePolicy', label: '保存策略', value: '请选择内容', options: ['每轮保存', '最优保存', '最终保存'] },
])

const configFields = ref([
  { key: 'source', label: '数据来源', value: '请选择内容', options: ['本地上传', '数据集管理', '外部服务'] },
  { key: 'dataType', label: '数据类型', value: '请选择内容', options: ['文本', '结构化', '多模态'] },
  { key: 'cleanRule', label: '清洗规则', value: '请选择内容', options: ['基础清洗', '严格清洗', '自定义规则'] },
  { key: 'chunkSize', label: '切分粒度', value: '请选择内容', options: ['短文本', '中等片段', '长上下文'] },
  { key: 'limit', label: '样本上限', value: '请输入', options: ['1000', '5000', '10000'] },
  { key: 'dedupe', label: '重复过滤', value: '请选择内容', options: ['开启', '关闭'] },
  { key: 'quality', label: '质量阈值', value: '请输入', options: ['0.6', '0.75', '0.9'] },
  { key: 'strategy', label: '生成策略', value: '请选择内容', options: ['快速生成', '均衡生成', '高质量生成'] },
])

const datasetColumns = ['数据集名称', '数据类型', '样本数量', '更新时间', '状态', '操作']
const datasetRows = [
  ['作战构想训练集', '预训练', '12,480', '2026-08-28', '已生成', '预览'],
  ['防务快报语料', '文本', '8,120', '2026-08-27', '处理中', '查看'],
  ['态势推演样本', '结构化', '3,640', '2026-08-26', '待配置', '配置'],
  ['目标识别数据', '多模态', '6,230', '2026-08-25', '已生成', '预览'],
]

const filteredDatasetRows = computed(() => {
  const text = keyword.value.trim()
  if (!text) return datasetRows
  return datasetRows.filter((row) => row.join('').includes(text))
})

const previewLines = [
  '任务背景：根据当前作战构想，对多源情报、态势变化、目标特征进行语料抽取。',
  '数据目标：形成可用于预训练和指令微调的结构化文本片段。',
  '生成规则：保留时间、地点、实体、行动关系，过滤重复和低质量样本。',
  '输出格式：JSONL / CSV，可进入训练任务配置流程。',
]

const taskSteps = ['选择数据', '规则配置', '生成预览', '创建任务']

const activePageProps = computed(() => {
  if (activePage.value.type === 'fineTuneBase' || activePage.value.type === 'fineTuneExpand') {
    return {
      fields: baseFields.value,
      openDropdown: openDropdown.value,
      activeTab: activeTab.value,
      openPanels: openPanels.value,
      selectArrow,
      topTabs,
      dropdownKey,
    }
  }

  if (activePage.value.type === 'datasetManage') {
    return {
      columns: datasetColumns,
      rows: filteredDatasetRows.value,
      selectedDataset: selectedDataset.value,
      keyword: keyword.value,
    }
  }

  if (activePage.value.type === 'datasetEmpty') {
    return {
      fields: configFields.value,
      openDropdown: openDropdown.value,
      selectArrow,
      filePreview,
      dropdownKey,
    }
  }

  if (activePage.value.type === 'datasetPreview') {
    return {
      fields: configFields.value,
      openDropdown: openDropdown.value,
      selectArrow,
      filePreview,
      previewLines,
      dropdownKey,
    }
  }

  if (activePage.value.type === 'datasetConfig') {
    return {
      fields: configFields.value,
      openDropdown: openDropdown.value,
      selectArrow,
      enabledRules: enabledRules.value,
      dropdownKey,
    }
  }

  return {
    showTaskModal: showTaskModal.value,
    taskSteps,
  }
})

function go(delta) {
  activeIndex.value = (activeIndex.value + delta + pages.length) % pages.length
}

function selectPage(index) {
  activeIndex.value = index
  openDropdown.value = ''
  if (pages[index].type === 'datasetTask') {
    showTaskModal.value = true
  }
}

function dropdownKey(group, field) {
  return `${group}-${field.key}`
}

function toggleDropdown(group, field) {
  const key = dropdownKey(group, field)
  openDropdown.value = openDropdown.value === key ? '' : key
}

function chooseOption(field, option) {
  field.value = option
  openDropdown.value = ''
}

function closeDropdown() {
  openDropdown.value = ''
}

function selectTab(index) {
  activeTab.value = index
}

function togglePanel(panel) {
  if (openPanels.value.includes(panel)) {
    openPanels.value = openPanels.value.filter((item) => item !== panel)
    return
  }
  openPanels.value = [...openPanels.value, panel]
}

function chooseDataset(row) {
  selectedDataset.value = row[0]
}

function toggleRule(rule) {
  if (enabledRules.value.includes(rule)) {
    enabledRules.value = enabledRules.value.filter((item) => item !== rule)
    return
  }
  enabledRules.value = [...enabledRules.value, rule]
}

function createDatasetTask() {
  selectPage(6)
}

function updateKeyword(value) {
  keyword.value = value
}

function closeTaskModal() {
  showTaskModal.value = false
}
</script>

<template>
  <main class="app-shell" @click="closeDropdown">
    <section class="stage">
      <header class="global-bar">
        <button class="brand-mark" type="button" aria-label="menu">⌘</button>
        <nav class="breadcrumbs">
          <span>上级页面</span>
          <span>上级页面</span>
          <strong>当前页面</strong>
        </nav>
        <div class="plan-name">当前作战构想名称：<strong>XXXXXX名称</strong></div>
      </header>

      <div class="workspace">
        <div class="sub-title">
          <div class="title-icon"></div>
          <h1>XXX大模型</h1>
          <span class="status-pill">请输入</span>
        </div>

        <div class="page-switcher">
          <button
            v-for="(page, index) in pages"
            :key="page.id"
            type="button"
            :class="{ active: activeIndex === index }"
            @click="selectPage(index)"
          >
            {{ page.code }}
          </button>
        </div>

        <section class="content-card">
          <div class="panel-heading">
            <span></span>
            <h2>{{ activeTitle }}</h2>
          </div>

          <component
            :is="activeComponent"
            v-bind="activePageProps"
            @toggle-dropdown="toggleDropdown"
            @choose-option="chooseOption"
            @select-tab="selectTab"
            @toggle-panel="togglePanel"
            @select-page="selectPage"
            @choose-dataset="chooseDataset"
            @update-keyword="updateKeyword"
            @toggle-rule="toggleRule"
            @create-task="createDatasetTask"
            @close-modal="closeTaskModal"
          />
        </section>

        <div class="action-bar">
          <div class="progress-block">
            <span>侦察威逼阶段</span>
            <strong>XXX大模型</strong>
            <div><i></i></div>
          </div>
          <button type="button" @click="go(-1)">上一步</button>
          <button type="button" @click="go(1)">下一步</button>
          <button type="button" class="primary" @click="createDatasetTask">确定</button>
        </div>
      </div>

      <footer class="dock">
        <img class="orb" :src="bottomOrb" alt="" />
        <nav>
          <button v-for="(item, index) in footerItems" :key="item.label" type="button" @click="selectPage(index < 3 ? index : 2)">
            <img :src="item.icon" alt="" />
            <span>{{ item.label }}</span>
          </button>
        </nav>
      </footer>

      <button class="page-nav prev" type="button" @click="go(-1)">‹</button>
      <button class="page-nav next" type="button" @click="go(1)">›</button>
    </section>
  </main>
</template>
