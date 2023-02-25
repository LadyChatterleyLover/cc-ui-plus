<template>
  <view class="flex a-center container">
    <view class="flex icon">
      <view
        v-for="(item, index) in cloneOptions"
        :key="index"
        class="flex f-col a-center icon-item"
        @click="handleClick(item, index)"
      >
        <view>
          <cc-icon :type="item.icon" :color="item.iconColor" :size="20" />
          <text
            v-if="item.info"
            class="info"
            :style="{ background: item.infoColor }"
            >{{ item.info }}</text
          >
          <view v-if="item.dot" class="dot" />
        </view>
        <view class="text flex a-center j-center">{{ item.text }}</view>
      </view>
    </view>
    <view class="f-1 flex a-center btn">
      <view
        class="btn-icon flex a-center j-center"
        :class="{ 'f-btn': !cloneButtons[0].background }"
        :style="{ background: cloneButtons[0].background }"
        @click="clickButton(cloneButtons[0], 0)"
        >{{ cloneButtons[0].text }}</view
      >
      <view
        v-if="cloneButtons[1]"
        class="btn-icon flex a-center j-center"
        :class="{ 'l-btn': !cloneButtons[1].background }"
        :style="{ background: cloneButtons[1].background }"
        @click="clickButton(cloneButtons[1], 1)"
        >{{ cloneButtons[1].text }}</view
      >
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface GoodsActionOptionItem {
  text: string
  icon: string
  dot?: boolean
  info?: string | number
  iconColor?: string
  infoColor?: string
}
export interface GoodsActionButtonItem {
  text: string
  background?: string
}

const props = withDefaults(
  defineProps<{
    options: GoodsActionOptionItem[]
    buttons: GoodsActionButtonItem[]
  }>(),
  {}
)

const emits = defineEmits<{
  (
    e: 'click',
    val: {
      item: GoodsActionOptionItem
      index: number
    }
  ): void
  (
    e: 'clickButton',
    val: {
      item: GoodsActionButtonItem
      index: number
    }
  ): void
}>()

const cloneOptions = ref<GoodsActionOptionItem[]>([])
const cloneButtons = ref<GoodsActionButtonItem[]>([])
const handleClick = (item: GoodsActionOptionItem, index: number) => {
  emits('click', {
    item,
    index,
  })
}
const clickButton = (item: GoodsActionButtonItem, index: number) => {
  emits('clickButton', {
    item,
    index,
  })
}
onMounted(() => {
  cloneOptions.value.forEach((item: GoodsActionOptionItem) => {
    if (!item.iconColor) item.iconColor = '#323233'
    if (item.info && !item.infoColor) item.infoColor = '#ee0a24'
  })
  cloneButtons.value.forEach((item: GoodsActionButtonItem, index: number) => {
    if (index === 0 && !item.background) item.background = '#ff8917'
    if (index === 1 && !item.background) item.background = '#ee0a24'
  })
})

watch(
  () => props.options,
  (val) => {
    cloneOptions.value = cloneDeep(val)
  },
  { deep: true, immediate: true }
)
watch(
  () => props.buttons,
  (val) => {
    cloneButtons.value = cloneDeep(val)
  },
  { deep: true, immediate: true }
)
</script>

<style scoped lang="scss">
.container {
  height: 100rpx;
  width: 100%;
  padding: 6rpx;
}
.flex {
  display: flex;
}
.f-col {
  flex-direction: column;
}
.a-center {
  align-items: center;
}
.j-center {
  justify-content: center;
}
.f-1 {
  flex: 1;
}
.icon {
  min-width: 96rpx;
  .icon-item {
    margin: 10rpx 24rpx;
    font-size: 14px;
    position: relative;
    top: 10rpx;
    .dot {
      width: 8px;
      min-width: 0;
      height: 8px;
      background-color: #ee0a24;
      border-radius: 100%;
      position: absolute;
      top: -8rpx;
      right: -4rpx;
    }
    .text {
      color: #646566;
    }
    .info {
      position: absolute;
      top: -12rpx;
      right: -8rpx;
      box-sizing: border-box;
      min-width: 16px;
      padding: 0 3px;
      color: #fff;
      font-weight: 500;
      font-size: 12px;
      font-family: -apple-system-font, Helvetica Neue, Arial, sans-serif;
      line-height: 1.2;
      text-align: center;
      border: 1px solid #fff;
      border-radius: 16px;
    }
  }
}
.btn {
  color: #fff;
  height: 100rpx;
  width: 100%;
  font-size: 14px;
  .btn-icon {
    height: 80rpx;
    width: 100%;
    position: relative;
    top: 10rpx;
    &:first-child {
      border-top-left-radius: 999px;
      border-bottom-left-radius: 999px;
    }
    &:last-child {
      border-top-right-radius: 999px;
      border-bottom-right-radius: 999px;
      margin-right: 10rpx;
    }
  }
}
.f-btn {
  background: #ff8917 !important;
}
.l-btn {
  background: #ee0a24 !important;
}
</style>
