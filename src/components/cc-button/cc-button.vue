<template>
  <button
    class="cc-button"
    :hover-class="hoverClassName"
    :loading="loading"
    :class="[
      `${typeClass}`,
      `${sizeClass}`,
      `${isPlain}`,
      { 'cc-button-block': block },
      { 'cc-button-disabled': disabled },
      { 'cc-button-round': round },
    ]"
    :style="{ background: color, ...customColor, ...customStyle }"
    @click="handleClick"
  >
    <div class="cc-button-content">
      <cc-icon
        v-if="icon"
        class="cc-button-icon"
        :color="iconColor"
        :type="icon"
        :size="iconSize"
      />
      <text :class="{ 'cc-button-text': icon }">
        <slot />
      </text>
    </div>
  </button>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import type { CSSProperties } from 'vue'
import type { SizeProps, TypeProps } from '../../types'

interface ColorList {
  type: string
  color: string
}
/**
 * Tag 按钮
 * @description 该组件是基于uni-button二次封装,支持更多扩展和自定义。
 *
 * @property {String}    type          按钮类型          可选值 default | primary | success | error | warning | info 默认为default
 * @property {String}    size          按钮尺寸          可选值 default | mini | small | large 默认为default
 * @property {String}    text          按钮文本          默认为'' 
 * @property {String}    icon          按钮图标          同icon组件type属性 
 * @property {Boolean}   plain         是否是朴素按钮     默认为false
 * @property {Boolean}   round         是否是圆角按钮     默认为false
 * @property {Boolean}   block         是否是块级按钮     默认为false
 * @property {Boolean}   disabled      是否是禁用按钮     默认为false
 * @property {Boolean}   loading       是否是加载状态     默认为false
 * @property {String}    color         按钮颜色          默认为''
 * @property {String}    hoverClass    点击时的类名       默认为''
 * @property {String}    hoverClass    点击时的类名       默认为''
 * @property {Object}    customStyle   自定义样式        默认为{}
 
 *
 * @event {Function}	click   点击按钮触发 参数为事件参数
 */

const props = withDefaults(
  defineProps<{
    type?: TypeProps
    size?: SizeProps
    text?: string
    color?: string
    icon?: string
    block?: boolean
    plain?: boolean
    round?: boolean
    disabled?: boolean
    loading?: boolean
    hoverClass?: string
    customStyle?: CSSProperties
  }>(),
  {
    type: 'default',
    size: 'default',
    text: '',
    color: '',
    icon: '',
    block: false,
    plain: false,
    round: false,
    disabled: false,
    loading: false,
    hoverClass: '',
  }
)

const emits = defineEmits(['click'])

// 自定义颜色时的朴素状态下边框和背景颜色
const customColor = ref()
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

// 是否是朴素按钮
const isPlain = computed(() => {
  if (props.plain) return `cc-button-${props.type}-plain`
  else return ''
})
// 按钮类型
const typeClass = computed(() => {
  if (props.type) return `cc-button-${props.type}`
  else return ''
})

// 尺寸
const sizeClass = computed(() => {
  if (props.size) return `cc-button-${props.size}`
  else return ''
})

// hoverclass
const hoverClassName = computed(() => {
  if (props.hoverClass) return props.hoverClass
  else {
    if (!props.plain) {
      return `cc-button-hover`
    } else {
      return `cc-button-${props.type}-hover`
    }
  }
})

// 图标大小
const iconSize = computed(() => {
  if (props.size === 'large') {
    return 28
  } else if (props.size === 'default') {
    return 24
  } else if (props.size === 'small') {
    return 20
  } else {
    return 16
  }
})

// 图标颜色
const iconColor = computed(() => {
  if (props.type) {
    if (!props.plain && props.type !== 'default') return '#fff'
    else if (!props.plain && props.type === 'default') return '#333'
    else {
      const item = colorList.find((item) => item.type === props.type)
      return item && item.color
    }
  } else {
    return '#000'
  }
})

const handleClick = () => {
  /** loading 状态不可点击触发click事件 */
  if (props.loading) return
  emits('click')
}

onMounted(() => {
  // 自定义颜色情况
  if (props.color) {
    customColor.value = {
      color: '#fff',
    }
    if (props.plain) {
      customColor.value = {
        color: props.color,
        border: `1px solid ${props.color}`,
        background: '#fff',
      }
    }
  }
})
</script>

<style scoped lang="scss">
.cc-button {
  position: relative;
  display: inline-flex;
  box-sizing: border-box;
  height: 88rpx;
  margin: 0;
  padding: 0;
  font-size: 32rpx;
  justify-content: center;
  align-items: center;
  border-radius: 4rpx;
  cursor: pointer;
  transition: opacity 0.2s;
  padding: 0 30rpx;
  background: #fff;
  color: #000;
  border: 1px solid #ebedf0;
  &:hover {
    opacity: 0.75;
    // transform: translate(1px, 1px);
  }
  &:active {
    transform: translate(2rpx, 2rpx);
  }
  .cc-button-content {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    div {
      height: 100%;
    }
    .cc-button-text {
      margin-left: 8rpx;
    }
    .cc-button-icon {
      margin-left: 12rpx;
    }
  }
  &-block {
    width: 100%;
  }
  &-hover {
    opacity: 0.8;
  }
  &-large {
    height: 100rpx;
  }
  &-small {
    height: 64rpx;
  }
  &-mini {
    height: 48rpx;
  }
  &-disabled {
    cursor: not-allowed;
    opacity: 0.5;
    // pointer-events: none;
  }
  &-round {
    border-radius: 48rpx;
  }
  &-primary {
    background: $primary;
    border: 1px solid $primary;
    color: #fff;
  }
  &-primary-plain {
    background: #fff;
    border: 1px solid $primary;
    color: $primary;
  }
  &-primary-hover {
    background: $primary;
    color: #fff;
  }
  &-success {
    background: $success;
    border: 1px solid $success;
    color: #fff;
  }
  &-success-plain {
    background: #fff;
    border: 1px solid $success;
    color: $success;
  }
  &-success-hover {
    background: $success;
    color: #fff;
  }
  &-warning {
    background: $warning;
    border: 1px solid $warning;
    color: #fff;
  }
  &-warning-plain {
    background: #fff;
    border: 1px solid $warning;
    color: $warning;
  }
  &-warning-hover {
    background: $warning;
    color: #fff;
  }
  &-info {
    background: $info;
    border: 1px solid $info;
    color: #fff;
  }
  &-info-plain {
    background: #fff;
    border: 1px solid $info;
    color: $info;
  }
  &-info-hover {
    background: $info;
    color: #fff;
  }
  &-error {
    background: $error;
    border: 1px solid $error;
    color: #fff;
  }
  &-error-plain {
    background: #fff;
    border: 1px solid $error;
    color: $error;
  }
  &-error-hover {
    background: $error;
    color: #fff;
  }
}
</style>
