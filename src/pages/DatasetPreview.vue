<script setup>
import { ref, watch } from 'vue'
import clearIcon from '../assets/image/figma-dataset-file-clear.svg'
import eyeIcon from '../assets/image/figma-dataset-file-eye.svg'
import copyIcon from '../assets/image/figma-dataset-preview-copy.svg'
import downloadIcon from '../assets/image/figma-dataset-preview-download.svg'
import generateIcon from '../assets/image/figma-dataset-generate-icon.svg'
import settingIcon from '../assets/image/figma-dataset-top-setting.svg'
import titleMarker from '../assets/image/figma-dataset-title-marker.svg'
import uploadEmpty from '../assets/image/figma-dataset-upload-empty.png'

const props = defineProps({
  fileName: String,
})

defineEmits(['select-page'])

const rpm = ref('1000')
const tpm = ref('50000')
const uploadInput = ref(null)
const currentFile = ref('txt_demo.txt')
const files = ['txt_demo.txt', 'jsonl_demo.jsonl', 'json_demo.json', 'csv_demo.csv']
const previewRows = [
  ['001', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。该品种外观特点为：颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大。'],
  ['002', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。'],
  ['003', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。该品种外观特点为：颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大。'],
  ['004', '颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大。'],
  ['005', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。该品种外观特点为：颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大，'],
  ['006', '云南省农业科学院粮食作物云米更26号，该品种外观特点为：颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大，'],
  ['007', '颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大。'],
  ['008', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。该品种外观特点为：颗尖无色、无芒，谷壳黄色，落粒性适中，米粒大。'],
  ['009', '云南省农业科学院粮食作物研究所于2005年育成早熟品种云米更26号。'],
]

watch(
  () => props.fileName,
  (fileName) => {
    currentFile.value = fileName || 'txt_demo.txt'
  },
  { immediate: true },
)

function openUpload() {
  uploadInput.value?.click()
}

function handleUpload(event) {
  const file = event.target.files?.[0]
  if (file) {
    currentFile.value = file.name
  }
}

function clearFile() {
  currentFile.value = ''
}
</script>

<template>
  <div class="dataset-generate-page dataset-generate-preview">
    <header class="generate-hero">
      <h2>预训练数据集生成</h2>
      <p>通过提取知识图谱生成预训练数据集</p>
      <button class="generate-icon-button" type="button" aria-label="设置">
        <img :src="settingIcon" alt="" />
      </button>
      <button class="generate-test-button" type="button">测试连接</button>
    </header>

    <section class="generate-panel generate-left-panel">
      <div class="generate-panel-title wide">
        <img :src="titleMarker" alt="" />
        <span>预训练数据集生成</span>
      </div>

      <button class="generate-upload" type="button" aria-label="点击/拖拽上传文件" @click="openUpload">
        <img :src="uploadEmpty" alt="" />
      </button>
      <input ref="uploadInput" class="generate-upload-input" type="file" accept=".txt,.pdf,.docx,.jsonl,.json,.csv" @change="handleUpload" />

      <div class="generate-file-card">
        <div class="generate-file-icon">txt</div>
        <a href="#" @click.prevent>{{ currentFile || 'txt_demo.txt' }}</a>
        <button type="button" aria-label="预览文件"><img :src="eyeIcon" alt="" /></button>
        <button type="button" aria-label="清理文件" @click="clearFile"><img :src="clearIcon" alt="" /></button>
      </div>

      <div class="generate-slider rpm">
        <label>RPM</label>
        <div class="generate-slider-control">
          <input v-model="rpm" type="range" min="10" max="10000" />
          <input v-model="rpm" type="number" min="10" max="10000" />
        </div>
        <div class="generate-slider-limits">
          <span>10</span>
          <span>10000</span>
        </div>
      </div>

      <div class="generate-slider tpm">
        <label>TPM</label>
        <div class="generate-slider-control">
          <input v-model="tpm" type="range" min="10" max="5000000" />
          <input v-model="tpm" type="number" min="10" max="5000000" />
        </div>
        <div class="generate-slider-limits">
          <span>10</span>
          <span>5000000</span>
        </div>
      </div>

      <div class="generate-example-label">示例文件</div>

      <div class="generate-file-tags">
        <span v-for="file in files" :key="file">{{ file }}<i>x</i></span>
      </div>

      <div class="generate-panel-actions">
        <button class="generate-secondary" type="button" @click="$emit('select-page', 2)">取消</button>
        <button class="generate-primary" type="button" @click="$emit('select-page', 5)">
          <img :src="generateIcon" alt="" />
          生成数据集
        </button>
      </div>
    </section>

    <section class="generate-panel generate-preview-panel">
      <div class="generate-panel-title file-preview-title">
        <img :src="titleMarker" alt="" />
        <span>文件预览</span>
      </div>
      <div class="file-preview-tools">
        <button type="button" aria-label="下载"><img :src="downloadIcon" alt="" /></button>
        <button type="button" aria-label="复制"><img :src="copyIcon" alt="" /></button>
      </div>
      <div class="file-preview-list">
        <div v-for="row in previewRows" :key="row[0]" class="file-preview-row">
          <span>{{ row[0] }}</span>
          <p>{{ row[1] }}</p>
        </div>
      </div>
    </section>
  </div>
</template>
