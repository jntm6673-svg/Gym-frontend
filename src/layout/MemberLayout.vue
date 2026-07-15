<template>
  <div class="member-layout" :style="bgStyle">
    <!-- 头部 -->
    <MemberHeader />
    <!-- 主体 -->
    <div class="member-body">
      <div class="w">
        <!-- 侧边栏 -->
        <MemberAside />
        <!-- 内容区域 -->
        <div class="content">
          <router-view />
        </div>
        <div class="clearfix"></div>
      </div>
    </div>
  </div>
</template>

<script>
import MemberHeader from "@/components/member/MemberHeader";
import MemberAside from "@/components/member/MemberAside";

export default {
  name: "MemberLayout",
  data() {
    return {
      flag: '',
      bgStyle: {
        backgroundImage: "url(" + require("@/assets/back.png") + ")",
        backgroundRepeat: "no-repeat",
        backgroundSize: "cover",
        backgroundPosition: "center top",
        backgroundAttachment: "scroll",
      },
    }
  },
  components: {
    MemberHeader,
    MemberAside,
  }
}
</script>

<style scoped>
.member-layout {
  min-height: 100vh;
  background-color: #1a1a1a;
}

.member-body {
  padding-top: 64px;
}

.w {
  width: 1200px;
  margin: 24px auto 0;
  background: rgba(0, 0, 0, 0.7);
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.4);
  overflow: hidden;
}

.content {
  float: left;
  width: calc(100% - 240px);
  min-height: calc(100vh - 88px);
  padding: 20px;
  background: transparent;
}

.clearfix {
  clear: both;
}
</style>

<!-- 非 scoped 样式：覆盖内容区 Element UI 组件为深色主题 -->
<style>
/* ===== 表格 ===== */
.content .el-table {
  background: rgba(255, 255, 255, 0.05) !important;
  color: #eee !important;
}

.content .el-table th.el-table__cell {
  background: rgba(255, 255, 255, 0.1) !important;
  color: #fff !important;
  font-weight: 600;
  border-bottom: 1px solid rgba(255, 255, 255, 0.15) !important;
}

.content .el-table td.el-table__cell {
  border-bottom: 1px solid rgba(255, 255, 255, 0.08) !important;
  color: #eee !important;
}

.content .el-table::before {
  background-color: transparent !important;
}

.content .el-table tr {
  background: transparent !important;
}

.content .el-table--striped .el-table__body tr.el-table__row--striped td.el-table__cell {
  background: rgba(255, 255, 255, 0.04) !important;
}

.content .el-table__body tr:hover > td.el-table__cell {
  background: rgba(255, 255, 255, 0.08) !important;
}

.content .el-table__expand-icon {
  color: #bbb !important;
}

/* ===== 表单 ===== */
.content .el-form-item__label {
  color: #eee !important;
  font-weight: 500;
}

.content .el-input__inner {
  background: rgba(255, 255, 255, 0.1) !important;
  border-color: rgba(255, 255, 255, 0.2) !important;
  color: #eee !important;
}

.content .el-input__inner::placeholder {
  color: #999 !important;
}

.content .el-input__inner:focus {
  border-color: #ec6508 !important;
}

.content .el-input.is-disabled .el-input__inner {
  background: rgba(255, 255, 255, 0.06) !important;
  color: #bbb !important;
  border-color: rgba(255, 255, 255, 0.1) !important;
}

.content .el-input__prefix,
.content .el-input__suffix {
  color: #bbb !important;
}

/* ===== 下拉框 ===== */
.content .el-select .el-input__inner {
  background: rgba(255, 255, 255, 0.1) !important;
  color: #eee !important;
}

/* ===== 弹窗 ===== */
.content .el-dialog {
  background: #2a2a3a !important;
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5) !important;
}

.content .el-dialog__title {
  color: #eee !important;
}

.content .el-dialog__body {
  color: #eee !important;
}

/* ===== 普通按钮（非 primary） ===== */
.content .el-button--default {
  background: rgba(255, 255, 255, 0.1) !important;
  border-color: rgba(255, 255, 255, 0.2) !important;
  color: #eee !important;
}

.content .el-button--default:hover {
  background: rgba(255, 255, 255, 0.15) !important;
  border-color: rgba(255, 255, 255, 0.3) !important;
  color: #fff !important;
}

/* ===== 单选按钮 ===== */
.content .el-radio-button__inner {
  background: rgba(255, 255, 255, 0.08) !important;
  border-color: rgba(255, 255, 255, 0.15) !important;
  color: #bbb !important;
}

.content .el-radio-button.is-active .el-radio-button__inner {
  background: #ec6508 !important;
  border-color: #ec6508 !important;
  color: #fff !important;
}

/* ===== 统计卡片（充值/VIP页） ===== */
.content .charge-header-left,
.content .charge-header-middle,
.content .charge-header-right {
  background: rgba(255, 255, 255, 0.08) !important;
  border-color: rgba(255, 255, 255, 0.15) !important;
  color: #eee !important;
}

.content .charge-header-left p,
.content .charge-header-middle p,
.content .charge-header-right p {
  color: #bbb !important;
}

.content .number {
  color: #ec6508 !important;
}

/* ===== VIP 卡片 ===== */
.content .vip-items .item {
  background: rgba(255, 255, 255, 0.08) !important;
}

.content .vip-items .item .price {
  color: #eee !important;
}

.content .vip-items .item .time {
  color: #bbb !important;
}

.content .vip-items .item ul {
  color: #ccc !important;
  border-top-color: rgba(255, 255, 255, 0.1) !important;
}

/* ===== 通用文字 ===== */
.content {
  color: #eee;
}

.content a {
  color: #ec6508;
}

/* ===== el-tabs ===== */
.content .el-tabs__item {
  color: #bbb !important;
}

.content .el-tabs__item.is-active {
  color: #ec6508 !important;
}

.content .el-tabs__nav-wrap::after {
  background-color: rgba(255, 255, 255, 0.1) !important;
}
</style>
