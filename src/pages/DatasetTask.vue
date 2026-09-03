<script setup>
defineProps({
  showTaskModal: Boolean,
  taskName: String,
  serviceName: String,
  taskSteps: Array,
})

defineEmits(['update-task-name', 'update-service-name', 'close-modal', 'select-page'])
</script>

<template>
  <div v-if="showTaskModal" class="modal-mask">
    <div class="task-modal">
      <h2>创建任务</h2>
      <div class="step-line">
        <span v-for="step in taskSteps" :key="step">{{ step }}</span>
      </div>
      <label class="field">
        <span>任务名称</span>
        <input
          :value="taskName"
          class="input-like"
          type="text"
          placeholder="请输入"
          @input="$emit('update-task-name', $event.target.value)"
        />
      </label>
      <label class="field">
        <span>生成服务</span>
        <input
          class="input-like"
          type="text"
          :value="serviceName"
          @input="$emit('update-service-name', $event.target.value)"
        />
      </label>
      <div class="ready-row">
        <span class="ok-dot"></span>
        CSV文件已准备就绪
      </div>
      <div class="modal-actions">
        <button type="button" @click="$emit('close-modal')">取消</button>
        <button type="button" class="primary" @click="$emit('close-modal')">确定</button>
      </div>
    </div>
  </div>
  <div v-else class="empty-state">
    <strong>任务已提交</strong>
    <p>可返回数据集管理继续查看生成状态</p>
    <button type="button" class="primary" @click="$emit('select-page', 2)">返回数据集管理</button>
  </div>
</template>
