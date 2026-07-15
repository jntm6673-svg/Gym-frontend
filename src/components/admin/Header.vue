<template>
  <div class="header">
    <div class="header-brand">
      <img src="../../assets/logo.png" class="brand-logo" alt="logo">
      <span class="brand-title">健身房后台管理</span>
    </div>
    <div class="header-spacer"></div>
    <div class="header-actions">
      <el-dropdown trigger="click">
        <div class="user-trigger">
          <div class="user-avatar">
            <i class="el-icon-user-solid"></i>
          </div>
          <span class="user-name">{{ admin.adminAccount }}</span>
          <i class="el-icon-arrow-down arrow-icon"></i>
        </div>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item @click.native="logout">
            <i class="el-icon-switch-button"></i> 退出系统
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      admin: {},
    }
  },
  created() {
    this.admin = JSON.parse(window.localStorage.getItem('access-admin'))
  },
  methods: {
    logout() {
      localStorage.removeItem('access-admin');
      this.$router.push('/login');
    },
  }
}
</script>

<style scoped>
.header {
  height: 64px;
  width: 100%;
  line-height: 64px;
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  background: #fff;
  z-index: 300;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  padding: 0 20px;
}

/* 品牌区域 */
.header-brand {
  display: flex;
  align-items: center;
  min-width: 200px;
  user-select: none;
}

.brand-logo {
  width: 36px;
  height: 36px;
  margin-right: 10px;
}

.brand-title {
  font-size: 18px;
  font-weight: 700;
  color: #ec6508;
  letter-spacing: 1px;
  white-space: nowrap;
}

/* 中间弹性空间 */
.header-spacer {
  flex: 1;
}

/* 用户操作区域 */
.header-actions {
  display: flex;
  align-items: center;
}

.user-trigger {
  display: flex;
  align-items: center;
  height: 64px;
  padding: 0 12px;
  cursor: pointer;
  border-radius: 6px;
  transition: background 0.2s ease;
}

.user-trigger:hover {
  background: #f5f7fa;
}

.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ec6508, #f0883e);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
  color: #fff;
  font-size: 14px;
}

.user-name {
  font-size: 14px;
  color: #303133;
  margin-right: 4px;
  max-width: 120px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.arrow-icon {
  font-size: 12px;
  color: #909399;
  transition: transform 0.2s ease;
}

.user-trigger:hover .arrow-icon {
  transform: rotate(180deg);
}
</style>
