<template>
  <view class="cc-rate">
    <view
      v-for="item in activeCount"
      :key="item"
      :style="{ marginRight: gutter + 'rpx' }"
      class="cc-rate-active"
      @click="clickActive(item)"
    >
      <cc-icon
        :class="{ disabled }"
        :type="activeIcon"
        :color="disabled ? '#c8c9cc' : activeColor"
        :size="size"
      />
    </view>
    <view
      v-for="item in count - activeCount"
      :key="item"
      :style="{ marginRight: gutter + 'rpx' }"
      class="cc-rate-inactive"
      @click="clickInActive(item)"
    >
      <cc-icon
        :type="inactiveIcon"
        :color="disabled ? '#c8c9cc' : inactiveColor"
        :size="size"
      />
    </view>
  </view>
</template>

<script setup lang="ts">
import { defineEmits, defineProps, onMounted, ref, watch } from 'vue'
const props = withDefaults(
  defineProps<{
    modelValue: number
    count?: number
    minCount?: number
    size?: string | number
    activeIcon?: string
    inactiveIcon?: string
    activeColor?: string
    inactiveColor?: string
    gutter?: number | string
    disabled?: boolean
    readonly?: boolean
    allowHalf?: boolean
  }>(),
  {
    count: 5,
    minCount: 0,
    size: 16,
    activeIcon: 'star-filled',
    inactiveIcon: 'star',
    activeColor: '#ffd21e',
    inactiveColor: '#c8c9cc',
    gutter: 8,
    disabled: false,
    readonly: false,
    allowHalf: false,
  }
)

const emits = defineEmits(['change'])
const activeCount = ref<number>(props.modelValue)

onMounted(() => {
  if (props.minCount && props.modelValue <= props.minCount)
    activeCount.value = props.minCount
})
const clickActive = (item: number) => {
  if (props.disabled || props.readonly) return
  if (activeCount.value === 1) {
    if (activeCount.value == 1) {
      activeCount.value = 0
    } else {
      activeCount.value = 1
    }
  } else {
    activeCount.value = item
  }
  if (props.minCount && activeCount.value < props.minCount)
    activeCount.value = props.minCount
}
const clickInActive = (item: number) => {
  if (props.disabled || props.readonly) return
  activeCount.value += item
  if (activeCount.value === props.count) activeCount.value = props.count
}
watch(
  () => activeCount.value,
  (val) => {
    emits('change', val)
  }
)
</script>

<style scoped lang="scss">
.cc-rate {
  display: flex;
  align-items: center;
}
</style>
