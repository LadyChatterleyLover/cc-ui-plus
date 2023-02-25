<template>
  <view class="cc-tree-select">
    <template v-if="isMultiple">
      <view class="cc-tree-select-index">
        <view
          v-for="(item, index) in list"
          :key="index"
          class="cc-tree-select-index-container"
          :class="{
            'cc-tree-select-index-active': currentIndex === index,
            'cc-tree-select-disabled': item.disabled,
          }"
          @click="clickNav(item, index)"
        >
          <cc-badge
            v-if="item.dot || item.badge"
            :dot="item.dot"
            :content="item.badge"
            >{{ item.text }}</cc-badge
          >
          <text v-else>{{ item.text }}</text>
        </view>
      </view>
      <view
        v-if="currentItem && currentItem.children"
        class="cc-tree-select-content"
      >
        <view
          v-for="(item, index) in currentItem.children"
          :key="index"
          class="cc-tree-select-content-item"
          @click="clickItem(item, index)"
        >
          <view
            class="cc-tree-select-content-item-text"
            :class="{ 'cc-tree-select-disabled': item.disabled }"
            :style="{ color: item.checked ? activeColor : '#000' }"
          >
            {{ item.text }}
          </view>
          <view v-if="item.checked"
            ><cc-icon :type="selectedIcon" :color="activeColor" size="16"
          /></view>
        </view>
      </view>
    </template>
    <template v-else>
      <view class="cc-tree-select-index">
        <view
          v-for="(item, index) in list"
          :key="index"
          class="cc-tree-select-index-container"
          :class="{
            'cc-tree-select-index-active': currentIndex === index,
            'cc-tree-select-disabled': item.disabled,
          }"
          @click="clickNav(item, index)"
        >
          <cc-badge
            v-if="item.dot || item.badge"
            :dot="item.dot"
            :content="item.badge"
            >{{ item.text }}</cc-badge
          >
          <text v-else>{{ item.text }}</text>
        </view>
      </view>
      <view
        v-if="currentItem && currentItem.children"
        class="cc-tree-select-content"
      >
        <view
          v-for="(item, index) in currentItem.children"
          :key="index"
          class="cc-tree-select-content-item"
          @click="clickItem(item, index)"
        >
          <view
            class="cc-tree-select-content-item-text"
            :class="{ 'cc-tree-select-disabled': item.disabled }"
            :style="{
              color:
                currentItem.currentChildIndex === index ? activeColor : '#000',
            }"
          >
            {{ item.text }}
          </view>
          <view v-if="currentItem.currentChildIndex === index"
            ><cc-icon :type="selectedIcon" :color="activeColor" size="16"
          /></view>
        </view>
      </view>
    </template>
  </view>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface TreeSelectItem {
  id: string | number
  text: string
  disabled?: boolean
  dot?: boolean
  badge?: string | number
  index?: number
  checked?: boolean
  currentChildIndex?: number
  children?: TreeSelectItem[]
}

const props = withDefaults(
  defineProps<{
    items: TreeSelectItem[]
    mainActiveIndex?: number | string
    activeId?: number | string | number[] | string[]
    selectedIcon?: string
    activeColor?: string
  }>(),
  {
    mainActiveIndex: 0,
    activeId: 0,
    selectedIcon: 'checkmarkempty',
    activeColor: '#ee0a24',
  }
)

const emits = defineEmits<{
  (e: 'clickNav', val: TreeSelectItem[]): void
  (e: 'clickItem', val: TreeSelectItem | TreeSelectItem[]): void
}>()

const currentIndex = ref<number>(-1)
const currentItem = ref<TreeSelectItem>()
const list = ref<TreeSelectItem[]>([])

const isMultiple = computed(() => Array.isArray(props.activeId))

const clickNav = (item: TreeSelectItem, index: number) => {
  if (item.disabled) return
  if (isMultiple.value) {
    currentIndex.value = index
    currentItem.value = list.value[currentIndex.value]
    list.value.forEach((item: TreeSelectItem) => {
      if ((props.activeId as any[]).includes(item.id)) {
        item.checked = true
      } else {
        item.checked = false
      }
    })
  } else {
    currentIndex.value = index
    currentItem.value = list.value[currentIndex.value]
    currentItem.value!.index = index
  }
  emits('clickNav', list.value)
}
const clickItem = (item: TreeSelectItem, index: number) => {
  if (isMultiple.value) {
    list.value.forEach((item1: TreeSelectItem) => {
      item1.children &&
        item1.children &&
        item1.children!.forEach((item2: TreeSelectItem) => {
          if (item1.index !== currentIndex.value) {
            item2.checked = false
          }
        })
    })
    item.checked = !item.checked
    emits('clickItem', list.value)
  } else {
    list.value.forEach((item: TreeSelectItem) => {
      if (item.index !== currentIndex.value) {
        item.currentChildIndex = -1
      } else {
        item.currentChildIndex = index
      }
    })
    emits('clickItem', item)
  }
}

const init = () => {
  if (isMultiple.value) {
    list.value.forEach((item: TreeSelectItem, index: number) => {
      item.index = index
      if (item.children && item.children.length) {
        item.children.forEach((item1: TreeSelectItem) => {
          item1.checked = false
          item1.index = index
          if (String(props.activeId).includes(String(item1.id))) {
            item1.checked = true
          }
        })
      }
    })
    currentIndex.value = props.mainActiveIndex as number
    currentItem.value = list.value[props.mainActiveIndex as number]
  } else {
    list.value.forEach((item: TreeSelectItem, index: number) => {
      item.index = index
    })
    currentIndex.value = props.mainActiveIndex as number
    currentItem.value = list.value[props.mainActiveIndex as number]
    currentItem.value.currentChildIndex = Number(props.activeId)
  }
}

onMounted(() => {
  init()
})

watch(
  () => props.items,
  (val) => {
    list.value = cloneDeep(val)
  },
  { deep: true, immediate: true }
)
</script>

<style scoped lang="scss">
.cc-tree-select {
  width: 100%;
  display: flex;
  &-disabled {
    color: #c8c9cc !important;
    cursor: not-allowed;
  }
  &-index {
    flex: 1;
    &-container {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 96rpx;
      background-color: #f7f8fa;
    }
    &-active {
      background: #fff;
    }
  }
  &-content {
    flex: 2;
    background: #fff;
    &-item {
      height: 96rpx;
      padding: 0 64rpx 0 32rpx;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
  }
}
</style>
