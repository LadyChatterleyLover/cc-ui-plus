<template>
  <view>
    <cc-popup
      v-model="visible"
      :close-on-click-overlay="closeOnClickOverlay"
      :close-icon="closeIcon"
      closeable
      width="500"
      @close="close"
    >
      <view class="cc-rotate-verify">
        <view
          class="cc-rotate-verify-title"
          :style="{ color: titleColor, fontSize: titleSize + 'rpx' }"
          >{{ title }}</view
        >
        <view
          class="cc-rotate-verify-desc"
          :style="{ color: descColor, fontSize: descSize }"
          >{{ desc }}</view
        >
        <view
          class="cc-rotate-verify-img"
          :style="{ transform: `rotate(${rotate}deg)`, transitionDuration }"
          ><image :src="src" class="cc-rotate-verify-img-image"
        /></view>
        <view
          class="cc-rotate-verify-bar"
          :style="{
            width: barWidth + 'rpx',
            height: barHeight + 'rpx',
            background: barBgColor,
          }"
        >
          <view
            class="cc-rotate-verify-bar-wrap"
            :style="{
              background: barColor,
              width: barHeight + 'rpx',
              height: barHeight + 'rpx',
              transitionDuration,
              transform: `translateX(${translateX}px)`,
            }"
            @touchstart="touchstart"
            @touchmove="touchmove"
            @touchend="touchend"
          >
            <view class="cc-rotate-verify-bar-wrap-arrow"
              ><cc-icon type="arrowright" size="14" color="#fff"
            /></view>
            <view class="cc-rotate-verify-bar-wrap-arrow"
              ><cc-icon type="arrowright" :size="14" color="#fff"
            /></view>
          </view>
        </view>
      </view>
    </cc-popup>
    <cc-toast ref="toast" />
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    src: string
    title?: string
    titleSize?: string | number
    descSize?: string | number
    titleColor?: string
    descColor?: string
    desc?: string
    bgColor?: string
    closeIcon?: string
    closeOnClickOverlay?: boolean
    barWidth?: number | string
    barHeight?: number | string
    barBgColor?: string
    barColor?: string
    angle?: number
    errorRange?: number
    successMsg?: string
    errMsg?: string
    duration?: string | number
  }>(),
  {
    title: '安全验证',
    titleSize: 28,
    descSize: 32,
    titleColor: '#999',
    descColor: '#333',
    bgColor: '#fff',
    closeIcon: 'closeempty',
    desc: '拖动滑块使图片角度为正',
    closeOnClickOverlay: true,
    barWidth: 540,
    barHeight: 90,
    barBgColor: 'rgba(86,119,252,.1)',
    barColor: '#5677fc',
    errorRange: 5,
    successMsg: '验证成功',
    errMsg: '验证失败, 请重新验证',
    duration: 2000,
    angle: 30,
  }
)

const emits = defineEmits<{
  (e: 'close'): void
  (e: 'success'): void
  (e: 'update:modelValue', val: boolean): void
}>()

const toast = ref()
const visible = ref(false)
const rotate = ref(0)
const startX = ref(0)
const translateX = ref(0)
const transitionDuration = ref('0s')
const status = ref<'none' | 'done'>('none')

const close = () => {
  emits('close')
}

const touchstart = (e: TouchEvent) => {
  startX.value = e.changedTouches[0].clientX
  transitionDuration.value = '0s'
}

const touchmove = (e: TouchEvent) => {
  const move = e.changedTouches[0].clientX
  if (startX.value - move > 0) return
  const dis =
    uni.upx2px(Number(props.barWidth)) - uni.upx2px(Number(props.barHeight))
  translateX.value =
    -Math.floor(startX.value - move) >= dis
      ? dis
      : -Math.floor(startX.value - move)
  rotate.value = Math.floor(
    360 *
      (translateX.value /
        (uni.upx2px(Number(props.barWidth)) - Number(props.barHeight))) +
      props.angle!
  )
}

const touchend = () => {
  const dis =
    uni.upx2px(Number(props.barWidth)) - uni.upx2px(Number(props.barHeight))
  if (translateX.value < dis) {
    transitionDuration.value = '0.6s'
    translateX.value = 0
  } else {
    translateX.value = dis
  }
  if (
    rotate.value >= 360 - props.errorRange &&
    rotate.value <= 360 + props.errorRange
  ) {
    status.value = 'done'
    toast.value?.show({
      type: 'success',
      title: props.successMsg,
    })
    setTimeout(() => {
      visible.value = false
    }, Number(props.duration))
    setTimeout(() => {
      rotate.value = props.angle as number
    }, Number(Number(props.duration) + 500))
  } else {
    toast.value?.show({
      type: 'error',
      title: props.errMsg,
    })
    transitionDuration.value = '0.6s'
    status.value = 'none'
    rotate.value = props.angle as number
  }
}

watch(
  () => props.modelValue,
  (val) => {
    visible.value = val
  },
  { immediate: true }
)
watch(
  () => props.angle,
  (val) => {
    rotate.value = val as number
  },
  { immediate: true }
)
watch(
  () => visible.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
watch(
  () => status.value,
  (val) => {
    if (val === 'done') emits('success')
  }
)
</script>

<style scoped lang="scss">
.cc-rotate-verify {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &-title {
    margin-bottom: 20rpx;
  }
  &-img {
    width: 200rpx;
    height: 200rpx;
    margin: 30rpx 0;
    transition: all 0.6s;
    &-image {
      width: 100%;
      height: 100%;
      border-radius: 100%;
    }
  }
  &-bar {
    border-radius: 48rpx;
    &-wrap {
      border-radius: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.6s;
      &-arrow {
        &:first-child {
          position: relative;
          left: 6rpx;
        }
      }
    }
  }
}
</style>
