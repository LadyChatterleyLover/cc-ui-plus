<template>
  <view class="cc-password-input">
    <view
      class="cc-password-input-content"
      :class="{
        'cc-password-input-content-show': value,
        'cc-password-input-content-hide': !value,
      }"
      :style="{ display: contentDisplay }"
    >
      <view
        v-for="(item, index) in list"
        :key="index"
        class="cc-password-input-content-item"
        :class="{
          'cc-password-input-content-item-noborder':
            Number(gutter) === 0 && index < list.length - 1,
        }"
        :style="{ marginRight: gutter + 'rpx' }"
      >
        <view
          v-if="item"
          class="cc-password-input-content-item-content"
          :class="{ 'cc-password-input-content-item-content-mask': mask }"
        >
          <view
            v-if="!mask"
            class="cc-password-input-content-item-content-nomask"
            >{{ item }}</view
          >
        </view>
      </view>
      <view
        v-if="closeable"
        class="cc-password-input-content-close"
        @click="close"
        ><cc-icon type="closeempty" size="22"
      /></view>
    </view>
    <cc-number-keyboard
      ref="keyboard"
      v-model="value"
      :show-tooltip="false"
      extra-key=" "
      @change="handleChange"
      @backspace="backspace"
    />
  </view>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    closeable?: boolean
    mask?: boolean
    length?: string | number
    gutter?: string | number
    initValue?: string
  }>(),
  {
    length: 6,
    gutter: 20,
    mask: true,
    closeable: true,
    initValue: '',
  }
)

const emits = defineEmits<{
  (e: 'update:modelValue', val: boolean): void
  (e: 'complete', val: string): void
  (e: 'backspace', val: string): void
}>()

const value = ref(false)
const list = ref<string[]>([])
const display = ref<'none' | 'block'>('none')
const contentDisplay = ref<'none' | 'flex'>('none')
const clickNum = ref(-1)

const handleChange = (val: string) => {
  clickNum.value++
  list.value[clickNum.value] = val
  if (clickNum.value >= Number(props.length) - 1) {
    clickNum.value = -1
    emits('complete', list.value.join(''))
    emits('update:modelValue', !value.value)
  }
}

const backspace = () => {
  list.value[clickNum.value] = ''
  clickNum.value--
  if (list.value.length > 0) {
    emits('backspace', list.value.join(''))
  }
  if (clickNum.value <= -1) {
    clickNum.value = -1
  }
}

const close = () => {
  list.value = Array.from({ length: Number(props.length) }).fill('') as string[]
  clickNum.value = -1
  emits('update:modelValue', !value.value)
}

onMounted(() => {
  if (!props.initValue)
    list.value = Array.from({ length: Number(props.length) }).fill(
      ''
    ) as string[]
  else {
    const strArr = props.initValue.split('')
    list.value = [
      ...strArr,
      ...(Array.from({
        length: Number(Number(props.length) - props.initValue.length),
      }).fill('') as string[]),
    ]
    clickNum.value = props.initValue.length - 1
  }
})

watch(
  () => props.modelValue,
  (val) => {
    value.value = val
    if (val) {
      display.value = 'block'
      contentDisplay.value = 'flex'
    } else {
      setTimeout(() => {
        display.value = 'none'
        contentDisplay.value = 'none'
        list.value = Array.from({ length: Number(props.length) }).fill(
          ''
        ) as string[]
      }, 80)
    }
  },
  { immediate: true }
)
watch(
  () => value.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
</script>

<style scoped lang="scss">
.cc-password-input {
  &-content {
    position: fixed;
    left: 50%;
    top: 40%;
    transform: translate(-50%, -50%);
    background: #fff;
    display: flex;
    align-items: center;
    padding: 56rpx 48rpx;
    opacity: 0;
    z-index: 2000;
    &-close {
      position: absolute;
      right: 8rpx;
      top: 8rpx;
      z-index: 99;
    }
    &-show {
      animation: show-content 0.3s linear forwards;
    }
    &-hide {
      animation: hide-content 0.3s linear forwards;
    }
    &-item {
      width: 80rpx;
      height: 80rpx;
      background: #fff;
      border: 1px solid #ccc;
      display: flex;
      align-items: center;
      justify-content: center;
      &-noborder {
        border-right: 0;
      }
      &-content {
        width: 20rpx;
        height: 20rpx;
        &-mask {
          background: #000;
          border-radius: 100%;
        }
        &-nomask {
          position: relative;
          top: -8rpx;
          left: 2rpx;
        }
      }
    }
  }
}
@keyframes show {
  from {
    opacity: 0;
    bottom: 0;
  }
  to {
    opacity: 1;
    bottom: 444rpx;
  }
}
@keyframes hide {
  from {
    opacity: 1;
    bottom: 444rpx;
  }
  to {
    opacity: 0;
    bottom: 0;
  }
}
@keyframes show-content {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes hide-content {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
