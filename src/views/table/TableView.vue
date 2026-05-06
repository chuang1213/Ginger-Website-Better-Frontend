<template>
  <div class="table-browser">
    <div class="table-header">
      <div class="header-title">
        <n-icon size="20" color="#18a058">
          <TableIcon />
        </n-icon>
        <h3>Select Table</h3>
      </div>
      <SelectTable v-model:value="currentTableId" class="table-select" />
    </div>

    <n-data-table
      remote
      :loading="loading"
      :columns="columns"
      :data="data"
      :pagination="false"
      :row-key="(row: TableLevelEntry) => row.level"
      class="alist-table"
    />
  </div>
</template>

<script setup lang="tsx">
import { selectLevelEntries, type TableLevelEntry } from '@/api/table'
import type { DataTableColumns } from 'naive-ui'
import { ref, watch, type Ref, type VNode } from 'vue'
import SelectTable from './SelectTable.vue'
import TableData from './TableData.vue'
import { GridOutline as TableIcon, FolderOutline as FolderIcon } from '@vicons/ionicons5'

const loading = ref(false)

const currentTableId: Ref<number | null> = ref(null)

let data: Ref<TableLevelEntry[]> = ref([])
const columns: DataTableColumns<TableLevelEntry> = [
  {
    type: "expand",
    renderExpand(row: TableLevelEntry): VNode {
      return (
        <TableData headerId={currentTableId.value ?? 0} level={row.level} />
      )
    }
  },
  {
    title: "Name",
    key: "name",
    render(row) {
      return (
        <div class="table-name-cell">
          <n-icon size="18" color="#18a058">
            <FolderIcon />
          </n-icon>
          <span>{row.name}</span>
        </div>
      )
    }
  }
]

function loadData() {
  loading.value = true
  selectLevelEntries(currentTableId.value ?? 0).then(result => {
    data.value = [...result]
  }).finally(() => loading.value = false)
}

watch(currentTableId, () => {
  loadData()
})
</script>

<style scoped>
.table-browser {
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 16px;
  flex-wrap: wrap;
}

.header-title {
  display: flex;
  align-items: center;
  gap: 10px;
}

.header-title h3 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #1d2129;
}

.table-select {
  min-width: 280px;
}

.table-name-cell {
  display: flex;
  align-items: center;
  gap: 8px;
}

:deep(.alist-table) {
  --n-td-color-hover: #f7f8fa;
  border-radius: 8px;
  overflow: hidden;
}

:deep(.n-data-table-th) {
  background: #f7f8fa;
  font-weight: 600;
  color: #4e5969;
}

:deep(.n-data-table-td) {
  padding: 14px 16px;
}

:deep(.n-data-table-expand-trigger) {
  color: #18a058;
}

@media (prefers-color-scheme: dark) {
  .table-browser {
    background: #232323;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  }

  .header-title h3 {
    color: #e5e6eb;
  }

  :deep(.alist-table) {
    --n-td-color-hover: #2a2a2a;
  }

  :deep(.n-data-table-th) {
    background: #2a2a2a;
    color: #c9cdd4;
  }
}
</style>
