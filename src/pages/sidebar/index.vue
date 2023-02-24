<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础语法</view>
      <cc-sidebar :list="list1" @change="clickItem" />
    </view>
    <view class="demo">
      <view class="demo-title">不显示左侧线条</view>
      <cc-sidebar :show-line="false" :list="list1" />
    </view>
    <view class="demo">
      <view class="demo-title">禁用选项</view>
      <cc-sidebar :list="list2" />
    </view>
    <view class="demo">
      <view class="demo-title">徽标提示</view>
      <cc-sidebar :list="list3" />
    </view>
    <view class="demo">
      <view class="demo-title">自定义左侧线条显示</view>
      <cc-sidebar width="120" :list="list4" />
    </view>
    <view class="demo">
      <view class="demo-title">自定义样式</view>
      <cc-sidebar width="120" :list="list5" />
    </view>
  </view>
</template>

<script setup lang="ts">
const list1 = [
  {
    title: '标签1',
  },
  {
    title: '标签2',
  },
  {
    title: '标签3',
  },
]
const list2 = [
  {
    title: '标签1',
  },
  {
    title: '标签2',
    disabled: true,
  },
  {
    title: '标签3',
  },
]
const list3 = [
  {
    title: '标签1',
  },
  {
    title: '标签2',
    dot: true,
  },
  {
    title: '标签3',
    badge: 5,
  },
]
const list4 = [
  {
    title: '标签1',
    lineColor: '#765489',
    lineWidth: 10,
    lineHeight: 20,
  },
  {
    title: '标签2',
    lineColor: '#765489',
  },
  {
    title: '标签3',
    lineColor: '#765489',
  },
  {
    title: '标签4',
    lineColor: '#765489',
  },
]
const list5 = [
  {
    title: '标签1',
    activeColor: 'red',
  },
  {
    title: '标签2',
    bgColor: '#675232',
    activeColor: '#fff',
  },
  {
    title: '标签3',
    textColor: '#976512',
  },
  {
    title: '标签4',
    fontSize: 30,
  },
]

const clickItem = (val: any) => {
  console.log('click', val)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 20rpx;
  &-title {
    margin: 20rpx;
  }
}
</style>
