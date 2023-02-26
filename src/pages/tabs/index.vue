<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-tabs v-model="value" :list="list1" />
      </view>
    </view>

    <view>
      <view class="title">禁用选项</view>
      <view class="item">
        <view>
          <cc-tabs v-model="value" :list="list2" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">徽标</view>
      <view class="item">
        <view>
          <cc-tabs v-model="value" :list="list3" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">禁止滚动</view>
      <view class="item">
        <view>
          <cc-tabs v-model="value" :list="list1" :scrollable="false" />
        </view>
      </view>
    </view>

    <view>
      <view class="title">自定义样式</view>
      <view class="item">
        <view>
          <cc-tabs
            v-model="value"
            :list="list1"
            line-color="#3c9cff"
            active-color="#f56c6c"
          />
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('1')
const list1 = [
  {
    name: '关注',
  },
  {
    name: '推荐',
  },
]
const list2 = [
  {
    name: '关注',
  },
  {
    name: '推荐',
  },
  {
    name: '电影',
    disabled: true,
  },
  {
    name: '科技',
  },
  {
    name: '音乐',
    disabled: true,
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
  {
    name: '关注',
  },
  {
    name: '推荐',
  },
  {
    name: '电影',
    disabled: true,
  },
  {
    name: '科技',
  },
  {
    name: '音乐',
    disabled: true,
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
const list3 = [
  {
    name: '关注',
  },
  {
    name: '推荐',
  },
  {
    name: '电影',
    dot: true,
  },
  {
    name: '科技',
  },
  {
    name: '音乐',
    badge: 5,
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
  width: 100%;
}
</style>
