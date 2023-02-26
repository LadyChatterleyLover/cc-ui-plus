<template>
  <view>
    <view v-if="$slots.tip" class="tip">
      <slot name="tip" />
    </view>
    <view class="container flex a-center">
      <slot />
      <view class="flex j-end a-center f-1">
        <view class="label">{{ label }}:</view>
        <view class="price flex a-center">
          <view class="currency">{{ currency }}</view>
          <view class="leftPrice">{{ leftPrice }}</view>
          <view class="rightPrice">.{{ rightPrice }}</view>
        </view>
        <view>
          <cc-button
            :loading="loading"
            :disabled="disabled"
            :color="buttonColor"
            round
            @click="submit"
          >
            {{ buttonText }}
          </cc-button>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

const props = withDefaults(
  defineProps<{
    price: number | string
    label?: string
    buttonText?: string
    buttonColor?: string
    currency?: string
    decimalLength?: string | number
    disabled?: boolean
    loading?: boolean
  }>(),
  {
    label: '合计',
    buttonText: '提交订单',
    buttonColor: '#ee0a24',
    currency: '¥',
    decimalLength: '2',
    disabled: false,
    loading: false,
  }
)

const emits = defineEmits<{
  (e: 'submit', val: Event): void
}>()

const leftPrice = ref<number>(0)
const rightPrice = ref<string>('')
const submit = (e: Event) => {
  if (!props.disabled && !props.loading) {
    emits('submit', e)
  }
}
onMounted(() => {
  leftPrice.value = Math.floor(Number(props.price) / 100)
  rightPrice.value = String(
    (Number(props.price) / 100).toFixed(props.decimalLength as number)
  ).split('.')[1]
})
</script>

<style scoped lang="scss">
.w100 {
  width: 100%;
}
.f-1 {
  flex: 1;
}
.flex {
  display: flex;
}
.a-center {
  align-items: center;
}
.j-center {
  justify-content: center;
}
.j-between {
  justify-content: space-between;
}
.j-end {
  justify-content: flex-end;
}
.container {
  height: 50px;
  padding: 0 32rpx;
  font-size: 14px;
  position: relative;
  .price {
    margin: 0 10rpx;
    color: #ee0a24;
    .currency {
      font-size: 12px;
      position: relative;
      top: 2rpx;
    }
    .leftPrice {
      font-size: 20px;
      font-weight: 500;
    }
    .rightPrice {
      font-size: 12px;
      position: relative;
      top: 2rpx;
    }
  }
  .button {
    width: 220rpx;
    height: 80rpx;
    border-radius: 1998rpx;
    color: #fff;
    margin-left: 10rpx;
    font-size: 16px;
  }
}
.tip {
  padding: 16rpx 24rpx;
  color: #f56723;
  font-size: 12px;
  line-height: 1.5;
  background-color: #fff7cc;
}
.disable {
  cursor: not-allowed;
  opacity: 0.5;
}
</style>
