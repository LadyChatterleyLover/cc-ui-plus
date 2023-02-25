<template>
  <view class="cc-address-list">
    <view
      v-for="(item, index) in addressList"
      :key="item.id"
      class="cc-address-list-item"
    >
      <view
        class="cc-address-list-item-content"
        @click="clickItem(item, index)"
      >
        <view
          class="cc-address-list-item-content-radio"
          :class="{
            'cc-address-list-item-content-radio-active':
              currentValue === item.id,
          }"
        >
          <cc-icon
            v-if="currentValue === item.id"
            type="checkmarkempty"
            color="#fff"
          />
        </view>
        <view class="cc-address-list-item-content-info">
          <view class="cc-address-list-item-content-info-top">
            <view>{{ item.name }}</view>
            <view style="margin-left: 16rpx">{{ item.tel }}</view>
            <view v-if="currentIndex === index" style="margin-left: 16rpx">
              <cc-tag round type="error">{{ defaultTagText }}</cc-tag>
            </view>
          </view>
          <view class="cc-address-list-item-content-info-address">{{
            item.address
          }}</view>
        </view>
        <view class="cc-address-list-item-edit" @click.stop="edit(item, index)">
          <cc-icon type="paperclip" color="#969799" />
        </view>
      </view>
    </view>

    <view class="cc-address-list-disabled-text">{{ disabledText }}</view>

    <view
      v-for="(item, index) in disabledList"
      :key="item.id"
      class="cc-address-list-item cc-address-list-item-disabled"
      @click="clickDisabledItem(item, index)"
    >
      <view class="cc-address-list-item-content">
        <view class="cc-address-list-item-content-info">
          <view class="cc-address-list-item-content-info-top">
            <view>{{ item.name }}</view>
            <view style="margin-left: 16rpx">{{ item.tel }}</view>
          </view>
          <view class="cc-address-list-item-content-info-address">{{
            item.address
          }}</view>
        </view>
        <view
          class="cc-address-list-item-edit"
          @click.stop="editDisabled(item, index)"
        >
          <cc-icon type="paperclip" color="#969799" />
        </view>
      </view>
    </view>

    <view class="cc-address-list-btn" @click="add">
      <cc-button :color="addButtonColor" round block>{{
        addButtonText
      }}</cc-button>
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'
export interface AddressListItemRadioList {
  value: string
  checkedColor: string
}
export interface AddressListItem {
  id: string
  name: string
  tel: string
  address: string
  isDefault?: boolean
  radioList?: AddressListItemRadioList[]
}

const props = withDefaults(
  defineProps<{
    list: AddressListItem[]
    disabledList?: AddressListItem[]
    disabledText?: string
    addButtonText?: string
    addButtonColor?: string
    defaultTagText?: string
    switchable?: boolean
    value?: string | number
  }>(),
  {
    value: '',
    disabledList: () => [],
    disabledText: '',
    defaultTagText: '',
    addButtonColor: '#e54d42',
    addButtonText: '新增地址',
    switchable: true,
  }
)

const emits = defineEmits<{
  (e: 'click', val: { item: AddressListItem; index: number }): void
  (e: 'select', val: { item: AddressListItem; index: number }): void
  (e: 'edit', val: { item: AddressListItem; index: number }): void
  (e: 'select-disabled', val: { item: AddressListItem; index: number }): void
  (e: 'edit-disabled', val: { item: AddressListItem; index: number }): void
  (e: 'add'): void
}>()

const addressList = ref<AddressListItem[]>(cloneDeep(props.list))
const currentIndex = ref<number>(-1)
const currentValue = ref<string>('')

const clickItem = (item: AddressListItem, index: number) => {
  emits('click', { item, index })
  currentValue.value = item.id
  if (currentValue.value === item.id) {
    emits('select', { item, index })
  }
}
const edit = (item: AddressListItem, index: number) => {
  emits('edit', { item, index })
}
const add = () => {
  emits('add')
}
const editDisabled = (item: AddressListItem, index: number) => {
  emits('edit-disabled', { item, index })
}
const clickDisabledItem = (item: AddressListItem, index: number) => {
  emits('click', { item, index })
  emits('select-disabled', { item, index })
}
const init = () => {
  addressList.value.forEach((item: AddressListItem, index: number) => {
    item.radioList = [{ value: item.id, checkedColor: '#e54d42' }]
    if (props.value === item.id) {
      currentValue.value = item.id
      currentIndex.value = index
    }
  })
}

onMounted(() => {
  init()
})
</script>

<style scoped lang="scss">
.cc-address-list {
  &-item {
    position: relative;
    padding: 24rpx;
    background-color: #fff;
    border-radius: 16rpx;
    margin-bottom: 24rpx;
    border-radius: 16rpx;
    &:last-child {
      margin-bottom: 0;
    }
    &-disabled {
      opacity: 0.4;
    }
    &-edit {
      position: absolute;
      right: 60rpx;
      top: 50%;
      transform: translateY(-50%);
      z-index: 999;
    }
    &-content {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding-right: 80rpx;
      &-info {
        flex: 1;
        &-top {
          display: flex;
          align-items: center;
        }
        &-address {
          font-size: 12px;
          color: #323233;
          margin-top: 20rpx;
        }
      }
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
  }
  &-disabled-text {
    padding: 40rpx 0 32rpx;
    color: #969799;
    font-size: 14px;
  }
  &-btn {
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 999;
    box-sizing: border-box;
    width: 100%;
    padding: 20rpx 32rpx;
    background: #fff;
  }
}
</style>
