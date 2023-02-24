<template>
  <view class="contanier">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-circle-progress :percent="50" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义颜色</view>
      <view class="demo-item">
        <cc-circle-progress fill-color="#39b54a" :percent="60" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义宽度</view>
      <view class="demo-item">
        <cc-circle-progress stroke-width="10" :percent="70" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义内容</view>
      <view class="demo-item" style="font-size: 12px">
        <cc-circle-progress :percent="80">
          <view>营业额</view>
        </cc-circle-progress>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.contanier {
  background: #fafafa;
  height: 100vh;
}
</style>
