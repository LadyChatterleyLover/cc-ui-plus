<template>
  <view class="cc-time-line">
    <slot />
  </view>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.cc-time-line {
  position: relative;
  &::before {
    content: ' ';
    position: absolute;
    left: 20rpx;
    top: 8rpx;
    width: 2rpx;
    bottom: 0;
    border-left: 2rpx solid #ddd;
    transform-origin: 0 0;
    transform: scaleX(0.5);
  }
}
</style>
