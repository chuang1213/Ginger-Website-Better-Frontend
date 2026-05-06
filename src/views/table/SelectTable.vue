<template>
  <n-select
    :loading="loading"
    v-model:value="tableId"
    :options="tableOptions"
    placeholder="Select a table..."
    class="alist-select"
  />
</template>

<script setup lang="tsx">
import { selectHeaderList, type TableHeader } from '@/api/table'
import type { SelectOption } from 'naive-ui'
import { onMounted, ref, type Ref } from 'vue'

const loading = ref(false)
const tableId = defineModel<number | null>("value")
const tableOptions: Ref<SelectOption[]> = ref([])

function loadTableOptions() {
  loading.value = true
  selectHeaderList({}).then(result => {
    tableOptions.value = result.map((row: TableHeader) => {
      const coverage = row.dataCount > 0
        ? ((row.dataCount - row.missingCount) / row.dataCount * 100).toFixed(2)
        : '0.00'
      return {
        label: `${row.name} (coverage: ${coverage}%)`,
        value: row.id
      }
    })
    if (tableOptions.value.length > 0) {
      tableId.value = tableOptions.value[0]?.value as number
    }
  }).finally(() => loading.value = false)
}

onMounted(() => {
  loadTableOptions()
})
</script>

<style scoped>
:deep(.alist-select) {
  --n-border-radius: 8px;
}

:deep(.n-base-selection) {
  border-radius: 8px !important;
}

@media (prefers-color-scheme: dark) {
  :deep(.n-base-selection) {
    background: #2a2a2a;
    border-color: #2d2d2d;
  }
}
</style>
