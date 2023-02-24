<template>
  <view>
    <view class="demo">
      <view class="demo-title">基础用法</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list1" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">展示点</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list1" dot />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义图标</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list2" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">自定义颜色</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list3" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">描述信息</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list4" />
      </view>
    </view>

    <view class="demo">
      <view class="demo-title">竖向</view>
      <view class="demo-item">
        <cc-steps v-model="active" :list="list4" vertical />
      </view>
    </view>

    <view class="demo-btn">
      <view class="demo-btn-item" @click="pre">
        <cc-button round type="primary" size="small">上一步</cc-button>
      </view>
      <view class="demo-btn-item" @click="next">
        <cc-button round type="error" size="small">下一步</cc-button>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const active = ref(0)
const list1 = [
  {
    name: '下单',
  },
  {
    name: '出库',
  },
  {
    name: '运输',
  },
  {
    name: '签收',
  },
]
const list2 = [
  {
    name: '下单',
    icon: 'chat',
  },
  {
    name: '出库',
    icon: 'search',
  },
  {
    name: '运输',
    icon: 'person',
  },
  {
    name: '签收',
    icon: 'heart',
  },
]
const list3 = [
  {
    name: '下单',
    activeColor: '#fa2c19',
  },
  {
    name: '出库',
    activeColor: '#fa2c19',
  },
  {
    name: '运输',
    activeColor: '#fa2c19',
  },
  {
    name: '签收',
    activeColor: '#fa2c19',
  },
]
const list4 = [
  {
    name: '下单',
    content: '描述信息',
  },
  {
    name: '出库',
    content: '描述信息',
  },
  {
    name: '运输',
    content: '描述信息',
  },
  {
    name: '签收',
    content: '描述信息',
  },
]

const pre = () => {
  active.value--
  if (active.value === 0) {
    active.value = 0
  }
}
const next = () => {
  active.value++
  if (active.value === 3) {
    active.value = 3
  }
}
</script>

<style scoped lang="scss">
.demo {
  margin-bottom: 40rpx;
  &-title {
    padding: 20rpx;
  }
  &-item {
    margin-left: 4rpx;
    view {
      margin-left: 30rpx;
    }
  }
  &-btn {
    display: flex;
    align-items: center;
    &-item {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
}
</style>
