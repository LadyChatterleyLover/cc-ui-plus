<template>
  <view class="cc-switch">
    <view
      class="cc-switch-wrap"
      :class="{ disabled }"
      :style="{
        background: modelValue ? activeColor : inactiveColor,
        fontSize: size + 'rpx',
      }"
      @click="clickSwitch"
    >
      <view
        class="cc-switch-wrap-node"
        :style="{ transform: `translateX(${move})` }"
      />
    </view>
    <view
      v-if="activeText"
      class="cc-switch-text"
      :style="{ color: modelValue ? activeTextColor : inactiveTextColor }"
      >{{
        modelValue ? activeText : inactiveText ? inactiveText : activeText
      }}</view
    >
  </view>
</template>

<script setup lang="ts">
import { defineEmits, defineProps, ref, watch } from 'vue'
const props = withDefaults(
  defineProps<{
    modelValue: string | number | boolean
    text?: string
    activeColor?: string
    inactiveColor?: string
    activeTextColor?: string
    inactiveTextColor?: string
    activeText?: string
    inactiveText?: string
    disabled?: boolean
    size?: string | number
  }>(),
  {
    text: '',
    activeColor: '#0081ff',
    inactiveColor: '#fff',
    activeTextColor: '#0081ff',
    inactiveTextColor: '#303133',
    activeText: '',
    inactiveText: '',
    disabled: false,
    size: 48,
  }
)

const emits = defineEmits(['update:modelValue', 'change'])
// 移动距离
const move = ref<string>('')
const checked = ref<boolean>(false)
const clickSwitch = () => {
  emits('update:modelValue', !props.modelValue)
}
watch(
  () => props.modelValue,
  (val) => {
    checked.value = val as boolean
    move.value = props.modelValue ? '1em' : '0'
  },
  { immediate: true }
)
watch(
  () => props.modelValue,
  (val) => {
    emits('change', val)
  }
)
</script>

<style scoped lang="scss">
.cc-switch {
  display: flex;
  align-items: center;
  .cc-switch-wrap {
    position: relative;
    display: flex;
    box-sizing: content-box;
    align-items: center;
    width: 2em;
    height: 1em;
    background-color: #fff;
    border: 2rpx solid rgba(0, 0, 0, 0.1);
    border-radius: 1em;
    transition: background-color 0.3s;
    &-node {
      position: absolute;
      top: 0;
      left: 0;
      width: 1em;
      height: 1em;
      background-color: #fff;
      border-radius: 100%;
      box-shadow: 0 6rpx 2rpx 0 rgb(0 0 0 / 5%), 0 4rpx 4rpx 0 rgb(0 0 0 / 10%),
        0 6rpx 6rpx 0 rgb(0 0 0 / 5%);
      transition: transform 0.3s;
    }
  }
  &-text {
    font-size: 32rpx;
    margin-left: 12rpx;
  }
}
.disabled {
  cursor: not-allowed;
  opacity: 0.5;
  pointer-events: none;
}
</style>
