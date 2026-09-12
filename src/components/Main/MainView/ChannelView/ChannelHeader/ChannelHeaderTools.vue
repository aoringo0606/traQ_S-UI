<template>
  <div>
    <ChannelHeaderToolsList
      :class="$style.tools"
      :channel-id="channelId"
      :is-forced-channel="channelState.forced"
      :is-starred="channelState.starred"
      :is-archived="channelState.archived"
      :is-popup-menu-shown="isPopupMenuShown"
      @click-more="togglePopupMenu"
      @click-outside="closePopupMenu"
    >
      <ChannelHeaderToolsMenu
        v-if="isPopupMenuShown"
        :class="$style.toolsMenu"
        :channel-id="channelId"
        :show-notification-setting-btn="!channelState.forced"
        :is-archived="channelState.archived"
        @click-item="closePopupMenu"
      />
    </ChannelHeaderToolsList>
  </div>
</template>

<script lang="ts" setup>
import useToggle from '/@/composables/utils/useToggle'
import type { ChannelId } from '/@/types/entity-ids'

import ChannelHeaderToolsList from './ChannelHeaderToolsList.vue'
import ChannelHeaderToolsMenu from './ChannelHeaderToolsMenu.vue'
import useChannelState from './composables/useChannelState'

const props = defineProps<{
  channelId: ChannelId
}>()

const {
  value: isPopupMenuShown,
  toggle: togglePopupMenu,
  close: closePopupMenu
} = useToggle(false)
const { channelState } = useChannelState(props)
</script>

<style lang="scss" module>
.tools {
  flex-shrink: 0;
}
.toolsMenu {
  position: absolute;
  right: 0;
  top: 100%;
  z-index: $z-index-header-tools;
  contain: content;
}
</style>
