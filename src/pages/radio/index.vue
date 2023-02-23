<template>
  <view>
    <view style="margin-bottom: 40rpx">
      <view class="title">基础用法</view>
      <view class="item">
        <cc-radio-group v-model="checked1" @change="handleChange">
          <cc-radio name="a">单选框</cc-radio>
          <cc-radio name="b">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">水平排列</view>
      <view class="item">
        <cc-radio-group v-model="checked2" direction="horizontal">
          <cc-radio name="a">单选框</cc-radio>
          <cc-radio name="b">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">禁用状态</view>
      <view class="item">
        <cc-radio-group v-model="checked3" disabled>
          <cc-radio name="a">单选框</cc-radio>
          <cc-radio name="b">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义形状</view>
      <view class="item">
        <cc-radio-group v-model="checked4">
          <cc-radio name="a" shape="square">单选框</cc-radio>
          <cc-radio name="b" shape="square">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义颜色</view>
      <view class="item">
        <cc-radio-group v-model="checked5">
          <cc-radio name="a" checked-color="#ee0a24">单选框</cc-radio>
          <cc-radio name="b" checked-color="#ee0a24">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义大小</view>
      <view class="item">
        <cc-radio-group v-model="checked6">
          <cc-radio name="a" icon-size="48">单选框</cc-radio>
          <cc-radio name="b" icon-size="48">单选框</cc-radio>
        </cc-radio-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义大小</view>
      <view class="item">
        <cc-radio-group v-model="checked7">
          <cc-radio name="a">
            单选框 1
            <template #icon="props">
              <img
                class="img-icon"
                :src="props.checked ? activeIcon : inactiveIcon"
              />
            </template>
          </cc-radio>
          <cc-radio name="b">
            单选框 2
            <template #icon="props">
              <img
                class="img-icon"
                :src="props.checked ? activeIcon : inactiveIcon"
              />
            </template>
          </cc-radio>
        </cc-radio-group>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checked1 = ref('a')
const checked2 = ref('a')
const checked3 = ref('a')
const checked4 = ref('a')
const checked5 = ref('a')
const checked6 = ref('a')
const checked7 = ref('a')

const activeIcon =
  'https://fastly.jsdelivr.net/npm/@vant/assets/user-active.png'
const inactiveIcon =
  'https://fastly.jsdelivr.net/npm/@vant/assets/user-inactive.png'

const handleChange = (val: any) => {
  console.log(val)
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
  margin: 20rpx;
}
.img-icon {
  width: 40rpx;
  height: 40rpx;
}
</style>
