<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-field
          v-model="value1"
          label="文本"
          :border="false"
          clearable
          @clear="handleClear"
          @input="handleInput"
          @change="handleChange"
          @focus="handleFocus"
          @blur="handleBlur"
          @click="handleClick"
          @click-left="clickLeft"
        />
      </view>
      <view v-if="value1">当前输入框值: {{ value1 }}</view>
    </view>

    <view class="demo">
      <view class="demo-title">输入框类型</view>
      <view class="demo-item">
        <cc-field v-model="value2" label="文本" />
      </view>
      <view class="demo-item">
        <cc-field v-model="value3" label="密码" type="password" />
      </view>
      <view class="demo-item">
        <cc-field v-model="value4" type="tel" label="手机号" />
      </view>
      <view class="demo-item">
        <cc-field v-model="value5" type="number" label="数字" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">禁用状态</view>
      <view class="demo-item">
        <cc-field v-model="value10" label="只读" readonly />
      </view>
      <view class="demo-item">
        <cc-field v-model="value10" label="禁用" disabled />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">显示图标</view>
      <view class="demo-item">
        <cc-field
          v-model="value8"
          left-icon="search"
          right-icon="person"
          placeholder="显示图标"
          label="文本"
          @click-right="clickRight"
        />
      </view>
      <view class="demo-item">
        <cc-field
          v-model="value9"
          left-icon="chat"
          placeholder="显示图标"
          label="文本"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">必填</view>
      <view class="demo-item">
        <cc-field v-model="value10" label="文本" required />
      </view>
      <view class="demo-item">
        <cc-field
          v-model="value11"
          label="文本"
          required
          error
          error-message="都怪那天雨下的太急"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">输入长度限制</view>
      <view class="demo-item">
        <cc-field
          v-model="value12"
          :maxlength="50"
          show-word-limit
          label="文本"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自适应高度</view>
      <view class="demo-item">
        <cc-field
          v-model="value13"
          :maxlength="200"
          show-word-limit
          type="textarea"
          rows="1"
          label="文本"
        />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">插入按钮</view>
      <view class="demo-item">
        <cc-field v-model="value14" label="文本">
          <template #button>
            <cc-button type="success" size="small">发送验证码</cc-button>
          </template>
        </cc-field>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const value1 = ref('')
const value2 = ref('')
const value3 = ref('')
const value4 = ref('')
const value5 = ref('')
const value8 = ref('')
const value9 = ref('')
const value10 = ref('')
const value11 = ref('')
const value12 = ref('')
const value13 = ref('')
const value14 = ref('')

const handleInput = (val: string) => {
  console.log('input', val)
}
const handleChange = (val: string) => {
  console.log('change', val)
}
const handleFocus = (e: Event) => {
  console.log('focus', e)
}
const handleBlur = (e: Event) => {
  console.log('blur', e)
}
const handleClick = (e: Event) => {
  console.log('click', e)
}
const handleClear = (e: Event) => {
  console.log('clear', e)
}
const clickLeft = (e: Event) => {
  console.log('clickLeft', e)
}
const clickRight = (e: Event) => {
  console.log('clickRight', e)
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  padding: 20rpx;
  &-title {
    padding: 20rpx;
  }
  &-item {
    padding: 0 5px;
    margin-bottom: 10px;
  }
}
</style>
