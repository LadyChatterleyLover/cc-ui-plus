<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-countup :end="end" :start-val="0" :end-val="1000" />
      </view>
      <view class="item">
        <cc-countup :start-val="100" :end-val="500" />
      </view>
    </view>

    <view>
      <view class="title">千分位分隔符</view>
      <view class="item">
        <cc-countup :start-val="0" :end-val="3000" />
      </view>
      <view class="item">
        <cc-countup :start-val="0" :end-val="3000" separator="." />
      </view>
      <view class="item">
        <cc-countup :start-val="0" :use-grouping="false" :end-val="3000" />
      </view>
    </view>

    <view>
      <view class="title">小数位和分割符号</view>
      <view class="item">
        <cc-countup :decimal-places="2" :start-val="0" :end-val="3000.55" />
      </view>
      <view class="item">
        <cc-countup :decimal-places="5" :start-val="0" :end-val="3000.55" />
      </view>
      <view class="item">
        <cc-countup
          :start-val="0"
          :decimal-places="2"
          decimal=","
          :end-val="3000.55"
        />
      </view>
    </view>

    <view>
      <view class="title">使用过渡动画</view>
      <view class="item">
        <cc-countup :start-val="0" :end-val="3000" />
      </view>
      <view class="item">
        <cc-countup :start-val="0" :use-easing="false" :end-val="3000" />
      </view>
    </view>

    <view>
      <view class="title">前缀和后缀内容</view>
      <view class="item">
        <cc-countup prefix="￥" :start-val="0" :end-val="3000" />
      </view>
      <view class="item">
        <cc-countup :start-val="0" suffix="元" :end-val="3000" />
      </view>
    </view>

    <view>
      <view class="title">滚动持续时长</view>
      <view class="item">
        <cc-countup :duration="5" :start-val="0" :end-val="3000" />
      </view>
      <view class="item">
        <cc-countup :duration="1" :start-val="0" :end-val="3000" />
      </view>
    </view>

    <view>
      <view class="title">逆向滚动</view>
      <view class="item">
        <cc-countup :start-val="3000" :end-val="0" />
      </view>
    </view>

    <view>
      <view class="title">实例方法</view>
      <view class="item">
        <cc-countup
          ref="countup"
          :end="end"
          :autoplay="false"
          :start-val="0"
          :end-val="10000"
        />
      </view>
      <view class="item">
        <cc-button type="primary" size="small" @click="start">开始</cc-button>
        <cc-button type="primary" size="small" @click="pauseResume"
          >暂停</cc-button
        >
        <cc-button type="primary" size="small" @click="reset">重置</cc-button>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const countup = ref()
const end = () => {
  console.log('end')
}
const start = () => {
  countup.value.countup.start(end)
}
const pauseResume = () => {
  countup.value.countup.pauseResume()
}
const reset = () => {
  countup.value.countup.reset()
}
</script>

<style lang="scss" scoped>
.title {
  margin: 20rpx;
}
.item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  button {
    margin-right: 10rpx;
  }
}
</style>
