<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-fab />
      </view>
    </view>

    <view>
      <view class="title">自定义位置</view>
      <view class="item">
        <cc-fab bottom="200" right="200" />
      </view>
    </view>

    <view>
      <view class="title">自定义内容</view>
      <view class="item">
        <cc-fab bottom="300"> 开 </cc-fab>
      </view>
    </view>

    <view>
      <view class="title">可拖拽</view>
      <view class="item">
        <cc-fab bottom="500" draggable icon="paperclip" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts"></script>

<style lang="scss" scoped>
.title {
  margin: 20rpx;
}
.item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin: 20rpx;
}
</style>
