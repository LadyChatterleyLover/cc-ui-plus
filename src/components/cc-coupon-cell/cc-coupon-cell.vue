<template>
  <view class="cc-coupon-cell">
    <cc-cell :title="cellTitle" :value="cellValue" is-link @click="clickCell" />
    <slot />
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

export interface CouponItemRadioListItem {
  value: string | number
  checkedColor: string
}
export interface CouponItem {
  id: string
  name: string
  condition?: string
  startAt?: number
  endAt?: number
  description?: string
  reason?: string
  value?: number
  valueDesc?: string
  unitDesc?: string
  startTime?: string
  endTime?: string
}

const props = withDefaults(
  defineProps<{
    title?: string
    chosenCoupon?: string | number
    coupons: CouponItem[]
    currency?: string
  }>(),
  {
    title: '优惠券',
    chosenCoupon: '',
    currency: '￥',
  }
)

const emits = defineEmits<{
  (e: 'click', val: Event): void
}>()

const cellTitle = ref<string>(props.title)
const cellValue = ref<string>('')
const clickCell = (e: Event) => {
  emits('click', e)
}
onMounted(() => {
  cellValue.value = `${props.coupons.length}张可用`
  uni.$on('changeCoupon', (val: any) => {
    if (val)
      cellValue.value = `-${props.currency} ${(val.value / 100).toFixed(2)}`
    else cellValue.value = `${props.coupons.length}张可用`
  })
})
</script>

<style scoped></style>
