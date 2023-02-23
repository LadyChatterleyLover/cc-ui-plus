<template>
  <view
    :id="`cc-slider-${id}`"
    class="cc-slider"
    :style="{ background: inactiveColor, height: height + 'rpx' }"
    @click="clickSlider"
  >
    <view
      class="cc-slider-wrap"
      :class="{ 'cc-slider-wrap-transition': isTransition }"
      :style="{ width: move + '%', background: activeColor }"
    >
      <view
        class="cc-slider-button-wrap"
        :style="{ right: buttonPosition + '%' }"
        @touchstart="touchstart"
        @touchmove="touchmove"
        @touchend="touchend"
      >
        <slot v-if="slots.button" name="button" />
        <view v-else class="cc-slider-button-wrap-content" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import {
  computed,
  defineEmits,
  defineProps,
  getCurrentInstance,
  nextTick,
  ref,
  useSlots,
  watch,
} from 'vue'
const instance = getCurrentInstance()
const id = instance!.uid

const props = withDefaults(
  defineProps<{
    modelValue: number | string
    step?: number | string
    activeColor?: string
    inactiveColor?: string
    disabled?: boolean
    min?: number | string
    max?: number | string
    height?: number | string
  }>(),
  {
    step: 1,
    activeColor: '#0081ff',
    inactiveColor: '#e5e5e5',
    disabled: false,
    min: 0,
    max: 100,
    height: 4,
  }
)
const emits = defineEmits([
  'update:modelValue',
  'change',
  'input',
  'touchstart',
  'touchend',
])
const slots = useSlots()

// 滑块移动占比
const move = ref<number | string>(0)
// 按钮定位位置
const buttonPosition = computed(() => 100 - Number(move.value))
// 外层滑块宽度
const containerWidth = ref<number>(0)
// 是否需要动画
const isTransition = ref<boolean>(true)
// 拖动状态
const touchStatus = ref<'' | 'end'>('')
nextTick(() => {
  uni
    .createSelectorQuery()
    .in(instance)
    .select(`#cc-slider-${id}`)
    .boundingClientRect((res: any) => {
      containerWidth.value = res.width
    })
    .exec()
})

// 开始位置
const startX = ref<number>(0)
// 拖动开始
const touchstart = (e: TouchEvent) => {
  if (props.disabled) {
    return
  }
  startX.value = e.touches[0].pageX
  emits('touchstart')
}
// 拖动中
const touchmove = (e: TouchEvent) => {
  if (props.disabled) {
    return
  }
  isTransition.value = false
  const dis: number = Math.ceil(startX.value - e.touches[0].pageX)
  if (props.step === undefined) {
    move.value = Math.abs(
      (((dis - startX.value) / containerWidth.value) as any).toFixed(2) * 100
    ).toFixed(0)
  } else {
    const d = Math.abs(
      (((dis - startX.value) / containerWidth.value) as any).toFixed(2) * 100
    ).toFixed(0)
    const percent =
      Math.round((Number(d) + Number(props.step)) / Number(props.step)) *
      Number(props.step)
    if (percent % Number(props.step) === 0) {
      move.value = percent
    }
  }
  if (props.max) {
    if (Number(move.value) >= Number(props.max)) move.value = Number(props.max)
  } else {
    if (Number(move.value) >= 100) move.value = 100
  }
  if (props.min) {
    if (Number(move.value) <= Number(props.min)) move.value = Number(props.min)
  } else {
    if (Number(move.value) <= 0) move.value = 0
  }
  emits('input', Number(move.value))
}
// 拖动结束
const touchend = () => {
  if (props.disabled) {
    return
  }
  isTransition.value = true
  touchStatus.value = 'end'
  emits('touchend')
}
// 点击滑块
const clickSlider = (e: MouseEvent) => {
  isTransition.value = false
  const dis: number = Math.ceil(startX.value - e.pageX)
  const d = Math.abs(
    (((dis - startX.value) / containerWidth.value) as any).toFixed(2) * 100
  ).toFixed(0)
  const percent =
    Math.round((Number(d) + Number(props.step)) / Number(props.step)) *
    Number(props.step)
  if (percent % Number(props.step) === 0) {
    move.value = percent
  }
}
watch(
  () => [move.value, touchStatus.value],
  (val) => {
    emits('update:modelValue', val[0])
    if (val[1] === 'end') emits('change', Number(val[0]))
  }
)
watch(
  () => props.modelValue,
  (val) => {
    move.value = val
  },
  { immediate: true }
)
</script>

<style scoped lang="scss">
.cc-slider {
  position: relative;
  width: 100%;
  background-color: #ebedf0;
  border-radius: 999px;
  cursor: pointer;
  &-wrap {
    width: 100%;
    height: 100%;
    background-color: #1989fa;
    border-radius: inherit;
    &-transition {
      transition: all 0.2s;
    }
  }
  &-button-wrap {
    position: absolute;
    top: 50%;
    transform: translate(0, -50%);
    &-content {
      height: 40rpx;
      width: 40rpx;
      background-color: rgb(255, 255, 255);
      border-radius: 50%;
      box-shadow: 0 2rpx 4rpx rgb(0 0 0 / 50%);
    }
  }
}
.disabled {
  cursor: not-allowed;
  opacity: 0.5;
  pointer-events: none;
}
</style>
