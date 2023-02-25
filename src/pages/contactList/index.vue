<template>
  <view style="background: #f7f7f7; height: 100vh">
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-contact-list :list="list1" :value="value" @select="handleSelect" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用联系人</view>
      <view class="demo-item">
        <cc-contact-list :list="list2" :value="value" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('1')
const list1 = [
  {
    id: '1',
    name: '张三',
    tel: '13000000000',
    isDefault: true,
  },
  {
    id: '2',
    name: '李四',
    tel: '1310000000',
  },
]
const list2 = [
  {
    id: '1',
    name: '张三',
    tel: '13000000000',
    isDefault: true,
  },
  {
    id: '2',
    name: '李四',
    tel: '1310000000',
    disabled: true,
  },
]

const handleSelect = (val: any) => {
  console.log('select', val)
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
