<script setup>
defineProps({
  columns: Array,
  rows: Array,
  selectedDataset: String,
  keyword: String,
})

defineEmits(['update-keyword', 'select-page', 'choose-dataset'])
</script>

<template>
  <div class="toolbar">
    <button type="button" @click="$emit('select-page', 3)">新建数据集</button>
    <button type="button" @click="$emit('select-page', 4)">导入文件</button>
    <input
      :value="keyword"
      class="toolbar-search"
      type="search"
      placeholder="请输入关键词"
      @input="$emit('update-keyword', $event.target.value)"
    />
  </div>
  <table class="data-table">
    <thead>
      <tr>
        <th v-for="column in columns" :key="column">{{ column }}</th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="row in rows"
        :key="row[0]"
        :class="{ selected: selectedDataset === row[0] }"
        @click="$emit('choose-dataset', row)"
      >
        <td v-for="cell in row" :key="cell">{{ cell }}</td>
      </tr>
    </tbody>
  </table>
</template>
