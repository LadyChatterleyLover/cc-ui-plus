<template>
  <view class="cc-avatar-group">
    <view
      v-for="(item, index) in urls.slice(0, Number(maxCount))"
      :key="index"
      :style="{ marginLeft: index === 0 ? '0' : -gap + 'rpx' }"
    >
      <cc-avatar :shape="shape" :size="avatarSize" :mode="mode" :src="item" />
    </view>
    <view v-if="urls.length > maxCount" style="margin-left: -20rpx">
      <cc-avatar
        :shape="shape"
        :size="avatarSize"
        :mode="mode"
        :text="`+${urls.length - maxCount}`"
      />
    </view>
  </view>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    urls?: string[]
    maxCount?: number
    shape?: 'circle' | 'square'
    mode?: string
    size?: string | number | 'large' | 'default' | 'small'
    gap?: string | number
  }>(),
  {
    urls: () => [],
    maxCount: 5,
    shape: 'circle',
    mode: 'aspectFill',
    gap: 30,
    size: 80,
  }
)

const avatarSize = computed(() => {
  if (props.size === 'large') {
    return 96
  } else if (props.size === 'default') {
    return 80
  } else if (props.size === 'small') {
    return 64
  } else {
    return props.size
  }
})
</script>

<style scoped lang="scss">
.cc-avatar-group {
  display: flex;
  align-items: center;
}
</style>
