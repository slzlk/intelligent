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
  <div class="form-grid">
    <label v-for="field in fields" :key="field.key" class="field">
      <span>{{ field.label }}</span>
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

  <div class="tab-strip">
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

  <div class="accordion-list">
    <button
      v-for="panel in ['训练数据集配置', '模型参数配置', '训练评估指标']"
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
