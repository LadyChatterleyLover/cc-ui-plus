<template>
  <view>
    <view v-if="imgList.length" class="cc-upload">
      <view
        v-for="(item, index) in imgList"
        :key="index"
        class="cc-upload-list-item"
      >
        <image
          class="cc-upload-list-item-img"
          :src="item.url"
          :mode="imageMode"
          @click="preview(item, index)"
        />
        <view
          v-if="deletable"
          class="cc-upload-list-item-delete"
          @click="del(item, index)"
        >
          <cc-icon
            class="cc-upload-list-item-delete-icon"
            type="closeempty"
            size="12"
            color="#fff"
          />
        </view>
      </view>
    </view>
    <view class="cc-upload-btn" @click="upload">
      <slot v-if="$slots.default" />
      <view v-else class="cc-upload-add">
        <cc-icon :type="uploadButtonIcon" :size="uploadButtonSize" />
        <view v-if="uploadButtonText" class="cc-upload-add-text">{{
          uploadButtonText
        }}</view>
      </view>
    </view>
    <cc-toast ref="toast" />
  </view>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'
import cloneDeep from 'lodash-es/cloneDeep'

export interface FileListItem {
  url: string
}

const props = withDefaults(
  defineProps<{
    action?: string
    maxCount?: string | number
    imageMode?: string
    name?: string
    sizeType?: string[]
    sourceType?: string[]
    header?: any
    isPreview?: boolean
    deletable?: boolean
    maxSize?: string | number
    fileList?: FileListItem[]
    formData?: any
    limitType?: string[]
    uploadButtonText?: string
    uploadButtonIcon?: string
    uploadButtonSize?: string | number
  }>(),
  {
    action: '',
    maxCount: 99,
    imageMode: 'aspectFill',
    name: 'file',
    sizeType: () => ['original', 'compressed'],
    sourceType: () => ['album', 'camera'],
    limitType: () => ['png', 'jpg', 'jpeg', 'webp', 'gif'],
    header: () => ({}),
    formData: () => ({}),
    fileList: () => [],
    isPreview: true,
    deletable: true,
    maxSize: Number.POSITIVE_INFINITY,
    uploadButtonText: '选择图片',
    uploadButtonIcon: 'plusempty',
    uploadButtonSize: '16',
  }
)

const toast = ref()
const imgList = ref<FileListItem[]>([])
const uploadTask = ref<any>(null)

const emits = defineEmits([
  'oversize',
  'uploadSuccess',
  'uploadError',
  'change',
  'chooseFail',
  'preview',
  'delete',
  'listChange',
])

const upload = () => {
  uni.chooseImage({
    count: Number(props.maxCount),
    sizeType: props.sizeType,
    sourceType: props.sourceType,
    success: (res) => {
      // 选择图片成功
      if (res.errMsg === 'chooseImage:ok') {
        const files: any = res.tempFiles
        const filePaths: any = res.tempFilePaths
        // 检验文件大小
        if (files.some((file: any) => file.size > props.maxSize)) {
          toast.value?.show({
            title: `上传图片最大尺寸为${props.maxSize}kb`,
            type: 'error',
          })
          files.forEach((file: any) => {
            if (file.size > props.maxSize) {
              emits('oversize', {
                file,
                fileList: imgList.value,
              })
            }
          })
          return
        }
        // 检验文件格式
        // #ifdef H5
        files.forEach((file: any) => {
          if (!props.limitType.includes(file.type.split('/')[1])) {
            toast.value?.show({
              title: `上传图片只支持${props.limitType.join(',')}格式`,
              type: 'error',
            })
            return
          }
        })
        // #endif
        // #ifndef H5
        files.forEach((file: any) => {
          if (!props.limitType.includes(file.path.split('.')[1])) {
            toast.value?.show({
              title: `上传图片只支持${props.limitType.join(',')}格式`,
              type: 'error',
            })
            return
          }
        })
        // #endif
        filePaths.forEach((path: any) => {
          uni.showLoading({
            title: '上传中...',
          })
          const obj = {
            url: '',
          }
          // 开始上传
          uploadTask.value = uni.uploadFile({
            url: props.action,
            filePath: path,
            name: props.name,
            formData: props.formData,
            header: props.header,
            success: (result) => {
              if (result.statusCode === 200) {
                emits('uploadSuccess', {
                  data: JSON.parse(result.data),
                  statusCode: result.statusCode,
                })
                obj.url = path
                imgList.value.push(obj)
                toast.value?.show({
                  title: '上传成功',
                  type: 'success',
                })
              } else {
                emits('uploadError', {
                  data: JSON.parse(result.data),
                  statusCode: result.statusCode,
                })
                toast.value?.show({
                  title: '上传失败',
                  type: 'error',
                })
              }
              emits('change', {
                data: JSON.parse(result.data),
                statusCode: result.statusCode,
              })
            },
            fail: (err) => {
              toast.value?.show({
                title: '上传失败',
                type: 'error',
              })
              emits('uploadError', err)
              console.log(err)
            },
            complete: () => {
              uni.hideLoading()
            },
          })
        })
      } else {
        toast.value.show({
          title: '选择图片失败,请重新选择',
          type: 'warning',
        })
        emits('chooseFail')
      }
    },
    fail: (err) => {
      console.log(err)
    },
  })
}

const preview = (item: FileListItem, index: number) => {
  if (!props.isPreview) return
  uni.previewImage({
    current: index,
    urls: imgList.value.map((file) => file.url),
  })
  emits('preview', { item, index })
}

const del = (item: FileListItem, index: number) => {
  imgList.value.splice(index, 1)
  emits('delete', { item, index })
}

const clear = () => {
  imgList.value = []
}
const abort = () => {
  uploadTask.value.abort()
}
const onProgressUpdate = () => {
  uploadTask.value.uploadTask()
}

defineExpose({
  clear,
  abort,
  onProgressUpdate,
  uploadTask,
})

watch(
  () => props.fileList,
  (val) => {
    imgList.value = cloneDeep(val)
  },
  { deep: true, immediate: true }
)

watch(
  () => imgList.value,
  (val) => {
    emits('listChange', val)
  },
  { deep: true }
)
</script>

<style scoped lang="scss">
.cc-upload {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  &-list-item {
    width: 160rpx;
    height: 160rpx;
    background: #f7f7f7;
    border-radius: 12rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 20rpx 20rpx 20rpx;
    position: relative;
    &-img {
      width: 120rpx;
      height: 120rpx;
    }
    &-delete {
      position: absolute;
      top: 0;
      right: 0;
      width: 14px;
      height: 14px;
      background-color: rgba(0, 0, 0, 0.7);
      border-radius: 0 0 0 12px;
      z-index: 999;
      &-icon {
        position: relative;
        /* #ifdef H5 */
        top: -2rpx;
        /* #endif */
        /* #ifndef H5 */
        top: -12rpx;
        /* #endif */
        left: 4rpx;
      }
    }
  }
  &-add {
    width: 160rpx;
    height: 160rpx;
    background: #f7f7f7;
    border-radius: 12rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    font-size: 12px;
    margin: 0 20rpx 20rpx 20rpx;
    &-text {
      margin-top: 10rpx;
    }
  }
}
</style>
