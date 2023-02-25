<template>
  <view class="cc-dropdown">
    <view
      v-for="(item, index) in cloneList"
      :key="index"
      class="cc-dropdown-item"
    >
      <view
        class="cc-dropdown-item-mask"
        :class="{
          'cc-dropdown-item-mask-active':
            item.activeItem && item.activeItem.actived,
        }"
        :style="{ display: item.activeItem && item.activeItem.display }"
        @click="clickMask(item)"
      />
      <view
        class="cc-dropdown-item-header"
        :class="{ 'cc-dropdown-item-header-active': zIndex }"
        @click="clickHeader(item)"
      >
        <view
          class="cc-dropdown-item-header-title"
          :class="{ disabled }"
          :style="{
            color:
              item.activeItem && item.activeItem.actived
                ? activeColor
                : '#323233',
          }"
        >
          {{
            item.title ? item.title : item.activeItem && item.activeItem.label
          }}
        </view>
        <view
          class="cc-dropdown-item-header-icon"
          :class="{
            'cc-dropdown-item-header-icon-active':
              item.activeItem && item.activeItem.actived,
          }"
        >
          <cc-icon
            type="arrowdown"
            :color="
              item.activeItem && item.activeItem.actived
                ? activeColor
                : '#c0c4cc'
            "
            size="14"
          />
        </view>
      </view>
      <view
        class="cc-dropdown-item-wrap"
        :class="{
          'cc-dropdown-item-wrap-active':
            item.activeItem && item.activeItem.actived,
        }"
      >
        <slot
          v-if="item.slots && item.activeItem && item.activeItem.actived"
          :name="item.slots"
        />
        <template v-if="!item.slots && item.activeItem">
          <view
            v-for="(item1, index1) in item.options"
            :key="index1"
            class="cc-dropdown-item-wrap-content"
            :style="{ display: item.activeItem.display }"
          >
            <cc-cell @click="clickItem(item, item1, index, index1)">
              <template #title>
                <view
                  :class="{ disabled: item1.disabled }"
                  :style="{
                    color:
                      item.activeIndex === index1 ? activeColor : '#323233',
                  }"
                  >{{ item1.label }}</view
                >
              </template>
              <template #right-icon>
                <cc-icon
                  v-if="item.activeIndex === index1"
                  type="checkmarkempty"
                  size="16"
                  :color="item.activeIndex === index1 ? activeColor : '#323233'"
                />
              </template>
            </cc-cell>
          </view>
        </template>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface DropdownOption {
  label: string
  value: string | number
  disabled?: boolean
}

export interface DropdownItem {
  value: string | number
  title?: string
  slots?: string
  options: DropdownOption[]
}

interface ActiveItem extends DropdownOption {
  actived?: boolean
  display?: 'none' | 'block'
}

interface CloneDrodownItem {
  value: string | number
  title?: string
  slots?: string
  options: DropdownOption[]
  activeIndex: number
  activeItem: ActiveItem
}

const props = withDefaults(
  defineProps<{
    list: DropdownItem[]
    activeColor?: string
    closeOnClickOverlay?: boolean
    disabled?: boolean
  }>(),
  {
    activeColor: '#ee0a24',
    closeOnClickOverlay: true,
    disabled: false,
  }
)

const emits = defineEmits<{
  (e: 'open'): void
  (e: 'close'): void
  (e: 'change', val: string[] | number[]): void
}>()

const cloneList = ref<CloneDrodownItem[]>([])
const checked = ref<any[]>([])
const zIndex = ref(false)

const init = () => {
  cloneList.value.forEach((item) => {
    if (item.options && item.options.length) {
      item.options.forEach((item1, index1) => {
        item.activeItem = item1
        item.activeIndex = index1
        item.activeItem.actived = false
        item.activeItem.display = 'none'
        checked.value.push(item1.value)
      })
    } else {
      item.activeItem = {
        value: '',
        label: '',
        disabled: false,
        actived: false,
        display: 'none',
      }
    }
  })
}

// 点击头部
const clickHeader = (item: CloneDrodownItem) => {
  if (props.disabled) {
    return
  }
  item.activeItem.actived = !item.activeItem.actived
  item.activeItem.display === 'none'
    ? (item.activeItem.display = 'block')
    : setTimeout(() => {
        item.activeItem.display = 'none'
      }, 80)
  cloneList.value.forEach((i) => {
    if (item !== i) {
      i.activeItem.actived = false
      i.activeItem.display = 'none'
    }
  })
  if (item.activeItem.actived) {
    emits('open')
    zIndex.value = true
  } else {
    emits('close')
    zIndex.value = false
  }
}

// 点击菜单
const clickItem = (
  item: CloneDrodownItem,
  item1: ActiveItem,
  index: number,
  index1: number
) => {
  if (item1.disabled) return
  item.activeItem = item1
  item.activeItem.actived = false
  item.activeIndex = index1
  setTimeout(() => {
    item.activeItem.display = 'none'
  }, 80)
  if (item.value !== item1.value) {
    item.value = item1.value
    checked.value[index] = item1.value
    emits('change', checked.value)
  }
  emits('close')
  zIndex.value = false
}

const clickMask = (item: CloneDrodownItem) => {
  if (props.closeOnClickOverlay) {
    clickHeader(item)
  }
}

onMounted(() => {
  init()
})

watch(
  () => props.list,
  (val) => {
    cloneList.value = cloneDeep(val as any)
  },
  { immediate: true, deep: true }
)

watch(
  () => checked.value,
  (val) => {
    emits('change', val)
  },
  { deep: true, immediate: true }
)
</script>

<style scoped lang="scss">
.cc-dropdown {
  display: flex;
  .cc-dropdown-item {
    display: flex;
    align-items: center;
    flex-direction: column;
    flex: 1;
    position: relative;
    &-mask {
      position: fixed;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0, 0, 0, 0.7);
      transition: all 0.3s;
      opacity: 0;
      z-index: 100;
      &-active {
        opacity: 1;
      }
    }
    &-header {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      background: #fff;
      box-shadow: 0 2px 12px #fff;
      height: 96rpx;
      &-active {
        z-index: 999;
      }
      &-title {
        height: 100%;
        display: flex;
        align-items: center;
      }
      &-icon {
        transition: all 0.3s;
        margin-left: 12rpx;
        &-active {
          transform: rotate(180deg);
        }
      }
    }
    &-wrap {
      width: 100%;
      position: absolute;
      top: 0;
      transition: all 0.3s;
      z-index: 999;
      background: #fff;
      &-active {
        top: 96rpx;
      }
    }
  }
}
.disabled {
  color: #969799 !important;
  pointer-events: none;
}
</style>
