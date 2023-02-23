<template>
  <view class="cc-open-more">
    <view
      class="cc-open-more-content"
      :style="{ textIndent: textIndent, height: height }"
    >
      <slot />
    </view>
    <view class="cc-open-more-btn" @click="toggle">
      <view v-if="!flag" :style="{ color: color }">{{ closeText }}</view>
      <view v-if="flag && showToggle" :style="{ color: color }">{{
        openText
      }}</view>
      <view class="cc-open-more-btn-icon">
        <cc-icon :type="icon" :color="color" size="12" />
      </view>
    </view>
    <view v-if="mask" class="cc-open-more-mask" />
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const props = withDefaults(
  defineProps<{
    // 文字首行缩进
    textIndent?: string
    // 内容超出此高度才会显示展开全文按钮
    openHeight?: string | number
    // 文字颜色
    color?: string
    // 关闭时的提示文字
    closeText?: string
    // 展开时的提示文字
    openText?: string
    // 展开后是否显示收起按钮
    showToggle?: boolean
    // 显示遮罩层
    showMask?: boolean
  }>(),
  {
    textIndent: '2em',
    openHeight: 400,
    color: '#2979ff',
    closeText: '展开更多',
    openText: '收起',
    showToggle: true,
    showMask: true,
  }
)

const emits = defineEmits(['open', 'close'])

const height = ref<string>(`${props.openHeight}rpx`)
const mask = ref<boolean>(props.showMask)
const icon = ref<string>('arrowdown')
const flag = ref<boolean>(false)

const toggle = () => {
  flag.value = !flag.value
  mask.value = !mask.value
  height.value = height.value !== 'auto' ? 'auto' : `${props.openHeight}rpx`
  icon.value = icon.value === 'arrowdown' ? 'arrowup' : 'arrowdown'
  if (flag.value) emits('open')
  else emits('close')
}
</script>

<style lang="scss" scoped>
.cc-open-more {
  position: relative;
  &-content {
    font-size: 24rpx;
    overflow: hidden;
    background-image: linear-gradient(
      -180deg,
      rgba(255, 255, 255, 0) 0%,
      rgb(255, 255, 255) 80%
    );
    padding-top: 108rpx;
    margin-top: -108rpx;
  }
  &-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24rpx;
    margin-top: 20rpx;
    &-icon {
      margin-left: 6rpx;
    }
  }
  &-mask {
    position: absolute;
    background-image: linear-gradient(
      -180deg,
      rgba(255, 255, 255, 0) 0%,
      rgb(255, 255, 255) 80%
    );
    width: 100%;
    height: 100rpx;
    bottom: 32rpx;
  }
}
</style>
