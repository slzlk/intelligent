<script setup>
import checkboxIcon from '../assets/image/figma-dataset-card-checkbox.svg'
import fieldNameIcon from '../assets/image/figma-dataset-icon-name.svg'
import fieldSizeIcon from '../assets/image/figma-dataset-icon-size.svg'
import fieldStateIcon from '../assets/image/figma-dataset-icon-state.svg'
import fieldTaskIcon from '../assets/image/figma-dataset-icon-task.svg'
import fieldFileIcon from '../assets/image/figma-dataset-icon-file.svg'
import refreshIcon from '../assets/image/figma-dataset-refresh.svg'
import searchIcon from '../assets/image/figma-dataset-search-icon.svg'
import tagBg from '../assets/image/figma-dataset-tag-bg.svg'
import titleMarker from '../assets/image/figma-dataset-title-marker.svg'

defineProps({
  keyword: String,
  pageJump: String,
})

defineEmits(['update-keyword', 'update-page-jump'])

const cards = Array.from({ length: 5 }, () => ({
  key: 'aefd8adc_0_118',
  tag: '块#118',
  summary: '(value=0.425)推理结果：维持“平台部署”类型不变。推理过程：[价值基线]价值基线处于中位区间；[价值稳定性]价值差异极...',
  metrics: [
    { label: '任务ID:', value: 'aefd8adc', icon: fieldTaskIcon },
    { label: '数据集名称:', value: '11dvsv', icon: fieldNameIcon },
    { label: '源文件:', value: '5151scdsd562sdvcnsjmkj222251514cds_txt.txt', icon: fieldFileIcon, wide: true },
    { label: '数据大小:', value: '111字符', icon: fieldSizeIcon },
    { label: '状态:', value: 'ACTIVE', icon: fieldStateIcon },
  ],
}))

const pageNumbers = ['1', '2', '3', '4', '5', '6']
const cardActions = ['编辑', '详情', '删除']
</script>

<template>
  <div class="dataset-manage-page">
    <div class="dataset-heading">
      <img :src="titleMarker" alt="" />
      <span>数据集管理</span>
    </div>

    <div class="dataset-filter-row">
      <button class="dataset-primary-button" type="button">确定</button>
      <button class="dataset-plain-button" type="button">取消</button>

      <label class="dataset-search">
        <input
          :value="keyword"
          type="search"
          placeholder="搜索数据集ID、任务ID、数据集名或文件名"
          @input="$emit('update-keyword', $event.target.value)"
        />
        <img :src="searchIcon" alt="" />
      </label>

      <button class="dataset-icon-button" type="button" aria-label="刷新">
        <img :src="refreshIcon" alt="" />
      </button>

      <div class="dataset-mode-tabs" role="tablist" aria-label="数据集类型">
        <button class="active" type="button" role="tab" aria-selected="true">无监督数据集管理</button>
        <button type="button" role="tab" aria-selected="false">监督数据集管理</button>
      </div>
    </div>

    <div class="dataset-divider"></div>

    <div class="dataset-bulk-row">
      <button class="dataset-check-button" type="button">
        <img :src="checkboxIcon" alt="" />
        <span>全选</span>
      </button>
      <button class="dataset-delete-selected" type="button">删除选中</button>
    </div>

    <section class="dataset-card-grid" aria-label="数据集列表">
      <article v-for="(card, index) in cards" :key="`${card.key}-${index}`" class="dataset-card">
        <div class="dataset-card-summary">
          <div class="dataset-card-key">
            <img :src="checkboxIcon" alt="" />
            <strong>{{ card.key }}</strong>
          </div>
          <span class="dataset-card-tag">
            <img :src="tagBg" alt="" />
            <span>{{ card.tag }}</span>
          </span>
          <p>{{ card.summary }}</p>
        </div>

        <div class="dataset-card-metrics">
          <div v-for="metric in card.metrics" :key="metric.label" class="dataset-metric" :class="{ wide: metric.wide }">
            <span class="dataset-metric-label">
              <img :src="metric.icon" alt="" />
              {{ metric.label }}
            </span>
            <strong>{{ metric.value }}</strong>
          </div>
        </div>

        <div class="dataset-card-actions">
          <button v-for="action in cardActions" :key="action" type="button">{{ action }}</button>
        </div>
      </article>
    </section>

    <nav class="dataset-pagination" aria-label="分页">
      <span>共 100 项数据</span>
      <button class="dataset-page-size" type="button">10 条/页</button>
      <button class="dataset-page-arrow" type="button" aria-label="上一页"></button>
      <button
        v-for="number in pageNumbers"
        :key="number"
        class="dataset-page-number"
        :class="{ active: number === '1' }"
        type="button"
      >
        {{ number }}
      </button>
      <button class="dataset-page-arrow next" type="button" aria-label="下一页"></button>
      <label class="dataset-page-jump">
        <span>跳至</span>
        <input
          :value="pageJump"
          type="text"
          placeholder="请输入"
          @input="$emit('update-page-jump', $event.target.value)"
        />
        <span>/20页</span>
      </label>
    </nav>
  </div>
</template>
