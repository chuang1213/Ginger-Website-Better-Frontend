<template>
  <n-layout-header bordered class="alist-header">
    <div class="header-left">
      <n-breadcrumb>
        <n-breadcrumb-item
          v-for="item in breadcrumbs"
          :key="item.path"
          @click="navigateTo(item.path)"
          class="breadcrumb-item"
        >
          <n-icon v-if="item.icon" size="16">
            <component :is="item.icon" />
          </n-icon>
          {{ item.label }}
        </n-breadcrumb-item>
      </n-breadcrumb>
    </div>
    <div class="header-right">
      <n-input
        v-model:value="searchQuery"
        placeholder="Search files..."
        class="header-search"
        round
        clearable
        @keyup.enter="handleSearch"
      >
        <template #prefix>
          <n-icon>
            <SearchIcon />
          </n-icon>
        </template>
      </n-input>
      <n-button-group class="view-toggle">
        <n-button
          :type="viewMode === 'list' ? 'primary' : 'default'"
          size="small"
          @click="viewMode = 'list'"
        >
          <n-icon>
            <ListIcon />
          </n-icon>
        </n-button>
        <n-button
          :type="viewMode === 'grid' ? 'primary' : 'default'"
          size="small"
          @click="viewMode = 'grid'"
        >
          <n-icon>
            <GridIcon />
          </n-icon>
        </n-button>
      </n-button-group>
    </div>
  </n-layout-header>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import {
  SearchOutline as SearchIcon,
  List as ListIcon,
  Grid as GridIcon,
  HomeOutline as HomeIcon
} from '@vicons/ionicons5'

const router = useRouter()
const route = useRoute()
const searchQuery = ref('')
const viewMode = ref<'list' | 'grid'>('list')

interface BreadcrumbItem {
  label: string
  path: string
  icon?: any
}

const breadcrumbs = computed<BreadcrumbItem[]>(() => {
  const items: BreadcrumbItem[] = [
    { label: 'Home', path: '/home', icon: HomeIcon }
  ]

  const pathMap: Record<string, string> = {
    '/home': 'Files',
    '/table': 'Tables',
    '/about': 'About'
  }

  const currentPath = route.path
  if (pathMap[currentPath]) {
    items.push({ label: pathMap[currentPath], path: currentPath })
  }

  return items
})

function navigateTo(path: string) {
  router.push(path)
}

function handleSearch() {
  // Emit search event or handle search logic
  console.log('Searching for:', searchQuery.value)
}
</script>

<style scoped>
.alist-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 24px;
  background: #ffffff;
  border-bottom: 1px solid #e5e6eb;
  height: 60px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 12px;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 12px;
}

.header-search {
  width: 280px;
}

.view-toggle {
  display: flex;
}

.breadcrumb-item {
  cursor: pointer;
  color: #4e5969;
  transition: color 0.2s;
}

.breadcrumb-item:hover {
  color: #18a058;
}

:deep(.n-breadcrumb-item-separator) {
  color: #c9cdd4;
}

:deep(.n-input) {
  --n-border-radius: 20px;
}

@media (prefers-color-scheme: dark) {
  .alist-header {
    background: #1a1a1a;
    border-bottom-color: #2d2d2d;
  }

  .breadcrumb-item {
    color: #c9cdd4;
  }

  .breadcrumb-item:hover {
    color: #18a058;
  }
}
</style>
