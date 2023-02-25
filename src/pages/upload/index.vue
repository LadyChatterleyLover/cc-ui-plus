<template>
  <view class="container">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-upload
          action="http://localhost:3000"
          @upload-success="uploadSuccess"
          @upload-error="uploadError"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义样式</view>
      <view class="demo-item">
        <cc-upload action="http://localhost:3000">
          <cc-button type="primary" round size="small">点击上传</cc-button>
        </cc-upload>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
const uploadSuccess = (data: any) => {
  console.log('uploadSuccess', data)
}
const uploadError = (err: any) => {
  console.log('uploadError', err)
}
</script>

<style scoped lang="scss">
.container {
  height: 100vh;
  .demo {
    margin-bottom: 20rpx;
    &-title {
      padding: #{topx(10)};
    }
    &-item {
      margin-bottom: #{topx(30)};
      padding: 40rpx;
      &:last-child {
        margin-bottom: 0;
      }
    }
  }
}
</style>
