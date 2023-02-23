<template>
  <view
    class="cc-subsection"
    :style="{ height: height + 'rpx', background: bgColor }"
  >
    <view
      v-for="(item, index) in list"
      :id="`cc-subsection-item-${index}`"
      :key="index"
      class="cc-subsection-item"
      :style="{
        color: currentIndex === index ? activeColor : inactiveColor,
        fontSize: fontSize + 'rpx',
      }"
      @click="clickItem(index)"
    >
      <view
        style="z-index: 1"
        :style="{ fontWeight: bold && currentIndex === index ? 700 : 400 }"
        >{{ item.name }}</view
      >
    </view>
    <view
      class="cc-subsection-mask"
      :style="{
        width: maskWidth + 'px',
        height: maskHeight + 'px',
        left: maskLeft + 'px',
      }"
    />
  </view>
</template>

<script lang="ts" setup>
import { getCurrentInstance, onMounted, ref, watch } from 'vue'

const instance = getCurrentInstance()

const props = withDefaults(
  defineProps<{
    // 当前激活项
    value?: number | string
    // 选项数组
    list: { name: string }[]
    // 激活时颜色
    activeColor?: string
    // 未激活时颜色
    inactiveColor?: string
    // 激活选项的字体是否加粗
    bold?: boolean
    // 高度
    height?: string | number
    // 背景颜色
    bgColor?: string
    // 字体大小
    fontSize?: number | string
  }>(),
  {
    value: 0,
    activeColor: '#0081ff',
    inactiveColor: '#606266',
    bold: true,
    height: 60,
    bgColor: '#eeeeef',
    fontSize: 28,
  }
)

const emits = defineEmits(['change'])

const currentIndex = ref(props.value)
const maskWidth = ref(0)
const maskHeight = ref(0)
const maskLeft = ref(0)

const init = () => {
  uni
    .createSelectorQuery()
    .in(instance)
    .select(`#cc-subsection-item-${currentIndex.value}`)
    .boundingClientRect((res) => {
      maskWidth.value = res.width
      maskHeight.value = res.height
      maskLeft.value = res.width * (currentIndex.value as number)
    })
    .exec()
}

const clickItem = (index: number) => {
  currentIndex.value = index
  init()
}

onMounted(() => {
  init()
})

watch(
  () => currentIndex.value,
  (val) => {
    emits('change', val)
  }
)
</script>

<style lang="scss" scoped>
.cc-subsection {
  width: 100%;
  display: flex;
  align-items: center;
  position: relative;
  &-item {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }
  &-mask {
    border: 16rpx;
    background: #fff;
    position: absolute;
    z-index: 0;
    transition: all 0.35s ease 0s;
  }
}
</style>
