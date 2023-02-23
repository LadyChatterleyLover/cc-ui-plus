<template>
  <view class="cc-radio" @click="change">
    <slot v-if="$slots.icon" name="icon" :checked="checked" />
    <view
      v-else
      class="cc-radio-icon"
      :class="[
        {
          'cc-radio-icon-round': shape === 'round',
        },
      ]"
      :style="{
        width: computedIconSize + 'rpx',
        height: computedIconSize + 'rpx',
        background: computedDisabled
          ? '#ebedf0'
          : checked
          ? computedCheckedColor
          : '#fff',
        border: computedDisabled
          ? '#c8c9cc'
          : checked
          ? `1px solid ${computedCheckedColor}`
          : '1px solid #c8c9cc',
      }"
      @click.stop="clickIcon"
    >
      <cc-icon
        v-if="checked"
        type="checkmarkempty"
        :size="computedIconSize"
        :color="computedDisabled ? '#c8c9cc' : '#fff'"
      />
    </view>
    <view
      class="cc-radio-label"
      :style="{
        color: computedDisabled ? '#c8c9cc' : '#323233',
      }"
    >
      <slot />
    </view>
  </view>
</template>

<script lang="ts" setup>
import { computed, getCurrentInstance, ref, watch } from 'vue'

const instance = getCurrentInstance()
let parent: any = null
/* #ifdef H5 */
parent = instance!.parent?.parent
/* #endif */
/* #ifndef H5 */
parent = instance!.parent
/* #endif */

const props = withDefaults(
  defineProps<{
    modelValue?: boolean
    shape?: 'round' | 'square'
    name: any
    disabled?: boolean
    labelDisabled?: boolean
    labelPosition?: 'left' | 'right'
    iconSize?: number | string
    checkedColor?: string
  }>(),
  {
    modelValue: false,
    shape: 'round',
    disabled: false,
    labelDisabled: false,
    labelPosition: 'left',
    iconSize: 40,
    checkedColor: '#1989fa',
  }
)
const emits = defineEmits(['update:modelValue'])

const groupProps = parent.props

const checked = ref(false)

const change = () => {
  if (props.labelDisabled || props.disabled) {
    return
  }
  parent.exposed.setChecked(props.name)
}

const clickIcon = () => {
  if (props.disabled) {
    return
  }
  parent.exposed.setChecked(props.name)
}

const computedIconSize = computed(() => {
  if (groupProps && groupProps.iconSize && !props.iconSize) {
    return groupProps.iconSize
  } else {
    return props.iconSize
  }
})

const computedDisabled = computed(() => {
  if (groupProps && groupProps.disabled && !props.disabled) {
    return groupProps.disabled
  } else {
    return props.disabled
  }
})

const computedCheckedColor = computed(() => {
  if (groupProps && groupProps.checkedColor && !props.checkedColor) {
    return groupProps.checkedColor
  } else {
    return props.checkedColor
  }
})

watch(
  () => props.modelValue,
  (val) => {
    checked.value = val
  },
  { immediate: true }
)

watch(
  () => props.name,
  (val) => {
    if (val) {
      parent.exposed.addChildName(val)
    }
  },
  { immediate: true }
)

watch(
  () => groupProps,
  (val) => {
    if (val) {
      const value = val.modelValue
      checked.value = props.name === value
    }
  },
  { immediate: true, deep: true }
)
</script>

<style lang="scss" scoped>
.cc-radio {
  display: flex;
  align-items: center;
  overflow: hidden;
  user-select: none;
  width: fit-content;
  box-sizing: border-box;
  margin-bottom: 16rpx;
  margin-right: 40rpx;
  &-icon {
    box-sizing: border-box;
    display: flex;
    align-items: center;
    justify-content: center;
    &-round {
      border-radius: 100%;
    }
  }
  &-label {
    margin-left: 16rpx;
    line-height: 40rpx;
  }
}
</style>
