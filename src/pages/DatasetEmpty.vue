<script setup>
import { ref } from 'vue'
import generateIcon from '../assets/image/figma-dataset-generate-icon.svg'
import previewEmpty from '../assets/image/figma-dataset-preview-empty.png'
import settingIcon from '../assets/image/figma-dataset-top-setting.svg'
import titleMarker from '../assets/image/figma-dataset-title-marker.svg'
import uploadEmpty from '../assets/image/figma-dataset-upload-empty.png'

const rpm = ref('1000')
const tpm = ref('50000')
const files = ['txt_demo.txt', 'jsonl_demo.jsonl', 'json_demo.json', 'csv_demo.csv']
</script>

<template>
  <div class="dataset-generate-page dataset-generate-empty">
    <header class="generate-hero">
      <h2>微调数据集生成</h2>
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

      <button class="generate-upload" type="button" aria-label="点击/拖拽上传文件">
        <img :src="uploadEmpty" alt="" />
      </button>

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
        <button class="generate-secondary" type="button">取消</button>
        <button class="generate-primary" type="button">
          <img :src="generateIcon" alt="" />
          生成数据集
        </button>
      </div>
    </section>

    <section class="generate-panel generate-preview-panel">
      <div class="generate-panel-title">
        <img :src="titleMarker" alt="" />
        <span>预览</span>
      </div>
      <div class="generate-empty-preview">
        <img :src="previewEmpty" alt="" />
      </div>
    </section>
  </div>
</template>
