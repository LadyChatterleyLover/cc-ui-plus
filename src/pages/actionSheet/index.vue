<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-action-sheet v-model="show1">
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open1">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">圆角菜单</view>
      <view class="item">
        <cc-action-sheet v-model="show2" round>
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open2">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">描述信息</view>
      <view class="item">
        <cc-action-sheet v-model="show3" description="这是一段描述信息">
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open3">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">取消按钮</view>
      <view class="item">
        <cc-action-sheet
          v-model="show4"
          show-cancel
          description="这是一段描述信息"
        >
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open4">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">关闭按钮</view>
      <view class="item">
        <cc-action-sheet
          v-model="show5"
          closeable
          show-cancel
          description="这是一段描述信息"
        >
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open5">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">点击取消按钮不可关闭</view>
      <view class="item">
        <cc-action-sheet
          v-model="show6"
          :close-on-click-action="false"
          closeable
          show-cancel
          description="这是一段描述信息"
        >
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open6">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">取消文字</view>
      <view class="item">
        <cc-action-sheet
          v-model="show7"
          cancel-text="关闭"
          show-cancel
          description="这是一段描述信息"
        >
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open7">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">禁用和自定义样式</view>
      <view class="item">
        <cc-action-sheet
          v-model="show8"
          cancel-text="关闭"
          show-cancel
          description="这是一段描述信息"
        >
          <cc-action-sheet-item name="选项一" color="#ee0a24" />
          <cc-action-sheet-item name="选项二" disabled />
          <cc-action-sheet-item name="选项三" font-size="50" />
          <cc-action-sheet-item name="选项三" loading />
        </cc-action-sheet>
        <cc-button @click="open8">打开操作列表</cc-button>
      </view>
    </view>

    <view>
      <view class="title">自定义标题</view>
      <view class="item">
        <cc-action-sheet v-model="show9" cancel-text="关闭" title="标题">
          <cc-action-sheet-item name="选项一" />
          <cc-action-sheet-item name="选项二" />
          <cc-action-sheet-item name="选项三" />
        </cc-action-sheet>
        <cc-button @click="open9">打开操作列表</cc-button>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const show1 = ref<boolean>(false)
const show2 = ref<boolean>(false)
const show3 = ref<boolean>(false)
const show4 = ref<boolean>(false)
const show5 = ref<boolean>(false)
const show6 = ref<boolean>(false)
const show7 = ref<boolean>(false)
const show8 = ref<boolean>(false)
const show9 = ref<boolean>(false)

const open1 = () => {
  show1.value = true
}
const open2 = () => {
  show2.value = true
}
const open3 = () => {
  show3.value = true
}
const open4 = () => {
  show4.value = true
}
const open5 = () => {
  show5.value = true
}
const open6 = () => {
  show6.value = true
}
const open7 = () => {
  show7.value = true
}
const open8 = () => {
  show8.value = true
}
const open9 = () => {
  show9.value = true
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
  margin: 20rpx 0;
  div {
    margin: 10rpx;
  }
}
.content {
  padding: 32rpx 32rpx 320rpx;
}
</style>
