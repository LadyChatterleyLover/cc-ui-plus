<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-stepper v-model="value1" @minus="minus" @plus="plus" />
      </view>
      <view>当前值: {{ value1 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">设置步长</view>
      <view class="demo-item">
        <cc-stepper v-model="value2" step="5" />
      </view>
      <view>当前值: {{ value2 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">最小值和最大值</view>
      <view class="demo-item">
        <cc-stepper v-model="value3" min="5" max="15" />
      </view>
      <view>当前值: {{ value3 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用状态</view>
      <view class="demo-item">
        <cc-stepper v-model="value4" disabled />
      </view>
      <view>当前值: {{ value4 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用输入框</view>
      <view class="demo-item">
        <cc-stepper v-model="value5" disable-input />
      </view>
      <view>当前值: {{ value5 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">限制输入整数</view>
      <view class="demo-item">
        <cc-stepper v-model="value6" integer />
      </view>
      <view>当前值: {{ value6 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">限制小数位位数</view>
      <view class="demo-item">
        <cc-stepper v-model="value7" decimal-length="3" />
      </view>
      <view>当前值: {{ value7 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义大小</view>
      <view class="demo-item">
        <cc-stepper v-model="value8" input-width="40" button-size="32" />
      </view>
      <view>当前值: {{ value8 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">圆角按钮</view>
      <view class="demo-item">
        <cc-stepper v-model="value9" disable-input round />
      </view>
      <view>当前值: {{ value9 }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const value1 = ref<number>(1)
const value2 = ref<number>(1)
const value3 = ref<number>(1)
const value4 = ref<number>(1)
const value5 = ref<number>(1)
const value6 = ref<number>(1)
const value7 = ref<number>(1)
const value8 = ref<number>(1)
const value9 = ref<number>(1)

const minus = () => {
  console.log('minus')
}
const plus = () => {
  console.log('plus')
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
