<script setup>
import { computed, ref } from 'vue'
import bottomOrb from './assets/image/bottom-orb.png'
import figmaPlatformIcon from './assets/image/figma-platform-icon.svg'
import figmaStartIcon from './assets/image/figma-start-icon.svg'
import figmaStatusBg from './assets/image/figma-status-bg.svg'
import figmaStatusCheck from './assets/image/figma-status-check.svg'
import figmaStopIcon from './assets/image/figma-stop-icon.svg'
import figmaTitleAction from './assets/image/figma-title-action.svg'
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

const requestedPage = new URLSearchParams(window.location.search).get('page')
const requestedIndex = pages.findIndex((page) => page.id === requestedPage || page.type === requestedPage)
const activeIndex = ref(requestedIndex >= 0 ? requestedIndex : 0)
const activePage = computed(() => pages[activeIndex.value])
const breadcrumbItems = computed(() => [
  { label: '上级页面', targetIndex: activeIndex.value - 2 },
  { label: '上级页面', targetIndex: activeIndex.value - 1 },
])
const isFineTunePage = computed(() => activePage.value.type === 'fineTuneBase' || activePage.value.type === 'fineTuneExpand')
const isTaskPage = computed(() => activePage.value.type === 'datasetTask')
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

const topTabs = ['训练', '评估与预测', '聊天', '导出']
const footerItems = [
  { label: '人机交互输入', icon: navModeling },
  { label: '知识库管理', icon: navScenario },
  { label: '协同工作流生成', icon: navTraining },
  { label: '大模型训练微调', icon: navDataset },
  { label: '训练数据集生成', icon: navSituation },
]
const actionButtons = [
  { label: '预览命令' },
  { label: '保存训练参数' },
  { label: '载入训练参数' },
  { label: '中断', icon: figmaStopIcon, type: 'muted' },
  { label: '开始', icon: figmaStartIcon, type: 'primary' },
]

const openDropdown = ref('')
const activeTab = ref(0)
const openPanels = ref(['训练数据集配置'])
const keyword = ref('')
const pageJump = ref('')
const uploadedFileName = ref('')

const baseFields = ref([
  { key: 'language', label: '语言', value: 'zh', options: ['zh', 'en'] },
  { key: 'finetuneMethod', label: '微调方法', value: 'lora', options: ['lora', 'full', 'freeze'] },
  { key: 'modelName', label: '模型名称', value: 'Deepseek-LLM-7B-Base', options: ['Deepseek-LLM-7B-Base'] },
  { key: 'modelPath', label: '模型路径', value: '请选择', options: ['请选择'] },
  { key: 'checkpointPath', label: '检查点路径', value: '请选择', options: ['请选择'] },
  { key: 'quantLevel', label: '量化等级', value: 'none', options: ['none', '8bit', '4bit'] },
  { key: 'quantMethod', label: '量化方法', value: 'bnb', options: ['bnb', 'gptq', 'awq'] },
  { key: 'chatTemplate', label: '对话模板', value: 'default', options: ['default', 'chatml', 'alpaca'] },
  { key: 'ropeScale', label: 'RoPE插值方法', value: 'none', options: ['none', 'linear', 'dynamic'] },
  { key: 'boostMethod', label: '加速方式', value: 'auto', options: ['auto', 'flash-attn', 'unsloth'] },
])

const trainFields = ref([
  { key: 'stage', label: '训练阶段', value: 'Supervised Fine-Tuning', hint: '目前采用的训练方式', options: ['Supervised Fine-Tuning'] },
  { key: 'dataPath', label: '数据路径', value: '请选择', hint: '数据文件夹的路径', options: ['请选择'] },
  { key: 'learningRate', label: '学习率', value: 'none', hint: 'AdamW优化器的初始学习率', options: ['none'] },
  { key: 'trainEpoch', label: '训练轮数', value: 'none', hint: '需要执行的训练总轮数', options: ['none'] },
  { key: 'batchSize', label: '最大梯度范数', value: 'none', hint: '用于梯度裁剪的范数', options: ['none'] },
  { key: 'sampleSize', label: '最大样本数', value: 'none', hint: '每个数据集的最大样本数', options: ['none'] },
  { key: 'computeType', label: '计算类型', value: 'bf16', hint: '是否使用混合精度训练', options: ['bf16', 'fp16', 'fp32'] },
  { key: 'lrScheduler', label: '学习率调节器', value: 'cosine', hint: '学习率调度器的名称', options: ['cosine'] },
])

const trainSliders = ref([
  { key: 'truncateLength', label: '截断长度', hint: '输入序列分词后的最大长度', value: 5, min: 4, max: 131072 },
  { key: 'batchProcessSize', label: '批处理大小', hint: '每个GPU处理的样本数量', value: 5, min: 4, max: 1260 },
  { key: 'gradientAccumulation', label: '梯度累积', hint: '梯度累积的步数', value: 5, min: 4, max: 1260 },
  { key: 'validationRatio', label: '验证集比例', hint: '验证集占全部样本的百分比', value: 5, min: 4, max: 1260 },
])

const activePageProps = computed(() => {
  if (activePage.value.type === 'fineTuneBase' || activePage.value.type === 'fineTuneExpand') {
    return {
      fields: baseFields.value,
      openDropdown: openDropdown.value,
      activeTab: activeTab.value,
      openPanels: openPanels.value,
      trainFields: trainFields.value,
      trainSliders: trainSliders.value,
      selectArrow,
      topTabs,
      dropdownKey,
    }
  }

  if (activePage.value.type === 'datasetManage') {
    return {
      keyword: keyword.value,
      pageJump: pageJump.value,
    }
  }

  if (activePage.value.type === 'datasetEmpty') {
    return {}
  }

  if (activePage.value.type === 'datasetPreview') {
    return {
      fileName: uploadedFileName.value,
    }
  }

  if (activePage.value.type === 'datasetConfig') {
    return {}
  }

  return {}
})

function selectPage(index) {
  if (index < 0 || index >= pages.length) {
    return
  }
  activeIndex.value = index
  openDropdown.value = ''
  window.history.replaceState({}, '', `?page=${pages[index].id}`)
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

function updateField(field, value) {
  field.value = value
}

function updateSlider(slider, value) {
  slider.value = Number(value)
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

function createDatasetTask() {
  selectPage(6)
}

function updateKeyword(value) {
  keyword.value = value
}

function updatePageJump(value) {
  pageJump.value = value
}

function selectUploadedFile(fileName) {
  uploadedFileName.value = fileName
  selectPage(4)
}

</script>

<template>
  <main class="app-shell" @click="closeDropdown">
    <component v-if="isTaskPage" :is="activeComponent" @select-page="selectPage" />

    <section v-else class="stage">
      <header class="global-bar">
        <button class="brand-mark" type="button" aria-label="menu">⌘</button>
        <nav class="breadcrumbs">
          <button
            v-for="item in breadcrumbItems"
            :key="`${item.label}-${item.targetIndex}`"
            type="button"
            :disabled="item.targetIndex < 0"
            @click="selectPage(item.targetIndex)"
          >
            {{ item.label }}
          </button>
          <strong>当前页面</strong>
        </nav>
        <div class="plan-name">当前用户：<strong>XXX</strong></div>
      </header>

      <div class="workspace" :class="{ 'dataset-workspace': !isFineTunePage }">
        <div v-if="isFineTunePage" class="sub-title">
          <img class="title-icon" :src="figmaPlatformIcon" alt="" />
          <h1>大模型高效微调平台</h1>
          <span class="status-pill">
            <img class="status-bg" :src="figmaStatusBg" alt="" />
            <img class="status-check" :src="figmaStatusCheck" alt="" />
            <span>已连接</span>
          </span>
          <img class="title-action" :src="figmaTitleAction" alt="" />
        </div>

        <section class="content-card">
          <component
            :is="activeComponent"
            v-bind="activePageProps"
            @toggle-dropdown="toggleDropdown"
            @choose-option="chooseOption"
            @update-field="updateField"
            @update-slider="updateSlider"
            @select-tab="selectTab"
            @toggle-panel="togglePanel"
            @select-page="selectPage"
            @update-keyword="updateKeyword"
            @update-page-jump="updatePageJump"
            @select-uploaded-file="selectUploadedFile"
            @create-task="createDatasetTask"
          />
        </section>

        <div v-if="isFineTunePage" class="action-bar">
          <div class="progress-block">
            <div class="progress-title">
              <span>设备显存</span>
              <small>当前设备的显存（GB）</small>
              <strong>37.74GB / 44.43GB</strong>
            </div>
            <div class="progress-meter"><i></i></div>
          </div>
          <button
            v-for="button in actionButtons"
            :key="button.label"
            type="button"
            :class="{ primary: button.type === 'primary', muted: button.type === 'muted' }"
            @click="button.label === '开始' && createDatasetTask()"
          >
            <img v-if="button.icon" :src="button.icon" alt="" />
            {{ button.label }}
          </button>
        </div>
      </div>

      <footer class="dock">
        <img class="orb" :src="bottomOrb" alt="" />
        <div class="dock-title">智能辅助决策支持大模型</div>
        <nav>
          <button v-for="(item, index) in footerItems" :key="item.label" type="button" @click="selectPage(index === 4 ? 2 : 0)">
            <img :src="item.icon" alt="" />
            <span>{{ item.label }}</span>
          </button>
        </nav>
      </footer>
    </section>
  </main>
</template>
