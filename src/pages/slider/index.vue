<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-slider
          v-model="sliderValue1"
          @touchstart="touchstart"
          @touchend="touchend"
          @change="onChange"
          @input="onInput"
        />
      </view>
      <view>当前滑块值: {{ sliderValue1 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用状态</view>
      <view class="demo-item">
        <cc-slider v-model="sliderValue2" disabled />
      </view>
      <view>当前滑块值: {{ sliderValue2 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义样式</view>
      <view class="demo-item">
        <cc-slider
          v-model="sliderValue3"
          active-color="#ee0a24"
          inactive-color="#abc123"
        />
      </view>
      <view>当前滑块值: {{ sliderValue3 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义按钮</view>
      <view class="demo-item">
        <cc-slider v-model="sliderValue4">
          <template #button>
            <view class="custom-button">{{ sliderValue4 }}</view>
          </template>
        </cc-slider>
      </view>
      <view>当前滑块值: {{ sliderValue4 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">最大值和最小值</view>
      <view class="demo-item">
        <cc-slider v-model="sliderValue5" min="10" max="80" />
      </view>
      <view>当前滑块值: {{ sliderValue5 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">步进</view>
      <view class="demo-item">
        <cc-slider v-model="sliderValue6" step="5" />
      </view>
      <view>当前滑块值: {{ sliderValue6 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义高度</view>
      <view class="demo-item">
        <cc-slider v-model="sliderValue7" height="6" />
      </view>
      <view>当前滑块值: {{ sliderValue7 }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const sliderValue1 = ref<number>(20)
const sliderValue2 = ref<number>(20)
const sliderValue3 = ref<number>(20)
const sliderValue4 = ref<number>(20)
const sliderValue5 = ref<number>(20)
const sliderValue6 = ref<number>(20)
const sliderValue7 = ref<number>(20)

const touchstart = () => {
  console.log('touchstart')
}
const touchend = () => {
  console.log('touchend')
}
const onInput = (val: string) => {
  console.log('input', val)
}
const onChange = (val: string) => {
  console.log('change', val)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  &-title {
    padding: 40rpx;
  }
  &-item {
    padding: 0 10rpx;
    margin-bottom: 20rpx;
  }
}
.custom-button {
  width: 26px;
  color: #fff;
  font-size: 10px;
  line-height: 18px;
  text-align: center;
  background-color: #ee0a24;
  border-radius: 100px;
}
</style>
