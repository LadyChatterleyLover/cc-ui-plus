<template>
  <view
    class="cc-cell"
    :class="[{ 'cc-cell-large': size }, { 'cc-cell-border': border }]"
    @click="onClick"
  >
    <view>
      <view class="cc-cell-title">
        <view v-if="icon" class="cc-cell-left-icon">
          <cc-icon :size="iconSize" :type="icon" color="#323233" />
          <slot name="left-icon" />
        </view>
        <view>
          {{ title }}
          <slot v-if="!title" name="title" />
        </view>
      </view>
      <view class="cc-cell-label">
        {{ label }}
        <slot v-if="!label" name="label" />
      </view>
    </view>
    <view class="cc-cell-value">
      <view class="cc-cell-value-wrap">
        {{ value }}
        <slot v-if="!value" name="value" />
      </view>
      <view class="cc-cell-right-icon">
        <cc-icon
          v-if="isLink"
          color="#969799"
          :type="`arrow${arrowDirection}`"
          :size="iconSize"
        />
        <slot name="right-icon" />
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
type CellSizeProps = '' | 'large'
type CellArrowDirectionProps = 'right' | 'left' | 'up' | 'down'

withDefaults(
  defineProps<{
    // 标题
    title?: string
    // 右侧内容
    value?: string
    // 标题下方描述
    label?: string
    // 左侧图标
    icon?: string
    // 是否显示边框
    border?: boolean
    // 尺寸
    size?: CellSizeProps
    // 显示右侧箭头
    isLink?: boolean
    // 箭头方向
    arrowDirection?: CellArrowDirectionProps
    iconSize?: number | string
  }>(),
  {
    title: '',
    value: '',
    label: '',
    icon: '',
    border: true,
    size: '',
    isLink: false,
    arrowDirection: 'right',
    iconSize: 16,
  }
)

const emits = defineEmits(['click'])
const onClick = () => {
  emits('click')
}
</script>

<style scoped lang="scss">
.cc-cell {
  position: relative;
  display: flex;
  align-items: center;
  box-sizing: border-box;
  width: 100%;
  padding: 20rpx 32rpx;
  overflow: hidden;
  color: #323233;
  font-size: 28rpx;
  background-color: #fff;
  &-border::after {
    position: absolute;
    box-sizing: border-box;
    content: ' ';
    pointer-events: none;
    right: 32rpx;
    bottom: 0;
    left: 32rpx;
    border-bottom: 2rpx solid #ebedf0;
    transform: scaleY(0.5);
  }
  &-left-icon {
    margin-right: 8rpx;
    flex-shrink: 0;
  }
  &-large {
    padding-top: 24rpx;
    padding-bottom: 24rpx;
  }
  &-title,
  &-value {
    flex: 1;
  }
  &-title {
    display: flex;
    align-items: center;
  }
  &-value {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    position: relative;
    overflow: hidden;
    color: #969799;
    text-align: right;
    vertical-align: middle;
    word-wrap: break-word;
    &-wrap {
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: flex-end;
    }
    .cc-cell-right-icon {
      position: relative;
      top: 2rpx;
      margin-left: 8rpx;
    }
  }
  &-label {
    margin-top: 8rpx;
    color: #969799;
    font-size: 24rpx;
    line-height: 36rpx;
  }
}
</style>
