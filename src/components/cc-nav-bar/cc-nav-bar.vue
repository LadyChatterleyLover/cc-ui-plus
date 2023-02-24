<template>
  <view>
    <view
      :class="{ 'cc-nav-bar-fixed': fixed }"
      class="cc-nav-bar"
      :style="{ background, zIndex: Number(zIndex) }"
    >
      <view class="cc-nav-bar-left" @click="clickLeft">
        <view>
          <slot v-if="$slots.left" name="left" />
          <view v-else class="cc-nav-bar-left-content">
            <view v-if="leftArrow" class="cc-nav-bar-left-content-icon"
              ><cc-icon
                :color="background ? '#fff' : leftIconColor"
                :type="leftIcon"
            /></view>
            <view
              :style="{ color: background ? '#fff' : leftTextColor }"
              class="cc-nav-bar-left-content-text"
              >{{ leftText }}</view
            >
          </view>
        </view>
      </view>
      <view @click="clickTitle">
        <slot v-if="$slots.default" />
        <view
          v-else
          class="cc-nav-bar-title"
          :style="{ color: background ? '#fff' : titleColor }"
          >{{ title }}</view
        >
      </view>
      <view class="cc-nav-bar-right" @click="clickRight">
        <view>
          <slot v-if="$slots.right" name="right" />
          <view
            v-else
            :style="{
              color: background ? '#fff' : rightColor,
              fontSize: titleSize + 'rpx',
            }"
            >{{ rightText }}</view
          >
        </view>
      </view>
    </view>
    <view v-if="fixed && placeholder" class="cc-nav-bar-placeholder" />
  </view>
</template>

<script lang="ts" setup>
withDefaults(
  defineProps<{
    title?: string
    titleColor?: string
    leftText?: string
    titleSize?: string | number
    leftIcon?: string
    leftIconColor?: string
    leftTextColor?: string
    rightText?: string
    rightColor?: string
    background?: string
    leftArrow?: boolean
    fixed?: boolean
    placeholder?: boolean
    zIndex?: string | number
  }>(),
  {
    title: '',
    titleColor: '#606266',
    titleSize: '28',
    leftText: '',
    leftIcon: 'arrowleft',
    leftIconColor: '#606266',
    leftTextColor: '#606266',
    rightText: '',
    rightColor: '#606266',
    background: '',
    leftArrow: false,
    fixed: false,
    placeholder: false,
    zIndex: 2023,
  }
)

const emits = defineEmits<{
  (e: 'clickLeft', val: Event): void
  (e: 'clickTitle', val: Event): void
  (e: 'clickRight', val: Event): void
}>()

const clickLeft = (e: Event) => {
  emits('clickLeft', e)
}
const clickTitle = (e: Event) => {
  emits('clickTitle', e)
}
const clickRight = (e: Event) => {
  emits('clickRight', e)
}
</script>

<style scoped lang="scss">
.cc-nav-bar {
  display: flex;
  align-items: center;
  height: 44px;
  justify-content: space-between;
  padding: 0 40rpx;
  background: #fff;
  &-placeholder {
    height: 88rpx;
  }
  &-fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
  }
  &-title {
    width: 400rpx;
    text-align: center;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }
  &-left,
  &-right {
    font-size: 28rpx;
    height: 88rpx;
    display: flex;
    align-items: center;
  }
  &-left {
    &-content {
      display: flex;
      align-items: center;
      &-icon {
        height: 88rpx;
        display: flex;
        align-items: center;
      }
      &-text {
        margin-left: 8rpx;
      }
    }
  }
}
</style>
