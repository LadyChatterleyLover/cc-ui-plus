<template>
  <view class="cc-contact-list">
    <view
      v-for="(item, index) in contactList"
      :key="item.id"
      class="cc-contact-list-item"
      :class="{ 'cc-contact-list-item-disabled': item.disabled }"
      @click="clickItem(item, index)"
    >
      <view class="cc-contact-list-item-wrap">
        <view class="cc-contact-list-item-edit" @click.stop="edit(item, index)"
          ><cc-icon type="paperclip" color="#969799"
        /></view>
        <view class="cc-contact-list-item-wrap-name">{{ item.name }},</view>
        <view class="cc-contact-list-item-wrap-tel">{{ item.tel }}</view>
        <view v-if="item.isDefault">
          <cc-tag type="error" round>{{ defaultTagText }}</cc-tag>
        </view>
      </view>
      <view class="cc-contact-list-check">
        <view
          class="cc-contact-list-check-radio"
          :class="{
            'cc-contact-list-check-radio-active': currentValue === item.id,
          }"
        >
          <cc-icon
            v-if="currentValue === item.id"
            type="checkmarkempty"
            color="#fff"
          />
        </view>
      </view>
    </view>
    <view class="cc-contact-list-button" @click="add">
      <cc-button round block :color="addButtonColor">{{ addText }}</cc-button>
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface ContactListItemRadioList {
  value: string
  checkedColor: string
  disabled?: boolean
}
export interface ContactListItem {
  id: string
  name: string
  tel: string
  isDefault?: boolean
  disabled?: boolean
  checked?: boolean
  radioList?: ContactListItemRadioList[]
}

const props = withDefaults(
  defineProps<{
    list: ContactListItem[]
    value?: string | number
    addText?: string
    addButtonColor?: string
    defaultTagText?: string
  }>(),
  {
    value: '',
    addText: '新建联系人',
    addButtonColor: '#ee0a24',
    defaultTagText: '默认',
  }
)

const emits = defineEmits<{
  (
    e: 'select',
    val: {
      item: ContactListItem
      index: number
    }
  ): void
  (
    e: 'edit',
    val: {
      item: ContactListItem
      index: number
    }
  ): void
  (e: 'add'): void
}>()

const contactList = ref<ContactListItem[]>()
const currentValue = ref<string>('')
const currentIndex = ref<number>(0)

const init = () => {
  contactList.value?.forEach((item: ContactListItem, index: number) => {
    item.checked = false
    if (item.disabled)
      item.radioList = [
        { value: item.id, checkedColor: '#e54d42', disabled: true },
      ]
    else item.radioList = [{ value: item.id, checkedColor: '#e54d42' }]
    if (props.value === item.id) {
      currentIndex.value = index
      currentValue.value = item.id
      item.checked = true
    }
  })
}

const clickItem = (item: ContactListItem, index: number) => {
  if (item.disabled) return
  currentValue.value = item.id
  emits('select', { item, index })
}
const edit = (item: ContactListItem, index: number) => {
  if (item.disabled) return
  emits('edit', { item, index })
}
const add = () => {
  emits('add')
}
onMounted(() => {
  init()
})

watch(
  () => props.list,
  (val) => {
    contactList.value = cloneDeep(val)
  },
  { deep: true, immediate: true }
)
</script>

<style scoped lang="scss">
.cc-contact-list {
  background: #fff;
  &-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 32rpx;
    &-disabled {
      opacity: 0.6;
    }
    &-wrap {
      display: flex;
      align-items: center;
      &-name {
        margin: 0 30rpx;
      }
      &-tel {
        margin-right: 30rpx;
      }
    }
  }
  &-check {
    &-radio {
      width: 36rpx;
      height: 36rpx;
      border-radius: 100%;
      margin-right: 30rpx;
      border: 1px solid #eee;
      display: flex;
      justify-content: center;
      align-items: center;
      &-active {
        background: #e54d42;
      }
    }
  }
  &-button {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: #fff;
    padding: 16rpx 30rpx;
  }
}
</style>
