<template>
  <div class="member-header">
    <div class="header-brand">
      <img src="../../assets/logo.png" class="brand-logo" alt="logo">
      <span class="brand-title">健身房系统</span>
    </div>
    <div class="header-spacer"></div>
    <div class="header-actions">
      <el-dropdown trigger="click">
        <div class="user-trigger">
          <div class="user-avatar">
            <i class="el-icon-user-solid"></i>
          </div>
          <span class="user-name">{{ member.memberUsername }}</span>
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
  name: "MemberHeader",
  data() {
    return {
      member: {},
    }
  },
  created() {
    this.member = JSON.parse(window.localStorage.getItem('access-member'))
  },
  methods: {
    logout() {
      localStorage.removeItem('access-member');
      this.$router.push('/login');
    },
  }
}
</script>

<style scoped>
.member-header {
  height: 64px;
  width: 100%;
  line-height: 64px;
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 300;
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
  color: #ec6905;
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
  background: rgba(255, 255, 255, 0.1);
}

.user-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ec6905, #f0883e);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
  color: #fff;
  font-size: 14px;
}

.user-name {
  font-size: 14px;
  color: #ddd;
  margin-right: 4px;
  max-width: 120px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.arrow-icon {
  font-size: 12px;
  color: #999;
  transition: transform 0.2s ease;
}

.user-trigger:hover .arrow-icon {
  transform: rotate(180deg);
}
</style>
