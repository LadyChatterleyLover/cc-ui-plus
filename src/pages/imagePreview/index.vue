<template>
  <view>
    <cc-button @click="open1">打开预览</cc-button>
    <cc-image-preview v-model="show1" :list="list" :actions="actions" />
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const show1 = ref(false)
const list = [
  {
    image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',
  },
  {
    image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',
  },
  {
    image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
  },
]
const actions = [
  {
    name: '选项一',
  },
  {
    name: '选项二',
  },
  {
    name: '选项三',
  },
]
const open1 = () => {
  show1.value = true
}
</script>

<style scoped></style>
