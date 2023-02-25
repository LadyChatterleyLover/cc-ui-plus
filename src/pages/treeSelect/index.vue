<template>
  <view style="background: #f7f7f7; height: 100vh">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-tree-select
          :items="items1"
          :active-id="activeId1"
          :main-active-index="activeIndex1"
          @click-nav="clickNav"
          @click-item="clickItem"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">多选模式</view>
      <view class="demo-item">
        <cc-tree-select
          :items="items1"
          :active-id="activeId2"
          :main-active-index="activeIndex2"
          @click-nav="clickNav"
          @click-item="clickItem"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">徽标提示</view>
      <view class="demo-item">
        <cc-tree-select
          :items="items2"
          :active-id="activeId1"
          :main-active-index="activeIndex1"
        />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const activeId1 = ref(1)
const activeIndex1 = ref(0)
const activeId2 = ref([1, 2])
const activeIndex2 = ref(0)
const items1 = [
  {
    text: '浙江',
    children: [
      {
        id: 1,
        text: '杭州',
      },
      {
        id: 2,
        text: '温州',
      },
      {
        id: 3,
        text: '宁波',
        disabled: true,
      },
      {
        id: 4,
        text: '义务',
      },
    ],
  },
  {
    text: '江苏',
    children: [
      {
        id: 11,
        text: '南京',
      },
      {
        id: 12,
        text: '无锡',
      },
      {
        id: 13,
        text: '徐州',
      },
      {
        id: 14,
        text: '苏州',
      },
    ],
  },
  {
    text: '福建',
    disabled: true,
  },
]
const items2 = [
  {
    text: '浙江',
    dot: true,
    children: [
      {
        id: 1,
        text: '杭州',
      },
      {
        id: 2,
        text: '温州',
      },
      {
        id: 3,
        text: '宁波',
        disabled: true,
      },
      {
        id: 4,
        text: '义务',
      },
    ],
  },
  {
    text: '江苏',
    badge: 5,
    children: [
      {
        id: 1,
        text: '南京',
      },
      {
        id: 2,
        text: '无锡',
      },
      {
        id: 3,
        text: '徐州',
      },
      {
        id: 4,
        text: '苏州',
      },
    ],
  },
  {
    text: '福建',
    disabled: true,
  },
]

const clickNav = (val: any) => {
  console.log('clickNav', val)
}

const clickItem = (val: any) => {
  console.log('clickItem', val)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  &-title {
    padding: 20rpx;
  }
}
</style>
