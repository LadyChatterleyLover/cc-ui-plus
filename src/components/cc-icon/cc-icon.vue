<template>
  <!-- #ifdef APP-NVUE -->
  <text
    :style="{ color: color, 'font-size': Number(iconSize) }"
    class="uni-icons"
    @click="handleClick"
    >{{ ccCode }}</text
  >
  <!-- #endif -->
  <!-- #ifndef APP-NVUE -->
  <text
    :style="{ color: color, 'font-size': Number(iconSize) }"
    class="uni-icons"
    :class="['uniui-' + type, customPrefix, customPrefix ? type : '']"
    @click="handleClick"
  />
  <!-- #endif -->
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import iconList from './icons'

const getVal = (val: number | string) => {
  const reg = /^[0-9]*$/g
  return typeof val === 'number' || reg.test(val) ? `${val}px` : val
}

const props = withDefaults(
  defineProps<{
    type?: string
    color?: string
    size?: number | string
    customPrefix?: string
  }>(),
  {
    type: '',
    color: '#333',
    size: 16,
    customPrefix: '',
  }
)

const icons = ref(iconList.glyphs)

const emits = defineEmits(['click'])

const ccCode = computed(() => {
  const code = icons.value.find((v) => v.font_class === props.type)
  if (code) {
    return unescape(`%u${code.unicode}`)
  }
  return ''
})
const iconSize = computed(() => getVal(props.size))

const handleClick = (e: Event) => {
  emits('click', e)
}
</script>

<style scoped lang="scss">
/* #ifndef APP-NVUE */
@import './icon.css';
@font-face {
  font-family: uniicons;
  src: url('./uniicons.ttf') format('truetype');
}
/* #endif */
.uni-icons {
  font-family: uniicons;
  text-decoration: none;
  text-align: center;
}
</style>
