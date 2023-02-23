<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-load-more text="点击或上拉加载更多" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义样式</view>
      <view class="demo-item">
        <cc-load-more
          line-width="120"
          color="red"
          font-size="20"
          text="点击或上拉加载更多"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">加载更多</view>
      <view class="demo-item">
        <cc-load-more loading text="加载中" />
        <cc-load-more loading text="加载中" loading-type="spinner" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">显示点</view>
      <view class="demo-item">
        <cc-load-more dot />
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
  }
}
</style>
