<template>
  <view class="demo">
    <view class="demo-title">基础用法</view>
    <view class="demo-item">
      <view>
        <cc-switch v-model="checked1" @change="onChange" />
      </view>
    </view>

    <view class="demo-title">禁用状态</view>
    <view class="demo-item">
      <view>
        <cc-switch v-model="checked2" disabled />
      </view>
    </view>

    <view class="demo-title">自定义颜色</view>
    <view class="demo-item">
      <view>
        <cc-switch
          v-model="checked3"
          active-color="#ee0a24"
          inactive-color="#abc123"
        />
      </view>
    </view>

    <view class="demo-title">自定义尺寸</view>
    <view class="demo-item">
      <view>
        <cc-switch v-model="checked4" size="16" />
      </view>
      <view>
        <cc-switch v-model="checked5" size="36" />
      </view>
    </view>

    <view class="demo-title">文字显示</view>
    <view class="demo-item">
      <view>
        <cc-switch v-model="checked6" active-text="搁浅" />
      </view>
      <view>
        <cc-switch
          v-model="checked7"
          active-text="威廉古堡"
          inactive-text="双节棍"
        />
      </view>
      <view>
        <cc-switch
          v-model="checked8"
          active-text-color="#ee0a24"
          inactive-text-color="#abc123"
          active-text="黑色幽默"
        />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const checked1 = ref<boolean>(false)
const checked2 = ref<boolean>(false)
const checked3 = ref<boolean>(false)
const checked4 = ref<boolean>(false)
const checked5 = ref<boolean>(false)
const checked6 = ref<boolean>(false)
const checked7 = ref<boolean>(false)
const checked8 = ref<boolean>(false)

const onChange = (val: boolean) => {
  console.log(val)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  &-title {
    padding: 20rpx;
  }
  &-item {
    display: flex;
    align-items: center;
    margin-left: 4rpx;
    view {
      margin-left: 30rpx;
    }
  }
}
</style>
