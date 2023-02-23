<template>
  <view>
    <view>
      <view class="title">基础语法</view>
      <view class="item">
        <cc-popup v-model="show1">红是朱砂痣烙印心口</cc-popup>
        <cc-button size="small" @click="open1">打开弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">弹出框方向</view>
      <view class="item">
        <cc-popup v-model="show2" mode="left">红是蚊子血般平庸</cc-popup>
        <cc-button @click="open2">左边打开</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show3" mode="right">
          <view class="flex">从背后抱你的时候</view>
        </cc-popup>
        <cc-button @click="open3">右边打开</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show4" mode="top">期待的却是他的面容</cc-popup>
        <cc-button @click="open4">顶部打开</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show5" mode="bottom">
          <view>握在手中却流失于指缝</view>
        </cc-popup>
        <cc-button @click="open5">底部打开</cc-button>
      </view>
    </view>

    <view>
      <view class="title">关闭按钮</view>
      <view class="item">
        <cc-popup v-model="show6" closeable>我不好爱</cc-popup>
        <cc-button @click="open6">关闭按钮</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show7" mode="left" closeable
          >你又美丽又伟大</cc-popup
        >
        <cc-button @click="open7">关闭按钮</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show8" close-icon="search" closeable
          >得你一人越来越可爱</cc-popup
        >
        <cc-button @click="open8">自定义关闭图标</cc-button>
      </view>
    </view>

    <view>
      <view class="title">自定义宽高</view>
      <view class="item">
        <cc-popup v-model="show9" width="600" closeable>烟味</cc-popup>
        <cc-button @click="open9">自定义宽度</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show10" width="600" mode="left" closeable
          >颓废,混杂着烟味</cc-popup
        >
        <cc-button @click="open10">自定义宽度</cc-button>
      </view>
      <view class="item">
        <cc-popup v-model="show11" height="600" mode="bottom" closeable>
          <view class="content">我满脸胡渣</view>
        </cc-popup>
        <cc-button @click="open11">自定义高度</cc-button>
      </view>
    </view>

    <view>
      <view class="title">圆角弹框</view>
      <view class="item">
        <cc-popup v-model="show12" round mode="bottom">一丝不挂</cc-popup>
        <cc-button @click="open12">圆角弹框</cc-button>
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
const show10 = ref<boolean>(false)
const show11 = ref<boolean>(false)
const show12 = ref<boolean>(false)
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
const open10 = () => {
  show10.value = true
}
const open11 = () => {
  show11.value = true
}
const open12 = () => {
  show12.value = true
}
</script>

<style scoped lang="scss">
.title {
  margin: 20rpx;
}
.item {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin: 20rpx;
}
.flex {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
.content {
  height: 1000rpx;
}
</style>
