<template>
  <cc-popup
    ref="popup"
    v-model="show"
    mode="bottom"
    :close-on-click-overlay="closeOnClickOverlay"
  >
    <view class="cc-number-keyboard cc-number-keyboard__svg">
      <view v-if="showTooltip" class="cc-number-keyboard-tooltip">
        <view
          v-if="showTooltip && showCancelBtn"
          class="cc-number-keyboard-tooltip-cancel"
          @click="cancel"
          >{{ cancelText }}</view
        >
        <view
          v-if="showTooltip && showTips"
          class="cc-number-keyboard-tooltip-text"
          >{{ tips }}</view
        >
        <view
          v-if="showTooltip && showConfirmBtn"
          class="cc-number-keyboard-tooltip-confirm"
          @click="confirm"
          >{{ confirmText }}</view
        >
      </view>
      <view class="cc-number-keyboard-wrap">
        <view class="cc-number-keyboard-wrap-content">
          <view
            v-for="(item, index) in keyboardItem"
            :key="index"
            class="cc-number-keyboard-wrap-content-item"
            @click="clickItem(item, index)"
          >
            <view
              v-if="index !== keyboardItem.length - 1"
              class="cc-number-keyboard-wrap-content-item-key"
              v-html="item"
            />
            <view v-else class="cc-number-keyboard-wrap-content-item-key"
              ><image style="width: 1.5em; height: 1em" :src="item" mode=""
            /></view>
          </view>
        </view>
      </view>
    </view>
  </cc-popup>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import closePng from '../../static/cha.png'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    mode?: string
    random?: boolean
    showTooltip?: boolean
    showTips?: boolean
    showConfirmBtn?: boolean
    showCancelBtn?: boolean
    confirmText?: string
    cancelText?: string
    tips?: string
    extraKey?: string
    closeOnClickOverlay?: boolean
  }>(),
  {
    mode: 'number',
    random: false,
    showTooltip: true,
    showTips: true,
    showConfirmBtn: true,
    showCancelBtn: true,
    confirmText: '确认',
    cancelText: '取消',
    tips: '数字键盘',
    extraKey: '',
    closeOnClickOverlay: true,
  }
)

const emits = defineEmits<{
  (e: 'confirm'): void
  (e: 'cancel'): void
  (e: 'change', val: string): void
  (e: 'backspace'): void
  (e: 'update:modelValue', val: boolean): void
}>()

const show = ref(false)

const keyboardItem = computed(() => {
  if (props.random) {
    const list = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0']
    const l = randomArr(list)
    l[10] = l[9]
    l[9] = props.extraKey
    l[11] = closePng
    return l
  } else
    return [
      '1',
      '2',
      '3',
      '4',
      '5',
      '6',
      '7',
      '8',
      '9',
      props.extraKey,
      '0',
      closePng,
    ]
})

const randomArr = (arr: unknown[]) => {
  return arr.sort(() => {
    return Math.random() - 0.5
  })
}

const confirm = () => {
  emits('confirm')
}

const cancel = () => {
  emits('cancel')
}

const clickItem = (item: any, index: number) => {
  if (index !== keyboardItem.value.length - 1) {
    emits('change', item)
  } else emits('backspace')
}

watch(
  () => props.modelValue,
  (val) => {
    show.value = val
  },
  { immediate: true }
)
watch(
  () => show.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
</script>

<style scoped lang="scss">
.cc-number-keyboard {
  background: #fff;
  &-tooltip {
    display: flex;
    align-items: center;
    font-size: 14px;
    margin: 20rpx 0;
    &-cancel,
    &-text,
    &-confirm {
      flex: 1;
      display: flex;
      align-items: center;
    }
    &-cancel {
      padding-left: 14px;
      color: #888;
    }
    &-text {
      justify-content: center;
    }
    &-confirm {
      justify-content: flex-end;
      padding-right: 14px;
      color: #2979ff;
    }
  }
  &-wrap {
    position: relative;
    z-index: 999;
    background: #f2f3f5;
    padding: 12rpx 0 0 12rpx;
    &-content {
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      &-item {
        box-sizing: border-box;
        padding: 0 12rpx 12rpx 0;
        flex-basis: 33.333333%;
        font-size: 18px;
        color: #333;
        font-weight: 500;
        border-radius: 16rpx;
        &-key {
          display: flex;
          align-items: center;
          justify-content: center;
          height: 96rpx;
          background: #fff;
        }
      }
    }
  }
}
</style>
