<template>
  <n-layout-content
    ref="contentRef"
    content-style="padding: 20px; background: #f7f8fa; min-height: calc(100vh - 60px)"
    :native-scrollbar="true"
    class="alist-content"
  >
    <router-view />
  </n-layout-content>
</template>

<script lang="ts" setup>
import { RouterView } from 'vue-router'
import router from "@/router"
import { provide, ref, watch } from 'vue'
import { NLayoutContent } from 'naive-ui'

provide("resetScrollBar", resetScrollBar)

const contentRef = ref<InstanceType<typeof NLayoutContent> | null>(null)
function resetScrollBar() {
  contentRef?.value?.scrollTo({ top: 0, left: 0 })
}

watch(router.currentRoute, () => {
  resetScrollBar()
})
</script>

<style scoped>
.alist-content {
  background: #f7f8fa !important;
}

:deep(.n-layout-scroll-container) {
  background: #f7f8fa;
}

@media (prefers-color-scheme: dark) {
  .alist-content,
  :deep(.n-layout-scroll-container) {
    background: #141414 !important;
  }
}
</style>
