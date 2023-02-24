<template>
  <view class="cc-collapse">
    <view
      v-for="(item, index) in cloneList"
      :key="index"
      class="cc-collapse-item"
    >
      <cc-cell
        :title="item.title"
        :value="item.value"
        :label="item.label"
        :icon="item.icon"
        :border="item.border"
        :size="item.size"
        :icon-size="item.iconSize"
        :is-link="false"
        :class="{ disabled: item.disabled }"
        :name="item.name"
        @click="clickItem(item, index)"
      >
        <template #title>
          <slot v-if="item.titleSlot" :name="item.titleSlot" />
        </template>
        <template #value>
          <slot v-if="item.rightSlot" :name="item.rightSlot" />
        </template>
        <template #right-icon>
          <view v-if="item.arrow" class="cc-collapse-item-icon">
            <view
              class="cc-collapse-item-icon-content"
              :class="{ 'cc-collapse-item-icon-active': item.show }"
            >
              <cc-icon
                :color="
                  item.disabled
                    ? '#c8c9cc'
                    : item.arrowColor
                    ? item.arrowColor
                    : '#969799'
                "
                type="arrowdown"
                :size="item.arrowSize ? item.arrowSize : 50"
              />
            </view>
          </view>
        </template>
      </cc-cell>
      <view
        class="cc-collapse-item-wrap"
        :style="{ height: item.show ? height + 'px' : 0 }"
      >
        <view
          :id="`cc-collapse-item-content-${uid}-${index}`"
          class="cc-collapse-item-content"
        >
          <slot v-if="item.contentSlot" :name="item.contentSlot" />
          <view v-else>{{ item.content }}</view>
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
import { getCurrentInstance, nextTick, onMounted, ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface CollapseItem {
  title: string
  content?: string
  value: string | number | boolean
  label?: string
  icon?: string
  border?: boolean
  show?: boolean
  arrow?: boolean
  disabled?: boolean
  size?: string | number
  iconSize?: string | number
  name?: string | number
  arrowColor?: string
  arrowSize?: string | number
  titleSlot?: string
  rightSlot?: string
  contentSlot?: string
}

const instance = getCurrentInstance()
const props = withDefaults(
  defineProps<{
    list: CollapseItem[]
    current?: string | number
    accordion?: boolean
  }>(),
  {
    current: '',
    accordion: false,
  }
)

const cloneList = ref<CollapseItem[]>([])
const height = ref(0)
const active = ref<number | string>(0)
const uid = ref('')

const init = () => {
  nextTick(() => {
    cloneList.value.forEach((item, index) => {
      if (item.show === undefined) {
        item.show = false
      }
      if (item.arrow === undefined) {
        item.arrow = true
      }
      if (item.border === undefined) {
        item.border = true
      }
      if (!item.iconSize) {
        item.iconSize = '16'
      }
      if (!item.arrowColor) {
        item.arrowColor = '#969799'
      }
      if (!item.name) {
        item.name = index
      }
    })
    if (!props.current) {
      active.value = ''
    } else {
      if (typeof props.current === 'number') {
        active.value = props.current
      } else {
        active.value = props.list.findIndex(
          (item, index) =>
            item.name === props.current || index === Number(props.current)
        )
      }
    }
  })
}

const clickItem = (item: CollapseItem, index: number) => {
  if (props.accordion) {
    active.value = index
    item.show = !item.show
    cloneList.value.forEach((i) => {
      if (active.value !== index) i.show = false
    })
  } else {
    item.show = !item.show
  }
  nextTick(() => {
    nextTick(() => {
      uni
        .createSelectorQuery()
        .in(instance)
        .select('.cc-collapse-item-content')
        .boundingClientRect()
        .exec((res) => {
          height.value = res[0].height
        })
    })
  })
}

watch(
  () => props.list,
  (val) => {
    cloneList.value = cloneDeep(val)
  },
  { immediate: true, deep: true }
)

watch(
  () => active.value,
  (val) => {
    cloneList.value.forEach((item, index) => {
      if (val === index) {
        item.show = true
        nextTick(() => {
          uni
            .createSelectorQuery()
            .in(instance)
            .select('.cc-collapse-item-content')
            .boundingClientRect()
            .exec((res) => {
              height.value = res[0].height
            })
        })
      } else {
        item.show = false
      }
    })
  }
)

onMounted(() => {
  init()
})
</script>

<style scoped lang="scss">
.cc-collapse {
  width: 100%;
  .cc-collapse-item {
    width: 100%;
    &-icon {
      display: flex;
      align-items: center;
      &-content {
        transition: all 0.3s;
        margin-left: 8rpx;
      }
      &-active {
        transform: rotate(180deg);
      }
    }
    &-wrap {
      height: 0;
      transition: all 0.3s;
      overflow: hidden;
    }
    &-content {
      padding: 24rpx 32rpx;
      color: #969799;
      font-size: 14px;
    }
  }
  .disabled {
    color: #c8c9cc;
    pointer-events: none;
    cursor: not-allowed;
  }
}
</style>
