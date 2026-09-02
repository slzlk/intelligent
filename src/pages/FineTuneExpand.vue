<script setup>
import FineTuneBase from './FineTuneBase.vue'

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
  <FineTuneBase
    :fields="fields"
    :open-dropdown="openDropdown"
    :active-tab="activeTab"
    :open-panels="openPanels"
    :select-arrow="selectArrow"
    :top-tabs="topTabs"
    :dropdown-key="dropdownKey"
    @toggle-dropdown="(...args) => $emit('toggle-dropdown', ...args)"
    @choose-option="(...args) => $emit('choose-option', ...args)"
    @select-tab="(...args) => $emit('select-tab', ...args)"
    @toggle-panel="(...args) => $emit('toggle-panel', ...args)"
  />
  <div class="assistant-box">
    <div class="assistant-icon">AI</div>
    <p>Ai助手，我能为你做些什么?</p>
  </div>
</template>
