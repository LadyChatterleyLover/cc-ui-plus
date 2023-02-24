<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-button @click="show1 = true">打开键盘</cc-button>
        <cc-number-keyboard
          v-model="show1"
          @backspace="backspace"
          @change="handleChange"
        />
      </view>
      <view>当前输入值: {{ value1 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">身份证键盘</view>
      <view class="demo-item">
        <cc-button @click="show2 = true">打开键盘</cc-button>
        <cc-number-keyboard v-model="show2" extra-key="x" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">随机数字键盘</view>
      <view class="demo-item">
        <cc-button @click="show3 = true">打开键盘</cc-button>
        <cc-number-keyboard v-model="show3" random />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">点击遮罩层不关闭</view>
      <view class="demo-item">
        <cc-button @click="show4 = true">打开键盘</cc-button>
        <cc-number-keyboard v-model="show4" :close-on-click-overlay="false" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const show1 = ref(false)
const show2 = ref(false)
const show3 = ref(false)
const show4 = ref(false)
const value1 = ref('')

const handleChange = (val: string) => {
  value1.value += val
}

const backspace = () => {
  if (value1.value.length)
    value1.value = value1.value.slice(0, Math.max(0, value1.value.length - 1))
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
