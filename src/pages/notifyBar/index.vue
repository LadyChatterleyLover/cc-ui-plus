<template>
  <view class="demo">
    <view class="title">基础用法</view>
    <view class="content">
      <cc-notify-bar volume closeable @click="clickBar"
        >闭起双眼你最挂念谁,眼睛张开身边竟是谁,感激车站里,尚有月台能让我们满足到落泪</cc-notify-bar
      >
    </view>
    <view class="content">
      <cc-notify-bar volume link
        >闭起双眼你最挂念谁,眼睛张开身边竟是谁,感激车站里,尚有月台能让我们满足到落泪</cc-notify-bar
      >
    </view>
  </view>

  <view class="demo">
    <view class="title">滚动速度</view>
    <view class="content">
      <cc-notify-bar
        text="拥不拥有也会记住谁,快不快乐有天总过去"
        speed="160"
        volume
        link
      />
    </view>
  </view>

  <view class="demo">
    <view class="title">无图标</view>
    <view class="content">
      <cc-notify-bar text="拥不拥有也会记住谁,快不快乐有天总过去" volume />
    </view>
    <view class="content">
      <cc-notify-bar text="拥不拥有也会记住谁,快不快乐有天总过去" />
    </view>
  </view>

  <view class="demo">
    <view class="title">垂直滚动</view>
    <view class="content">
      <cc-notify-bar volume link vertical :list="list" />
    </view>
  </view>

  <view class="demo">
    <view class="title">自定义内容</view>
    <view class="content">
      <cc-notify-bar text="当你孤单你会想起谁">
        <template #left>
          <cc-icon type="person" />
        </template>
      </cc-notify-bar>
    </view>
    <view class="content">
      <cc-notify-bar bg-color="#ecf9ff">
        <template #default>
          <view style="color: #1989fa">你想不想找个人来陪</view>
        </template>
      </cc-notify-bar>
    </view>
    <view class="content">
      <cc-notify-bar text="你的快乐伤悲,只有我能体会">
        <template #right>
          <view class="text">体会</view>
        </template>
      </cc-notify-bar>
    </view>
  </view>
</template>

<script setup lang="ts">
const list = [
  '一起长大的约定',
  '那样清晰 打过勾的我相信',
  '说好要一起旅行',
  '是你如今 唯一坚持的任性',
]
const clickBar = () => {
  console.log('click')
}
const clickLeft = () => {
  console.log('clickLeft')
}
const clickRight = () => {
  console.log('clickRight')
}
</script>

<style lang="scss" scoped>
.demo {
  margin: 40rpx 0;
}
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
.text {
  height: 80rpx;
  line-height: 80rpx;
}
</style>
