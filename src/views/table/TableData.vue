<template>
  <div class="table-data">
    <n-data-table
      remote
      :data="data"
      :columns="columns"
      :pagination="pagination"
      :row-key="(row: DownloadableTableDataDto) => row.id"
      class="alist-table"
    />
  </div>
</template>

<script setup lang="tsx">
import { selectDataList, type DownloadableTableDataDto, type TableData } from '@/api/table'
import { NIcon, NButton, type DataTableColumns, createDiscreteApi } from 'naive-ui'
import { reactive, ref, watch, type Ref, type VNode, h } from 'vue'
import { DownloadOutline as DownloadIcon, WarningOutline as WarningIcon, DocumentTextOutline as FileIcon } from '@vicons/ionicons5'

interface Props {
  headerId?: number
  level?: string
}

const props = defineProps<Props>()

const loading = ref(false)
const pagination = reactive({
  page: 1,
  pageSize: 10,
  pageCount: 0,
  showSizePicker: true,
  pageSizes: [10, 20, 50],
  onChange: (page: number) => {
    pagination.page = page
    loadData()
  },
  onUpdatePageSize: (pageSize: number) => {
    pagination.pageSize = pageSize
    pagination.page = 1
    loadData()
  }
})
const columns: DataTableColumns<DownloadableTableDataDto> = [
  {
    title: "Title",
    key: "title",
    render(row: DownloadableTableDataDto): VNode[] {
      let ret: VNode[] = []
      if (row.fileSize == 0) {
        ret.push((
          <NIcon color="#f53f3f" size={18}>
            <WarningIcon />
          </NIcon>
        ))
      } else {
        ret.push((
          <NIcon color="#18a058" size={18}>
            <FileIcon />
          </NIcon>
        ))
      }
      ret.push(<span class="table-data-title">{row.title as any as string}</span>)
      return ret
    }
  },
  {
    title: "Size",
    key: "fileSize",
    render(row) {
      return <span class="table-data-size">{humanFileSize(row.fileSize)}</span>
    }
  },
  {
    title: "Actions",
    key: "actions",
    render(row): VNode | null {
      if (row.fileSize == 0) {
        return null
      }
      const renderIcon = () => h(NIcon, null, { default: () => h(DownloadIcon) })
      return (
        <NButton type="primary" size="small" secondary round onClick={() => window.open(row.downloadURL, '_blank')} renderIcon={renderIcon}>
          Download
        </NButton>
      )
    }
  }
]
const data: Ref<TableData[]> = ref([])

function loadData() {
  if (props.headerId == null) {
    return
  }
  selectDataList({
    headerID: props.headerId,
    level: props.level ?? "",
    pageRequest: {
      page: pagination.page,
      pageSize: pagination.pageSize
    },
  }).then(result => {
    if (result.data != null) {
      data.value = [...result.data]
      pagination.pageCount = result.pageCount
    }
  }).finally(() => loading.value = false)
}

watch(props, () => {
  loadData()
}, { immediate: true })

function humanFileSize(bytes: number, si = false, dp = 1) {
  const thresh = si ? 1000 : 1024

  if (Math.abs(bytes) < thresh) {
    return bytes + ' B'
  }

  const units = si
    ? ['kB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB']
    : ['KiB', 'MiB', 'GiB', 'TiB', 'PiB', 'EiB', 'ZiB', 'YiB']
  let u = -1
  const r = 10 ** dp

  do {
    bytes /= thresh
    ++u
  } while (Math.round(Math.abs(bytes) * r) / r >= thresh && u < units.length - 1)

  return bytes.toFixed(dp) + ' ' + units[u]
}
</script>

<style scoped>
.table-data {
  padding: 8px 0;
}

.table-data-title {
  margin-left: 8px;
  display: inline-flex;
  align-items: center;
}

.table-data-size {
  color: #86909c;
  font-size: 13px;
}

:deep(.alist-table) {
  --n-td-color-hover: #f7f8fa;
}

:deep(.n-data-table-th) {
  background: #f7f8fa;
  font-weight: 600;
  color: #4e5969;
}

:deep(.n-data-table-td) {
  padding: 12px 16px;
}

:deep(.n-data-table-thead) {
  border-radius: 8px;
}

@media (prefers-color-scheme: dark) {
  .table-data-size {
    color: #a0a0a0;
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
