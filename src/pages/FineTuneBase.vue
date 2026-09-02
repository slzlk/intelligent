<script setup>
defineProps({
  fields: Array,
  openDropdown: String,
  activeTab: Number,
  openPanels: Array,
  selectArrow: String,
  topTabs: Array,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option', 'select-tab', 'toggle-panel'])
</script>

<template>
  <section class="figma-section base-config">
    <div class="section-title">基础配置</div>
    <div class="figma-form-grid">
      <label v-for="field in fields" :key="field.key" class="field">
        <span>
          {{ field.label }}
          <small v-if="field.key === 'modelName'">输入当前可以拉取的模型</small>
          <small v-if="field.key === 'modelPath'">仅支持本地模型路径</small>
          <small v-if="field.key === 'checkpointPath'">检查点路径</small>
          <small v-if="field.key === 'quantLevel'">启用量化（QLoRA）</small>
          <small v-if="field.key === 'quantMethod'">使用的量化算法</small>
          <small v-if="field.key === 'chatTemplate'">构造提示词时使用的模板</small>
          <small v-if="field.key === 'ropeScale'">RoPE插值时使用的方法</small>
          <small v-if="field.key === 'boostMethod'">提升前向速度的方法</small>
        </span>
        <button class="select-like" type="button" @click.stop="$emit('toggle-dropdown', 'base', field)">
          {{ field.value }}
          <img :src="selectArrow" alt="" />
        </button>
        <div v-if="openDropdown === dropdownKey('base', field)" class="select-menu">
          <button
            v-for="option in field.options"
            :key="option"
            type="button"
            @click.stop="$emit('choose-option', field, option)"
          >
            {{ option }}
          </button>
        </div>
      </label>
    </div>
  </section>

  <div class="tab-strip training-tabs">
    <button
      v-for="(tab, index) in topTabs"
      :key="tab"
      type="button"
      :class="{ active: activeTab === index }"
      @click="$emit('select-tab', index)"
    >
      {{ tab }}
    </button>
  </div>

  <section class="figma-section train-config">
    <div class="figma-form-grid train-grid">
      <label
        v-for="field in [
          { key: 'stage', label: '训练阶段', value: 'Supervised', hint: '启用某种训练方式' },
          { key: 'dataPath', label: '数据路径', value: '请选择', hint: '数据文件夹的路径' },
          { key: 'dataset', label: '数据集', value: '请选择', hint: '' },
          { key: 'learningRate', label: '学习率', value: 'none', hint: 'AdamW优化器的初始学习率' },
          { key: 'trainEpoch', label: '训练轮数', value: 'none', hint: '要执行的训练总轮数' },
          { key: 'batchSize', label: '最大梯度范数', value: 'none', hint: '用于梯度裁剪的范数' },
          { key: 'sampleSize', label: '最大样本数', value: 'none', hint: '每个数据集的最大样本数' },
          { key: 'computeType', label: '计算类型', value: 'bf16', hint: '是否使用混合精度训练' },
        ]"
        :key="field.key"
        class="field"
      >
        <span>{{ field.label }}<small>{{ field.hint }}</small></span>
        <button class="select-like" type="button">
          {{ field.value }}
          <img :src="selectArrow" alt="" />
        </button>
      </label>
    </div>
    <div class="slider-grid">
      <div v-for="item in ['截断长度', '批处理大小', '学习率调节器', '验证集比例']" :key="item" class="slider-field">
        <span>{{ item }}</span>
        <div class="range-row"><i></i><input value="5" readonly /></div>
      </div>
    </div>
  </section>

  <div class="accordion-list figma-accordion-list">
    <button
      v-for="panel in ['其他参数设置', '评分参数微调设置', 'LoRA参数设置']"
      :key="panel"
      class="accordion-row"
      :class="{ open: openPanels.includes(panel) }"
      type="button"
      @click="$emit('toggle-panel', panel)"
    >
      <span>{{ panel }}</span>
      <small v-if="openPanels.includes(panel)">已展开</small>
    </button>
  </div>
</template>
