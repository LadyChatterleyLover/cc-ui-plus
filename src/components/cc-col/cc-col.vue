<template>
  <view class="cc-col" :style="style" @click.stop="handleClick">
    <slot />
  </view>
</template>

<script setup lang="ts">
import { computed, inject, onMounted, ref } from 'vue'
import type { CSSProperties, ComputedRef } from 'vue'

const props = withDefaults(
  defineProps<{
    span?: number | string
    offset?: number | string
  }>(),
  {
    span: '',
    offset: '',
  }
)
const emits = defineEmits(['click'])

const gutter = inject('gutter')
const screenWidth = ref(0)

// 计算宽度
const width = computed(() => {
  return ((props.span as number) / 24) * 100 === 0
    ? '100%'
    : `${((props.span as number) / 24) * screenWidth.value * 2}rpx`
})
// 计算传了gutter的padding值
const padding = computed(() => {
  if (gutter) {
    return `${gutter as number}rpx`
  }
  return 0
})
// 计算偏移量
const offset = computed(() => {
  if (props.offset) {
    return `${((props.offset as number) / 24) * screenWidth.value * 2}rpx`
  }
  return 0
})

const style: ComputedRef<CSSProperties> = computed(() => {
  return {
    width: width.value,
    marginLeft: offset.value,
    paddingLeft: padding.value,
    paddingRight: padding.value,
    boxSizing: 'border-box',
  }
})

const handleClick = (e) => {
  emits('click', e)
}

onMounted(() => {
  const res = uni.getSystemInfoSync()
  screenWidth.value = res.screenWidth
})
</script>

<style scoped lang="scss">
.cc-col {
  width: 100%;
}
</style>
