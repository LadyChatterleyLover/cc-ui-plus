<template>
  <view
    :class="{
      'cc-modal-mask-show': modelValue,
      'cc-modal-mask-hide': !modelValue && num > 0,
    }"
    class="cc-modal-mask"
    @click="clickMask"
  />
  <view
    :class="{
      'cc-modal-show': modelValue,
      'cc-modal-hide': !modelValue && num > 0,
    }"
    class="cc-modal"
    :style="{ width: width + 'rpx' }"
  >
    <view class="cc-modal-content">
      <view
        v-if="title"
        :class="{ 'cc-modal-content-nocontent': !content && !contentSlot }"
        class="cc-modal-content-title"
        >{{ title }}</view
      >
      <view
        v-if="content || contentSlot"
        :class="{ 'cc-modal-content-content-padding': !contentSlot }"
        class="cc-modal-content-content"
      >
        <slot v-if="contentSlot" name="content" />
        <view v-else>{{ content }}</view>
      </view>
      <view class="cc-modal-content-button">
        <slot v-if="btnSlot" name="button" />
        <template v-else>
          <view
            v-if="showCancelButton"
            class="cc-modal-content-button-cancel"
            :style="{ color: cancelColor }"
            @click="cancel"
            >{{ cancelText }}</view
          >
          <view
            v-if="showConfirmButton"
            class="cc-modal-content-button-confirm"
            :style="{ color: confirmColor }"
            @click="confirm"
          >
            <view v-if="loading" class="loading">
              <cc-icon type="spinner-cycle" size="16" color="#c8c9cc" />
            </view>
            <view v-else>{{ confirmText }}</view>
          </view>
        </template>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref, useSlots, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    // 弹框标题
    title?: string
    // 弹框内容
    content?: string
    // 弹框宽度
    width?: string | number
    // 是否展示确认按钮
    showConfirmButton?: boolean
    // 是否展示取消按钮
    showCancelButton?: boolean
    // 确认按钮文字
    confirmText?: string
    // 取消按钮文字
    cancelText?: string
    // 确认按钮颜色
    confirmColor?: string
    // 取消按钮颜色
    cancelColor?: string
    // 点击遮罩层是否关闭
    closeOnClickOverlay?: boolean
    // 是否异步关闭
    asyncClose?: boolean
  }>(),
  {
    modelValue: false,
    title: '',
    content: '',
    width: 640,
    showConfirmButton: true,
    showCancelButton: false,
    confirmText: '确认',
    cancelText: '取消',
    confirmColor: '#2979ff',
    cancelColor: '#000',
    closeOnClickOverlay: false,
    asyncClose: false,
  }
)

const emits = defineEmits(['update:modelValue', 'confirm', 'cancel'])

const loading = ref<boolean>(false)
// 内容插槽
const contentSlot = ref()
// 按钮插槽
const btnSlot = ref()
const num = ref(0)
const slots = useSlots()

// 确认事件
const confirm = () => {
  if (props.asyncClose) {
    loading.value = true
  } else {
    emits('update:modelValue', !props.modelValue)
    loading.value = false
  }
  emits('confirm')
}
// 取消事件
const cancel = () => {
  emits('update:modelValue', !props.modelValue)
  emits('cancel')
}
// 点击遮罩层
const clickMask = () => {
  if (props.closeOnClickOverlay) emits('update:modelValue', !props.modelValue)
}
// 关闭
const close = () => {
  emits('update:modelValue', !props.modelValue)
}
watch(
  () => props.modelValue,
  (val) => {
    if (val === false) loading.value = false
    if (val) {
      num.value++
      if (slots.content) {
        contentSlot.value = slots.content
      }
      if (slots.button) {
        btnSlot.value = slots.button
      }
    }
  }
)
defineExpose({
  close,
})
</script>

<style scoped lang="scss">
.cc-modal {
  position: fixed;
  top: 34%;
  left: 6%;
  right: 6%;
  overflow: hidden;
  font-size: 16px;
  background-color: #fff;
  border-radius: 32rpx;
  transform: translate3d(-50%, -50%, 0);
  min-width: 72rpx;
  min-height: 72rpx;
  opacity: 0;
  z-index: -1;
  &-show {
    animation: show 0.2s linear forwards;
    &.cc-modal {
      z-index: 999;
    }
  }
  &-hide {
    animation: hide 0.2s linear forwards;
    &.cc-modal {
      z-index: -1;
    }
  }
  &-mask {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: -1;
    background-color: rgba(0, 0, 0, 0.7);
    opacity: 0;
    &-show {
      animation: mask-show 0.2s linear forwards;
    }
    &-hide {
      animation: mask-hide 0.2s linear forwards;
    }
  }
  &-content {
    &-title {
      padding-top: 52rpx;
      font-weight: 500;
      line-height: 24rpx;
      text-align: center;
    }
    &-nocontent {
      padding-bottom: 52rpx;
    }
    &-content {
      white-space: pre-wrap;
      text-align: center;
      word-wrap: break-word;
      color: #646566;
      font-size: 14px;
      &-padding {
        padding: 34rpx;
      }
    }
    &-button {
      height: 96rpx;
      border: 1px solid #ebedf0;
      display: flex;
      align-items: center;
      justify-content: center;
      view {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
      }
      &-cancel {
        border-right: 1px solid #ebedf0;
      }
    }
  }
}
.loading {
  animation: loading 1.5s linear infinite;
}
@keyframes mask-show {
  from {
    opacity: 0;
    z-index: -1;
  }
  to {
    opacity: 1;
    z-index: 998;
  }
}
@keyframes mask-hide {
  from {
    opacity: 1;
    z-index: 998;
  }
  to {
    opacity: 0;
    z-index: -1;
  }
}
@keyframes show {
  from {
    transform: scale(0);
    z-index: -1;
    opacity: 0;
  }
  to {
    transform: scale(1);
    z-index: 999;
    opacity: 1;
  }
}
@keyframes hide {
  from {
    transform: scale(1);
    z-index: 999;
    opacity: 1;
  }
  to {
    transform: scale(0);
    z-index: -1;
    opacity: 0;
  }
}
@keyframes loading {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
