<template>
  <view>
    <cc-button type="primary" @click="show = true">开始验证</cc-button>
    <cc-rotate-verify v-model="show" :src="src" />
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const show = ref(false)
const src = '../../static/logo.png'
</script>

<style scoped></style>
