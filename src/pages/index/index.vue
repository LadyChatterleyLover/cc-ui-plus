<template>
  <view class="demo">
    <view v-for="(item, index) in list" :key="index" class="demo-item">
      <view class="demo-title">{{ item.title }}</view>
      <view
        v-for="(item1, index1) in item.item"
        :key="index1"
        class="demo-content"
        @click="clickItem(item1)"
      >
        <view class="demo-content-left">
          <view class="demo-content-left-label">{{ item1.label }}</view>
          <view class="demo-content-left-title">{{ item1.title }}</view>
        </view>
        <view class="demo-content-icon">
          <cc-icon type="arrowright" size="22" color="#979797" />
        </view>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
interface Item {
  label: string
  title: string
  url: string
}
interface ListItem {
  title: string
  item: Item[]
}
const list: ListItem[] = [
  {
    title: '基础组件',
    item: [
      {
        label: 'Button',
        title: '按钮',
        url: '/pages/button/index',
      },
      {
        label: 'Cell',
        title: '单元格',
        url: '/pages/cell/index',
      },
      {
        label: 'Layout',
        title: '布局',
        url: '/pages/layout/index',
      },
      {
        label: 'Popup',
        title: '弹出层',
        url: '/pages/popup/index',
      },
      {
        label: 'Toast',
        title: '轻提示',
        url: '/pages/toast/index',
      },
    ],
  },
  {
    title: '表单组件',
    item: [
      {
        title: 'Calendar',
        label: '日历',
        url: '/pages/calendar/index',
      },
      {
        title: 'Checkbox',
        label: '复选框',
        url: '/pages/checkbox/index',
      },
      {
        title: 'Field',
        label: '输入框',
        url: '/pages/field/index',
      },
      {
        title: 'Form',
        label: '表单',
        url: '/pages/form/index',
      },
      {
        title: 'NumberKeyboard',
        label: '数字键盘',
        url: '/pages/numberKeyboard/index',
      },
      {
        title: 'PasswordInput',
        label: '密码输入框',
        url: '/pages/passwordInput/index',
      },
      {
        title: 'Radio',
        label: '单选框',
        url: '/pages/radio/index',
      },
      {
        title: 'Rate',
        label: '评分',
        url: '/pages/rate/index',
      },
      {
        title: 'Search',
        label: '搜索框',
        url: '/pages/search/index',
      },
      {
        title: 'Slider',
        label: '滑块',
        url: '/pages/slider/index',
      },
      {
        title: 'Stepper',
        label: '步进器',
        url: '/pages/stepper/index',
      },
      {
        title: 'Switch',
        label: '开关',
        url: '/pages/switch/index',
      },
      {
        title: 'Checker',
        label: '选择项',
        url: '/pages/checker/index',
      },
      {
        title: 'Upload',
        label: '文件上传',
        url: '/pages/upload/index',
      },
    ],
  },
  {
    title: '反馈组件',
    item: [
      {
        title: 'ActionSheet',
        label: '动作面板',
        url: '/pages/actionSheet/index',
      },
      {
        title: 'modal',
        label: '弹出框',
        url: '/pages/modal/index',
      },
      {
        title: 'Alert',
        label: '警告提示',
        url: '/pages/alert/index',
      },
      {
        title: 'DropdownMenu',
        label: '下拉菜单',
        url: '/pages/dropdown/index',
      },
      {
        title: 'SlideVerify',
        label: '滑动验证码',
        url: '/pages/slideVerify/index',
      },
      {
        title: 'rotateVerify',
        label: '旋转验证',
        url: '/pages/rotateVerify/index',
      },
      {
        title: 'Loading',
        label: '加载',
        url: '/pages/loading/index',
      },
      {
        title: 'Notify',
        label: '消息通知',
        url: '/pages/notify/index',
      },
      {
        title: 'Overlay',
        label: '遮罩层',
        url: '/pages/overlay/index',
      },
      {
        title: 'PullRefresh',
        label: '下拉刷新',
        url: '/pages/pullRefresh/index',
      },
      {
        title: 'SwipeCell',
        label: '滑动单元格',
        url: '/pages/swipeCell/index',
      },
      {
        title: 'Avatar',
        label: '头像',
        url: '/pages/avatar/index',
      },
      {
        title: 'Qrcode',
        label: '生成二维码',
        url: '/pages/qrcode/index',
      },
    ],
  },
  {
    title: '展示组件',
    item: [
      {
        title: 'Badge',
        label: '徽标',
        url: '/pages/badge/index',
      },
      {
        title: 'CircleProgress',
        label: '圆形进度条',
        url: '/pages/circleProgress/index',
      },
      {
        title: 'Collapse',
        label: '折叠面板',
        url: '/pages/collapse/index',
      },
      {
        title: 'CountDown',
        label: '倒计时',
        url: '/pages/countDown/index',
      },
      {
        title: 'Countup',
        label: '数字滚动',
        url: '/pages/countup/index',
      },
      {
        title: 'TimeLine',
        label: '时间轴',
        url: '/pages/timeLine/index',
      },
      {
        title: 'Divider',
        label: '分割线',
        url: '/pages/divider/index',
      },
      {
        title: 'Steps',
        label: '步骤条',
        url: '/pages/steps/index',
      },
      {
        title: 'ImagePreview',
        label: '图片预览',
        url: '/pages/imagePreview/index',
      },
      {
        title: 'NotifyBar',
        label: '通知栏',
        url: '/pages/notifyBar/index',
      },
      {
        title: 'Progress',
        label: '进度条',
        url: '/pages/progress/index',
      },
      {
        title: 'Skeleton',
        label: '骨架屏',
        url: '/pages/skeleton/index',
      },
      {
        title: 'Subsection',
        label: '分段器',
        url: '/pages/subsection/index',
      },
      {
        title: 'Sticky',
        label: '粘性布局',
        url: '/pages/sticky/index',
      },
      {
        title: 'Swiper',
        label: '轮播',
        url: '/pages/swiper/index',
      },
      {
        title: 'Tag',
        label: '标签',
        url: '/pages/tag/index',
      },
      {
        title: 'OpenMore',
        label: '展开更多',
        url: '/pages/openMore/index',
      },
      {
        title: 'LoadMore',
        label: '加载更多',
        url: '/pages/loadMore/index',
      },
      {
        title: 'Gap',
        label: '间隔槽',
        url: '/pages/gap/index',
      },
      {
        title: 'Fab',
        label: '悬浮按钮',
        url: '/pages/fab/index',
      },
    ],
  },
  {
    title: '导航组件',
    item: [
      {
        title: 'Grid',
        label: '宫格',
        url: '/pages/grid/index',
      },
      {
        title: 'IndexBar',
        label: '索引栏',
        url: '/pages/indexBar/index',
      },
      {
        title: 'NavBar',
        label: '导航栏',
        url: '/pages/navBar/index',
      },
      {
        title: 'BackTop',
        label: '回到顶部',
        url: '/pages/backTop/index',
      },
      {
        title: 'Pagination',
        label: '分页',
        url: '/pages/pagination/index',
      },
      {
        title: 'Sidebar',
        label: '侧边导航',
        url: '/pages/sidebar/index',
      },
      {
        title: 'Tabs',
        label: '标签页',
        url: '/pages/tabs/index',
      },
      {
        title: 'FullTabs',
        label: '全屏选项卡',
        url: '/pages/fullTabs/index',
      },
      {
        title: 'Tabbar',
        label: '标签栏',
        url: '/pages/tabbar/index',
      },
      {
        title: 'Link',
        label: '超链接',
        url: '/pages/link/index',
      },
      {
        title: 'TreeSelect',
        label: '分类选择',
        url: '/pages/treeSelect/index',
      },
    ],
  },
  {
    title: '业务组件',
    item: [
      {
        title: 'AddressEdit',
        label: '地址编辑',
        url: '/pages/addressEdit/index',
      },
      {
        title: 'AddressList',
        label: '地址列表',
        url: '/pages/addressList/index',
      },
      {
        title: 'Area',
        label: '省市区选择',
        url: '/pages/area/index',
      },
      {
        title: 'GoodsCard',
        label: '商品卡片',
        url: '/pages/goodsCard/index',
      },
      {
        title: 'ContactCard',
        label: '联系人卡片',
        url: '/pages/contactCard/index',
      },
      {
        title: 'ContactEdit',
        label: '联系人编辑',
        url: '/pages/contactEdit/index',
      },
      {
        title: 'ContactList',
        label: '联系人列表',
        url: '/pages/contactList/index',
      },
      {
        title: 'GoodsAction',
        label: '商品导航',
        url: '/pages/goodsAction/index',
      },
      {
        title: 'SubmitBar',
        label: '提交订单栏',
        url: '/pages/submitBar/index',
      },
      {
        title: 'Coupon',
        label: '优惠券',
        url: '/pages/coupon/index',
      },
      {
        title: 'ScrollBar',
        label: '滚动导航',
        url: '/pages/scrollBar/index',
      },
    ],
  },
  {
    title: '项目模板',
    item: [
      {
        title: 'ScrollBar',
        label: '滚动导航(左右联动)',
        url: '/pages/scrollBarLinkage/index',
      },
      {
        title: 'ScrollBar',
        label: '滚动导航(左右独立)',
        url: '/pages/scrollBar/index',
      },
      {
        title: 'ChooseCity',
        label: '城市选择',
        url: '/pages/chooseCity/index',
      },
    ],
  },
]
const clickItem = (item: Item) => {
  uni.navigateTo({
    url: item.url,
  })
}
</script>

<style lang="scss" scoped>
.demo {
  background: #f7f8fa;
  padding: 60rpx 50rpx;
  &-title {
    font-size: 14px;
    color: #909ca4;
    margin-bottom: 40rpx;
  }
  &-content {
    position: relative;
    display: flex;
    align-items: center;
    padding: 0 80rpx;
    background: #fff;
    height: 90rpx;
    background: #fff;
    border-radius: 44rpx;
    box-shadow: 0 1px 8rpx 0 rgb(102 102 102 / 6%);
    margin-bottom: 26rpx;
    &-left {
      display: flex;
      align-items: center;
      font-weight: 700;
      color: #333;
      &-title {
        margin-left: 20rpx;
      }
    }
    &-icon {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 60rpx;
    }
  }
}
</style>
