<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-button @click="show1 = true">打开键盘</cc-button>
        <cc-password-input
          v-model="show1"
          @complete="handleComplete"
          @change="handleChange"
          @backspace="handleBackspace"
        />
      </view>
      <view>当前输入值: {{ value1 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义长度</view>
      <view class="demo-item">
        <cc-button @click="show2 = true">打开键盘</cc-button>
        <cc-password-input v-model="show2" length="4" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义间距</view>
      <view class="demo-item">
        <cc-button @click="show3 = true">打开键盘</cc-button>
        <cc-password-input v-model="show3" gutter="0" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">明文显示</view>
      <view class="demo-item">
        <cc-button @click="show4 = true">打开键盘</cc-button>
        <cc-password-input v-model="show4" :mask="false" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">设置初始值</view>
      <view class="demo-item">
        <cc-button @click="show5 = true">打开键盘</cc-button>
        <cc-password-input v-model="show5" init-value="1234" :mask="false" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value1 = ref('')
const show1 = ref(false)
const show2 = ref(false)
const show3 = ref(false)
const show4 = ref(false)
const show5 = ref(false)

const handleComplete = (val: string) => {
  console.log('change', val)
}

const handleChange = (val: string) => {
  console.log('complete', val)
}

const handleBackspace = (val: string) => {
  console.log('bckspace', val)
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
