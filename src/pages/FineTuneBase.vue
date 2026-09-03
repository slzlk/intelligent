<script setup>
import FormFieldControl from '../components/FormFieldControl.vue'

defineProps({
  fields: Array,
  openDropdown: String,
  activeTab: Number,
  openPanels: Array,
  trainFields: Array,
  trainSliders: Array,
  selectArrow: String,
  topTabs: Array,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option', 'update-field', 'update-slider', 'select-tab', 'toggle-panel'])

const baseFieldHints = {
  modelName: '输入首单词以检索模型',
  modelPath: '仅支持本地模型路径（离线模式）',
  checkpointPath: '检查点路径',
  quantLevel: '启用量化（QLoRA）',
  quantMethod: '使用的量化算法',
  chatTemplate: '构造提示词时使用的模板',
  ropeScale: 'RoPE插值时使用的方法',
  boostMethod: '提升前向速度的方法',
}
</script>

<template>
  <section class="figma-section base-config">
    <div class="section-title">基础配置</div>
    <div class="base-fields-row base-first-row">
      <FormFieldControl
        v-for="field in fields.slice(0, 2)"
        :key="field.key"
        :field="field"
        group="base"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
          {{ field.label }}
          <small v-if="baseFieldHints[field.key]">{{ baseFieldHints[field.key] }}</small>
      </FormFieldControl>
    </div>
    <div class="base-fields-row base-second-row">
      <FormFieldControl
        v-for="field in fields.slice(2, 5)"
        :key="field.key"
        :field="field"
        group="base"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
          {{ field.label }}
          <small v-if="baseFieldHints[field.key]">{{ baseFieldHints[field.key] }}</small>
      </FormFieldControl>
    </div>
    <div class="base-fields-row base-third-row">
      <FormFieldControl
        v-for="field in fields.slice(5)"
        :key="field.key"
        :field="field"
        group="base"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
          {{ field.label }}
          <small v-if="baseFieldHints[field.key]">{{ baseFieldHints[field.key] }}</small>
      </FormFieldControl>
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
    <div class="train-fields-row train-first-row">
      <FormFieldControl
        v-for="field in trainFields.slice(0, 2)"
        :key="field.key"
        :field="field"
        group="train"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
        {{ field.label }}<small>{{ field.hint }}</small>
      </FormFieldControl>
    </div>
    <div class="train-fields-row train-second-row">
      <FormFieldControl
        v-for="field in trainFields.slice(2, 7)"
        :key="field.key"
        :field="field"
        group="train"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
        {{ field.label }}<small>{{ field.hint }}</small>
      </FormFieldControl>
    </div>
    <div class="train-fields-row train-third-row">
      <div v-for="slider in trainSliders.slice(0, 2)" :key="slider.key" class="slider-field">
        <span>{{ slider.label }}<small>{{ slider.hint }}</small></span>
        <div class="range-row">
          <input class="range-input" type="range" :min="slider.min" :max="slider.max" :value="slider.value" @input="$emit('update-slider', slider, $event.target.value)" />
          <input :value="slider.value" type="number" :min="slider.min" :max="slider.max" @input="$emit('update-slider', slider, $event.target.value)" />
        </div>
        <div class="range-limits"><span>{{ slider.min }}</span><span>{{ slider.max }}</span></div>
      </div>
      <FormFieldControl
        :field="trainFields[7]"
        group="train"
        :open-dropdown="openDropdown"
        :select-arrow="selectArrow"
        :dropdown-key="dropdownKey"
        @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
        @choose-option="(...args) => $emit('choose-option', ...args)"
        @update-field="(...args) => $emit('update-field', ...args)"
      >
        {{ trainFields[7].label }}<small>{{ trainFields[7].hint }}</small>
      </FormFieldControl>
      <div v-for="slider in trainSliders.slice(2)" :key="slider.key" class="slider-field">
        <span>{{ slider.label }}<small>{{ slider.hint }}</small></span>
        <div class="range-row">
          <input class="range-input" type="range" :min="slider.min" :max="slider.max" :value="slider.value" @input="$emit('update-slider', slider, $event.target.value)" />
          <input :value="slider.value" type="number" :min="slider.min" :max="slider.max" @input="$emit('update-slider', slider, $event.target.value)" />
        </div>
        <div class="range-limits"><span>{{ slider.min }}</span><span>{{ slider.max }}</span></div>
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
