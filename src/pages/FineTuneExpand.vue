<script setup>
import figmaCheckboxEmpty from '../assets/image/figma-checkbox-empty.svg'
import figmaCollapseArrow from '../assets/image/figma-collapse-arrow.svg'
import figmaCollapsedSectionBg from '../assets/image/figma-collapsed-section-bg.svg'
import figmaLossIcon from '../assets/image/figma-loss-icon.svg'
import figmaLossPanel from '../assets/image/figma-loss-panel.png'
import FormFieldControl from '../components/FormFieldControl.vue'

defineProps({
  openDropdown: String,
  activeTab: Number,
  trainFields: Array,
  trainSliders: Array,
  selectArrow: String,
  topTabs: Array,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option', 'update-field', 'update-slider', 'select-tab'])

const outputFields = [
  {
    key: 'outputDir',
    label: '输出目录',
    hint: '保存结果的路径',
    value: 'train_2026-08-17-16-20-00',
    options: ['train_2026-08-17-16-20-00'],
  },
  {
    key: 'configPath',
    label: '配置路径',
    hint: '保存训练参数的配置文件路径',
    value: '2026-08-17-16-20-00.yaml',
    options: ['2026-08-17-16-20-00.yaml'],
  },
]

const repeatedFields = [
  { key: 'deviceCountA', label: '设备数量', hint: '当前可用的设备数', value: '3', options: ['3'] },
  { key: 'deepSpeedA', label: 'DeepSpeed Stage', hint: '多卡训练的DeepSpeed Stage', value: 'none', options: ['none'] },
  { key: 'deviceCountB', label: '设备数量', hint: '当前可用的设备数', value: '3', options: ['3'] },
  { key: 'deepSpeedB', label: 'DeepSpeed Stage', hint: '多卡训练的DeepSpeed Stage', value: 'none', options: ['none'] },
]

const repeatedRows = [
  repeatedFields.slice(0, 2),
  repeatedFields.slice(2, 4),
]
</script>

<template>
  <section class="collapsed-section">
    <img class="collapsed-bg" :src="figmaCollapsedSectionBg" alt="" />
    <span>基础配置</span>
    <img class="collapsed-arrow" :src="figmaCollapseArrow" alt="" />
  </section>

  <div class="tab-strip training-tabs expanded-tabs">
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

  <section class="figma-section train-config expanded-train-config">
    <div class="train-fields-row train-first-row">
      <FormFieldControl
        v-for="field in trainFields.slice(0, 2)"
        :key="field.key"
        :field="field"
        group="expand-train"
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
        group="expand-train"
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
        group="expand-train"
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

    <div class="expanded-accordion-list">
      <button v-for="panel in ['其他参数设置', '评分参数微调设置', 'LoRA参数设置']" :key="panel" class="accordion-row" type="button">
        <span>{{ panel }}</span>
      </button>
    </div>

    <div class="expanded-bottom">
      <div class="output-area">
        <div class="expanded-output-fields">
          <FormFieldControl
            v-for="field in outputFields"
            :key="field.key"
            :field="field"
            group="expand-output"
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

        <div v-for="(row, rowIndex) in repeatedRows" :key="rowIndex" class="repeated-row">
          <FormFieldControl
            v-for="field in row"
            :key="field.key"
            :field="field"
            group="expand-extra"
            :open-dropdown="openDropdown"
            :select-arrow="selectArrow"
            :dropdown-key="dropdownKey"
            @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
            @choose-option="(...args) => $emit('choose-option', ...args)"
            @update-field="(...args) => $emit('update-field', ...args)"
          >
            {{ field.label }}<small>{{ field.hint }}</small>
          </FormFieldControl>
          <label class="offload-field">
            <img :src="figmaCheckboxEmpty" alt="" />
            <span>使用offload</span>
            <small>使用DeepSpeed offload（会减慢速度）</small>
          </label>
        </div>
      </div>

      <div class="loss-panel">
        <img class="loss-bg" :src="figmaLossPanel" alt="" />
        <div>
          <img :src="figmaLossIcon" alt="" />
          <span>损失</span>
        </div>
      </div>
    </div>
  </section>
</template>
