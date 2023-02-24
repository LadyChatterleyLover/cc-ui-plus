<template>
  <view
    class="cc-slide-verify"
    :style="{
      width: slideWidth + 'rpx',
      height: slideHeight + 'rpx',
      fontSize,
      background: bgColor,
    }"
  >
    <view
      class="cc-slide-verify-content"
      :style="{
        background: activeBgColor,
        width: activeBarWidth + 'px',
        transitionDuration,
      }"
    >
      <view
        class="cc-slide-verify-content"
        :style="{
          width: slideWidth + 'rpx',
          height: slideHeight + 'rpx',
          fontSize: fontSize + 'rpx',
        }"
        >{{ tip }}</view
      >
    </view>
    <view
      class="cc-slide-verify-drag"
      :style="{
        transitionDuration,
        transform: `translateX(${activeBarWidth}px)`,
      }"
      @touchstart="touchstart"
      @touchmove="touchmove"
      @touchend="touchend"
    >
      <template v-if="status === 'none'">
        <view class="cc-slide-verify-drag-arrow"
          ><cc-icon type="arrowright" :size="iconSize" :color="iconColor"
        /></view>
        <view class="cc-slide-verify-drag-arrow"
          ><cc-icon type="arrowright" :size="iconSize" :color="iconColor"
        /></view>
      </template>
      <view v-else class="cc-slide-verify-drag-check"
        ><cc-icon :type="succeccIcon" :size="iconSize" :color="activeIconColor"
      /></view>
    </view>
    <view>{{ tip }}</view>
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    tip?: string
    slideWidth?: string | number
    slideHeight?: string | number
    iconSize?: string | number
    fontSize?: string | number
    bgColor?: string
    activeBgColor?: string
    iconColor?: string
    activeIconColor?: string
    succeccIcon?: string
  }>(),
  {
    tip: '拖动滑块验证',
    slideWidth: 600,
    slideHeight: 60,
    bgColor: '#E9E9E9',
    activeBgColor: '#19be6b',
    iconColor: '#cbcbcb',
    activeIconColor: '#19be6b',
    iconSize: '14',
    fontSize: '28',
    succeccIcon: 'checkbox',
  }
)

const emits = defineEmits<{
  (e: 'success'): void
}>()

const activeBarWidth = ref(0)
const startX = ref(0)
const transitionDuration = ref('0s')
const status = ref<'none' | 'done'>('none')

const touchstart = (e: TouchEvent) => {
  startX.value = e.changedTouches[0].clientX
  transitionDuration.value = '0s'
}

const touchmove = (e: TouchEvent) => {
  const move = e.changedTouches[0].clientX
  if (startX.value - move > 0) return
  const dis = uni.upx2px(Number(props.slideWidth)) - uni.upx2px(80)
  activeBarWidth.value =
    -Math.floor(startX.value - move) >= dis
      ? dis
      : -Math.floor(startX.value - move)
}

const touchend = () => {
  const dis = uni.upx2px(Number(props.slideWidth)) - uni.upx2px(80)
  if (activeBarWidth.value < dis) {
    transitionDuration.value = '0.6s'
    activeBarWidth.value = 0
  } else {
    activeBarWidth.value = dis
    status.value = 'done'
    return
  }
}

watch(
  () => status.value,
  (val) => {
    if (val === 'done') emits('success')
  }
)
</script>

<style scoped lang="scss">
.cc-slide-verify {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  &-drag {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    width: 80rpx;
    background: #fff;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 2;
    transition: all;
    &-arrow {
      &:first-child {
        position: relative;
        left: 6rpx;
      }
    }
  }
  &-content {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
    overflow: hidden;
    transition: all;
    color: #fff;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1;
  }
}
</style>
