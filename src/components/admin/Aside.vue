<template>
  <div class="aside">
    <!-- 侧边栏头部 -->
    <div class="aside-header">
      <i class="el-icon-menu"></i>
      <span>导航菜单</span>
    </div>

    <el-menu
      class="side-menu"
      default-active="0"
      background-color="#1e1e2d"
      text-color="#9899ac"
      active-text-color="#ffffff"
      :unique-opened="true"
      @open="handleOpen"
      @close="handleClose">

      <el-submenu index="0">
        <template slot="title">
          <i class="el-icon-s-home"></i>
          <span>主页</span>
        </template>
        <el-menu-item index="0-1" @click="toIndex">
          <i class="el-icon-data-line"></i>
          <span>主页</span>
        </el-menu-item>
      </el-submenu>

      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-user-solid"></i>
          <span>会员管理</span>
        </template>
        <el-menu-item index="1-1" @click="toMemberManage">
          <i class="el-icon-view"></i>
          <span>查看会员</span>
        </el-menu-item>
        <el-menu-item index="1-2" @click="toAddMembers">
          <i class="el-icon-circle-plus-outline"></i>
          <span>新增会员</span>
        </el-menu-item>
      </el-submenu>

      <el-submenu index="2">
        <template slot="title">
          <i class="el-icon-office-building"></i>
          <span>健身房管理</span>
        </template>
        <el-menu-item index="2-1" @click="toCourseManage">
          <i class="el-icon-notebook-2"></i>
          <span>课程管理</span>
        </el-menu-item>
        <el-menu-item index="2-2" @click="toEquipmentManage">
          <i class="el-icon-medal"></i>
          <span>器材管理</span>
        </el-menu-item>
      </el-submenu>

      <el-submenu index="3">
        <template slot="title">
          <i class="el-icon-s-custom"></i>
          <span>员工管理</span>
        </template>
        <el-menu-item index="3-1" @click="toEmployeeManage">
          <i class="el-icon-view"></i>
          <span>查看员工</span>
        </el-menu-item>
        <el-menu-item index="3-2" @click="toAddEmployee">
          <i class="el-icon-circle-plus-outline"></i>
          <span>新增员工</span>
        </el-menu-item>
      </el-submenu>

      <el-submenu index="4">
        <template slot="title">
          <i class="el-icon-s-order"></i>
          <span>报名表管理</span>
        </template>
        <el-menu-item index="4-1" @click="toRegisterManage">
          <i class="el-icon-document"></i>
          <span>查看报名表</span>
        </el-menu-item>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script>
export default {
  name: "Aside",
  data() {
    return {}
  },
  methods: {
    handleOpen(key, keyPath) {},
    handleClose(key, keyPath) {},
    toIndex() {
      this.$router.push('/layout/index')
    },
    toMemberManage() {
      this.$router.push('/layout/MemberManage')
    },
    toAddMembers() {
      this.$router.push('/layout/AddMembers')
    },
    toCourseManage() {
      this.$router.push('/layout/CourseManage')
    },
    toEquipmentManage() {
      this.$router.push('/layout/EquipmentManage')
    },
    toEmployeeManage() {
      this.$router.push('/layout/EmployeeManage')
    },
    toAddEmployee() {
      this.$router.push('/layout/AddEmployee')
    },
    toRegisterManage() {
      this.$router.push('/layout/RegisterManage')
    },
  }
}
</script>

<style scoped>
.aside {
  position: fixed;
  top: 64px;
  left: 0;
  width: 220px;
  height: calc(100vh - 64px);
  background: #1e1e2d;
  overflow-y: auto;
  z-index: 200;
}

/* 侧边栏头部 */
.aside-header {
  height: 48px;
  line-height: 48px;
  padding: 0 20px;
  color: #a2a3b7;
  font-size: 13px;
  letter-spacing: 1px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  user-select: none;
}

.aside-header i {
  margin-right: 8px;
}

/* el-menu 自定义 */
.side-menu {
  border-right: none !important;
  width: 100%;
}

/* 子菜单标题样式 */
::v-deep .el-submenu .el-submenu__title {
  height: 50px;
  line-height: 50px;
  font-size: 14px;
  padding-left: 20px !important;
}

::v-deep .el-submenu .el-submenu__title:hover {
  background-color: rgba(255, 255, 255, 0.04) !important;
  color: #fff !important;
}

/* 展开的子菜单 */
::v-deep .el-submenu.is-opened > .el-submenu__title {
  color: #fff !important;
}

/* 子菜单图标颜色 */
::v-deep .el-submenu__title i {
  color: #9899ac;
  transition: color 0.2s ease;
}

::v-deep .el-submenu__title:hover i {
  color: #ec6508;
}

::v-deep .el-submenu.is-opened > .el-submenu__title i {
  color: #ec6508;
}

/* 菜单项样式 */
::v-deep .el-menu-item {
  height: 44px;
  line-height: 44px;
  font-size: 13px;
  padding-left: 56px !important;
  position: relative;
  transition: all 0.2s ease;
}

::v-deep .el-menu-item:hover {
  background-color: rgba(236, 101, 8, 0.08) !important;
  color: #fff !important;
}

::v-deep .el-menu-item.is-active {
  background: linear-gradient(90deg, rgba(236, 101, 8, 0.15), transparent) !important;
  color: #ec6508 !important;
  font-weight: 500;
}

/* 活跃指示器 */
::v-deep .el-menu-item.is-active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 20px;
  background: #ec6508;
  border-radius: 0 2px 2px 0;
}

/* 菜单项图标 */
::v-deep .el-menu-item i {
  margin-right: 8px;
  font-size: 14px;
}

/* 弹出子菜单时的小箭头动画 */
::v-deep .el-submenu__icon-arrow {
  transition: transform 0.3s ease;
  color: #9899ac;
}

/* 全局 el-menu 覆盖 - el-menu--popup 下拉子菜单 */
</style>

<style>
/* 非 scoped — 覆盖 el-menu 弹出层样式 */
.el-menu--popup {
  background-color: #1e1e2d !important;
  border: 1px solid rgba(255, 255, 255, 0.06) !important;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.3) !important;
}

.el-menu--popup .el-menu-item {
  color: #9899ac !important;
  background-color: #1e1e2d !important;
}

.el-menu--popup .el-menu-item:hover {
  background-color: rgba(236, 101, 8, 0.08) !important;
  color: #fff !important;
}
</style>
