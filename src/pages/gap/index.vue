<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-gap bg-color="#ccc" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">上下间距</view>
      <view class="demo-item">
        <cc-gap bg-color="#ccc" margin-bottom="30" margin-top="30" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts"></script>

<style scoped lang="scss">
.demo {
  &-title {
    padding: 20rpx;
  }
  &-item {
    margin-bottom: 60rpx;
    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>
