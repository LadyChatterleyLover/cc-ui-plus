<template>
  <view class="cc-search">
    <view class="cc-search-content" :class="{ 'cc-search-round': round }">
      <view class="cc-search-content-label">
        <view v-if="$slots.label">
          <slot name="label" />
        </view>
        <view v-else>{{ label }}</view>
      </view>
      <view class="cc-search-content-cell">
        <view
          class="cc-search-content-icon"
          :class="{
            'cc-search-content-icon-align-center': iconAlign === 'center',
            'cc-search-content-icon-align-right': iconAlign === 'right',
          }"
        >
          <view v-if="$slots.icon">
            <slot name="icon" />
          </view>
          <view v-else>
            <cc-icon v-if="icon" :type="icon" color="#323233" size="20" />
            <cc-icon v-else type="search" color="#323233" size="20" />
          </view>
        </view>
        <view
          class="cc-search-content-input"
          :class="{
            'cc-search-content-text-align-center': textAlign === 'center',
            'cc-search-content-text-align-right': textAlign === 'right',
          }"
        >
          <input
            class="cc-search-content-input-wrap"
            placeholder-class="cc-search-content-input-placeholder"
            type="text"
            :placeholder="placeholder"
            placeholder-style="font-size: 14px"
            :value="value"
            :disabled="disabled"
            :maxlength="maxlength"
            @input="handleInput"
            @focus="handleFocus"
            @blur="handleBlur"
            @confirm="handleConfirm"
          />
          <view
            v-if="value || !clearable"
            class="cc-search-content-input-close"
            @click="handleClear"
            @mousedown="mousedown"
          >
            <cc-icon color="rgb(96, 98, 102)" type="close" size="14" />
          </view>
        </view>
      </view>
    </view>
    <view v-if="showAction" class="cc-search-action" @click="cancel">
      <view v-if="$slots.action">
        <slot name="action" />
      </view>
      <view v-else>{{ actionText }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

interface Value {
  detail: {
    value: string
  }
}

const props = withDefaults(
  defineProps<{
    modelValue: string
    label?: string
    icon?: string
    actionText?: string
    placeholder?: string
    disabled?: boolean
    showAction?: boolean
    clearable?: boolean
    round?: boolean
    maxlength?: number
    textAlign?: 'left' | 'center' | 'right'
    iconAlign?: 'left' | 'center' | 'right'
  }>(),
  {
    label: '',
    icon: '',
    actionText: '取消',
    placeholder: '请搜索',
    disabled: false,
    showAction: true,
    round: false,
    clearable: true,
    maxlength: -1,
    textAlign: 'left',
    iconAlign: 'left',
  }
)

const emits = defineEmits<{
  (e: 'update:modelValue', val: string): void
  (e: 'input', val: string): void
  (e: 'change', val: string): void
  (e: 'focus', val: Value): void
  (e: 'blur', val: Value): void
  (e: 'confirm', val: Value): void
  (e: 'clear', val: Event): void
  (e: 'cancel', val: Event): void
}>()

const value = ref('')

const handleInput = (e: any) => {
  emits('input', e.detail.value)
  emits('update:modelValue', e.detail.value)
}

const handleFocus = (e: Value) => {
  emits('focus', e)
}
const handleBlur = (e: Value) => {
  emits('blur', e)
}
const handleConfirm = (e: Value) => {
  emits('confirm', e)
}
const handleClear = (e: Event) => {
  emits('clear', e)
}
const cancel = (e: Event) => {
  if (!props.disabled) {
    emits('cancel', e)
  }
}
const mousedown = (e: Event) => {
  e.preventDefault()
}

watch(
  () => props.modelValue,
  (val) => {
    value.value = val
  },
  { immediate: true }
)
watch(
  () => value.value,
  (val) => {
    emits('change', val)
  }
)
</script>

<style scoped lang="scss">
.cc-search {
  display: flex;
  align-items: center;
  padding: 20rpx 24rpx;
  background: #fff;
  input {
    outline: none;
    border: none;
  }
  &-round {
    border-radius: 48rpx !important;
  }
  &-content {
    position: relative;
    display: flex;
    flex: 1;
    padding-left: 48rpx;
    background-color: #f7f8fa;
    border-radius: 4rpx;
    &-text-align-center {
      justify-content: center;
      text-align: center;
    }
    &-text-align-right {
      justify-content: right;
      text-align: right;
    }
    &-icon-align-center {
      position: absolute;
      left: 38%;
    }
    &-icon-align-right {
      position: absolute;
      right: 20%;
    }
    &-label {
      font-size: 14px;
      display: flex;
      align-items: center;
      color: #323233;
      padding: 0 10rpx;
    }
    &-cell {
      flex: 1;
      display: flex;
      align-items: center;
      padding: 10rpx 16rpx 10rpx 0;
    }
    &-input {
      position: relative;
      flex: 1;
      margin-left: 12rpx;
      display: flex;
      align-items: center;
      &-wrap {
        flex: 1;
      }
      &-close {
        width: 48rpx;
        z-index: 999;
        display: flex;
        justify-content: center;
        align-items: center;
      }
    }
    &-input-placeholder {
      color: #969799;
    }
  }
  &-action {
    width: 66rpx;
    margin-left: 12rpx;
    font-size: 14px;
  }
}
</style>
