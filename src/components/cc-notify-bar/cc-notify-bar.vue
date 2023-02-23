<template>
  <view class="cc-notify-bar" :style="{ background: bgColor }">
    <view class="cc-notify-bar-left" @click="clickLeft">
      <view v-if="volume"
        ><cc-icon :color="color" type="sound" size="16"
      /></view>
      <view v-else>
        <slot v-if="$slots.left" name="left" />
        <cc-icon v-else :color="color" :type="leftIcon" />
      </view>
    </view>
    <view class="cc-notify-bar-wrap" @click="onClick">
      <view
        v-if="!vertical"
        class="cc-notify-bar-wrap-content"
        :style="{ color, animationDuration }"
      >
        <template v-if="list.length">
          <view>{{ list.join(',') }}</view>
        </template>
        <template v-else>
          <slot v-if="$slots.default" />
          <view v-else>{{ text }}</view>
        </template>
      </view>
      <view
        v-else
        class="cc-notify-bar-wrap-content-vertical"
        :style="{ color }"
      >
        <swiper
          class="cc-notify-bar-wrap-content-vertical-swiper"
          vertical
          circular
          :indicator-dots="false"
          :autoplay="true"
          :interval="interval"
          :duration="500"
        >
          <swiper-item v-for="(item, index) in list" :key="index">
            <view class="cc-notify-bar-wrap-content-vertical-swiper-item">{{
              item
            }}</view>
          </swiper-item>
        </swiper>
      </view>
    </view>
    <view class="cc-notify-bar-right" @click="clickRight">
      <view v-if="closeable || link">
        <cc-icon v-if="closeable" type="closeempty" :color="color" size="16" />
        <cc-icon v-else type="arrowright" :color="color" size="16" />
      </view>
      <view v-else>
        <slot v-if="$slots.right" name="right" />
        <cc-icon v-else :type="$slots.right" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import {
  defineEmits,
  defineProps,
  getCurrentInstance,
  nextTick,
  onMounted,
  ref,
} from 'vue'

const instance = getCurrentInstance()
const id = instance.uid
const props = withDefaults(
  defineProps<{
    list?: string[]
    volume?: boolean
    closeable?: boolean
    link?: boolean
    vertical?: boolean
    text?: string
    bgColor?: string
    color?: string
    leftIcon?: string
    rightIcon?: string
    speed?: string | number
    interval?: string | number
  }>(),
  {
    list: () => [],
    volume: false,
    closeable: false,
    link: false,
    vertical: false,
    text: '',
    bgColor: '#fff7cc',
    color: '#f60',
    leftIcon: '',
    rightIcon: '',
    speed: 60,
    interval: 2000,
  }
)

const emits = defineEmits(['click', 'clickLeft', 'clickRight'])

// 滚动的文字宽度
const textWidth = ref<number>(0)
// 动画时长
const animationDuration = ref<string>('10s')
// 初始化动画时长
const init = () => {
  nextTick(() => {
    uni
      .createSelectorQuery()
      .in(instance)
      .select(`#cc-notify-bar-wrap-content-${id}`)
      .boundingClientRect((res) => {
        textWidth.value = res?.width
      })
      .exec()
    animationDuration.value = `${textWidth.value / (props.speed as number)}s`
  })
}
// 点击通知栏
const onClick = () => {
  emits('click')
}
// 点击左侧图标
const clickLeft = () => {
  if (props.volume) emits('clickLeft')
}
// 点击右侧图标
const clickRight = () => {
  if (props.closeable || props.link) emits('clickRight')
}
onMounted(() => {
  init()
})
</script>

<style scoped lang="scss">
.cc-notify-bar {
  width: 100%;
  display: flex;
  align-items: center;
  height: 40px;
  padding: 0 32rpx;
  font-size: 14px;
  &-left {
    margin-right: 16rpx;
  }
  &-wrap {
    position: relative;
    height: 100%;
    display: flex;
    align-items: center;
    flex: 1;
    overflow: hidden;
    &-content {
      white-space: nowrap;
      position: absolute;
      padding-left: 100%;
      animation: loop 10s linear infinite both;
      &-vertical {
        display: flex;
        height: 100%;
        width: 100%;
        &-swiper {
          width: 100%;
          height: 100%;
          &-item {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
          }
        }
      }
    }
  }
  &-right {
    position: relative;
    left: 16rpx;
  }
}
@keyframes loop {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-100%);
  }
}
</style>
