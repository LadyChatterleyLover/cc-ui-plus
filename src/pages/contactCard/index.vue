<template>
  <view style="background: #f7f7f7; height: 100vh">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-contact-card type="add" @click="onAdd" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">编辑联系人</view>
      <view class="demo-item">
        <cc-contact-card type="edit" name="eason" tel="13888888888" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">不可编辑</view>
      <view class="demo-item">
        <cc-contact-card
          type="edit"
          name="eason"
          tel="13888888888"
          :editable="false"
          @click="onAdd"
        />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
const onAdd = () => {
  console.log('add')
}
</script>

<style scoped lang="scss">
.demo {
  &-title {
    padding: 40rpx;
  }
  &-item {
    margin-bottom: 60rpx;
    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>
