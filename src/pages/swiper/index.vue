<template>
  <view class="container">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <cc-swiper :list="list" @click="handleClick" @change="handleChange" />
    </view>

    <view class="demo">
      <view class="demo-title">指示点位置</view>
      <cc-swiper :list="list" bottom="30" right="100" />
    </view>

    <view class="demo">
      <view class="demo-title">指示点样式</view>
      <cc-swiper
        :list="list"
        :dot-style="{ width: '30rpx', height: '30rpx' }"
        active-color="red"
        indicator-active-color="#fff"
      />
    </view>

    <view class="demo">
      <view class="demo-title">指示点模式</view>
      <view style="margin-bottom: 20rpx"
        ><cc-swiper :list="list" mode="round"
      /></view>
      <view style="margin-bottom: 20rpx"
        ><cc-swiper :list="list" mode="rect"
      /></view>
      <view style="margin-bottom: 20rpx"
        ><cc-swiper :list="list" mode="number"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义指示点</view>
      <cc-swiper :list="list">
        <template #dot="{ current }">
          <view class="demo-custom"
            >第{{ current }}页 / 总共{{ list.length }}页</view
          >
        </template>
      </cc-swiper>
    </view>

    <view class="demo">
      <view class="demo-title">不显示指示点</view>
      <cc-swiper :list="list" mode="none" />
    </view>

    <view class="demo">
      <view class="demo-title">自定义内容</view>
      <cc-swiper :list="list">
        <template #default>
          <view v-for="(item, index) in list" :key="index" class="custom-img"
            ><image class="custom-img-item" :src="item.image"
          /></view>
        </template>
      </cc-swiper>
    </view>
  </view>
</template>

<script setup lang="ts">
const list = [
  {
    image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',
  },
  {
    image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',
  },
  {
    image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
  },
]

const handleClick = (val: any) => {
  console.log('click', val)
}
const handleChange = (val: any) => {
  console.log('click', val)
}
</script>

<style scoped lang="scss">
.container {
  padding-bottom: 20rpx;
}
.demo {
  &-title {
    margin: 16rpx;
  }
  &-custom {
    position: absolute;
    font-size: 12px;
    color: #fff;
    right: 50rpx;
    bottom: 30rpx;
  }
}
.custom-img {
  width: 90%;
  margin-left: 5%;
  &-item {
    width: 100%;
    height: 300rpx;
    border-radius: 12rpx;
  }
}
</style>
