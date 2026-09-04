<script setup>
import { ref } from 'vue'
import radioOff from '../assets/image/figma-task-radio-off.svg'
import radioOn from '../assets/image/figma-task-radio-on.svg'
import readyBg from '../assets/image/figma-task-ready-bg.svg'
import readyCheck from '../assets/image/figma-task-ready-check.svg'

const inputType = ref('单个文件')
const documentFile = ref('请选择文档文件')
const datasetName = ref('11')
const chunkSize = ref(300)
const uploadInput = ref(null)

defineEmits(['select-page'])

function openUpload() {
  uploadInput.value?.click()
}

function handleUpload(event) {
  const file = event.target.files?.[0]
  if (file) {
    documentFile.value = file.name
  }
}
</script>

<template>
  <section class="task-creation-page">
    <div class="task-scrim"></div>
    <div class="task-window">
      <header class="task-title-bar">
        <h1>创建任务</h1>
        <span class="task-window-control" aria-hidden="true"></span>
      </header>

      <form class="task-form">
        <fieldset class="task-type-field">
          <legend>输入类型</legend>
          <label class="task-radio">
            <input v-model="inputType" type="radio" value="单个文件" />
            <img :src="inputType === '单个文件' ? radioOn : radioOff" alt="" />
            <span>单个文件</span>
          </label>
          <label class="task-radio">
            <input v-model="inputType" type="radio" value="文件夹" />
            <img :src="inputType === '文件夹' ? radioOn : radioOff" alt="" />
            <span>文件夹</span>
          </label>
        </fieldset>

        <label class="task-field task-file-field">
          <span>文档文件</span>
          <input v-model="documentFile" type="text" />
          <button type="button" @click="openUpload">浏览</button>
          <input ref="uploadInput" class="task-file-input" type="file" accept=".txt,.pdf,.docx" @change="handleUpload" />
        </label>
        <p class="task-help file-help">支持的文件格式：txt/pdf/docx</p>

        <label class="task-field task-name-field">
          <span>训练数据集名称</span>
          <input v-model="datasetName" type="text" />
        </label>

        <label class="task-field task-chunk-field">
          <span>分块大小（字）</span>
          <input v-model.number="chunkSize" class="task-number" type="number" min="1" step="1" />
          <input v-model.number="chunkSize" class="task-range" type="range" min="1" max="650" />
        </label>
        <p class="task-help chunk-help">建议值：100~500字（过小可能影响语义完整性，过大会增加处理负担）</p>

        <div class="task-ready-row">
          <img class="task-ready-bg" :src="readyBg" alt="" />
          <img class="task-ready-check" :src="readyCheck" alt="" />
          <span>CSV文件已准备就绪</span>
        </div>

        <div class="task-actions">
          <button class="task-cancel" type="button" @click="$emit('select-page', 5)">取消</button>
          <button class="task-submit" type="button">开始生成预训练数据</button>
        </div>
      </form>
    </div>
  </section>
</template>
