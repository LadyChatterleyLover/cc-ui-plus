<template>
  <movable-area class="cc-fab-movable-area">
    <movable-view
      class="cc-fab-movable-view"
      :direction="draggable ? 'all' : 'none'"
      :style="{ ...computedStyle }"
    >
      <view class="cc-fab">
        <slot>
          <cc-icon :type="icon" :color="iconColor" :size="iconSize" />
        </slot>
      </view>
    </movable-view>
  </movable-area>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import type { CSSProperties } from 'vue'

const props = withDefaults(
  defineProps<{
    right?: string | number
    bottom?: string | number
    width?: string | number
    height?: string | number
    bgColor?: string
    fontSize?: string | number
    icon?: string
    iconColor?: string
    iconSize?: string | number
    draggable?: boolean
  }>(),
  {
    right: 40,
    bottom: 60,
    width: 80,
    height: 80,
    bgColor: '#1989fa',
    fontSize: 28,
    icon: 'notification',
    iconColor: '#fff',
    iconSize: '20',
    draggable: false,
  }
)

const screenWidth = ref(0)
const screenHeight = ref(0)

uni.getSystemInfo({
  success: (res) => {
    screenWidth.value = res.screenWidth
    screenHeight.value = res.screenHeight - res.windowTop
  },
})

const top = computed(
  () =>
    screenHeight.value -
    uni.upx2px(Number(props.bottom)) -
    uni.upx2px(Number(props.height))
)
const left = computed(
  () =>
    screenWidth.value -
    uni.upx2px(Number(props.right)) -
    uni.upx2px(Number(props.width))
)

const computedStyle = computed<CSSProperties>(() => ({
  left: `${left.value}px`,
  top: `${top.value}px`,
  width: `${props.width}rpx`,
  height: `${props.height}rpx`,
  fontSize: `${props.fontSize}rpx`,
  background: props.bgColor,
}))
</script>

<style scoped lang="scss">
.cc-fab {
  &-movable-area {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 2023;
  }
  &-movable-view {
    pointer-events: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 100%;
    color: #fff;
  }
}
</style>
