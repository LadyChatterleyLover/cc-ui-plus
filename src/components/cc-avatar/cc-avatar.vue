<template>
  <view
    class="cc-avatar"
    :class="{
      'cc-avatar-circle': shape === 'circle',
    }"
    :style="style"
  >
    <view v-if="text">
      <text>{{ text }}</text>
    </view>
    <view v-else-if="icon" class="cc-avatar-icon">
      <cc-icon :type="icon" color="#fff" :size="Number(avatarSize) + 4" />
    </view>
    <image
      v-else
      :src="!isError ? src : defaultUrl"
      :mode="mode"
      class="cc-avatar-image"
      :class="{
        'cc-avatar-image-circle': shape === 'circle',
      }"
      @load="handleLoad"
      @error="handleError"
    />
  </view>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { useRandomColor } from '../../hooks/useRandomColor'
import type { CSSProperties } from 'vue'

const props = withDefaults(
  defineProps<{
    src?: string
    shape?: 'circle' | 'square'
    size?: string | number | 'large' | 'default' | 'small'
    mode?: string
    text?: string
    bgColor?: string
    color?: string
    fontSize?: string | number
    icon?: string
    defaultUrl?: string
    randomBgColor?: boolean
  }>(),
  {
    src: '',
    shape: 'circle',
    size: 80,
    mode: 'scaleToFill',
    text: '',
    bgColor: '',
    color: '#fff',
    fontSize: '32',
    icon: '',
    randomBgColor: false,
    defaultUrl:
      'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png',
  }
)
const emits = defineEmits<{
  (e: 'load', val: Event): void
  (e: 'error', val: Event): void
}>()

const randomColor = useRandomColor()
const isError = ref(false)

const avatarSize = computed(() => {
  if (props.size === 'large') {
    return 96
  } else if (props.size === 'default') {
    return 80
  } else if (props.size === 'small') {
    return 64
  } else {
    return props.size
  }
})

const style = computed<CSSProperties>(() => ({
  fontSize: props.fontSize,
  background: props.icon
    ? '#c0c4cc'
    : props.randomBgColor
    ? randomColor
    : props.text
    ? '#c0c4cc'
    : props.bgColor,
  width: `${avatarSize.value}rpx`,
  height: `${avatarSize.value}rpx`,
}))

const handleLoad = (e: Event) => {
  emits('load', e)
}

const handleError = (e: Event) => {
  isError.value = true
  emits('error', e)
  console.log('error')
}
</script>

<style scoped lang="scss">
.cc-avatar {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  &-image {
    width: 100%;
    height: 100%;
    &-circle {
      border-radius: 100%;
    }
  }
  &-circle {
    border-radius: 100%;
  }
}
</style>
