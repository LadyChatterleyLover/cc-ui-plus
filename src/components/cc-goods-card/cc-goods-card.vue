<template>
  <view class="cc-goods-card">
    <view class="cc-goods-card-wrap" @click="handleClick">
      <view class="cc-goods-card-thumb">
        <image
          :src="thumb"
          style="width: 100%; height: 100%"
          @click="clickImage"
        />
        <view v-if="tag" class="cc-goods-card-thumb-tag">
          <cc-tag type="error" circle-right>{{ tag }}</cc-tag>
        </view>
      </view>
      <view class="cc-goods-card-content">
        <view class="cc-goods-card-content-info">
          <view v-if="title" class="cc-goods-card-content-info-title">{{
            title
          }}</view>
          <view v-if="desc" class="cc-goods-card-content-info-desc">{{
            desc
          }}</view>
          <view v-if="$slots.tags" class="cc-goods-card-content-info-tags"
            ><slot name="tags"
          /></view>
        </view>
        <view class="cc-goods-card-content-bottom">
          <view class="cc-goods-card-content-bottom-price">
            <view v-if="price" class="cc-goods-card-content-bottom-price-p">
              {{ currency }}
              <text style="font-size: 16px">{{
                String(price).slice(0, 1)
              }}</text>
              {{ String(price).slice(1) }}
            </view>
            <view
              v-if="originPrice"
              class="cc-goods-card-content-bottom-price-o"
              >{{ currency }}{{ originPrice }}</view
            >
          </view>
          <view v-if="num" class="cc-goods-card-content-bottom-num"
            >x{{ num }}</view
          >
        </view>
      </view>
    </view>
    <view class="cc-goods-card-footer"><slot name="footer" /></view>
  </view>
</template>

<script setup lang="ts">
const props = withDefaults(
  defineProps<{
    thumb?: string
    title?: string
    desc?: string
    tag?: string
    currency?: string
    num?: string | number
    price?: string | number
    originPrice?: string | number
  }>(),
  {
    thumb: '',
    title: '',
    desc: '',
    tag: '',
    num: '',
    currency: '￥',
    price: '',
    originPrice: '',
  }
)

const emits = defineEmits<{
  (e: 'click', val: Event): void
  (e: 'click-thumb', val: Event): void
}>()

const handleClick = (e: Event) => {
  emits('click', e)
}

const clickImage = (e: Event) => {
  emits('click-thumb', e)
}
</script>

<style scoped lang="scss">
.cc-goods-card {
  position: relative;
  box-sizing: border-box;
  padding: 16px 32px;
  color: #323233;
  font-size: 12px;
  background-color: #fafafa;
  &-wrap {
    display: flex;
  }
  &-thumb {
    width: 190rpx;
    height: 190rpx;
    right: 8px;
    position: relative;
    &-tag {
      position: absolute;
      top: 0;
      left: 0;
    }
  }
  &-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 180rpx;
    font-size: 12px;
    &-info {
      &-desc {
        color: #646566;
        margin-bottom: 10rpx;
      }
    }
    &-tags {
      display: flex;
      align-items: center;
    }
    &-bottom {
      display: flex;
      align-items: center;
      justify-content: space-between;
      &-price {
        display: flex;
        align-items: center;
        &-p {
          margin-right: 10rpx;
          font-weight: 500;
        }
        &-o {
          color: #969799;
          text-decoration: line-through;
          position: relative;
          top: 2rpx;
        }
      }
      &-num {
        color: #969799;
      }
    }
  }
  &-footer {
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
}
</style>
