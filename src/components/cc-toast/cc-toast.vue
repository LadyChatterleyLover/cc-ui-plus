<template>
  <view
    v-if="showToast"
    ref="cToast"
    :animation="animationData"
    class="cc-toast"
  >
    <view class="cc-toast-content" :style="{ background: bgColor, top }">
      <view
        v-if="iconValue"
        class="cc-toast-icon"
        :class="{ loading: isLoading }"
      >
        <cc-icon :type="iconValue" color="#fff" size="14" />
      </view>
      <view>{{ text }}</view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { onMounted, onUnmounted, ref } from 'vue'

export interface ToastOptions {
  title: string
  type?: 'primary' | 'success' | 'error' | 'warning' | 'info'
  icon?: boolean
  customIcon?: string
  loading?: boolean
  duration?: number
  position?: 'top' | 'bottom' | 'center'
  url?: string
  back?: boolean
  query?: any
  callback?: () => void
}

const cToast = ref()
// 显示toast
const showToast = ref(false)
// toast内容
const text = ref('')
// toast背景颜色
const bgColor = ref()
// 销毁toast定时器
const timer = ref()
const timer1 = ref()
const timer2 = ref()
// 图标
const iconValue = ref('')
// toast出现位置
const top = ref('')
// 是否加载状态
const isLoading = ref(false)

const animation = ref<any>({})
const animationData = ref<any>({})

// 显示toast方法
const show = (options: ToastOptions) => {
  showToast.value = true
  timer1.value = setTimeout(() => {
    animation.value.opacity(1).step()
    animationData.value = animation.value.export()
  }, 50)
  const {
    title,
    type = 'info',
    icon = true,
    customIcon = '',
    loading = false,
    duration = 2000,
    position = 'center',
    url = '',
    back = false,
    query = null,
    callback,
  } = options
  if (position === 'top') {
    top.value = '30%'
  }
  if (position === 'bottom') {
    top.value = '70%'
  }
  if (position === 'center') {
    top.value = '50%'
  }
  if (type === 'primary') {
    bgColor.value = '#0081ff'
    if (customIcon) iconValue.value = customIcon
    else if (loading) {
      iconValue.value = 'spinner-cycle'
      isLoading.value = true
    } else iconValue.value = ''
  }
  if (type === 'success') {
    bgColor.value = '#39b54a'
    if (customIcon) iconValue.value = customIcon
    else if (loading) {
      iconValue.value = 'spinner-cycle'
      isLoading.value = true
    } else {
      if (icon) iconValue.value = 'checkbox'
      else iconValue.value = ''
    }
  }
  if (type === 'error') {
    bgColor.value = '#e54d42'
    if (customIcon) iconValue.value = customIcon
    else if (loading) {
      iconValue.value = 'spinner-cycle'
      isLoading.value = true
    } else {
      if (icon) iconValue.value = 'close'
      else iconValue.value = ''
    }
  }
  if (type === 'warning') {
    bgColor.value = '#f37b1d'
    if (customIcon) iconValue.value = customIcon
    else if (loading) {
      iconValue.value = 'spinner-cycle'
      isLoading.value = true
    } else {
      if (icon) iconValue.value = 'info'
      else iconValue.value = ''
    }
  }
  if (type === 'info') {
    bgColor.value = '#333'
    if (customIcon) iconValue.value = customIcon
    else if (loading) {
      iconValue.value = 'spinner-cycle'
      isLoading.value = true
    } else iconValue.value = ''
  }
  text.value = title
  timer.value = setTimeout(() => {
    animation.value.opacity(0).step()
    animationData.value = animation.value.export()
    timer2.value = setTimeout(() => {
      showToast.value = false
    }, 200)
    if (back) {
      uni.navigateBack({})
    } else {
      if (url) {
        let q = ''
        for (const i in query) {
          q += `${i}=${query[i]}&`
        }
        uni.navigateTo({
          url: query ? `${url}?${q.slice(0, -1)}` : url,
        })
      }
    }
    if (callback) callback()
    isLoading.value = false
  }, duration)
}

onMounted(() => {
  animation.value = uni.createAnimation({
    duration: 150,
    timingFunction: 'linear',
  })
})

onUnmounted(() => {
  timer.value = null
  timer1.value = null
  timer2.value = null
  clearTimeout(timer.value)
  clearTimeout(timer1.value)
  clearTimeout(timer2.value)
})

defineExpose({
  show,
})
</script>

<style scoped lang="scss">
.cc-toast {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 999;
  display: flex;
  justify-content: center;
  opacity: 0;
  .cc-toast-content {
    position: absolute;
    font-size: 28rpx;
    padding: 20rpx 50rpx;
    border-radius: 4rpx;
    color: #fff;
    display: flex;
    align-items: center;
    transform: translateY(-50%);
    .cc-toast-icon {
      margin-right: 8rpx;
      position: relative;
      top: 2rpx;
    }
  }
}
.loading {
  animation: spin 1s linear infinite;
}
@keyframes spin {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
