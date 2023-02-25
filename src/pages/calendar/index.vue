<template>
  <view>
    <cc-calendar v-model="show" @confirm="confirm" />
    <cc-button type="primary" size="small" @click="show = true"
      >打开日历</cc-button
    >
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const show = ref(false)

const confirm = (val: string) => {
  console.log('confirm', val)
}
</script>

<style scoped></style>
