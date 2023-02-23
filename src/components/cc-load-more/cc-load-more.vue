<template>
  <view
    class="cc-load-more"
    :style="{
      marginTop: marginTop + 'rpx',
      marginBottom: marginBottom + 'rpx',
    }"
  >
    <cc-divider :width="lineWidth">
      <view v-if="dot" class="cc-load-more-dot" />
      <view v-else class="cc-load-more-content">
        <view v-if="loading">
          <cc-loading
            :size="loadingType === 'circle' ? '12' : '20'"
            :type="loadingType"
          />
        </view>
        <view :style="{ color, fontSize: fontSize + 'rpx' }">{{ text }}</view>
      </view>
    </cc-divider>
  </view>
</template>

<script lang="ts" setup>
withDefaults(
  defineProps<{
    text?: string
    // 线条宽度
    lineWidth?: string | number
    // 是否加载中
    loading?: boolean
    // 文字颜色
    color?: string
    // 文字大小
    fontSize?: string | number
    // 与前一个元素的距离
    marginTop?: string | number
    // 与后一个元素的距离
    marginBottom?: string | number
    // 加载图标类型
    loadingType?: 'circle' | 'spinner'
    // 是否显示点的模式
    dot?: boolean
  }>(),
  {
    text: '',
    lineWidth: 80,
    loading: false,
    color: '#606266',
    fontSize: 28,
    marginTop: 14,
    // 与后一个元素的距离
    marginBottom: 14,
    loadingType: 'circle',
    // 是否显示点的模式
    dot: false,
  }
)
</script>

<style lang="scss" scoped>
.cc-load-more {
  &-dot {
    height: 20rpx;
    width: 20rpx;
    background: #606266;
    border-radius: 100%;
  }
  &-content {
    display: flex;
    align-items: center;
  }
}
</style>
