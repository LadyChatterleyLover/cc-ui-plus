<template>
  <view class="cc-checker">
    <template v-if="!multiple">
      <view
        v-for="(item, index) in checkList"
        :key="index"
        class="cc-checker-item"
        :class="{
          'cc-checker-item-round': item.round,
          'cc-checker-item-disabled': item.disabled,
        }"
        :style="{
          background: currentIndex === index ? item.bgColor : '#f5f5f5',
          color: currentIndex === index ? item.color : '#333',
        }"
        @click="clickItem(item, index)"
      >
        <view>{{ item.label }}</view>
        <view v-if="item.info" class="cc-checker-item-info">{{
          item.info
        }}</view>
      </view>
    </template>
    <template v-else>
      <view
        v-for="(item, index) in checkList"
        :key="index"
        class="cc-checker-item"
        :class="{
          'cc-checker-item-round': item.round,
          'cc-checker-item-disabled': item.disabled,
        }"
        :style="{
          background: item.checked ? item.bgColor : '#f5f5f5',
          color: item.checked ? item.color : '#333',
        }"
        @click="clickItem(item, index)"
      >
        <view>{{ item.label }}</view>
        <view v-if="item.info" class="cc-item-info">{{ item.info }}</view>
        <i
          v-if="item.checked"
          class="cc-checker-item-icon"
          :style="{ fill: item.checked ? item.color : '#f5f5f5' }"
        >
          <svg
            width="100%"
            height="100%"
            viewBox="0 0 16 16"
            xmlns="http://www.w3.org/2000/svg"
          >
            <g fill-rule="evenodd">
              <path d="M16 0v12a4 4 0 01-4 4H0L16 0z" />
              <path
                d="M7.854 11.146a.5.5 0 00-.708.708l2 2a.5.5 0 00.708 0l4-4a.5.5 0 00-.708-.708L9.5 12.793l-1.646-1.647z"
                fill="#FFF"
                fill-rule="nonzero"
              />
            </g>
          </svg>
        </i>
      </view>
    </template>
  </view>
</template>

<script setup lang="ts">
import { defineEmits, defineProps, onMounted, ref } from 'vue'
import cloneDeep from 'lodash/cloneDeep'
export interface CheckerItem {
  label: string
  value: string | number | boolean
  round?: boolean
  readonly?: boolean
  disabled?: boolean
  info?: string
  checked?: boolean
  color?: string
  bgColor?: string
}
const props = withDefaults(
  defineProps<{
    modelValue: string | number | boolean | string[] | number[] | boolean[]
    list: CheckerItem[]
    multiple?: boolean
  }>(),
  {
    multiple: false,
  }
)
const emits = defineEmits(['change'])
const current = ref<CheckerItem>()
const currentIndex = ref<number>(0)
const checkList = ref<CheckerItem[]>(cloneDeep(props.list))
const multipleList = ref<any[]>([])
const init = () => {
  checkList.value.forEach((item: CheckerItem) => {
    if (!item.color) item.color = '#0081ff'
    if (!item.bgColor) item.bgColor = '#EBF4FF'
  })
  if (Array.isArray(props.modelValue)) {
    checkList.value.forEach((item: CheckerItem) => {
      item.checked = false
      if ((props.modelValue as any[]).includes(item.value)) {
        multipleList.value.push(item.value)
        item.checked = true
      }
    })
  } else {
    current.value = props.list.find((item) => item.value === props.modelValue)
    currentIndex.value = props.list.findIndex(
      (item) => item.value === props.modelValue
    )
  }
}
const clickItem = (item: CheckerItem, index: number) => {
  if (item.disabled || item.readonly) return
  if (!props.multiple) {
    currentIndex.value = index
    current.value = item
    emits('change', item.value)
  } else {
    item.checked = !item.checked
    const checked = checkList.value
      .filter((item) => item.checked)
      .map((item) => {
        return {
          label: item.label,
          value: item.value,
        }
      })
    emits('change', checked)
  }
}
onMounted(() => {
  init()
})
</script>

<style scoped lang="scss">
.cc-checker {
  display: flex;
  align-items: center;
  &-item {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;
    font-size: 14px;
    padding: 6px 25px;
    position: relative;
    &-round {
      border-radius: 24px;
    }
    &-disabled {
      opacity: 0.4;
    }
    &-info {
      position: absolute;
      top: -14px;
      right: -14px;
      background: #e54d42;
      color: #fff;
      border-radius: 100%;
      padding: 3px;
      font-size: 12px;
    }
    &-icon {
      width: 12px;
      height: 12px;
      position: absolute;
      right: 0;
      bottom: 0;
    }
  }
}
</style>
