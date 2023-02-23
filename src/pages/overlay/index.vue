<template>
  <view style="padding: 40rpx">
    <view>
      <view class="title">基础用法</view>
      <view>
        <cc-overlay v-model="show1" @click="handleClick" />
        <cc-button @click="open1">打开遮罩层</cc-button>
      </view>
    </view>

    <view style="margin-top: 40rpx">
      <view class="title">嵌套内容</view>
      <view>
        <cc-overlay v-model="show2">
          <view class="content">
            <view class="block" />
          </view>
        </cc-overlay>
        <cc-button @click="open2">打开遮罩层</cc-button>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const show1 = ref<boolean>(false)
const show2 = ref<boolean>(false)
const open1 = () => {
  show1.value = true
}
const open2 = () => {
  show2.value = true
}
const handleClick = () => {
  console.log('click')
}
</script>

<style scoped lang="scss">
.title {
  margin: #{topx(10)};
}
.item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin: 20rpx;
  div {
    margin: 10rpx;
  }
}
.content {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  .block {
    width: 240rpx;
    height: 240rpx;
    background-color: #fff;
  }
}
</style>
