<template>
  <view class="container">
    <view>
      <view class="title">基础语法</view>
      <view class="content">
        <view>
          <cc-tag @click="onClick">标签</cc-tag>
        </view>
        <view>
          <cc-tag type="success">标签</cc-tag>
        </view>
        <view>
          <cc-tag type="warning">标签</cc-tag>
        </view>
        <view>
          <cc-tag type="info">标签</cc-tag>
        </view>
        <view>
          <cc-tag type="error">标签</cc-tag>
        </view>
      </view>
    </view>

    <view>
      <view class="title">空心样式</view>
      <view class="content">
        <view>
          <cc-tag plain>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="success" plain>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="warning" plain>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="info" plain>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="error" plain>标签</cc-tag>
        </view>
      </view>
    </view>

    <view>
      <view class="title">圆角样式</view>
      <view class="content">
        <view>
          <cc-tag round>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="success" round>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="warning" round>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="info" round>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="error" round>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="primary" circle-left>标签</cc-tag>
        </view>
        <view>
          <cc-tag type="error" circle-right>标签</cc-tag>
        </view>
      </view>
    </view>

    <view>
      <view class="title">标签尺寸</view>
      <view class="content">
        <view>
          <cc-tag>普通标签</cc-tag>
        </view>
        <view>
          <cc-tag size="medium">中号标签</cc-tag>
        </view>
        <view>
          <cc-tag type="success" size="large">大号标签</cc-tag>
        </view>
      </view>
    </view>

    <view>
      <view class="title">自定义颜色</view>
      <view class="content">
        <view>
          <cc-tag color="#7232dd">标签</cc-tag>
        </view>
        <view>
          <cc-tag color="#7232dd" plain>标签</cc-tag>
        </view>
        <view>
          <cc-tag color="#ffe1e1" text-color="#ad0000">标签</cc-tag>
        </view>
      </view>
    </view>

    <view>
      <view class="title">可关闭</view>
      <view class="content">
        <view>
          <cc-tag closeable @close="onClick">标签</cc-tag>
        </view>
        <view>
          <cc-tag closeable type="success">标签</cc-tag>
        </view>
        <view>
          <cc-tag closeable plain>标签</cc-tag>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
const onClick = () => {
  console.log('close')
}
</script>

<style lang="scss" scoped>
.title {
  margin: 20rpx;
}
.content {
  margin: 20rpx;
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  view {
    margin: 10rpx;
  }
}
</style>
