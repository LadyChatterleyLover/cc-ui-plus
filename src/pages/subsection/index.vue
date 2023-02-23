<template>
  <view style="background: #f7f7f7; height: 100vh">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <view class="demo-item-content">
          <cc-subsection :list="list" :value="value" />
        </view>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const list = [
  {
    name: '待发货',
  },
  {
    name: '待付款',
  },
  {
    name: '待评价',
  },
]

const value = ref(0)
</script>

<style lang="scss" scoped>
.demo {
  &-title {
    padding: 20rpx;
  }
  &-item {
    margin-bottom: 60rpx;
    display: flex;
    justify-content: center;
    &-content {
      width: 500rpx;
    }
  }
}
</style>
