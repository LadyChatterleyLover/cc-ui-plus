<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <view>
          <cc-loading />
        </view>
        <view>
          <cc-loading type="spinner" />
        </view>
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义颜色</view>
      <view class="demo-item">
        <view>
          <cc-loading color="#0081ff" />
        </view>
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义大小</view>
      <view class="demo-item">
        <view>
          <cc-loading size="30" />
        </view>
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">加载文案</view>
      <view class="demo-item">
        <view>
          <cc-loading>加载中...</cc-loading>
        </view>
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">垂直排列</view>
      <view class="demo-item">
        <view>
          <cc-loading vertical>加载中...</cc-loading>
        </view>
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义文本颜色和大小</view>
      <view class="demo-item">
        <view>
          <cc-loading text-color="#0081ff" text-size="20">加载中...</cc-loading>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts"></script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  padding: 20rpx;
  &-title {
    padding: 20rpx;
  }
  &-item {
    padding: 0 10rpx;
    margin-bottom: 20rpx;
    display: flex;
  }
}
</style>
