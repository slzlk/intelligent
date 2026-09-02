<script setup>
defineProps({
  fields: Array,
  openDropdown: String,
  selectArrow: String,
  enabledRules: Array,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option', 'toggle-rule', 'create-task'])
</script>

<template>
  <div class="config-layout">
    <div class="form-grid dense">
      <label v-for="field in fields" :key="field.key" class="field">
        <span>{{ field.label }}</span>
        <button class="select-like" type="button" @click.stop="$emit('toggle-dropdown', 'config', field)">
          {{ field.value }}<img :src="selectArrow" alt="" />
        </button>
        <div v-if="openDropdown === dropdownKey('config', field)" class="select-menu">
          <button v-for="option in field.options" :key="option" type="button" @click.stop="$emit('choose-option', field, option)">
            {{ option }}
          </button>
        </div>
      </label>
    </div>
    <div class="rule-card">
      <h3>规则配置</h3>
      <label v-for="rule in ['去重过滤', '敏感词清洗', '自动补全标签']" :key="rule">
        <input type="checkbox" :checked="enabledRules.includes(rule)" @change="$emit('toggle-rule', rule)" />
        {{ rule }}
      </label>
      <button type="button" class="primary" @click="$emit('create-task')">创建任务</button>
    </div>
  </div>
</template>
