<template>
  <view v-if="loading" class="cc-skeleton">
    <view
      v-if="avatar"
      class="cc-skeleton-avatar"
      :class="{
        'cc-skeleton-avatar-round': round,
        'cc-skeleton-animate': animate,
      }"
      :style="{
        width: avatarSize + 'px',
        height: avatarSize + 'px',
        background: bgColor,
      }"
    />
    <view
      class="cc-skeleton-wrap"
      :class="{ 'cc-skeleton-wrap-hasavatar': avatar }"
    >
      <view
        v-if="title"
        class="cc-skeleton-content cc-skeleton-title"
        :class="{ 'cc-skeleton-animate': animate }"
        :style="{ background: bgColor }"
      />
      <template v-if="Number(row) > 0">
        <view
          v-for="item in row"
          :key="item"
          class="cc-skeleton-content cc-skeleton-row"
          :class="{
            'cc-skeleton-row-last': item === row,
            'cc-skeleton-animate': animate,
          }"
          :style="{ background: bgColor }"
        />
      </template>
    </view>
  </view>
</template>

<script setup lang="ts">
withDefaults(
  defineProps<{
    row?: number | string
    animate?: boolean
    title?: boolean
    avatar?: boolean
    loading?: boolean
    avatarSize?: number | string
    round?: boolean
    bgColor?: string
  }>(),
  {
    row: 3,
    animate: false,
    title: false,
    avatar: false,
    loading: true,
    avatarSize: 32,
    round: true,
    bgColor: '#f2f3f5',
  }
)
</script>

<style scoped lang="scss">
.cc-skeleton {
  width: 100%;
  display: flex;
  &-animate {
    animation: skeleton-animate 1.2s ease-in-out infinite;
  }
  &-title {
    /* #ifdef H5 */
    width: 40%;
    /* #endif */
    /* #ifndef H5 */
    width: 300rpx;
    /* #endif */
  }
  &-wrap {
    flex: 1;
    &-hasavatar {
      padding-top: 16rpx;
    }
  }
  &-content {
    height: 16px;
  }
  &-avatar {
    margin-right: 32rpx;
    &-round {
      border-radius: 100%;
    }
  }
  &-row {
    /* #ifdef H5 */
    width: 100%;
    /* #endif */
    /* #ifndef H5 */
    width: 750rpx;
    /* #endif */
    margin-top: 40rpx;
    &-last {
      /* #ifdef H5 */
      width: 60%;
      /* #endif */
      /* #ifndef H5 */
      width: 450rpx;
      /* #endif */
    }
  }
}
@keyframes skeleton-animate {
  from {
    opacity: 1;
  }
  to {
    opacity: 0.6;
  }
}
</style>
