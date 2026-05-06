<template>
  <div class="file-browser">
    <div class="browser-header">
      <div class="search-bar">
        <n-input
          v-model:value="fileNameLike"
          placeholder="Search packages..."
          size="large"
          round
          clearable
          @keyup.enter="loadData()"
          class="search-input"
        >
          <template #prefix>
            <n-icon>
              <SearchIcon />
            </n-icon>
          </template>
        </n-input>
      </div>
      <div class="view-controls">
        <n-button-group>
          <n-button
            :type="viewMode === 'list' ? 'primary' : 'default'"
            size="medium"
            @click="viewMode = 'list'"
          >
            <n-icon>
              <ListIcon />
            </n-icon>
          </n-button>
          <n-button
            :type="viewMode === 'grid' ? 'primary' : 'default'"
            size="medium"
            @click="viewMode = 'grid'"
          >
            <n-icon>
              <GridIcon />
            </n-icon>
          </n-button>
        </n-button-group>
      </div>
    </div>

    <div class="file-list" v-loading="loading">
      <template v-if="viewMode === 'list'">
        <div class="list-header">
          <div class="col-name">Name</div>
          <div class="col-size">Size</div>
          <div class="col-actions">Actions</div>
        </div>
        <div
          v-for="file in data"
          :key="file.downloadURL"
          class="list-item"
          @click="handleFileClick(file)"
        >
          <div class="col-name">
            <n-icon size="20" class="file-icon">
              <DocumentIcon />
            </n-icon>
            <span class="file-name">{{ file.fileName }}</span>
          </div>
          <div class="col-size">{{ humanFileSize(file.fileSize) }}</div>
          <div class="col-actions">
            <n-button
              type="primary"
              size="small"
              secondary
              round
              @click.stop="downloadFile(file.downloadURL)"
            >
              <template #icon>
                <n-icon>
                  <DownloadIcon />
                </n-icon>
              </template>
              Download
            </n-button>
          </div>
        </div>
      </template>

      <template v-else>
        <div class="grid-view">
          <div
            v-for="file in data"
            :key="file.downloadURL"
            class="grid-item"
            @click="handleFileClick(file)"
          >
            <div class="grid-icon">
              <n-icon size="48" color="#18a058">
                <DocumentIcon />
              </n-icon>
            </div>
            <div class="grid-name" :title="file.fileName">{{ file.fileName }}</div>
            <div class="grid-size">{{ humanFileSize(file.fileSize) }}</div>
            <n-button
              type="primary"
              size="small"
              circle
              class="grid-download"
              @click.stop="downloadFile(file.downloadURL)"
            >
              <template #icon>
                <n-icon>
                  <DownloadIcon />
                </n-icon>
              </template>
            </n-button>
          </div>
        </div>
      </template>

      <n-empty v-if="data.length === 0 && !loading" description="No files found" size="large" />
    </div>

    <div class="pagination-wrapper">
      <n-pagination
        v-model:page="pagination.page"
        v-model:page-size="pagination.pageSize"
        :page-count="pagination.pageCount"
        :page-sizes="pagination.pageSizes"
        show-size-picker
        @update:page="pagination.onChange"
        @update:page-size="pagination.onUpdatePageSize"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { findFileEntries, type FileEntryDto } from '@/api/files'
import { reactive, ref, type Ref } from 'vue'
import {
  SearchOutline as SearchIcon,
  List as ListIcon,
  Grid as GridIcon,
  DocumentTextOutline as DocumentIcon,
  DownloadOutline as DownloadIcon
} from '@vicons/ionicons5'

let loading = ref(false)
let data: Ref<Array<FileEntryDto>> = ref([])
const fileNameLike = ref<string | null>(null)
const viewMode = ref<'list' | 'grid'>('list')

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

function loadData() {
  loading.value = true
  findFileEntries({
    pageRequest: {
      page: pagination.page,
      pageSize: pagination.pageSize
    },
    fileNameLike: fileNameLike.value ?? "",
    md5: ""
  })
    .then(result => {
      if (result.data != null) {
        data.value = [...result.data]
        pagination.pageCount = result.pageCount
      }
    }).finally(() => { loading.value = false })
}

function handleFileClick(file: FileEntryDto) {
  // Could open preview or details
  console.log('File clicked:', file.fileName)
}

function downloadFile(url: string) {
  window.open(url, '_blank')
}

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

loadData()
</script>

<style scoped>
.file-browser {
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
}

.browser-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 16px;
}

.search-bar {
  flex: 1;
}

.search-input {
  max-width: 500px;
}

.file-list {
  min-height: 400px;
}

.list-header {
  display: grid;
  grid-template-columns: 1fr 120px 140px;
  padding: 12px 16px;
  background: #f7f8fa;
  border-radius: 8px;
  font-weight: 600;
  color: #4e5969;
  font-size: 13px;
  margin-bottom: 8px;
}

.list-item {
  display: grid;
  grid-template-columns: 1fr 120px 140px;
  padding: 14px 16px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: all 0.2s;
  align-items: center;
}

.list-item:hover {
  background: #f7f8fa;
  border-radius: 8px;
}

.list-item:last-child {
  border-bottom: none;
}

.col-name {
  display: flex;
  align-items: center;
  gap: 10px;
  overflow: hidden;
}

.file-icon {
  color: #18a058;
  flex-shrink: 0;
}

.file-name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.col-size {
  color: #86909c;
  font-size: 13px;
}

.col-actions {
  display: flex;
  justify-content: flex-end;
}

.grid-view {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 16px;
  padding: 8px 0;
}

.grid-item {
  background: #f7f8fa;
  border-radius: 12px;
  padding: 20px 16px;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s;
  position: relative;
}

.grid-item:hover {
  background: #e8f3ed;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(24, 160, 88, 0.15);
}

.grid-icon {
  margin-bottom: 12px;
}

.grid-name {
  font-size: 13px;
  color: #1d2129;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 6px;
}

.grid-size {
  font-size: 12px;
  color: #86909c;
}

.grid-download {
  position: absolute;
  top: 8px;
  right: 8px;
  opacity: 0;
  transition: opacity 0.2s;
}

.grid-item:hover .grid-download {
  opacity: 1;
}

.pagination-wrapper {
  display: flex;
  justify-content: center;
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid #f0f0f0;
}

@media (prefers-color-scheme: dark) {
  .file-browser {
    background: #232323;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  }

  .list-header {
    background: #2a2a2a;
    color: #c9cdd4;
  }

  .list-item {
    border-bottom-color: #2d2d2d;
  }

  .list-item:hover {
    background: #2a2a2a;
  }

  .col-size {
    color: #a0a0a0;
  }

  .grid-item {
    background: #2a2a2a;
  }

  .grid-item:hover {
    background: #2d3d33;
  }

  .grid-name {
    color: #e5e6eb;
  }

  .grid-size {
    color: #a0a0a0;
  }

  .pagination-wrapper {
    border-top-color: #2d2d2d;
  }
}
</style>
