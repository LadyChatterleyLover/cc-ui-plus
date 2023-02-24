<template>
  <view class="cc-pagination">
    <view
      class="cc-pagination-pre"
      :class="{ 'cc-pagination-disabled': activeIndex === 1 }"
      @click="pre"
    >
      <slot v-if="$slots.pre" name="pre" />
      <view v-else>{{ prevText }}</view>
    </view>
    <view v-if="!simple" class="cc-pagination-content">
      <view
        v-for="(item, index) in showCount"
        :key="index"
        class="cc-pagination-content-item"
        :class="{
          'cc-pagination-content-item-active': activeIndex === item.value,
        }"
        @click="clickItem(item)"
      >
        <slot
          v-if="$slots.page"
          name="page"
          :current="activeIndex"
          :text="item"
          :active="activeIndex === item.value"
        />
        <view v-else>{{ item.text }}</view>
      </view>
    </view>
    <view v-else class="cc-pagination-desc">
      <slot v-if="$slots.page" name="page" :current="activeIndex" />
      <view v-else>{{ activeIndex }} / {{ total }}</view>
    </view>
    <view
      class="cc-pagination-next"
      :class="{ 'cc-pagination-disabled': activeIndex === total }"
      @click="next"
    >
      <slot v-if="$slots.next" name="next" />
      <view v-else>{{ nextText }}</view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    current?: number
    pageSize?: number
    showPageSize?: number
    simple?: boolean
    forceEllipses?: boolean
    prevText?: string
    nextText?: string
    total?: string | number
  }>(),
  {
    current: 1,
    pageSize: 10,
    simple: false,
    forceEllipses: false,
    prevText: '上一页',
    nextText: '下一页',
    total: 0,
    showPageSize: 5,
  }
)

const emits = defineEmits<{
  (e: 'change', val: number): void
}>()

const activeIndex = ref(0)

const pre = () => {
  if (activeIndex.value === 1) {
    return
  }
  activeIndex.value--
}

const next = () => {
  if (activeIndex.value === props.total) {
    return
  }
  activeIndex.value++
}

const clickItem = (item: { value: number }) => {
  activeIndex.value = item.value
  emits('change', activeIndex.value)
}

const showCount = computed(() => {
  const arr: { value: number; text: number | string }[] = []
  for (let i = 1; i <= Number(props.total); i++) {
    arr.push({
      value: i,
      text: i,
    })
  }
  if (!props.simple) {
    const total = Number(props.total)
    const showPageSize = Number(props.showPageSize)
    const half = Math.ceil(showPageSize / 2)
    const diff = showPageSize - half

    if (activeIndex.value <= half) return arr.slice(0, showPageSize)
    else if (activeIndex.value > total - diff) {
      return arr.slice(total - showPageSize, total)
    } else {
      if (!props.forceEllipses) {
        return arr.slice(activeIndex.value - half, activeIndex.value + diff)
      } else {
        const arr1 = arr.slice(
          activeIndex.value - half,
          activeIndex.value + diff
        )
        arr1[0].text = '...'
        arr1[arr1.length - 1].text = '...'
        return arr1
      }
    }
  }
  return arr
})

watch(
  () => props.current,
  (val) => {
    activeIndex.value = val
  },
  { immediate: true }
)
</script>

<style scoped lang="scss">
.cc-pagination {
  display: flex;
  align-items: center;
  &-desc {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 80rpx;
    color: #646566;
  }
  &-pre,
  &-next {
    min-width: 72rpx;
    height: 80rpx;
    user-select: none;
    flex: 1;
    background: #fff;
    color: #0081ff;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 8rpx;
  }
  &-content {
    display: flex;
    align-items: center;
    &-item {
      height: 80rpx;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #0081ff;
      min-width: 72rpx;
      height: 80rpx;
      user-select: none;
      flex: 1;
      background: #fff;
      &-active {
        background: #0081ff;
        color: #fff;
      }
    }
  }
  &-disabled {
    color: #646566;
    background-color: #f7f8fa;
    cursor: not-allowed;
    opacity: 0.5;
  }
}
</style>
