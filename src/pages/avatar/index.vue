<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <view>
          <cc-avatar :src="src" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar :src="src" shape="square" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">头像尺寸</view>
      <view class="item">
        <view>
          <cc-avatar :src="src" :size="70" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar :src="src" size="small" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar :src="src" size="large" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">图标头像</view>
      <view class="item">
        <view>
          <cc-avatar icon="star" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar icon="chat" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">文字头像</view>
      <view class="item">
        <view>
          <cc-avatar text="A" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar text="张" />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar text="王" random-bg-color />
        </view>
        <view style="margin-left: 40rpx">
          <cc-avatar text="李" random-bg-color />
        </view>
      </view>
    </view>

    <view>
      <view class="title">加载失败</view>
      <view class="item">
        <view>
          <cc-avatar :src="errSrc" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">头像组</view>
      <view class="item">
        <view>
          <cc-avatar-group :urls="urls" />
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
const src = 'https://cdn.uviewui.com/uview/album/1.jpg'
const errSrc = 'https://cdn.uviewui.com/uview/album/123.jpg'
const urls = [
  'https://cdn.uviewui.com/uview/album/1.jpg',
  'https://cdn.uviewui.com/uview/album/2.jpg',
  'https://cdn.uviewui.com/uview/album/3.jpg',
  'https://cdn.uviewui.com/uview/album/4.jpg',
  'https://cdn.uviewui.com/uview/album/7.jpg',
  'https://cdn.uviewui.com/uview/album/6.jpg',
  'https://cdn.uviewui.com/uview/album/5.jpg',
]
</script>

<style lang="scss" scoped>
.title {
  margin: 20rpx;
}
.item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin: 20rpx;
}
</style>
