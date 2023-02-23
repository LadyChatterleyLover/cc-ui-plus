<template>
  <view
    class="cc-gap"
    :style="{
      background: bgColor,
      height: height + 'rpx',
      marginTop: marginTop + 'rpx',
      marginBottom: marginBottom + 'rpx',
    }"
  />
</template>

<script lang="ts" setup>
withDefaults(
  defineProps<{
    // 背景颜色
    bgColor?: string
    // 间隔槽高度
    height?: string | number
    // 与前一个元素的距离
    marginTop?: string | number
    // 与后一个元素的距离
    marginBottom?: string | number
  }>(),
  {
    bgColor: 'transparent',
    height: 60,
    marginTop: 0,
    marginBottom: 0,
  }
)
</script>

<style lang="scss" scoped></style>
