<template>
  <view>
    <view>
      <view class="title">基础语法</view>
      <view>
        <cc-search
          v-model="value"
          @input="handleInput"
          @focus="handleFocus"
          @blur="handleBlur"
          @confirm="handleConfirm"
          @clear="clear"
          @cancel="cancel"
        />
      </view>
    </view>

    <view>
      <view class="title">圆形</view>
      <view>
        <cc-search v-model="value" round />
      </view>
    </view>

    <view>
      <view class="title">不展示右侧文字</view>
      <view>
        <cc-search v-model="value" :show-action="false" />
      </view>
    </view>

    <view>
      <view class="title">文字对齐方式</view>
      <view>
        <cc-search v-model="value" text-align="center" />
      </view>
      <view>
        <cc-search v-model="value" text-align="right" />
      </view>
    </view>

    <view>
      <view class="title">图标对齐方式</view>
      <view>
        <cc-search v-model="value" icon-align="center" text-align="center" />
      </view>
      <view>
        <cc-search v-model="value" icon-align="right" text-align="right" />
      </view>
    </view>

    <view>
      <view class="title">自定义图标</view>
      <view>
        <cc-search v-model="value" round icon="person" />
      </view>
    </view>

    <view>
      <view class="title">左侧内容</view>
      <view>
        <cc-search v-model="value" round label="地址" />
      </view>
    </view>

    <view>
      <view class="title">禁用</view>
      <view>
        <cc-search v-model="value" round disabled />
      </view>
    </view>

    <view>
      <view class="title">自定义左侧内容</view>
      <view>
        <cc-search v-model="value" round>
          <template #label>
            <view class="label">
              <view>地址</view>
              <view style="margin-left: 8rpx">
                <cc-icon type="arrowdown" size="10" />
              </view>
            </view>
          </template>
        </cc-search>
      </view>
    </view>

    <view>
      <view class="title">自定义右侧内容</view>
      <view>
        <cc-search v-model="value" round>
          <template #action>
            <view>搜索</view>
          </template>
        </cc-search>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value = ref('')
const clear = () => {
  console.log('clear')
}
const cancel = () => {
  console.log('cancel')
}
const handleFocus = () => {
  console.log('focus')
}
const handleInput = () => {
  console.log('input')
}
const handleBlur = () => {
  console.log('blur')
}
const handleConfirm = () => {
  console.log('confirm')
}
</script>

<style scoped lang="scss">
.title {
  margin: 20rpx;
}
.label {
  display: flex;
  align-items: center;
}
</style>
