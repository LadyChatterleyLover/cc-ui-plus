<template>
  <view
    class="cc-field cc-filed-cell-left-icon"
    :class="{ 'cc-field-start': !$slots.rightIcon }"
  >
    <cc-cell :border="border" :icon="leftIcon">
      <template #title>
        <view
          v-if="label"
          class="cc-field-label"
          :class="{ 'cc-field-label-disabled': disabled }"
          @click="clickLeft"
        >
          <view v-if="required" class="cc-field-label-required">*</view>
          <view>{{ label }}</view>
        </view>
      </template>
      <template #value>
        <view style="width: 100%; display: flex; align-items: center">
          <textarea
            v-if="type === 'textarea'"
            v-model="inputValue"
            class="cc-field-input"
            :style="{ height: 24 * Number(rows) + 'px' }"
            :maxlength="maxlength"
          />
          <input
            v-else
            v-model="inputValue"
            class="cc-field-input"
            :class="{
              'cc-field-input-disabled': disabled,
              'cc-field-input-error': error,
            }"
            placeholder-style="font-size: 14px"
            :placeholder="placeholder"
            :type="type"
            :readonly="readonly"
            :disabled="disabled"
            :maxlength="Number(maxlength)"
            @input="handleInput"
            @change="handleChange"
            @focus="handleFocus"
            @blur="handleBlur"
            @click="handleClick"
          />
          <view v-if="$slots.button" style="width: 100%"
            ><slot name="button"
          /></view>
        </view>
      </template>
      <template #right-icon>
        <view style="z-index: 999">
          <cc-icon
            v-if="inputValue"
            type="close"
            size="16"
            color="#969799"
            @click="handleClear"
          />
          <cc-icon
            :type="rightIcon"
            size="16"
            color="#969799"
            @click="clickRight"
          />
        </view>
      </template>
    </cc-cell>
    <view class="cc-field-error-message">{{ errorMessage }}</view>
    <view v-if="maxlength && showWordLimit" class="cc-field-word-limit"
      >{{ inputValue.length }} / {{ maxlength }}</view
    >
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: string
    label?: string
    placeholder?: string
    border?: boolean
    clearable?: boolean
    readonly?: boolean
    disabled?: boolean
    required?: boolean
    error?: boolean
    showWordLimit?: boolean
    type?: string
    leftIcon?: string
    rightIcon?: string
    errorMessage?: string
    maxlength?: number
    rows?: number | string
  }>(),
  {
    label: '',
    placeholder: '请输入',
    border: true,
    clearable: false,
    readonly: false,
    disabled: false,
    required: false,
    error: false,
    showWordLimit: false,
    type: 'text',
    leftIcon: '',
    rightIcon: '',
    errorMessage: '',
    maxlength: -1,
    rows: 1,
  }
)

const emits = defineEmits<{
  (e: 'update:modelValue', val: string): void
  (e: 'change', val: string): void
  (e: 'input', val: string): void
  (e: 'clear', val: Event): void
  (e: 'focus', val: Event): void
  (e: 'blur', val: Event): void
  (e: 'click', val: Event): void
  (e: 'clickLeft', val: Event): void
  (e: 'clickRight', val: Event): void
}>()

const inputValue = ref('')

const handleClear = (e: Event) => {
  inputValue.value = ''
  emits('clear', e)
}

const handleInput = () => {
  emits('input', inputValue.value)
}

const handleChange = () => {
  emits('change', inputValue.value)
}

const handleFocus = (e: Event) => {
  emits('focus', e)
}

const handleBlur = (e: Event) => {
  emits('blur', e)
}

const handleClick = (e: Event) => {
  emits('click', e)
}

const clickLeft = (e: Event) => {
  emits('clickLeft', e)
}

const clickRight = (e: Event) => {
  emits('clickRight', e)
}

watch(
  () => props.modelValue,
  (val) => {
    inputValue.value = val
  },
  { immediate: true }
)

watch(
  () => inputValue.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
</script>

<style scoped lang="scss">
.cc-field {
  position: relative;
  width: 100%;
  textarea {
    position: relative;
    top: 8rpx;
  }
  &-icon {
    position: relative;
    top: 4rpx;
  }
  &-error-message {
    position: absolute;
    color: #ee0a24;
    font-size: 12px;
    bottom: -40rpx;
    left: 164rpx;
  }
  &-word-limit {
    position: absolute;
    color: #646566;
    font-size: 12px;
    bottom: -36rpx;
    right: 0;
  }
  &-start {
    .cc-cell-value {
      justify-content: flex-start;
    }
  }
  &-label {
    min-width: 108rpx;
    margin-right: 24rpx;
    position: relative;
    top: 4rpx;
    &-disabled {
      color: #c8c9cc;
    }
    &-required {
      position: absolute;
      left: -16rpx;
      top: -4rpx;
      color: #ee0a24;
      font-size: 14px;
    }
  }
  &-input {
    display: block;
    box-sizing: border-box;
    width: 100%;
    margin: 0;
    padding: 0;
    color: #323233;
    line-height: inherit;
    text-align: left;
    resize: none;
    outline: none;
    &-error {
      color: #ee0a24;
    }
    &-disabled {
      color: #c8c9cc;
      cursor: not-allowed;
      opacity: 1;
    }
  }
}
</style>
