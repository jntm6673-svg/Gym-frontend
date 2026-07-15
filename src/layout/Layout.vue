<template>
  <div class="layout">
    <!-- 头部 -->
    <Header />
    <!-- 主体 -->
    <div class="layout-body">
      <!-- 侧边栏 -->
      <Aside />
      <!-- 内容区域 -->
      <div class="content">
        <router-view />
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/admin/Header";
import Aside from "@/components/admin/Aside";

export default {
  name: "Layout",
  data() {
    return {
      flag: '',
    }
  },
  components: {
    Header,
    Aside,
  }
}
</script>

<style scoped>
.layout {
  min-height: 100vh;
  background: #f0f2f5;
}

.layout-body {
  display: flex;
  padding-top: 64px;
}

.content {
  width: calc(100% - 220px);
  margin-left: 220px;
  padding: 20px;
  min-height: calc(100vh - 64px);
  background: #f0f2f5;
  overflow-x: hidden;
}
</style>

<!-- 非 scoped：管理端文字加深，提高白底对比度 -->
<style>
.content .el-form-item__label {
  color: #303133 !important;
  font-weight: 500;
}

.content .el-table {
  color: #303133 !important;
}

.content .el-table th.el-table__cell {
  color: #303133 !important;
  font-weight: 600;
}

.content .el-table td.el-table__cell {
  color: #303133 !important;
}

.content .el-input__inner {
  color: #303133 !important;
  border-color: #c0c4cc !important;
}

.content .el-input__inner:focus {
  border-color: #ec6508 !important;
}

.content .el-input__inner:hover {
  border-color: #909399 !important;
}

.content .el-textarea__inner {
  color: #303133 !important;
  border-color: #c0c4cc !important;
}

.content .el-textarea__inner:focus {
  border-color: #ec6508 !important;
}

.content .el-select .el-input__inner {
  color: #303133 !important;
  border-color: #c0c4cc !important;
}

.content .el-tag--info {
  color: #303133 !important;
}

.content .el-page-header__content {
  color: #303133 !important;
  font-weight: 600;
}

.content .el-card__body {
  color: #303133;
}
</style>
