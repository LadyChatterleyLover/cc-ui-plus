<template>
  <view class="cc-swiper" :style="{ height: height + 'rpx' }">
    <swiper
      class="cc-swiper-wrap"
      :autoplay="autoplay"
      :duration="duration"
      :indicator-dots="false"
      :circular="circular"
      :vertical="vertical"
      :current="current"
      :interval="interval"
      :disable-touch="disableTouch"
      @change="handleChange"
    >
      <swiper-item
        v-for="(item, index) in list"
        :key="index"
        class="cc-swiper-item"
        @longpress.prevent="longpress(item, index, $event)"
        @click="clickItem(item, index)"
      >
        <view class="cc-swiper-item-img">
          <view v-if="$slots.default"
            ><slot :current="currentIndex" :item="item" :index="index"
          /></view>
          <image
            v-else
            class="cc-swiper-item-img-content"
            :src="item.image"
            :mode="imgMode"
          />
        </view>
      </swiper-item>
    </swiper>
    <view v-if="$slots.dot"
      ><slot name="dot" :current="currentIndex + 1"
    /></view>
    <view v-else>
      <view
        v-if="showDot && mode !== 'number'"
        class="cc-swiper-dot"
        :class="{ 'cc-swiper-dot-translate': !right }"
        :style="{ bottom: bottom + 'rpx', right: right + 'rpx' }"
      >
        <view
          v-for="(item, index) in list.length"
          :key="index"
          class="cc-swiper-dot-item"
          :class="{
            'cc-swiper-dot-item-round':
              currentIndex === index && mode === 'round',
            'cc-swiper-dot-item-rect': mode === 'rect',
          }"
          :style="{
            background:
              currentIndex === index ? activeColor : indicatorActiveColor,
            ...dotStyle,
          }"
        />
      </view>
      <text v-if="mode === 'number'" class="cc-swiper-dot-number">
        <text>{{ currentIndex + 1 }} / {{ list.length }}</text>
      </text>
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import type { CSSProperties } from 'vue'

export interface SwiperItem {
  image: string
}

const props = withDefaults(
  defineProps<{
    list: SwiperItem[]
    current?: number | string
    swipeDuration?: number | string
    height?: number | string
    mode?: 'round' | 'rect' | 'number' | 'circle' | 'none'
    imgMode?: string
    bottom?: string | number
    right?: string | number
    activeColor?: string
    indicatorActiveColor?: string
    circular?: boolean
    autoplay?: boolean
    interval?: number
    vertical?: boolean
    disableTouch?: boolean
    dotStyle?: CSSProperties
    duration?: number | string
  }>(),
  {
    current: 0,
    swipeDuration: 300,
    mode: 'circle',
    imgMode: 'aspectFill',
    height: 300,
    bottom: 20,
    right: '',
    activeColor: '#fff',
    indicatorActiveColor: '#ccc',
    circular: true,
    autoplay: true,
    vertical: false,
    interval: 5000,
    duration: 500,
    disableTouch: false,
    dotStyle: () => ({}),
  }
)

const emits = defineEmits<{
  (e: 'change', val: number): void
  (e: 'click', val: { item: SwiperItem; index: number }): void
  (e: 'longpress', val: { item: SwiperItem; index: number; e: Event }): void
}>()

const showDot = ref(true)
const currentIndex = ref(0)

const handleChange = (e: { detail: { current: number } }) => {
  currentIndex.value = e.detail.current
  emits('change', currentIndex.value)
}

const clickItem = (item: SwiperItem, index: number) => {
  emits('click', {
    item,
    index,
  })
}

const longpress = (item: SwiperItem, index: number, e: Event) => {
  e.preventDefault()
  emits('longpress', {
    item,
    index,
    e,
  })
}

onMounted(() => {
  if (props.mode === 'none') {
    showDot.value = false
  }
})

watch(
  () => props.current,
  (val) => {
    currentIndex.value = val as number
  },
  { immediate: true }
)
</script>

<style scoped lang="scss">
.cc-swiper {
  width: 100%;
  position: relative;
  &-wrap {
    width: 100%;
    height: 100%;
  }
  &-item {
    width: 100%;
    height: 100%;
    &-img {
      width: 100%;
      height: 100%;
      &-content {
        width: 100%;
        height: 100%;
      }
    }
  }
  &-dot {
    display: flex;
    align-items: center;
    position: absolute;
    &-translate {
      left: 50%;
      transform: translateX(-50%);
    }
    &-number {
      padding: 4rpx 12rpx;
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 48rpx;
      font-size: 12px;
      color: #fff;
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      bottom: 20rpx;
    }
    &-item {
      margin: 0 4rpx;
      border-radius: 16rpx;
      transition: all 0.3s;
      width: 12rpx;
      height: 12rpx;
      &-round {
        width: 28rpx;
      }
      &-rect {
        width: 20rpx;
        height: 6rpx;
      }
    }
  }
}
</style>
