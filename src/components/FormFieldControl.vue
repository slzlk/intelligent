<script setup>
defineProps({
  field: {
    type: Object,
    required: true,
  },
  group: {
    type: String,
    required: true,
  },
  openDropdown: String,
  selectArrow: String,
  dropdownKey: {
    type: Function,
    required: true,
  },
})

defineEmits(['toggle-dropdown', 'choose-option', 'update-field'])
</script>

<template>
  <label class="field">
    <span>
      <slot>{{ field.label }}</slot>
    </span>
    <input
      v-if="field.input"
      :value="field.value"
      class="input-like"
      :type="field.inputType || 'text'"
      :placeholder="field.placeholder || '请输入'"
      :min="field.min"
      :max="field.max"
      :step="field.step"
      @input="$emit('update-field', field, $event.target.value)"
    />
    <button v-else class="select-like" type="button" @click.stop="$emit('toggle-dropdown', group, field)">
      {{ field.value }}
      <img :src="selectArrow" alt="" />
    </button>
    <div v-if="!field.input && openDropdown === dropdownKey(group, field)" class="select-menu">
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
</template>
