<script setup lang='ts'>
import { computed, ref } from 'vue'
import { NModal, NTabPane, NTabs } from 'naive-ui'
import General from './General.vue'
import Advance from './Advance.vue'
import { SvgIcon } from '@/components/common'

const props = defineProps<Props>()

const emit = defineEmits<Emit>()

interface Props {
  visible: boolean
}

interface Emit {
  (e: 'update:visible', visible: boolean): void
}

const active = ref('Advance')

const reload = ref(false)

const show = computed({
  get() {
    return props.visible
  },
  set(visible: boolean) {
    emit('update:visible', visible)
  },
})

function handleReload() {
  reload.value = true
  setTimeout(() => {
    reload.value = false
  }, 0)
}
</script>

<template>
  <NModal v-model:show="show" :auto-focus="false" preset="card" style="width: 95%; max-width: 640px">
    <div>
      <NTabs v-model:value="active" type="line" animated>
        <NTabPane name="Advance" tab="Advance">
          <template #tab>
            <SvgIcon class="text-lg" icon="streamline:interface-setting-slider-horizontal-square-adjust-controls-fader-horizontal-settings-slider-square" />
            <span class="ml-2">{{ $t('setting.advance') }}</span>
          </template>
          <Advance />
        </NTabPane>
        <NTabPane name="General" tab="General">
          <template #tab>
            <SvgIcon class="text-lg" icon="tabler:user-cog" />
            <span class="ml-2">{{ $t('setting.general') }}</span>
          </template>
          <div class="min-h-[100px]">
            <General v-if="!reload" @update="handleReload" />
          </div>
        </NTabPane>
      </NTabs>
    </div>
  </NModal>
</template>
