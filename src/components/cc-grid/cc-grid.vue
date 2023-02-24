<template>
  <view class="cc-grid" :style="{ paddingLeft: gutter + 'rpx' }">
    <slot v-if="$slots.default" />
    <template v-else>
      <view
        v-for="(item, index) in list"
        :key="index"
        class="cc-grid-item"
        :style="{
          flexBasis: basis,
          paddingRight: gutter + 'rpx',
          marginBottom: gutter + 'rpx',
        }"
        @click="clickItem(item, index)"
      >
        <view>
          <slot v-if="$slots.default" />
          <view
            v-else
            :style="{ flexDirection: vertical ? 'row' : 'column' }"
            class="cc-grid-item-content"
          >
            <view class="cc-grid-item-icon">
              <cc-badge
                v-if="item.dot || item.badge"
                :dot="item.dot"
                :content="item.badge"
              >
                <cc-icon
                  :type="item.icon"
                  :color="item.iconColor"
                  :size="item.iconSize"
                />
              </cc-badge>
              <cc-icon
                v-if="!item.dot && !item.badge"
                :type="item.icon"
                :color="item.iconColor"
                :size="item.iconSize"
              />
            </view>
            <view
              class="cc-grid-item-text"
              :class="{ 'cc-grid-item-content': vertical }"
              :style="{
                fontSize: item.textSize + 'rpx',
                color: item.textColor,
              }"
              >{{ item.text }}</view
            >
          </view>
        </view>
        <view v-if="!gutter" class="cc-grid-item-border-right" />
        <view v-if="!gutter" class="cc-grid-item-border-bottom" />
      </view>
    </template>
  </view>
</template>

<script lang="ts" setup>
import { computed } from 'vue'

export interface GridItem {
  text?: string
  dot?: boolean
  badge?: string | number
  textSize?: string | number
  textColor?: string
  icon?: string
  iconColor?: string
  iconSize?: string | number
}

const props = withDefaults(
  defineProps<{
    list: GridItem[]
    column?: string | number
    gutter?: string | number
    vertical?: boolean
  }>(),
  {
    column: 4,
    gutter: 0,
    vertical: false,
  }
)

const emits = defineEmits<{
  (
    e: 'clickItem',
    val: {
      item: GridItem
      index: number
    }
  ): void
}>()

const clickItem = (item: GridItem, index: number) => {
  emits('clickItem', {
    item,
    index,
  })
}

const basis = computed(() => `${100 / Number(props.column)}%`)
</script>

<style scoped lang="scss">
.cc-grid {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
  .cc-grid-item {
    box-sizing: border-box;
    position: relative;
    &-content {
      padding: 32rpx 16rpx;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #fff;
    }
    &-content-row {
      margin-left: 16rpx;
    }
    &-border-right {
      position: absolute;
      height: 100%;
      width: 1px;
      background: #ebedf0;
      right: 0;
      top: 0;
      bottom: 0;
    }
    &-border-bottom {
      position: absolute;
      height: 1px;
      width: 100%;
      background: #ebedf0;
      bottom: 0;
    }
  }
}
</style>
