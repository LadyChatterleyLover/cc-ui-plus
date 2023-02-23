<template>
  <view class="cc-count-down">
    <slot v-if="defaultSlots" :time-data="timeData" />
    <template v-else>
      <view v-if="showDays && day > 0">{{ dayValue }}</view>
      <view
        v-if="showDays && day > 0"
        :style="{
          fontSize: separatorSize + 'rpx',
          color: separatorColor,
          margin: `0 ${gutter}rpx`,
        }"
        >{{ separator === 'colon' ? ':' : '天' }}</view
      >
      <view v-if="showHours">{{ hourValue }}</view>
      <view
        v-if="showHours"
        :style="{
          fontSize: separatorSize + 'rpx',
          color: separatorColor,
          margin: `0 ${gutter}rpx`,
        }"
        >{{ separator === 'colon' ? ':' : '时' }}</view
      >
      <view v-if="showMinutes">{{ minuteValue }}</view>
      <view
        v-if="showMinutes"
        :style="{
          fontSize: separatorSize + 'rpx',
          color: separatorColor,
          margin: `0 ${gutter}rpx`,
        }"
        >{{ separator === 'colon' ? ':' : '分' }}</view
      >
      <view v-if="showSeconds">{{ secondValue }}</view>
      <view
        v-if="showMinutes"
        :style="{
          fontSize: separatorSize + 'rpx',
          color: separatorColor,
          margin: `0 ${gutter}rpx`,
        }"
        >{{ separator === 'colon' ? '' : '秒' }}</view
      >
    </template>
  </view>
</template>

<script lang="ts" setup>
import { computed, onMounted, onUnmounted, ref, useSlots, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    // 倒计时时间
    time?: number
    // 分隔符 可选择 zh
    separator?: 'colon' | 'zh'
    // 分隔符大小
    separatorSize?: string | number
    // 分割符颜色
    separatorColor?: string
    // 分割符与文字之间的间距
    gutter?: string | number
    // 是否显示倒计时的天
    showDays?: boolean
    // 是否显示倒计时的时
    showHours?: boolean
    // 是否显示倒计时的分
    showMinutes?: boolean
    // 是否显示倒计时的秒
    showSeconds?: boolean
  }>(),
  {
    time: 0,
    separator: 'colon',
    separatorSize: 32,
    separatorColor: '#000',
    gutter: 0,
    showDays: true,
    showHours: true,
    showMinutes: true,
    showSeconds: true,
  }
)

const slots = useSlots()
const emits = defineEmits(['end', 'change'])
const day = ref<number>(0)
const hour = ref<number>(0)
const minute = ref<number>(0)
const second = ref<number>(0)
const timer = ref<any>()
const timeValue = ref<number>(0)
const timeData = ref<any>({})
const defaultSlots = ref()
const formatNum = (num: number) => {
  return num < 10 ? `0${num}` : `${num}`
}

const countDown = () => {
  clearInterval(timer.value)
  timer.value = setInterval(() => {
    if (hour.value === 0) {
      if (minute.value !== 0 && second.value === 0) {
        second.value = 59
        minute.value -= 1
      } else if (minute.value === 0 && second.value === 0) {
        second.value = 0
        emits('end')
        clearInterval(timer.value)
      } else {
        second.value -= 1
      }
    } else {
      if (minute.value !== 0 && second.value === 0) {
        second.value = 59
        minute.value -= 1
      } else if (minute.value === 0 && second.value === 0) {
        hour.value -= 1
        minute.value = 59
        second.value = 59
      } else {
        second.value -= 1
      }
    }
    timeValue.value =
      hour.value * 1000 * 3600 + minute.value * 1000 * 60 + second.value * 1000
    emits('change', timeValue.value)
  }, 1000)
}

const dayValue = computed(() => formatNum(day.value))
const hourValue = computed(() => formatNum(hour.value))
const minuteValue = computed(() => formatNum(minute.value))
const secondValue = computed(() => formatNum(second.value))

onMounted(() => {
  if (slots.default) defaultSlots.value = slots.default
  if (props.time > 0 && props.time < 1000 * 60 * 60 * 24) {
    day.value = 0
    hour.value = Math.floor((props.time / 3600 / 1000) % 24)
    minute.value = Math.floor((props.time / 60 / 1000) % 60)
    second.value = Math.floor((props.time / 1000) % 60)
  } else {
    // 如果时间大于1天
    // 如果刚好是整数天
    if (props.time % (1000 * 60 * 60 * 24) === 0) {
      day.value = props.time / (1000 * 60 * 60 * 24)
      hour.value = 24 * day.value
      minute.value = 0
      second.value = 0
    } else {
      day.value = Math.floor(props.time / (1000 * 60 * 60 * 24))
      hour.value = day.value * 24 + Math.floor((props.time / 3600 / 1000) % 24)
      minute.value = Math.floor((props.time / 60 / 1000) % 60)
      second.value = Math.floor((props.time / 1000) % 60)
    }
  }
  timeData.value.days = dayValue.value
  timeData.value.hours = hourValue.value
  timeData.value.minutes = minuteValue.value
  timeData.value.seconds = secondValue.value
  countDown()
})
onUnmounted(() => {
  clearInterval(timer.value)
  timer.value = null
})
watch(
  () => timeValue.value,
  () => {
    timeData.value.days = dayValue.value
    timeData.value.hours = hourValue.value
    timeData.value.minutes = minuteValue.value
    timeData.value.seconds = secondValue.value
  },
  { deep: true }
)
</script>

<style lang="scss" scoped>
.cc-count-down {
  display: flex;
  align-items: center;
}
</style>
