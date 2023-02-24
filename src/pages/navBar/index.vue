<template>
  <view class="demo">
    <cc-nav-bar
      fixed
      placeholder
      left-arrow
      left-text="倾城"
      title="普通成熟时"
      right-text="无条件"
    />
    <view>占位之后的内容: 岁月如歌</view>
    <view class="item1">
      <cc-nav-bar
        left-text="单车"
        title="富士山下"
        right-text="人来人往"
        @click-title="click"
        @click-right="right"
        @click-left="left"
      />
    </view>

    <view class="item2">
      <cc-nav-bar
        left-arrow
        left-text="十年"
        title="不要说话"
        right-text="浮夸"
      />
    </view>

    <view class="item3">
      <cc-nav-bar
        left-arrow
        left-icon="search"
        left-icon-color="#acf967"
        left-text-color="#1989fa"
        title-color="red"
        left-text="K歌之王"
        right-color="#976622"
        title="苦瓜"
        right-text="你的背包"
      />
    </view>

    <view class="item4">
      <cc-nav-bar
        background="#1989fa"
        left-arrow
        left-text="十年"
        title="不要说话"
        right-text="浮夸"
      />
    </view>

    <view class="item5">
      <cc-nav-bar title="陈奕迅林夕黄伟文陪你度过漫长岁月" />
    </view>

    <view class="item6">
      <cc-nav-bar>
        <template #left>
          <cc-tag>内疚</cc-tag>
        </template>
        <template #default>
          <view class="demo-rate">
            <cc-rate v-model="value" />
          </view>
        </template>
        <template #right>
          <cc-tag type="success">陀飞轮</cc-tag>
        </template>
      </cc-nav-bar>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const value = ref(3)

const click = () => {
  console.log('click')
}
const right = () => {
  console.log('right')
}
const left = () => {
  console.log('left')
}
</script>

<style scoped lang="scss">
.demo {
  background: #f7f8fa;
  height: 100vh;
  position: relative;
  &-rate {
    display: flex;
    justify-content: center;
  }
  .item1 {
    position: absolute;
    top: 100px;
    width: 100%;
  }
  .item2 {
    position: absolute;
    top: 200px;
    width: 100%;
  }
  .item3 {
    position: absolute;
    top: 300px;
    width: 100%;
  }
  .item4 {
    position: absolute;
    top: 400px;
    width: 100%;
  }
  .item5 {
    position: absolute;
    top: 500px;
    width: 100%;
  }
  .item6 {
    position: absolute;
    top: 600px;
    width: 100%;
  }
}
</style>
