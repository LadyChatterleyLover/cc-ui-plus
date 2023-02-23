<template>
  <view
    class="cc-row"
    :style="{
      'margin-left': margin,
      'margin-right': margin,
      'justify-content': justifyValue,
    }"
    @click="handleClick"
  >
    <slot />
  </view>
</template>

<script setup lang="ts">
import { computed, provide } from 'vue'
import type { JustifyProps } from '../../types'

const props = withDefaults(
  defineProps<{
    gutter?: number | string
    justify?: JustifyProps
  }>(),
  {
    gutter: '',
    justify: '',
  }
)

const emits = defineEmits(['click'])

const margin = computed(() => {
  if (props.gutter) {
    return `${props.gutter as number}rpx`
  }
  return 0
})

const justifyValue = computed(() => {
  if (!props.justify) return 'flex-start'
  else if (props.justify === 'end') return 'flex-end'
  else return props.justify
})

provide('gutter', props.gutter!)

const handleClick = (e) => {
  emits('click', e)
}
</script>

<style scoped lang="scss">
.cc-row {
  display: flex;
  height: 100%;
  box-sizing: border-box;
}
</style>
