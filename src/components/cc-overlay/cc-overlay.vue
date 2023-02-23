<template>
  <view
    v-if="visible"
    class="cc-overlay"
    :class="{ flex }"
    :style="{ zIndex: Number(zIndex), background }"
    :animation="animationData"
    @click="handleClick"
  >
    <slot />
  </view>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    zIndex?: string | number
    duration?: string | number
    background?: string
    flex?: boolean
    closeOnClickOverlay?: boolean
  }>(),
  {
    modelValue: false,
    zIndex: 999,
    duration: 300,
    background: 'rgba(0,0,0,.5)',
    flex: true,
    closeOnClickOverlay: true,
  }
)

const emits = defineEmits(['update:modelValue', 'click'])

const zIndex = ref<number>(Number(props.zIndex))
const animation = ref<any>({})
const animationData = ref<any>({})
const visible = ref<boolean>(false)

onMounted(() => {
  animation.value = uni.createAnimation({
    duration: Number(props.duration),
    timingFunction: 'linear',
  })
})

const handleClick = (e: Event) => {
  if (props.closeOnClickOverlay) {
    emits('update:modelValue', !props.modelValue)
  }
  emits('click', e)
}

let cleanup = () => {
  //
}

const stopWatch = watch(
  () => props.modelValue,
  (val) => {
    let timer1: number | null = null
    let timer2: number | null = null
    if (val) {
      visible.value = true
      timer1 = setTimeout(() => {
        animation.value.opacity(1).step()
        animationData.value = animation.value.export()
      }, 50)
    } else {
      animation.value.opacity(0).step()
      animationData.value = animation.value.export()
      timer2 = setTimeout(() => {
        visible.value = false
      }, Number(props.duration) + 50)
    }
    cleanup = () => {
      clearTimeout(timer1 as number)
      clearTimeout(timer2 as number)
    }
  }
)

onUnmounted(() => {
  stopWatch()
  cleanup()
})
</script>

<style scoped lang="scss">
.cc-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  opacity: 0;
}
.flex {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
