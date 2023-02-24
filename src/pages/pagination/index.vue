<template>
  <view class="container">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item"
        ><cc-pagination :current="current" :page-size="pageSize" :total="total"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">简单模式</view>
      <view class="demo-item"
        ><cc-pagination
          simple
          :current="current"
          :page-size="pageSize"
          :total="total"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">显示省略号</view>
      <view class="demo-item"
        ><cc-pagination
          force-ellipses
          :current="current"
          :page-size="pageSize"
          :total="total"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义内容</view>
      <view class="demo-item">
        <cc-pagination :current="current" :page-size="pageSize" :total="total">
          <template #pre>
            <cc-icon type="arrowleft" color="#0081ff" />
          </template>
          <template #next>
            <cc-icon type="arrowright" color="#0081ff" />
          </template>
          <template #page="{ text }">
            {{ text.value }}
          </template>
        </cc-pagination>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const current = ref(1)
const pageSize = ref(5)
const total = ref(25)
</script>

<style scoped lang="scss">
.container {
  background: #f7f7f7;
  height: 100vh;
}
.demo {
  margin: 0 10rpx 60rpx 10rpx;
  &-title {
    margin: 10rpx 0;
  }
}
</style>
