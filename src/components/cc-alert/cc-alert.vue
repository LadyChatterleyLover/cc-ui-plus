<template>
  <view v-if="visible" class="cc-alert" :style="{ borderColor, background }">
    <view v-if="showIcon" class="cc-alert-icon"
      ><cc-icon :type="icon" :color="iconColor" size="14"
    /></view>
    <view class="cc-alert-content">
      <view v-if="title" class="cc-alert-content-title">{{ title }}</view>
      <view v-if="description" class="cc-alert-content-description">{{
        description
      }}</view>
    </view>
    <view v-if="closeable" class="cc-alert-close" @click="close"
      ><cc-icon type="closeempty" size="14" color="#c0c4cc"
    /></view>
  </view>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue?: boolean
    type?: 'primary' | 'success' | 'warning' | 'error' | 'info'
    title?: string
    description?: string
    showIcon?: boolean
    closeable?: boolean
  }>(),
  {
    modelValue: true,
    type: 'warning',
    title: '',
    description: '',
    showIcon: false,
    closeable: false,
  }
)

const emits = defineEmits<{
  (e: 'close'): void
  (e: 'update:modelValue', val: boolean): void
}>()

const iconList = [
  {
    type: 'primary',
    icon: 'checkbox',
  },
  {
    type: 'success',
    icon: 'info',
  },
  {
    type: 'error',
    icon: 'close',
  },
  {
    type: 'warning',
    icon: 'info',
  },
  {
    type: 'info',
    icon: 'info',
  },
]
const colorList = [
  {
    type: 'primary',
    borderColor: '#a0cfff',
    background: '#ecf5ff',
  },
  {
    type: 'success',
    borderColor: '#71d5a1',
    background: '#dbf1e1',
  },
  {
    type: 'error',
    borderColor: '#a0cfff',
    background: '#fef0f0',
  },
  {
    type: 'warning',
    borderColor: '#fcbd71',
    background: '#fdf6ec',
  },
  {
    type: 'info',
    borderColor: '#c8c9cc',
    background: '#f4f4f5',
  },
]
const iconColorList = [
  {
    type: 'primary',
    color: '#2979ff',
  },
  {
    type: 'success',
    color: '#19be6b',
  },
  {
    type: 'error',
    color: '#fa3534',
  },
  {
    type: 'warning',
    color: '#f90',
  },
  {
    type: 'info',
    color: '#909399',
  },
]
const visible = ref(false)

const close = () => {
  visible.value = false
  emits('close')
}

const icon = computed(
  () => iconList.find((item) => props.type === item.type)?.icon
)
const iconColor = computed(
  () => iconColorList.find((item) => props.type === item.type)?.color
)
const borderColor = computed(
  () => colorList.find((item) => props.type === item.type)?.borderColor
)
const background = computed(
  () => colorList.find((item) => props.type === item.type)?.background
)

watch(
  () => props.modelValue,
  (val) => {
    visible.value = val
  },
  { immediate: true }
)
watch(
  () => visible.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
</script>

<style scoped lang="scss">
.cc-alert {
  display: flex;
  align-items: center;
  position: relative;
  width: fit-content;
  border: 2rpx solid;
  padding: 12rpx 24rpx;
  border-radius: 4rpx;
  &-icon {
    margin-right: 12rpx;
  }
  &-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &-title {
      font-size: 26rpx;
      color: #303133;
      font-weight: 500;
    }
    &-description {
      font-size: 24rpx;
      text-align: left;
      color: #606266;
    }
  }
  &-close {
    position: absolute;
    top: 12rpx;
    right: 12rpx;
    z-index: 99;
  }
}
</style>
