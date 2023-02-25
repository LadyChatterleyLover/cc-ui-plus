<template>
  <view
    class="cc-checkbox-group"
    :style="{
      flexDirection: direction === 'vertical' ? 'column' : 'row',
    }"
  >
    <slot />
  </view>
</template>

<script lang="ts" setup>
import { provide, reactive, ref, watch } from 'vue'

const props = withDefaults(
  defineProps<{
    modelValue: any
    disabled?: boolean
    max?: number | string
    direction?: 'vertical' | 'horizontal'
    iconSize?: number | string
    checkedColor?: string
  }>(),
  {
    disabled: false,
    max: 0,
    direction: 'vertical',
    iconSize: 40,
    checkedColor: '#1989fa',
  }
)

const emits = defineEmits(['update:modelValue', 'change'])

const checked = ref([])
const childNameList = ref<any[]>([])

provide('checkboxGroupProps', reactive(props))

watch(
  () => props.modelValue,
  (val) => {
    checked.value = val
  },
  { immediate: true }
)

watch(
  () => checked.value,
  (val) => {
    emits('update:modelValue', val)
    emits('change', val)
  }
)

// 添加子组件实例
const addChildName = (name: any) => {
  childNameList.value.push(name)
}

const setChecked = (name: any) => {
  checked.value = name
}

provide('ccRadioGroupAddChildName', addChildName)
provide('ccRadioGroupSetChecked', setChecked)

defineExpose({
  modelValue: props.modelValue,
})
</script>

<style lang="scss" scoped>
.cc-checkbox-group {
  display: flex;
}
</style>
