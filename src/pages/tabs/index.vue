<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <view>
          <cc-tabs v-model="value" :list="list" />
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('1')
const list = [
  {
    name: '关注',
  },
  {
    name: '推荐',
  },
  {
    name: '电影',
  },
  {
    name: '科技',
  },
  {
    name: '音乐',
  },
  {
    name: '美食',
  },
  {
    name: '文化',
  },
  {
    name: '财经',
  },
  {
    name: '手工',
  },
]
</script>

<style scoped lang="scss">
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
