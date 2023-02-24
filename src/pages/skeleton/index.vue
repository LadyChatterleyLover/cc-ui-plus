<template>
  <view style="padding: 20rpx">
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-skeleton title :row="3" />
      </view>
    </view>

    <view>
      <view class="title">展示头像</view>
      <view class="item">
        <cc-skeleton title avatar :row="3" />
      </view>
    </view>

    <view>
      <view class="title">头像大小</view>
      <view class="item">
        <cc-skeleton title avatar avatar-size="40" :row="3" />
      </view>
    </view>

    <view>
      <view class="title">方形头像</view>
      <view class="item">
        <cc-skeleton title avatar avatar-size="40" :round="false" :row="3" />
      </view>
    </view>

    <view>
      <view class="title">展示动画</view>
      <view class="item">
        <cc-skeleton animate title avatar :row="5" />
      </view>
    </view>

    <view>
      <view class="title">自定义颜色</view>
      <view class="item">
        <cc-skeleton bg-color="red" title avatar :row="5" />
      </view>
    </view>

    <view>
      <view class="title">控制显示</view>
      <cc-button size="small" type="primary" @click="handleClick">
        点击2秒之后骨架屏消失
      </cc-button>
      <view v-if="loading" class="item">
        <cc-skeleton :loading="loading" title :row="3" />
      </view>
      <view v-if="!loading">你的背包,让我走的好缓慢</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const loading = ref<boolean>(true)
const handleClick = () => {
  setTimeout(() => {
    loading.value = false
  }, 2000)
}
</script>

<style scoped lang="scss">
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
