<template>
  <view
    class="cc-image-preview"
    :class="{
      'cc-image-preview-show': modelValue,
      'cc-image-preview-hide': !modelValue,
    }"
    :style="{ display, animationDuration: Number(swipeDuration) / 1000 + 's' }"
  >
    <view class="cc-image-preview-mask" @click="clickMask" />
    <view class="cc-image-preview-content">
      <cc-swiper
        :list="list"
        :autoplay="false"
        :indicator-dots="showIndicators"
        :circular="circular"
        :current="current"
        :height="height"
        :mode="mode"
        :img-mode="imgMode"
        :bottom="bottom"
        :right="right"
        :active-color="activeColor"
        :indicator-active-color="indicatorActiveColor"
        @change="handleChange"
        @click="clickItem"
        @longpress="longpress"
      />
    </view>
    <view v-if="showIndex" class="cc-image-preview-dot"
      >{{ currentIndex + 1 }} / {{ list.length }}</view
    >
    <cc-action-sheet v-model="showAction" @select="handleSelect">
      <cc-action-sheet-item
        v-for="(item, index) in actions"
        :key="index"
        :name="item.name"
      />
    </cc-action-sheet>
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    list: string[]
    current?: number | string
    swipeDuration?: number | string
    height?: number | string
    actions?: { name: string }[]
    showIndex?: boolean
    showIndicators?: boolean
    mode?: 'round' | 'rect' | 'number' | 'circle' | 'none'
    imgMode?: string
    bottom?: string | number
    right?: string | number
    activeColor?: string
    indicatorActiveColor?: string
    closeable?: boolean
    closeOnClickOverlay?: boolean
    closeOnImage?: boolean
    circular?: boolean
  }>(),
  {
    current: 0,
    swipeDuration: 300,
    actions: () => [],
    showIndex: true,
    showIndicators: false,
    mode: 'circle',
    imgMode: 'aspectFill',
    height: 300,
    bottom: 20,
    right: '',
    activeColor: '#fff',
    indicatorActiveColor: '#ccc',
    closeable: false,
    closeOnClickOverlay: true,
    closeOnImage: true,
    circular: true,
  }
)

const emits = defineEmits<{
  (e: 'update:modelValue', val: boolean): void
  (e: 'click', val: string): void
  (e: 'select', val: { name: string }): void
}>()

const currentIndex = ref(-1)
const showAction = ref(false)
const display = ref<'none' | 'block'>('none')

const clickMask = () => {
  if (props.closeOnClickOverlay) {
    emits('update:modelValue', !props.modelValue)
  }
}

const handleChange = (index: number) => {
  currentIndex.value = index
}

const clickItem = (val: string) => {
  if (props.closeOnImage) {
    emits('update:modelValue', !props.modelValue)
  }
  emits('click', val)
}

const longpress = (val: { e: Event }) => {
  val.e.preventDefault()
  if (props.actions && props.actions.length) {
    showAction.value = true
  }
}

const handleSelect = (val: { name: string }) => {
  emits('select', val)
  emits('update:modelValue', !props.modelValue)
}

watch(
  () => props.modelValue,
  (val) => {
    if (val) display.value = 'block'
    else {
      showAction.value = false
      setTimeout(() => {
        display.value = 'none'
      }, 100)
    }
  }
)

watch(
  () => props.current,
  (val) => {
    currentIndex.value = val as number
  },
  { immediate: true }
)
</script>

<style scoped lang="scss">
.cc-image-preview {
  display: flex;
  align-items: center;
  justify-content: center;
  &-show {
    animation: show 1s linear forwards;
  }
  &-hide {
    animation: hide 3s linear forwards;
  }
  &-mask {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.9);
  }
  &-content {
    z-index: 1000;
    position: absolute;
    width: 100%;
    top: 50%;
    transform: translate(0, -50%);
  }
  &-dot {
    color: #fff;
    position: absolute;
    top: 40rpx;
    left: 50%;
    transform: translateX(-50%);
    font-size: 14px;
  }
}
@keyframes show {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes hide {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
