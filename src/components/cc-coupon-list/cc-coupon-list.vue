<template>
  <view class="cc-coupon-list">
    <view class="cc-coupon-list-exchange">
      <view class="cc-coupon-list-exchange-search">
        <cc-search
          v-model="inputValue"
          round
          :show-action="false"
          :placeholder="inputPlaceholder"
        />
      </view>
      <view class="cc-coupon-list-exchange-btn">
        <cc-button
          :disabled="disabled"
          :loading="exchangeButtonLoading"
          :style="{ color: exchangeButtonColor }"
          size="small"
          @click="exchange"
          >{{ exchangeButtonText }}</cc-button
        >
      </view>
    </view>
    <view class="cc-coupon-list-tabs">
      <cc-tabs
        v-model="currentValue"
        :list="tabs"
        active-color="#323233"
        inactive-color="#646566"
        :line-color="lineColor"
        :scrollable="false"
        @change="changeTab"
      >
        <view class="cc-coupon-list-content">
          <view v-if="current === 0">
            <view
              v-for="(item, index) in cloneCoupons"
              :key="index"
              class="cc-coupon-list-content-enable"
              @click="clickItem(item, index)"
            >
              <view class="cc-coupon-list-content-enable-info">
                <view class="cc-coupon-list-content-enable-info-left">
                  <view class="cc-coupon-list-content-enable-info-left-price">
                    {{ item.valueDesc }}
                    <text style="font-size: 12px">{{ item.unitDesc }}</text>
                  </view>
                  <view
                    class="cc-coupon-list-content-enable-info-left-condition"
                  >
                    {{ item.condition }}
                  </view>
                </view>
                <view class="cc-coupon-list-content-enable-info-center">
                  <view class="cc-coupon-list-content-enable-info-center-name">
                    {{ item.name }}
                  </view>
                  <view class="cc-coupon-list-content-enable-info-center-time">
                    {{ item.startTime }} - {{ item.endTime }}
                  </view>
                </view>
                <view class="cc-coupon-list-content-enable-info-right">
                  <view
                    class="cc-coupon-list-content-enable-info-right-radio"
                    :class="{
                      'cc-coupon-list-content-enable-info-right-radio-active':
                        currentValue === Number(item.id),
                    }"
                  >
                    <cc-icon
                      v-if="currentValue === Number(item.id)"
                      type="checkmarkempty"
                      color="#fff"
                    />
                  </view>
                </view>
              </view>
              <view class="cc-coupon-list-content-enable-label">
                {{ item.description }}
              </view>
            </view>
          </view>
          <view v-if="current === 1">
            <view
              v-for="(item, index) in cloneDisableCoupons"
              :key="index"
              class="cc-coupon-list-content-enable"
            >
              <view class="cc-coupon-list-content-enable-info">
                <view
                  class="cc-coupon-list-content-enable-info-left"
                  style="color: inherit"
                >
                  <view class="cc-coupon-list-content-enable-info-left-price">
                    {{ item.valueDesc }}
                    <text style="font-size: 12px">{{ item.unitDesc }}</text>
                  </view>
                  <view
                    class="cc-coupon-list-content-enable-info-left-condition"
                  >
                    {{ item.condition }}
                  </view>
                </view>
                <view class="cc-coupon-list-content-enable-info-center">
                  <view class="cc-coupon-list-content-enable-info-center-name">
                    {{ item.name }}
                  </view>
                  <view class="cc-coupon-list-content-enable-info-center-time">
                    {{ item.startTime }} - {{ item.endTime }}
                  </view>
                </view>
              </view>
              <view class="cc-coupon-list-content-enable-label">
                {{ item.reason }}
              </view>
            </view>
          </view>
        </view>
      </cc-tabs>
    </view>
    <view class="cc-coupon-list-btn" @click="close">
      <cc-button round block :color="closeButtonColor">{{
        closeButtonText
      }}</cc-button>
    </view>
  </view>
</template>

<script setup lang="ts">
import { computed, inject, onMounted, ref } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'
import type { CouponItem } from '../cc-coupon-cell/cc-coupon-cell.vue'

const props = withDefaults(
  defineProps<{
    value?: string
    chosenCoupon?: number
    coupons?: CouponItem[]
    disabledCoupons?: CouponItem[]
    enabledTitle?: string
    disabledTitle?: string
    exchangeButtonColor?: string
    exchangeButtonText?: string
    exchangeButtonLoading?: boolean
    exchangeButtonDisabled?: boolean
    showCloseButton?: boolean
    exchangeMinLength?: number | string
    closeButtonText?: string
    closeButtonColor?: string
    inputPlaceholder?: string
    lineColor?: string
    showExchangeBar?: boolean
  }>(),
  {
    value: '',
    chosenCoupon: -1,
    coupons: () => [],
    disabledCoupons: () => [],
    enabledTitle: '可用',
    disabledTitle: '不可用',
    exchangeButtonColor: '#ee0a24',
    exchangeButtonText: '兑换',
    exchangeButtonLoading: false,
    exchangeButtonDisabled: false,
    showCloseButton: true,
    exchangeMinLength: 1,
    closeButtonText: '不使用优惠',
    closeButtonColor: '#ee0a24',
    lineColor: '#ee0a24',
    inputPlaceholder: '请输入优惠码',
    showExchangeBar: true,
  }
)

const emits = defineEmits<{
  (e: 'change', val: { item: CouponItem; index: number }): void
  (e: 'changeCoupon', val: CouponItem): void
  (e: 'exchange', val: string): void
}>()

const closePopup = inject<null | (() => void)>('cc-popup-close', null)

const current = ref<number>(0)
const cloneCoupons = ref<CouponItem[]>([])
const cloneDisableCoupons = ref<CouponItem[]>([])
const currentValue = ref<number>(props.chosenCoupon)
const inputValue = ref<string>(props.value)

const formatTime = (date: number | Date) => {
  const time = new Date(date)
  const year = time.getFullYear()
  const mon =
    time.getMonth() + 1 < 10 ? `0${time.getMonth() + 1}` : time.getMonth() + 1
  const data = time.getDate() < 10 ? `0${time.getDate()}` : time.getDate()
  const hour = time.getHours() < 10 ? `0${time.getHours()}` : time.getHours()
  const min =
    time.getMinutes() < 10 ? `0${time.getMinutes()}` : time.getMinutes()
  const seon =
    time.getSeconds() < 10 ? `0${time.getSeconds()}` : time.getSeconds()

  const newDate = `${year}-${mon}-${data} ${hour}:${min}:${seon}`
  return newDate
}

const changeTab = (val: { index: number }) => {
  current.value = val.index
}
// 选中优惠券
const clickItem = (item: CouponItem, index: number) => {
  closePopup?.()
  currentValue.value = index
  emits('change', { item, index })
  uni.$emit('changeCoupon', item)
}
// 点击兑换按钮
const exchange = () => {
  if (
    !props.exchangeButtonDisabled ||
    !inputValue.value ||
    !inputValue.value.length
  )
    return
  emits('exchange', inputValue.value)
}
// 点击不使用优惠券按钮
const close = () => {
  closePopup?.()
  uni.$emit('changeCoupon', null)
}
onMounted(() => {
  cloneCoupons.value = cloneDeep(props.coupons)
  cloneDisableCoupons.value = cloneDeep(props.disabledCoupons)
  cloneCoupons.value.forEach((item: CouponItem) => {
    item.startTime = formatTime(item.startAt as number)
    item.endTime = formatTime(item.endAt as number)
  })
  cloneDisableCoupons.value.forEach((item: CouponItem) => {
    item.startTime = formatTime(item.startAt as number)
    item.endTime = formatTime(item.endAt as number)
  })
})
const tabs = computed(() => {
  const arr = []
  arr.push(
    {
      name: props.enabledTitle,
    },
    {
      name: props.disabledTitle,
    }
  )
  return arr
})
const disabled = computed(
  () =>
    props.exchangeButtonDisabled ||
    !inputValue.value ||
    !inputValue.value.length
)
</script>

<style scoped lang="scss">
.cc-coupon-list {
  &-exchange {
    display: flex;
    align-items: center;
    height: 88rpx;
    padding: 12rpx 20rpx;
    &-search {
      flex: 1;
    }
  }
  &-content {
    background: #f7f8fa;
    height: 860rpx;
    padding: 20rpx 30rpx;
    &-enable {
      margin: 0 24rpx 24rpx;
      overflow: hidden;
      background: #fff;
      border-radius: 16rpx;
      &-info {
        display: flex;
        align-items: center;
        box-sizing: border-box;
        min-height: 168rpx;
        padding: 28rpx;
        color: #323233;
        position: relative;
        &-left {
          color: #ee0a24;
          min-width: 192rpx;
          display: flex;
          flex-direction: column;
          align-items: center;
          &-price {
            font-weight: 500;
            font-size: 30px;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
            margin-bottom: 12rpx;
          }
          &-condition {
            text-align: center;
            font-size: 12px;
            line-height: 32rpx;
            white-space: pre-wrap;
            text-overflow: ellipsis;
            overflow: hidden;
          }
        }
        &-center {
          &-name {
            font-weight: 700;
            font-size: 28rpx;
            margin-bottom: 20rpx;
          }
          &-time {
            font-size: 12px;
          }
        }
        &-right {
          position: absolute;
          top: 70rpx;
          right: 50rpx;
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
      &-label {
        padding: 16rpx 32rpx;
        font-size: 24rpx;
        border-top: 1px dashed #ebedf0;
        color: #323233;
      }
    }
  }
  &-btn {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    padding: 20rpx 30rpx;
  }
}
.cc-button {
  border: none;
  background: #fff;
}
</style>
