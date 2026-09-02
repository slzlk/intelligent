<script setup>
defineProps({
  fields: Array,
  openDropdown: String,
  selectArrow: String,
  filePreview: String,
  previewLines: Array,
  dropdownKey: Function,
})

defineEmits(['toggle-dropdown', 'choose-option'])
</script>

<template>
  <div class="two-column">
    <aside class="side-form">
      <label v-for="field in fields.slice(0, 6)" :key="field.key" class="field">
        <span>{{ field.label }}</span>
        <button class="select-like" type="button" @click.stop="$emit('toggle-dropdown', 'preview', field)">
          {{ field.value }}<img :src="selectArrow" alt="" />
        </button>
        <div v-if="openDropdown === dropdownKey('preview', field)" class="select-menu">
          <button v-for="option in field.options" :key="option" type="button" @click.stop="$emit('choose-option', field, option)">
            {{ option }}
          </button>
        </div>
      </label>
    </aside>
    <div class="preview-pane">
      <div class="file-title">
        <img :src="filePreview" alt="" />
        <span>预训练数据预览</span>
      </div>
      <p v-for="line in previewLines" :key="line">{{ line }}</p>
    </div>
  </div>
</template>
