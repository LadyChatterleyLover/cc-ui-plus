<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item"
        ><cc-alert
          type="primary"
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="success"
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="error"
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="info"
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">左侧图标</view>
      <view class="demo-item"
        ><cc-alert
          type="primary"
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="success"
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="error"
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          title="说好的幸福呢"
          show-icon
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="info"
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">可关闭</view>
      <view class="demo-item"
        ><cc-alert
          type="primary"
          closeable
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="success"
          show-icon
          closeable
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="error"
          show-icon
          closeable
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          title="说好的幸福呢"
          show-icon
          closeable
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
      <view class="demo-item"
        ><cc-alert
          type="info"
          show-icon
          closeable
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
    </view>

    <view class="demo">
      <view class="demo-title">手动控制显示隐藏</view>
      <cc-button size="small" type="primary" @click="show = !show">{{
        show ? '隐藏' : '显示'
      }}</cc-button>
      <view class="demo-item"
        ><cc-alert
          v-model="show"
          type="primary"
          closeable
          show-icon
          title="说好的幸福呢"
          description="怎么了,你累了,说好的,幸福呢"
      /></view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const show = ref(false)
</script>

<style scoped lang="scss">
.demo {
  margin: 20rpx;
  &-title {
    margin: 10rpx;
  }
  &-item {
    margin: 20rpx 0;
  }
}
</style>
