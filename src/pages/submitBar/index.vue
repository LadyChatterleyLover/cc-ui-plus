<template>
  <view>
    <view class="item">普通用法</view>
    <cc-submit-bar :price="price" @submit="submit" />
    <view class="item">禁止提交</view>
    <cc-submit-bar :price="price" :disabled="true" @submit="submit" />
    <view class="item">加载状态</view>
    <cc-submit-bar :price="price" :loading="true" />
    <view class="item">控制显示小数点位数</view>
    <cc-submit-bar :price="price" :decimal-length="1" />
    <view class="item">控制显示货币符号</view>
    <cc-submit-bar :price="price" :decimal-length="1" currency="$" />
    <view class="item">自定义按钮颜色和文字</view>
    <cc-submit-bar :price="price" button-color="#07c160" button-text="结算" />
    <view class="item">自定义内容</view>
    <cc-submit-bar :price="price">
      <checkbox checked="true" />全选
    </cc-submit-bar>
    <view class="item">自定义顶部内容</view>
    <cc-submit-bar :price="price">
      <template #tip>
        你的收货地址不支持同城送, <text style="color: #007aff">修改地址</text>
      </template>
    </cc-submit-bar>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const price = ref(3050)

const submit = () => {
  console.log('submit')
}
</script>

<style scoped>
.item {
  font-size: 18px;
  font-weight: 700;
}
</style>
