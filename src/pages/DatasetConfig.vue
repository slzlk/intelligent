<script setup>
import { ref } from 'vue'
import checkboxIcon from '../assets/image/figma-dataset-card-checkbox.svg'
import refreshModelIcon from '../assets/image/figma-dataset-refresh-model.svg'
import settingIcon from '../assets/image/figma-dataset-top-setting.svg'
import titleMarker from '../assets/image/figma-dataset-title-marker.svg'

const fields = ref({
  model: '请选择',
  chunkSize: '1024',
  chunkOverlap: '100',
  maxTokens: '256',
  maxDepth: '3',
})

const sliders = [
  { key: 'chunkSize', label: '分块大小', min: '256', max: '4096', left: 'config-left-top' },
  { key: 'chunkOverlap', label: '分块重叠', min: '0', max: '500', left: 'config-right-top' },
  { key: 'maxTokens', label: '最大token数', min: '64', max: '1024', left: 'config-left-bottom' },
  { key: 'maxDepth', label: '最大深度', min: '1', max: '5', left: 'config-right-bottom' },
]

const radioGroups = [
  { className: 'data-type', label: '输出数据类型', options: ['atomic', 'multi_hop', 'aggregated'] },
  { className: 'data-format', label: '输出数据格式', options: ['Alpaca', 'Sharegpt', 'ChatML'] },
  { className: 'expand-mode', label: '扩展方式', options: ['max_width', 'max_tokens'] },
  { className: 'orphan-strategy', label: '孤立节点策略', options: ['add', 'ignore'] },
  { className: 'loss-strategy', label: '损失策略', options: ['only_edge', 'both'] },
]
</script>

<template>
  <div class="dataset-config-page">
    <section class="config-main-panel">
      <header class="generate-hero">
        <h2>预训练数据集生成</h2>
        <p>通过提取知识图谱生成预训练数据集</p>
        <button class="generate-icon-button" type="button" aria-label="设置">
          <img :src="settingIcon" alt="" />
        </button>
        <button class="generate-test-button" type="button">测试连接</button>
      </header>

      <div class="config-section-title config-model-title">
        <img :src="titleMarker" alt="" />
        <span>模型配置</span>
      </div>

      <label class="config-select-field">
        <span>合成器模型（本地Ollama）</span>
        <button type="button">{{ fields.model }}</button>
      </label>

      <button class="config-refresh-model" type="button">
        <img :src="refreshModelIcon" alt="" />
        刷新模型列表
      </button>

      <div class="config-section-title config-param-title">
        <img :src="titleMarker" alt="" />
        <span>生成参数配置</span>
      </div>

      <label
        v-for="slider in sliders"
        :key="slider.key"
        class="config-param-slider"
        :class="slider.left"
      >
        <span>{{ slider.label }}</span>
        <div>
          <input v-model="fields[slider.key]" type="range" :min="slider.min" :max="slider.max" />
          <input v-model="fields[slider.key]" type="number" :min="slider.min" :max="slider.max" />
        </div>
        <small>
          <span>{{ slider.min }}</span>
          <span>{{ slider.max }}</span>
        </small>
      </label>

      <label class="config-checkbox-row">
        <img :src="checkboxIcon" alt="" />
        <span>双向扩展</span>
      </label>

      <div
        v-for="group in radioGroups"
        :key="group.label"
        class="config-radio-group"
        :class="group.className"
      >
        <span>{{ group.label }}</span>
        <div>
          <label v-for="(option, index) in group.options" :key="option">
            <input type="radio" :name="group.label" :checked="index === 0" />
            {{ option }}
          </label>
        </div>
      </div>
    </section>
  </div>
</template>
