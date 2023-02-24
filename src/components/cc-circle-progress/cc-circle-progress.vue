<template>
  <view class="cc-circle-progress">
    <view
      class="cc-circle-progress-box"
      :style="{ width: width + 'px', height: width + 'px' }"
    >
      <canvas
        class="cc-circle-progress-bg"
        :style="{ width: width + 'px', height: width + 'px' }"
        :canvas-id="`canvasProgressbg-${id}`"
      />
      <canvas
        class="cc-circle-progress-canvas"
        :style="{ width: width + 'px', height: width + 'px' }"
        :canvas-id="`canvasProgress-${id}`"
      />
      <view class="cc-circle-progress-text">
        <slot v-if="$slots.default" />
        <view v-else>{{ percent }}</view>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { getCurrentInstance, onMounted } from 'vue'

const instance = getCurrentInstance()
const id = instance?.uid

const props = withDefaults(
  defineProps<{
    percent?: number | string
    width?: number | string
    strokeWidth?: number | string
    layerColor?: string
    fillColor?: string
  }>(),
  {
    percent: 0,
    width: 100,
    layerColor: '#fff',
    fillColor: '#0081ff',
    strokeWidth: 4,
  }
)

const drawProgressbg = () => {
  const ctx = uni.createCanvasContext(`canvasProgressbg-${id}`, instance)
  ctx.beginPath()
  ctx.setLineWidth(Number(props.strokeWidth)) // 设置圆环的宽度
  ctx.setStrokeStyle(props.layerColor) // 设置圆环的颜色
  ctx.setLineCap('round') // 设置圆环端点的形状
  ctx.beginPath() //开始一个新的路径
  ctx.arc(
    Number(props.width) / 2,
    Number(props.width) / 2,
    Number(props.width) / 2 - 10,
    0,
    2 * Math.PI,
    false
  )
  ctx.stroke() //对当前路径进行描边
  ctx.draw()
}
const drawCircle = (step: number) => {
  const context = uni.createCanvasContext(`canvasProgress-${id}`, instance)
  context.setLineWidth(Number(props.strokeWidth))
  context.setStrokeStyle(props.fillColor)
  context.setLineCap('round')
  context.beginPath()
  // 参数step 为绘制的圆环周长，从0到2为一周 。 -Math.PI / 2 将起始角设在12点钟位置 ，结束角 通过改变 step 的值确定
  context.arc(
    Number(props.width) / 2,
    Number(props.width) / 2,
    Number(props.width) / 2 - 10,
    -Math.PI / 2,
    step * Math.PI - Math.PI / 2,
    false
  )
  context.stroke()
  context.draw()
}

onMounted(() => {
  drawProgressbg()
  drawCircle(Number(props.percent) / 50)
})
</script>

<style scoped lang="scss">
.cc-circle-progress {
  &-box {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  &-bg {
    position: absolute;
  }
  &-text {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
