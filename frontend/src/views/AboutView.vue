<script setup lang="ts">
import { useI18n } from 'vue-i18n'

import { RestartApp, BrowserOpenURL } from '@/bridge'
import { useAppStore } from '@/stores'
import { APP_TITLE, APP_VERSION, PROJECT_URL, TG_GROUP, TG_CHANNEL, message } from '@/utils'

const { t } = useI18n()
const appStore = useAppStore()

const handleRestartApp = async () => {
  try {
    await RestartApp()
  } catch (error: any) {
    message.error(error)
  }
}

appStore.checkForUpdates()
</script>

<template>
  <div class="about flex flex-col items-center">
    <img src="@/assets/logo.png" style="width: 128px" draggable="false" />
    <div class="py-8 font-bold">{{ APP_TITLE }}</div>
    <div class="flex items-center pb-8 my-4">
      <Button v-if="appStore.restartable" @click="handleRestartApp" size="small" type="primary">
        <Icon icon="restartApp" fill="var(--btn-primary-color)" style="margin-top: 1px" />
        <span style="margin-left: 4px">{{ t('about.restart') }}</span>
      </Button>
      <template v-else>
        <Button
          @click="appStore.checkForUpdates(true)"
          :loading="appStore.checkForUpdatesLoading"
          type="link"
          size="small"
        >
          {{ APP_VERSION }}
        </Button>
        <Button
          v-if="appStore.updatable"
          @click="appStore.downloadApp"
          :loading="appStore.downloading"
          size="small"
        >
          {{ t('about.new') }}: {{ appStore.remoteVersion }}
        </Button>
      </template>
    </div>
    <div @click="BrowserOpenURL(PROJECT_URL)" class="text-12 underline flex items-center">
      <Icon icon="github" />GitHub
    </div>
    <div @click="BrowserOpenURL(TG_GROUP)" class="text-12 underline flex items-center">
      <Icon icon="telegram" />Telegram Group
    </div>
    <div @click="BrowserOpenURL(TG_CHANNEL)" class="text-12 underline flex items-center">
      <Icon icon="telegram" />Telegram Channel
    </div>
  </div>
</template>

<style lang="less" scoped>
.about {
  padding: 36px 0 0 0;
}
</style>
