<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-checker v-model="value1" :list="list1" @change="handleChange" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">多选</view>
      <view class="demo-item">
        <cc-checker
          v-model="value2"
          :list="list1"
          multiple
          @change="handleChange"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">圆角类型</view>
      <view class="demo-item">
        <cc-checker v-model="value1" :list="list2" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">只读状态</view>
      <view class="demo-item">
        <cc-checker v-model="value1" :list="list3" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用状态</view>
      <view class="demo-item">
        <cc-checker v-model="value1" :list="list4" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">徽标</view>
      <view class="demo-item">
        <cc-checker v-model="value1" :list="list5" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const value1 = ref<number>(0)
const value2 = ref<number[]>([1, 2])
const list1 = [
  {
    label: '彷徨',
    value: 0,
  },
  {
    label: '呐喊',
    value: 1,
  },
  {
    label: '动物凶猛',
    value: 2,
  },
]
const list2 = [
  {
    label: '彷徨',
    value: 0,
    round: true,
  },
  {
    label: '呐喊',
    value: 1,
    round: true,
  },
  {
    label: '动物凶猛',
    value: 2,
    round: true,
  },
]
const list3 = [
  {
    label: '彷徨',
    value: 0,
    readonly: true,
  },
  {
    label: '呐喊',
    value: 1,
    readonly: true,
  },
  {
    label: '动物凶猛',
    value: 2,
    readonly: true,
  },
]
const list4 = [
  {
    label: '彷徨',
    value: 0,
    disabled: true,
  },
  {
    label: '呐喊',
    value: 1,
    disabled: true,
  },
  {
    label: '动物凶猛',
    value: 2,
    disabled: true,
  },
]
const list5 = [
  {
    label: '彷徨',
    value: 0,
    info: '99',
  },
  {
    label: '呐喊',
    value: 1,
    disabled: true,
  },
  {
    label: '动物凶猛',
    value: 2,
    info: 'ok',
    round: true,
  },
]
const handleChange = (val: any) => {
  console.log(val)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  padding: 20rpx;
  &-title {
    padding: 20rpx;
  }
  &-item {
    padding: 0 10rpx;
    margin-bottom: 20rpx;
  }
}
</style>
