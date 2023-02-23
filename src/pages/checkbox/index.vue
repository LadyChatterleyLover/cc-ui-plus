<template>
  <view>
    <view style="margin-bottom: 40rpx">
      <view class="title">基础用法</view>
      <view class="item">
        <cc-checkbox v-model="checked1">复选框</cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">禁用状态</view>
      <view class="item">
        <cc-checkbox v-model="checked2" disabled>复选框</cc-checkbox>
      </view>
      <view class="item">
        <cc-checkbox v-model="checked3" disabled>复选框</cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义形状</view>
      <view class="item">
        <cc-checkbox v-model="checked4" shape="square">复选框</cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义颜色</view>
      <view class="item">
        <cc-checkbox v-model="checked5" checked-color="#ee0a24"
          >复选框</cc-checkbox
        >
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义大小</view>
      <view class="item">
        <cc-checkbox v-model="checked6" icon-size="48">复选框</cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">自定义图标</view>
      <view class="item">
        <cc-checkbox v-model="checked7">
          自定义图标
          <template #icon="props">
            <img
              class="img-icon"
              :src="
                props.checked
                  ? 'https://fastly.jsdelivr.net/npm/@vant/assets/user-active.png'
                  : 'https://fastly.jsdelivr.net/npm/@vant/assets/user-inactive.png'
              "
            />
          </template>
        </cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">禁用文本点击</view>
      <view class="item">
        <cc-checkbox v-model="checked8" label-disabled>复选框</cc-checkbox>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">复选框组</view>
      <view class="item">
        <cc-checkbox-group v-model="checked9">
          <cc-checkbox name="a">复选框 a</cc-checkbox>
          <cc-checkbox name="b">复选框 b</cc-checkbox>
        </cc-checkbox-group>
      </view>
    </view>

    <view style="margin-bottom: 40rpx">
      <view class="title">水平排列</view>
      <view class="item">
        <cc-checkbox-group
          v-model="checked10"
          direction="horizontal"
          @change="handleChange"
        >
          <cc-checkbox name="a">复选框 a</cc-checkbox>
          <cc-checkbox name="b">复选框 b</cc-checkbox>
        </cc-checkbox-group>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const checked1 = ref(false)
const checked2 = ref(false)
const checked3 = ref(true)
const checked4 = ref(false)
const checked5 = ref(false)
const checked6 = ref(false)
const checked7 = ref(false)
const checked8 = ref(false)
const checked9 = ref(['a', 'b'])
const checked10 = ref(['a', 'b'])

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
