<template>
  <view class="cc-tabs cc-tabs-scroll-view">
    <scroll-view
      :scroll-x="scrollable"
      :scroll-left="scrollLeft"
      scroll-with-animation
      @scroll="handleScroll"
    >
      <view class="cc-tabs-wrap">
        <view
          v-for="(item, index) in list"
          :key="index"
          class="cc-tabs-content"
          @click="clickItem(item, index)"
        >
          <view
            class="cc-tabs-content-title"
            :class="{ disabled: item.disabled }"
            :style="{ color: active === index ? activeColor : inactiveColor }"
          >
            <cc-badge
              v-if="item.badge || item.dot"
              :content="item.badge"
              :dot="item.dot"
              style="height: 100%"
              >{{ item.name }}</cc-badge
            >
            <text v-else>{{ item.name }} </text>
          </view>
        </view>
        <view
          class="cc-tabs-content-line"
          :style="{
            width: lineWidth + 'rpx',
            height: lineHeight + 'rpx',
            background: lineColor,
            transform: `translateX(${translateX}px)`,
          }"
        />
      </view>
    </scroll-view>
    <slot />
  </view>
</template>

<script setup lang="ts">
import { getCurrentInstance, onMounted, ref } from 'vue'
export interface TabItem {
  name: string
  disabled?: boolean
  badge?: string | number
  dot?: boolean
}
const instance = getCurrentInstance()

const props = withDefaults(
  defineProps<{
    list: TabItem[]
    modelValue: string | number
    scrollable?: boolean
    lineWidth?: string | number
    lineHeight?: string | number
    lineColor?: string
    activeColor?: string
    inactiveColor?: string
  }>(),
  {
    scrollable: true,
    lineWidth: 40,
    lineHeight: 6,
    lineColor: '#f56c6c',
    activeColor: '#2979ff',
    inactiveColor: '#303133',
  }
)

const emits = defineEmits<{
  (
    e: 'change',
    val: {
      item: TabItem
      index: number
    }
  ): void
}>()

const scrollLeft = ref(0)
const translateX = ref(0)
const active = ref(0)
const scrollX = ref(0)

const setPosition = () => {
  uni
    .createSelectorQuery()
    .in(instance)
    .selectAll('.cc-tabs-content')
    .boundingClientRect()
    .exec((res) => {
      const width = res[0][0].width
      translateX.value = width / 2 - Number(props.lineWidth) / 4
    })
}

const handleScroll = (val: any) => {
  scrollX.value = val.detail.scrollLeft
}

const clickItem = (item: TabItem, index: number) => {
  if (item.disabled) {
    return
  }
  active.value = index
  uni
    .createSelectorQuery()
    .in(instance)
    .selectAll('.cc-tabs-content')
    .boundingClientRect()
    .exec((res) => {
      const width = res[0][active.value].width
      const offsetLeft = res[0][active.value].left
      const left = offsetLeft - (width - Number(props.lineWidth)) / 2
      scrollLeft.value = left < 0 ? 0 : left
      translateX.value =
        res[0][active.value].left +
        res[0][active.value].width / 2 -
        Number(props.lineWidth) / 2 +
        scrollX.value
    })
  emits('change', {
    item,
    index,
  })
}

onMounted(() => {
  setPosition()
})
</script>

<style lang="scss">
scroll-view ::v-deep ::-webkit-scrollbar {
  display: none;
  width: 0 !important;
  height: 0 !important;
  -webkit-appearance: none;
  background: transparent;
}
.cc-tabs {
  position: relative;
  &-wrap {
    flex: 1;
    display: flex;
    align-items: center;
    position: relative;
  }
  &-content {
    flex: 1 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    height: 88rpx;
    &-title {
      padding: 0px 20rpx;
      font-size: 12px;
      margin-bottom: 20rpx;
    }
    &-line {
      position: absolute;
      bottom: 16rpx;
      transition-duration: 300ms;
    }
  }
  .disabled {
    color: #c8c9cc !important;
    cursor: not-allowed;
  }
}
</style>
