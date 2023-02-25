<template>
  <cc-popup
    ref="popup"
    v-model="flag"
    height="800"
    mode="bottom"
    closeable
    @close="close"
  >
    <view class="cc-calendar">
      <view class="cc-calendar-title">{{ title }}</view>
      <view class="cc-calendar-text">
        <view class="cc-calendar-text-year" @click="preYear">
          <cc-icon type="arrowleft" size="10" />
          <cc-icon
            type="arrowleft"
            size="10"
            style="position: relative; right: 6px"
          />
        </view>
        <view class="cc-calendar-text-month" @click="preMonth">
          <cc-icon type="arrowleft" size="10" />
        </view>
        <view class="cc-calendar-text-current">
          {{ time.year }}年{{ time.month + 1 }}月
        </view>
        <view class="cc-calendar-text-month" @click="nextMonth">
          <cc-icon type="arrowright" size="10" />
        </view>
        <view class="cc-calendar-text-year" @click="nextYear">
          <cc-icon type="arrowright" size="10" />
          <cc-icon
            style="position: relative; left: -6px"
            type="arrowright"
            size="10"
          />
        </view>
      </view>
      <view class="cc-calendar-days">
        <view
          v-for="(item, index) in days"
          :key="index"
          class="cc-calendar-days-item"
        >
          {{ item }}
        </view>
      </view>
      <view class="cc-calendar-content">
        <view v-for="i in 6" :key="i" class="cc-calendar-content-item">
          <view v-for="j in 7" :key="j" class="cc-calendar-content-item-text">
            <view
              v-if="showDays[(i - 1) * 7 + (j - 1)]"
              :key="j"
              class="cc-calendar-content-item-text-value"
              :class="{
                'cc-calendar-content-item-text-nocurrent': !isCurrentMonth(
                  showDays[(i - 1) * 7 + (j - 1)]
                ),
                'cc-calendar-content-item-text-today': isToday(
                  showDays[(i - 1) * 7 + (j - 1)]
                ),
              }"
              :style="{
                background: isToday(showDays[(i - 1) * 7 + (j - 1)])
                  ? bgColor
                  : '#fff',
                color: isToday(showDays[(i - 1) * 7 + (j - 1)])
                  ? '#fff'
                  : '#303133',
              }"
              @click="chooseDay(showDays[(i - 1) * 7 + (j - 1)])"
            >
              <view>{{ showDays[(i - 1) * 7 + (j - 1)].getDate() }}</view>
            </view>
          </view>
        </view>
      </view>
      <view class="cc-calendar-value">{{ formatDate }}</view>
      <view class="cc-calendar-btn" @click="confirm">
        <cc-button block round :color="bgColor">{{ buttonText }}</cc-button>
      </view>
    </view>
  </cc-popup>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: boolean
    title?: string
    buttonText?: string
    bgColor?: string
    range?: boolean
  }>(),
  {
    title: '日期选择',
    buttonText: '确认',
    bgColor: '#ee0a24',
    range: false,
  }
)

const emits = defineEmits(['select', 'confirm', 'update:modelValue'])
// 获取时间年月日起
const getYearMonthDay = (date: any) => {
  const year = date.getFullYear()
  const month = date.getMonth()
  const day = date.getDate()
  return {
    year,
    month,
    day,
  }
}
// 显示弹框
const flag = ref<boolean>(props.modelValue)
// 当前选中日期 默认今天
const value = ref<any>(new Date())
// 当前日期
const days = ref<string[]>(['日', '一', '二', '三', '四', '五', '六'])
// 选择的时间
const time = ref<any>({
  year: getYearMonthDay(new Date()).year,
  month: getYearMonthDay(new Date()).month,
})
const dateValue = ref<any>(null)
const popup = ref()
// 关闭弹框
const close = (val: boolean) => {
  console.log(val)
  emits('update:modelValue', val)
}
// 获取日期
const getDate = (year: number, month: number, day: number) => {
  return new Date(year, month, day)
}
// 判断是否是当前月份
const isCurrentMonth = (date: any) => {
  const { year, month } = getYearMonthDay(date)
  const { year: y, month: m } = getYearMonthDay(
    getDate(time.value.year, time.value.month, 1)
  )
  return year === y && month === m
}
// 判断是否是今天
const isToday = (date: any) => {
  const { year, month, day } = getYearMonthDay(value.value)
  const { year: y, month: m, day: d } = getYearMonthDay(date)
  return year === y && month === m && day === d
}
// 上一年
const preYear = () => {
  time.value.year--
}
// 下一年
const nextYear = () => {
  time.value.year++
}
// 上一月
const preMonth = () => {
  const d = getDate(time.value.year, time.value.month, 1)
  d.setMonth(d.getMonth() - 1)
  time.value = getYearMonthDay(d)
}
// 下一月
const nextMonth = () => {
  const d = getDate(time.value.year, time.value.month, 1)
  d.setMonth(d.getMonth() + 1)
  time.value = getYearMonthDay(d)
}
// 选择日期
const chooseDay = (item: any) => {
  if (!props.range) {
    value.value = item
    const { year, month, day } = getYearMonthDay(item)
    emits('select', {
      year,
      month: month + 1,
      day,
    })
    const isToday =
      year === new Date().getFullYear() &&
      month === new Date().getMonth() &&
      day === new Date().getDate()
        ? true
        : false
    dateValue.value = {
      year,
      month: month + 1,
      day,
      week: item.getDay(),
      date: `${year}-${month + 1}-${day}`,
      isToday,
    }
  } else {
    // 多选日期
  }
}
// 确认
const confirm = () => {
  emits('confirm', dateValue.value)
  emits('update:modelValue', false)
}
// 格式化时间
const formatDate = computed(() => {
  const { year, month, day } = getYearMonthDay(value.value)
  return `${year}-${month + 1}-${day}`
})
const showDays = computed(() => {
  const { year, month } = getYearMonthDay(
    getDate(time.value.year, time.value.month, 1)
  )
  // 获取当月第一天
  const firstDay: any = getDate(year, month, 1)
  // 获取当月第一天是星期几
  const week = firstDay.getDay()
  // 开始日期
  const startDay = firstDay - week * 1000 * 60 * 60 * 24
  const arr = []
  // 循环42天 因为日历是6 * 7的布局
  for (let i = 0; i < 42; i++) {
    arr.push(new Date(startDay + i * 1000 * 60 * 60 * 24))
  }
  return arr
})

const init = () => {
  time.value.year = getYearMonthDay(value.value).year
  time.value.month = getYearMonthDay(value.value).month
  dateValue.value = {
    year: getYearMonthDay(value.value).year,
    month: getYearMonthDay(value.value).month + 1,
    day: value.value.getDate(),
    week: value.value.getDay(),
    date: `${getYearMonthDay(value.value).year}-${
      Number(getYearMonthDay(value.value).month) + 1
    }-${value.value.getDate()}`,
    isToday: isToday(value.value),
  }
}
watch(
  () => props.modelValue,
  (val) => {
    flag.value = val
  }
)
watch(
  () => flag.value,
  (val) => {
    emits('update:modelValue', val)
  }
)
onMounted(() => {
  init()
})
</script>

<style scoped lang="scss">
.cc-calendar {
  &-title {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    font-weight: 500;
    background-color: #fff;
    color: #303133;
    width: 100%;
    height: 88rpx;
  }
  &-text {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    margin-top: 20rpx;
    margin-bottom: 30rpx;
    &-current {
      margin: 0 16rpx;
      color: #303133;
      font-weight: 700;
    }
    &-year,
    &-month {
      margin: 0 16rpx;
    }
    &-year {
      color: #909399;
    }
    &-month {
      color: #606266;
    }
  }
  &-days {
    display: flex;
    align-items: center;
    padding: 12rpx 0;
    &-item {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 14px;
    }
  }
  &-value {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 12rpx 0;
    color: #909399;
    font-size: 14px;
  }
  &-content {
    height: 400rpx;
    display: flex;
    flex-direction: column;
    &-item {
      flex: 1;
      display: flex;
      align-items: center;
      &-text {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 14px;
        border-radius: 8rpx;
        &-nocurrent {
          color: #c8c9cc !important;
        }
        &-value {
          padding: 12rpx 16rpx;
        }
        &-today {
          background: #ee0a24;
          color: #fff;
          border-radius: 8rpx;
        }
      }
    }
  }
  &-btn {
    padding: 0 16rpx;
  }
}
</style>
