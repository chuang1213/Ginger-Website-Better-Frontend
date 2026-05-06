<template>
  <n-config-provider :hljs="hljs" :theme="theme" :theme-overrides="themeOverrides">
    <n-notification-provider>
      <n-modal-provider>
        <n-global-style />
        <n-loading-bar-provider>
          <n-message-provider>
            <n-dialog-provider>
              <slot />
            </n-dialog-provider>
          </n-message-provider>
        </n-loading-bar-provider>
      </n-modal-provider>
    </n-notification-provider>
  </n-config-provider>
</template>

<script lang="ts" setup>
import { darkTheme, useOsTheme, type GlobalThemeOverrides } from "naive-ui"
import { computed } from "vue"
import hljs from "highlight.js/lib/core"
import json from "highlight.js/lib/languages/json"
import javascript from "highlight.js/lib/languages/javascript"

hljs.registerLanguage('json', json)
hljs.registerLanguage('javascript', javascript)

const osTheme = useOsTheme()
const theme = computed(() => (osTheme.value == "dark" ? darkTheme : null))

const themeOverrides: GlobalThemeOverrides = {
  common: {
    primaryColor: '#18a058',
    primaryColorHover: '#36b37e',
    primaryColorPressed: '#0d8a4a',
    primaryColorSuppl: '#36b37e',
    borderRadius: '8px',
    fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif'
  },
  Layout: {
    siderColor: '#f7f8fa'
  },
  Menu: {
    itemTextColorHorizontal: '#4e5969',
    itemTextColorActiveHorizontal: '#18a058',
    itemTextColorHoverHorizontal: '#18a058',
    itemColorHoverHorizontal: '#e8f3ed',
    itemColorActiveHoverHorizontal: '#e8f3ed'
  },
  Input: {
    borderRadius: '20px'
  },
  Card: {
    borderRadius: '12px',
    paddingMedium: '20px'
  },
  DataTable: {
    thColor: '#f7f8fa',
    tdColor: '#ffffff',
    tdColorHover: '#f7f8fa',
    borderColor: '#f0f0f0',
    borderRadius: '8px'
  },
  Pagination: {
    itemBorderRadius: '8px'
  },
  Select: {
    peers: {
      InternalSelection: {
        borderRadius: '8px'
      }
    }
  }
}
</script>
