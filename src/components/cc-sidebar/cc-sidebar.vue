<template>
  <view class="cc-sidebar" :style="{ width: width + 'px' }">
    <view
      v-for="(item, index) in list"
      :key="index"
      class="cc-sidebar-item"
      :style="{
        background:
          active === index ? (item.bgColor ? item.bgColor : '#fff') : '#f7f8fa',
        fontSize: item.fontSize ? item.fontSize : 14 + 'px',
      }"
      @click="clickItem(item, index)"
    >
      <cc-badge
        v-if="item.dot || item.badge"
        :dot="item.dot"
        :content="item.badge"
      >
        <view class="cc-sidebar-item-title">{{ item.title }}</view>
      </cc-badge>
      <view
        v-else
        class="cc-sidebar-item-title"
        :class="{
          'cc-sidebar-item-weight': active === index,
          disabled: item.disabled,
        }"
        :style="{
          color:
            active === index
              ? item.activeColor
                ? item.activeColor
                : '#323233'
              : item.textColor
              ? item.textColor
              : '#323233',
        }"
      >
        {{ item.title }}
      </view>
      <view
        v-if="showLine && index === active"
        class="cc-sidebar-item-active"
        :style="{
          background: item.lineColor ? item.lineColor : '#ee0a24',
          width: item.lineWidth ? item.lineWidth + 'rpx' : '8rpx',
          height: item.lineHeight ? item.lineHeight + 'rpx' : '32rpx',
        }"
      />
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

export interface SideBarItem {
  title: string
  bgColor?: string
  lineColor?: string
  lineWidth?: string | number
  lineHeight?: string | number
  fontSize?: string | number
  activeColor?: string
  textColor?: string
  badge?: string | number
  disabled?: boolean
  dot?: boolean
}

const props = withDefaults(
  defineProps<{
    list: SideBarItem[]
    current?: number | string
    width?: number | string
    showLine?: boolean
  }>(),
  {
    current: 0,
    width: 80,
    showLine: true,
  }
)

const emits = defineEmits<{
  (
    e: 'change',
    val: {
      item: SideBarItem
      index: number
    }
  ): void
}>()

const active = ref(0)

const clickItem = (item: SideBarItem, index: number) => {
  if (item.disabled) return
  active.value = index
  emits('change', {
    item,
    index,
  })
}

watch(
  () => props.current,
  (val) => {
    if (typeof props.current === 'number') {
      active.value = val as number
    } else {
      active.value = props.list.findIndex(
        (item, index) => item.title === val || index === Number(val)
      )
    }
  },
  { immediate: true }
)
</script>

<style scoped lang="scss">
.cc-sidebar {
  display: flex;
  flex-direction: column;
  &-item {
    position: relative;
    padding: 40rpx;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    user-select: none;
    &-active {
      position: absolute;
      top: 50%;
      left: 0;
      transform: translateY(-50%);
    }
    &-weight {
      font-weight: 500;
    }
  }
}
.disabled {
  color: #c8c9cc !important;
  cursor: not-allowed;
  pointer-events: none;
}
</style>
