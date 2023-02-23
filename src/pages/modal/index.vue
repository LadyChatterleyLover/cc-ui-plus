<template>
  <view>
    <view>
      <view class="title">基础用法</view>
      <view class="item">
        <cc-modal
          v-model="show1"
          title="最佳损友"
          content="问我有没有,确实也没有,一直躲避的借口,非什么大仇,为何旧知己到最后,变不到老友"
        />
        <cc-button type="primary" @click="open1">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">无标题</view>
      <view class="item">
        <cc-modal
          v-model="show2"
          content="朋友我当你一秒朋友,朋友,我当你一世朋友"
        />
        <cc-button type="primary" @click="open2">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">确认弹框</view>
      <view class="item">
        <cc-modal
          v-model="show3"
          show-cancel-button
          title="最佳损友"
          content="严重似情侣讲分手"
        />
        <cc-button type="primary" @click="open3">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">点击遮罩层可关闭</view>
      <view class="item">
        <cc-modal
          v-model="show4"
          title="最佳损友"
          close-on-click-overlay
          content="我有痛快过你有没有"
        />
        <cc-button type="primary" @click="open4">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">自定义颜色和文字</view>
      <view class="item">
        <cc-modal
          v-model="show5"
          show-cancel-button
          confirm-text="立即登录"
          cancel-text="继续浏览"
          confirm-color="#e54d42"
          cancel-color="#0081ff"
          title="最佳损友"
          content="严重似情侣讲分手"
        />
        <cc-button type="primary" @click="open5">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">异步关闭</view>
      <view class="item">
        <cc-modal
          ref="modal"
          v-model="show6"
          async-close
          show-cancel-button
          title="最佳损友"
          content="挂念这旧友"
          @confirm="confirm"
        />
        <cc-button type="primary" @click="open6">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">富文本内容</view>
      <view class="item">
        <cc-modal v-model="show7" show-cancel-button title="东邪西毒">
          <template #content>
            <view style="height: 300rpx" v-html="content" />
          </template>
        </cc-modal>
        <cc-button type="primary" @click="open7">提示弹框</cc-button>
      </view>
    </view>

    <view>
      <view class="title">自定义按钮</view>
      <view class="item">
        <cc-modal
          v-model="show8"
          async-close
          show-cancel-button
          title="东邪西毒"
          content="看见他走的时候,我的内心在嫉妒"
        >
          <template #button>
            <view class="btn" @click="clickButton">确认</view>
          </template>
        </cc-modal>
        <cc-button type="primary" @click="open8">提示弹框</cc-button>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
const show1 = ref(false)
const show2 = ref(false)
const show3 = ref(false)
const show4 = ref(false)
const show5 = ref(false)
const show6 = ref(false)
const show7 = ref(false)
const show8 = ref(false)

const content = `
      <span>
        嫣: 告诉我,你最爱的女人是哪一位?  
      </span>
      <span>
        峰: 不是你咯。  
      </span>
    `
const modal = ref()

const confirm = () => {
  setTimeout(() => {
    modal.value.close()
  }, 3000)
}
const clickButton = () => {
  show8.value = false
}
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
}
.btn {
  background: linear-gradient(to right, #ff6034, #ee0a24);
  color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 66rpx;
  width: 92%;
  margin-left: 4%;
  border-radius: 48rpx;
  font-size: 28rpx;
}
</style>
