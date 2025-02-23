<script lang="ts" setup>
import { useGlobalStore, type TabPane } from '@/store/useGlobalStore'
import { uniqueId } from 'lodash-es'
import { computed } from 'vue'
import { ok } from 'vue3-ts-util'
import { FileDoneOutlined } from '@/icon'
import { t } from '@/i18n'
import { cloneDeep } from 'lodash-es'

const global = useGlobalStore()
const props = defineProps<{ tabIdx: number; paneIdx: number }>()
const compCnMap: Partial<Record<TabPane['type'], string>> = {
  local: t('local'),
  'tag-search': t('imgSearch'),
  'fuzzy-search': t('fuzzy-search'),
  'global-setting': t('globalSettings')
}
const openInCurrentTab = (type: TabPane['type'], path?: string, walkMode = false) => {
  let pane: TabPane
  switch (type) {
    case 'tag-search-matched-image-grid':
      return
    case 'global-setting':
    case 'tag-search':
    case 'fuzzy-search':
    case 'empty':
      pane = { type, name: compCnMap[type]!, key: Date.now() + uniqueId() }
      break
    case 'local':
      pane = {
        type,
        name: compCnMap[type]!,
        key: Date.now() + uniqueId(),
        path,
        walkModePath: walkMode ? path: undefined
      }
  }
  const tab = global.tabList[props.tabIdx]
  tab.panes.splice(props.paneIdx, 1, pane)
  tab.key = pane.key
}

const lastRecord = computed(() => global.tabListHistoryRecord?.[1])


const walkModeSupportedDir = computed(() =>
  global.quickMovePaths.filter(
    ({ key: k }) =>
      k === 'outdir_txt2img_samples' ||
      k === 'outdir_img2img_samples'
  )
)
const canpreviewInNewWindow = window.parent !== window
const previewInNewWindow = () => window.parent.open('/infinite_image_browsing')

const restoreRecord = () => {
  ok(lastRecord.value)
  global.tabList = cloneDeep(lastRecord.value.tabs)
  
}
</script>
<template>
  <div class="container">
    <div class="header">
      <h1>{{ $t('welcome') }}</h1>
      <div flex-placeholder />
      <a href="https://github.com/zanllp/sd-webui-infinite-image-browsing/issues/131" target="_blank"
        class="last-record">{{ $t('changlog') }}</a>
      <a href="https://github.com/zanllp/sd-webui-infinite-image-browsing/issues/90" target="_blank"
        class="last-record">{{ $t('faq') }}</a>
    </div>
    <div class="content">
      <div class="quick-start" v-if="walkModeSupportedDir.length">
        <h2>{{ $t('walkMode') }}</h2>
        <ul>
          <li v-for="item in walkModeSupportedDir" :key="item.dir" class="item">
            <AButton @click="openInCurrentTab('local', item.dir, true)" ghost type="primary" block>{{ item.zh }}</AButton>
          </li>
        </ul>
      </div>
      <div class="quick-start" v-if="global.quickMovePaths.length">
        <h2>{{ $t('launchFromQuickMove') }}</h2>
        <ul>
          <li v-for="dir in global.quickMovePaths" :key="dir.key" class="item"
            @click.prevent="openInCurrentTab('local', dir.dir)">
            <span class="text line-clamp-1">{{ dir.zh }}</span>
          </li>
        </ul>
      </div>
      <div class="quick-start">
        <h2>{{ $t('launch') }}</h2>
        <ul>
          <li v-for="comp in Object.keys(compCnMap) as TabPane['type'][]" :key="comp" class="item"
            @click.prevent="openInCurrentTab(comp)">
            <span class="text line-clamp-1">{{ compCnMap[comp] }}</span>
          </li>
          <li class="item" v-if="canpreviewInNewWindow" @click="previewInNewWindow">
            <span class="text line-clamp-1">{{ $t('openInNewWindow') }}</span>
          </li>
          <li class="item" v-if="lastRecord?.tabs.length" @click="restoreRecord">
            <span class="text line-clamp-1">{{ $t('restoreLastRecord') }}</span>
          </li>
        </ul>
      </div>
      <div class="quick-start" v-if="global.recent.length">
        <h2>{{ $t('recent') }}</h2>
        <ul>
          <li v-for="item in global.recent" :key="item.key" class="item"
            @click.prevent="openInCurrentTab('local', item.path)">
            <FileDoneOutlined class="icon" />
            <span class="text line-clamp-1">{{ item.path }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.container {
  padding: 20px;
  background-color: var(--zp-secondary-background);
  height: 100%;
  overflow: auto;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.header h1 {
  font-size: 28px;
  font-weight: bold;
  color: var(--zp-primary);
}

.last-record {
  margin-left: 16px;
  font-size: 14px;
  color: var(--zp-secondary);
}

.last-record a {
  text-decoration: none;
  color: var(--zp-secondary);
}

.last-record a:hover {
  color: var(--zp-primary);
}

.content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  grid-gap: 20px;
}

.quick-start {
  background-color: var(--zp-primary-background);
  border-radius: 8px;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  padding: 20px;

  ul {
    list-style: none;
    padding: 4px;
  }

  .item {
    margin-bottom: 10px;
    padding: 4px 8px;
    display: flex;
    align-items: center;

    &:hover {
      background: var(--zp-secondary-background);
      border-radius: 4px;
      color: var(--primary-color);
      cursor: pointer;
    }
  }

  .icon {
    margin-right: 8px;
  }
}

.quick-start h2 {
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 20px;
  font-weight: bold;
  color: var(--zp-primary);
}


.text {
  flex: 1;
  font-size: 16px;
}
</style>
