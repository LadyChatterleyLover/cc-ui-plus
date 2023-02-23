<template>
  <view
    class="cc-link"
    :style="{
      fontSize: fontSize + 'rpx',
      color,
      borderBottom: underLine ? `2rpx solid ${lineColor}` : 'none',
    }"
    @click="goto"
  >
    <slot />
  </view>
  <cc-toast ref="toast" />
</template>

<script lang="ts" setup>
import { nextTick, ref } from 'vue'

const props = withDefaults(
  defineProps<{
    // 链接地址
    href: string
    // 文字颜色
    color?: string
    // 字体大小
    fontSize?: string | number
    // 是否显示下划线
    underLine?: boolean
    // 下划线颜色
    lineColor?: string
    // 小程序平台复制提示
    mpTips?: string
  }>(),
  {
    color: '#0081ff',
    fontSize: 28,
    underLine: true,
    lineColor: '#0081ff',
    mpTips: '链接已复制, 请在浏览器打开',
  }
)
const toast = ref()

const goto = () => {
  // #ifdef APP-PLUS
  plus.runtime.openURL(props.href)
  // #endif
  // #ifdef H5
  window.open(props.href)
  // #endif
  // #ifdef MP
  uni.setClipboardData({
    data: props.href,
    success: () => {
      uni.hideToast()
      nextTick(() => {
        toast.value.show({ title: props.mpTips })
      })
    },
  })
  // #endif
}
</script>

<style lang="scss" scoped>
.cc-link {
  width: fit-content;
}
</style>
