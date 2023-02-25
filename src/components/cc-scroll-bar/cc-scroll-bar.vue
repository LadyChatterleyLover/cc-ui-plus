<template>
  <view class="cc-scroll-bar">
    <view
      class="cc-scroll-bar-left"
      :style="{ width: leftWidth + 'rpx', top: navHeight + 'rpx' }"
    >
      <scroll-view
        class="cc-scroll-bar-left-scroll"
        scroll-y
        scroll-with-animation
      >
        <view
          v-for="(item, index) in cloneList"
          :key="index"
          class="cc-scroll-bar-left-scroll-item"
          :class="{
            'cc-scroll-bar-left-scroll-item-active': activeIndex === index,
          }"
          :style="{ height: leftHeight + 'rpx', background: leftBgColor }"
          @click="clickLeft(item, index)"
        >
          <view
            v-if="activeIndex === index"
            class="cc-scroll-bar-left-scroll-item-line"
            :style="{
              width: lineWidth + 'rpx',
              height: lineHeight + 'rpx',
              background: lineBgColor,
            }"
          />
          <view class="cc-scroll-bar-left-scroll-item-name">{{
            item.name
          }}</view>
        </view>
      </scroll-view>
    </view>

    <view
      class="cc-scroll-bar-right"
      :style="{
        left: leftWidth + 'rpx',
        width: `calc(100% - ${leftWidth}rpx)`,
      }"
    >
      <scroll-view
        :scroll-into-view="scrollTop"
        scroll-with-animation
        scroll-y
        class="cc-scroll-bar-right-scroll"
        @scroll="scroll"
      >
        <view
          v-for="(item, index) in cloneList"
          :id="item.id"
          :key="index"
          class="cc-scroll-bar-right-scroll-wrap"
          :style="{ height: 74 * Math.ceil(item.goods.length / 3) + 32 + 'px' }"
        >
          <view :key="index" class="cc-scroll-bar-right-scroll-name">{{
            item.name
          }}</view>
          <view class="cc-scroll-bar-right-scroll-content">
            <view
              v-for="(item1, index1) in item.goods"
              :key="index1"
              class="cc-scroll-bar-right-scroll-content-item"
            >
              <view class="cc-scroll-bar-right-scroll-content-item-img"
                ><image :src="item1.img" style="width: 100%; height: 100%"
              /></view>
              <view class="cc-scroll-bar-right-scroll-content-item-name">{{
                item1.name
              }}</view>
            </view>
          </view>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { getCurrentInstance, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface ScrollItemChildren {
  name: string
  key: string
  img: string
  cat: string | number
}

export interface ScrollItem {
  name: string
  id?: string
  goods: ScrollItemChildren[]
}

const instance = getCurrentInstance()

const props = withDefaults(
  defineProps<{
    list: ScrollItem[]
    leftWidth?: number | string
    leftHeight?: number | string
    lineWidth?: number | string
    lineHeight?: number | string
    leftBgColor?: string
    lineBgColor?: string
  }>(),
  {
    leftWidth: 120,
    leftHeight: 80,
    lineWidth: 8,
    lineHeight: 24,
    leftBgColor: '#f6f6f6',
    lineBgColor: '#0081ff',
  }
)

const activeIndex = ref(0)
const scrollTop = ref('')
const cloneList = ref<ScrollItem[]>()
const navHeight = ref(0)

const clickLeft = (item: ScrollItem, index: number) => {
  activeIndex.value = index
  scrollTop.value = `cc-scroll-bar-right-scroll-wrap-${index}`
}

const scroll = () => {
  const offset = 0
  const query = uni.createSelectorQuery().in(instance)
  cloneList.value?.forEach((item, index) => {
    query
      .select(`#${item.id}`)
      .boundingClientRect(({ top, height }: any) => {
        if (top + offset < 0 && Math.abs(top + offset) < height) {
          activeIndex.value = index
        }
      })
      .exec()
  })
}
uni.getSystemInfo({
  success: (res) => {
    navHeight.value = res.windowTop * 2
  },
})
watch(
  () => props.list,
  (val) => {
    cloneList.value = cloneDeep(val)
    cloneList.value.forEach((item, index) => {
      item.id = `cc-scroll-bar-right-scroll-wrap-${index}`
    })
  },
  { deep: true, immediate: true }
)
</script>

<style scoped lang="scss">
.cc-scroll-bar {
  display: flex;
  background: #f6f6f6;
  &-left {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0;
    height: 100%;
    &-scroll {
      height: 100%;
      &-item {
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        font-size: 12px;
        color: #444;
        &-line {
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &-active {
          font-weight: 500;
          color: #000;
          background: #fff !important;
        }
      }
    }
  }
  &-right {
    position: relative;
    padding: 12rpx;
    &-scroll {
      height: 100vh;
      &-wrap {
        margin-bottom: 14rpx;
      }
      &-name {
        font-size: 14px;
        font-weight: 500;
        background: #fff;
        padding: 12rpx;
      }
      &-content {
        width: 100%;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        background: #fff;
        &-item {
          width: 33.3333%;
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          font-size: 12px;
          margin-top: 14rpx;
          &-img {
            width: 100rpx;
            height: 100rpx;
          }
        }
      }
    }
  }
}
</style>
