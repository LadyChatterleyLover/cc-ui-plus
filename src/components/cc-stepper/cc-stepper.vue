<template>
  <view class="cc-stepper">
    <view
      v-if="showPlus"
      class="cc-stepper-button"
      :class="{
        disabled: Number(modelValue) <= minValue || disablePlus || disabled,
        'cc-stepper-button-round': round,
      }"
      :style="{
        width: buttonSize + 'rpx',
        height: buttonSize + 'rpx',
      }"
      @click="minus"
      >-</view
    >
    <input
      v-model="activeValue"
      class="cc-stepper-value"
      :class="{
        disabled: disabled || disableInput || disabled,
        'cc-stepper-value-round': round,
      }"
      :style="{
        width: inputWidth + 'rpx',
        height: buttonSize + 'rpx',
      }"
      type="number"
      @blur="onBlur"
      @focus="onFocus"
      @input="onInput"
    />
    <view
      v-if="showMinus"
      class="cc-stepper-button"
      :class="{
        disabled: Number(modelValue) >= maxValue || disableMinus || disabled,
        'cc-stepper-button-round': round,
      }"
      :style="{
        width: buttonSize + 'rpx',
        height: buttonSize + 'rpx',
      }"
      @click="plus"
      >+</view
    >
  </view>
</template>

<script setup lang="ts">
import { defineEmits, defineProps, ref, watch } from 'vue'
const props = withDefaults(
  defineProps<{
    modelValue: number | string
    min?: number | string
    max?: number | string
    step?: number | string
    buttonSize?: number | string
    inputWidth?: number | string
    disabled?: boolean
    disableInput?: boolean
    disablePlus?: boolean
    disableMinus?: boolean
    showPlus?: boolean
    showMinus?: boolean
    integer?: boolean
    round?: boolean
    decimalLength?: number | string
  }>(),
  {
    min: 1,
    max: '',
    step: 1,
    buttonSize: 56,
    inputWidth: 64,
    disabled: false,
    disableInput: false,
    disablePlus: false,
    disableMinus: false,
    showPlus: true,
    showMinus: true,
    integer: false,
    round: false,
    decimalLength: Number.POSITIVE_INFINITY,
  }
)

const emits = defineEmits([
  'update:modelValue',
  'minus',
  'plus',
  'change',
  'focus',
  'blur',
])
const activeValue = ref<number | string>(props.modelValue)
const minValue = ref<number>(0)
const maxValue = ref<number>(0)
if (props.min === undefined || !props.min)
  minValue.value = Number.NEGATIVE_INFINITY
else minValue.value = Number(props.min)
if (props.max === undefined || !props.max)
  maxValue.value = Number.POSITIVE_INFINITY
else maxValue.value = Number(props.max)
// 点击减少按钮
const minus = () => {
  if (props.disabled || props.disableMinus) {
    return
  }
  if (props.step) (activeValue.value as number) -= Number(props.step as number)
  else (activeValue.value as number)--
  emits('minus', activeValue.value)
  if (Number(activeValue.value) <= minValue.value) {
    emits('update:modelValue', minValue.value)
    return
  }
}
// 点击增加按钮
const plus = () => {
  if (props.disabled || props.disablePlus) {
    return
  }
  if (props.step) (activeValue.value as number) += Number(props.step as number)
  else (activeValue.value as number)++
  emits('plus', activeValue.value)
  if (Number(activeValue.value) >= maxValue.value)
    emits('update:modelValue', maxValue.value)
}
// 获取焦点事件
const onFocus = (e: FocusEvent) => {
  emits('focus', e)
}
// 失去焦点事件
const onBlur = (e: FocusEvent) => {
  if (Number(activeValue.value) <= minValue.value) {
    activeValue.value = minValue.value
  }
  if (Number(activeValue.value) >= maxValue.value) {
    activeValue.value = maxValue.value
  }
  emits('update:modelValue', Number(activeValue.value))
  emits('blur', e)
}
// 输入事件
const onInput = () => {
  if (props.disabled || props.disableInput) {
    return
  }
  if (props.integer) {
    const str = `${activeValue.value}`
    if (str.includes('.')) {
      const arr = str.split('')
      arr.splice(-1)
      const str2 = arr.join('')
      activeValue.value = +str2
    }
  }
  if (props.decimalLength && props.decimalLength !== Number.POSITIVE_INFINITY) {
    const num = String(activeValue.value).split('.')[0]
    const str = String(activeValue.value).split('.')[1]
    if (str && str.length > Number(props.decimalLength)) {
      activeValue.value = `${num}.${str.slice(
        0,
        Math.max(0, props.decimalLength as number)
      )}`
    }
  }
}
watch(
  () => activeValue.value,
  (val) => {
    emits('update:modelValue', val)
    emits('change', val)
  }
)
watch(
  () => props.modelValue,
  (val) => {
    activeValue.value = val
  }
)
</script>

<style scoped lang="scss">
.cc-stepper {
  display: flex;
  align-items: center;
  &-button {
    background: #f2f3f5;
    color: #323233;
    display: flex;
    align-items: center;
    justify-content: center;
    &-round {
      color: #ee0a24 !important;
      background-color: #fff !important;
      border: 2rpx solid #ee0a24 !important;
      border-radius: 100% !important;
    }
    &:first-child {
      border-radius: 8rpx 0 0 8rpx;
    }
    &:last-child {
      border-radius: 0 8rpx 8rpx 0;
    }
  }
  &-value {
    margin: 0 4rpx;
    color: #323233;
    font-size: 28rpx;
    text-align: center;
    vertical-align: middle;
    background-color: #f2f3f5;
    border: 0;
    border-width: 2rpx 0;
    border-radius: 0;
    &-round {
      background-color: transparent !important;
    }
  }
}
.disabled {
  color: #c8c9cc;
  background-color: #f7f8fa;
  cursor: not-allowed;
  pointer-events: none;
}
</style>
