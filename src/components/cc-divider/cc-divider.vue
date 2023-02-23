<template>
  <div
    v-if="!$slots.default"
    class="cc-divider-nocontent"
    :style="{ width: widthValue }"
  />
  <div v-else class="cc-divider-content">
    <div
      class="cc-divider-content-left-line"
      :class="{
        'f-1': !width || position === 'left',
        'f-8': position === 'right',
      }"
      :style="{ width: widthValue, background: color }"
    />
    <div>
      <slot />
    </div>
    <div
      class="cc-divider-content-right-line"
      :class="{
        'f-1': !width || position === 'right',
        'f-8': position === 'left',
      }"
      :style="{ width: widthValue, background: color }"
    />
  </div>
</template>

<script lang="ts" setup>
import { computed } from 'vue'
type Align = 'center' | 'left' | 'right'

const props = withDefaults(
  defineProps<{
    width?: number | string
    // 文字位置
    position?: Align
    // 边框颜色
    color?: string
    dashed?: boolean
    hairline?: boolean
  }>(),
  {
    width: '',
    position: 'center',
    // color: '#dcdfe6',
    color: 'red',
    dashed: false,
    hairline: true,
  }
)
const widthValue = computed(() => {
  if (String(props.width).includes('%')) return props.width
  else return `${props.width as number}rpx`
})
</script>

<style lang="scss">
.cc-divider {
  width: 100%;
  &-nocontent {
    height: 2rpx;
    background: #ebedf0;
    width: 100%;
  }
  &-content {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    &-left-line,
    &-right-line {
      height: 2rpx;
    }
    &-left-line {
      margin-right: 30rpx;
    }
    &-right-line {
      margin-left: 30rpx;
    }
  }
}
.f-1 {
  flex: 1;
}
.f-8 {
  flex: 8;
}
</style>
