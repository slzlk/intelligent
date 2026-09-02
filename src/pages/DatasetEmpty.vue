<script setup>
defineProps({
  fields: Array,
  openDropdown: String,
  selectArrow: String,
  filePreview: String,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option', 'select-page'])
</script>

<template>
  <div class="two-column">
    <aside class="side-form">
      <label v-for="field in fields.slice(0, 5)" :key="field.key" class="field">
        <span>{{ field.label }}</span>
        <button class="select-like" type="button" @click.stop="$emit('toggle-dropdown', 'empty', field)">
          {{ field.value }}<img :src="selectArrow" alt="" />
        </button>
        <div v-if="openDropdown === dropdownKey('empty', field)" class="select-menu">
          <button v-for="option in field.options" :key="option" type="button" @click.stop="$emit('choose-option', field, option)">
            {{ option }}
          </button>
        </div>
      </label>
    </aside>
    <div class="empty-state">
      <img :src="filePreview" alt="" />
      <strong>暂无文件预览</strong>
      <p>请选择数据来源并生成预训练数据集</p>
      <button type="button" class="primary" @click="$emit('select-page', 4)">选择文件</button>
    </div>
  </div>
</template>
