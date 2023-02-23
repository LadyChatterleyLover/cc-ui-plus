<template>
  <view
    class="cc-tag"
    :class="[
      `cc-tag-${type}`,
      `${isPlain}`,
      `cc-tag-${size}`,
      { 'cc-tag-plain': plain },
      { 'cc-tag-round': round },
      { 'cc-tag-circle-left': circleLeft },
      { 'cc-tag-circle-right': circleRight },
    ]"
    :style="{ background: color, ...customColor }"
    @click="handleClick"
  >
    <text :style="{ color: textColor }">
      <slot />
    </text>
    <text v-if="closeable" class="cc-tag-close" @click="close">
      <cc-icon type="closeempty" :color="iconColor" size="12" />
    </text>
  </view>
</template>

<script lang="ts" setup>
import { computed, onMounted, ref } from 'vue'

type TagTypeProps = 'primary' | 'success' | 'error' | 'warning' | 'info'
type TagSizeProps = '' | 'medium' | 'large'

interface ColorList {
  type: 'primary' | 'success' | 'error' | 'warning' | 'info'
  color: string
}
/**
 * Tag 标签
 * @description 该组件用于列表的单个展示项
 *
 * @property {String}    type          标签类型          可选值 primary | success | error | warning | info 默认为primary
 * @property {Boolean}   plain         是否是朴素标签     默认为false
 * @property {Boolean}   round         是否是圆角标签     左右均为圆角  默认为false
 * @property {Boolean}   circleLeft    是否是左圆角标签   仅左侧为圆角  默认为false
 * @property {Boolean}   circleRight   是否是右圆角标签   仅右侧为圆角  默认为false
 * @property {String}    size          标签尺寸          可选值 '' | medium | large 默认为''
 * @property {String}    color         标签颜色          默认为''
 * @property {String}    textColor     标签文字颜色       默认为''
 * @property {Boolean}   closeable     是否可关闭         默认为false
 *
 * @event {Function}	click   点击标签触发
 * @event {Function}	close   可关闭状态点击关闭图标触发
 */
const props = withDefaults(
  defineProps<{
    // 标签类型
    type?: TagTypeProps
    // 朴素标签
    plain?: boolean
    // 圆角标签
    round?: boolean
    // 左圆角
    circleLeft?: boolean
    // 右圆角
    circleRight?: boolean
    // 尺寸
    size?: TagSizeProps
    // 自定义颜色
    color?: string
    // 自定义文字颜色
    textColor?: string
    // 是否可关闭
    closeable?: boolean
  }>(),
  {
    type: 'primary',
    plain: false,
    round: false,
    circleLeft: false,
    circleRight: false,
    size: '',
    color: '',
    textColor: '',
    closeable: false,
  }
)

const emits = defineEmits(['close', 'click'])

// 颜色列表
const colorList: ColorList[] = [
  {
    type: 'primary',
    color: '#0081ff',
  },
  {
    type: 'success',
    color: '#39b54a',
  },
  {
    type: 'error',
    color: '#e54d42',
  },
  {
    type: 'warning',
    color: '#f37b1d',
  },
  {
    type: 'info',
    color: '#909399',
  },
]

// 朴素标签
const isPlain = computed(() => {
  if (props.plain) return `cc-tag-${props.type}-plain`
  else return ''
})
// 自定义颜色时的朴素状态下边框和背景颜色
const customColor = ref()
// 图标颜色
const iconColor = computed(() => {
  if (props.type) {
    if (!props.plain) return '#fff'
    else {
      const item = colorList.find((item) => item.type === props.type)
      return item && item.color
    }
  } else {
    return '#000'
  }
})

// 关闭事件
const close = (e: Event) => {
  emits('close', e)
}
const handleClick = (e: Event) => {
  emits('click', e)
}
onMounted(() => {
  // 自定义颜色情况
  if (props.color) {
    customColor.value = {
      color: '#fff',
      border: `none`,
    }
    if (props.plain) {
      customColor.value = {
        color: props.color,
        background: '#fff',
      }
    }
  }
})
</script>

<style lang="scss" scoped>
.cc-tag {
  position: relative;
  display: inline-flex;
  align-items: center;
  padding: 12rpx;
  color: #fff;
  font-size: 24rpx;
  line-height: 16rpx;
  border-radius: 4px;
  &-close {
    z-index: 999;
  }
  &-plain::after {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    border: 2px solid;
    border-color: inherit;
    border-radius: inherit;
    content: '';
    pointer-events: none;
  }
  &-round {
    border-radius: 48rpx;
  }
  &-circle-left {
    border-radius: 48rpx 0 0 48rpx;
  }
  &-circle-right {
    border-radius: 0 48rpx 48rpx 0;
  }
  &-medium {
    padding: 16rpx;
  }
  &-large {
    padding: 20rpx;
    font-size: 28rpx;
  }
  &-primary {
    background: $primary;
    color: #fff;
  }
  &-primary-plain {
    background: #fff;
    color: $primary;
  }
  &-success {
    background: $success;
    color: #fff;
  }
  &-success-plain {
    background: #fff;
    border-color: $success;
    color: $success;
  }
  &-warning {
    background: $warning;
    color: #fff;
  }
  &-warning-plain {
    background: #fff;
    border-color: $warning;
    color: $warning;
  }
  &-info {
    background: $info;
    color: #fff;
  }
  &-info-plain {
    background: #fff;
    border-color: $info;
    color: $info;
  }
  &-error {
    background: $error;
    color: #fff;
  }
  &-error-plain {
    background: #fff;
    border-color: $error;
    color: $error;
  }
}
</style>
