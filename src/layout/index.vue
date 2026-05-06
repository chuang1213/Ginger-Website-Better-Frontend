<template>
  <n-layout has-sider position="absolute" class="alist-layout">
    <n-layout-sider
      bordered
      collapse-mode="width"
      :collapsed-width="64"
      :width="240"
      :native-scrollbar="false"
      class="alist-sider"
    >
      <div class="sider-header">
        <n-icon size="28" color="#18a058">
          <FolderIcon />
        </n-icon>
        <span class="sider-title" v-show="!collapsed">Ginger Rush</span>
      </div>
      <n-menu
        :collapsed-width="64"
        :collapsed-icon-size="22"
        :options="menuOptions"
        :default-value="'home'"
        :inverted="false"
        class="alist-menu"
      />
    </n-layout-sider>
    <n-layout>
      <Header />
      <Content />
    </n-layout>
  </n-layout>
</template>

<script lang="ts" setup>
import { ref, h } from 'vue'
import { NIcon } from 'naive-ui'
import { RouterLink } from 'vue-router'
import {
  HomeOutline as HomeIcon,
  GridOutline as TableIcon,
  InformationCircleOutline as AboutIcon,
  FolderOutline as FolderIcon
} from '@vicons/ionicons5'
import Content from './content.vue'
import Header from './header.vue'

const collapsed = ref(false)

function renderIcon(icon: any) {
  return () => h(NIcon, null, { default: () => h(icon) })
}

function renderOption(path: string, name: string) {
  return () => h(
    RouterLink,
    { to: path },
    { default: () => name }
  )
}

const menuOptions = [
  {
    label: renderOption("/home", "Files"),
    key: 'home',
    icon: renderIcon(HomeIcon)
  },
  {
    label: renderOption("/table", "Tables"),
    key: 'table',
    icon: renderIcon(TableIcon)
  },
  {
    label: renderOption("/about", "About"),
    key: 'about',
    icon: renderIcon(AboutIcon)
  }
]
</script>

<style scoped>
.alist-layout {
  --alist-bg: #ffffff;
  --alist-sider-bg: #f7f8fa;
  --alist-border-color: #e5e6eb;
}

.alist-sider {
  background: var(--alist-sider-bg) !important;
}

.sider-header {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px 16px;
  border-bottom: 1px solid var(--alist-border-color);
}

.sider-title {
  font-size: 18px;
  font-weight: 600;
  color: #1d2129;
  white-space: nowrap;
}

.alist-menu {
  margin-top: 8px;
}

:deep(.n-menu-item-content) {
  border-radius: 8px !important;
  margin: 2px 8px !important;
}

:deep(.n-menu-item-content:hover) {
  background-color: #e8f3ed !important;
}

:deep(.n-menu-item-content--active) {
  background-color: #18a058 !important;
  color: #ffffff !important;
}

:deep(.n-menu-item-content--active .n-icon) {
  color: #ffffff !important;
}

@media (prefers-color-scheme: dark) {
  .alist-layout {
    --alist-bg: #1a1a1a;
    --alist-sider-bg: #232323;
    --alist-border-color: #2d2d2d;
  }

  .sider-title {
    color: #e5e6eb;
  }

  :deep(.n-menu-item-content:hover) {
    background-color: #2d2d2d !important;
  }
}
</style>
