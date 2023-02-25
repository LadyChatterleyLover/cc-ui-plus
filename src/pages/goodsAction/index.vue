<template>
  <view class="demo">
    <view class="demo-content">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-goods-action :options="options1" :buttons="buttons1" />
      </view>
    </view>

    <view class="demo-content">
      <view class="demo-title">徽标</view>
      <view class="demo-item">
        <cc-goods-action :options="options2" :buttons="buttons2" />
      </view>
    </view>

    <view class="demo-content">
      <view class="demo-title">自定义颜色</view>
      <view class="demo-item">
        <cc-goods-action :options="options3" :buttons="buttons3" />
      </view>
    </view>

    <view class="demo-content">
      <view class="demo-title">自定义按钮颜色</view>
      <view class="demo-item">
        <cc-goods-action :options="options4" :buttons="buttons4" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
const options1 = [
  {
    text: '客服',
    icon: 'chat',
  },
  {
    text: '收藏',
    icon: 'heart',
  },
  {
    text: '购物车',
    icon: 'cart',
  },
]
const buttons1 = [
  {
    text: '加入购物车',
  },
]
const options2 = [
  {
    text: '客服',
    icon: 'chat',
    dot: true,
  },
  {
    text: '收藏',
    icon: 'heart',
  },
  {
    text: '购物车',
    icon: 'cart',
    info: '10',
  },
]
const buttons2 = [
  {
    text: '加入购物车',
  },
  {
    text: '立即购买',
  },
]
const options3 = [
  {
    text: '客服',
    icon: 'chat',
    iconColor: 'orange',
  },
  {
    text: '购物车',
    icon: 'cart',
    info: '3',
    iconColor: '#258369',
    infoColor: '#abc123',
  },
]
const buttons3 = [
  {
    text: '加入购物车',
  },
  {
    text: '立即购买',
  },
]
const options4 = [
  {
    text: '客服',
    icon: 'chat',
  },
  {
    text: '收藏',
    icon: 'heart',
  },
  {
    text: '购物车',
    icon: 'cart',
  },
]
const buttons4 = [
  {
    text: '加入购物车',
    background: 'rgb(190, 153, 255)',
  },
  {
    text: '立即购买',
    background: 'rgb(114, 50, 221)',
  },
]
</script>

<style scoped lang="scss">
.demo {
  background: #f7f7f7;
  height: 100vh;
  .demo-content {
    margin-bottom: 40rpx;
    padding: 20rpx;
  }
  &-title {
    padding: 20rpx;
  }
  &-item {
    padding: 0 5px;
    margin-bottom: 10px;
    background: #fff;
  }
}
</style>
