<template>
  <div class="member-aside">
    <!-- 用户信息卡片 -->
    <div class="user-card">
      <div class="avatar-wrapper">
        <div class="avatar-default">
          <i class="el-icon-user-solid"></i>
        </div>
      </div>
      <h3 class="user-phone">{{ memberPhone }}</h3>
      <div class="checkin-area">
        <button
          class="checkin-btn"
          :class="{ 'checked': isActive }"
          :disabled="isActive"
          @click="checkIn"
        >
          <i :class="isActive ? 'el-icon-check' : 'el-icon-star-off'"></i>
          {{ checkin }}
        </button>
        <p class="checkin-hint">每日签到送积分</p>
      </div>
    </div>

    <!-- 导航菜单 -->
    <nav class="nav-menu">
      <ul>
        <li
          v-for="(option, index) in options"
          :key="index"
          class="nav-item"
          :class="{ 'nav-item-active': currentRoute === option.url }"
          @click="toContent(option.url)"
        >
          <i :class="option.icon"></i>
          <span>{{ option.name }}</span>
          <i class="el-icon-arrow-right nav-arrow"></i>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import { addCheckIn, getMemberIntegral, updateMemberIntegral } from "@/api/allApi";

export default {
  name: "MemberAside",
  data() {
    return {
      options: [
        { name: '全部课程', url: 'allCourse', icon: 'el-icon-notebook-2' },
        { name: '在线充值', url: 'onlinePay', icon: 'el-icon-wallet' },
        { name: '充值记录', url: 'rechargeRecord', icon: 'el-icon-tickets' },
        { name: '升级VIP', url: 'vipCard', icon: 'el-icon-medal' },
        { name: '购买记录', url: 'buyRecord', icon: 'el-icon-s-order' },
        { name: '我的资料', url: 'myProfile', icon: 'el-icon-user' },
        { name: '修改密码', url: 'changePassword', icon: 'el-icon-lock' },
      ],
      isActive: false,
      checkin: '今日签到',
      checkTime: '',
      today: '',
      memberPhone: '',
      member: {},
      currentRoute: '',
    }
  },
  created() {
    this.member = JSON.parse(window.localStorage.getItem('access-member'))
    this.checkTime = window.localStorage.getItem('access-checkTime')
  },
  mounted() {
    let date = new Date()
    let year = date.getFullYear();
    let month = date.getMonth() + 1;
    let day = date.getDate();  // 修复: getDay() → getDate()
    month = (month > 9) ? month : ("0" + month);
    day = (day < 10) ? ("0" + day) : day;
    let today = year + "-" + month + "-" + day;
    this.today = today
    if (today == this.checkTime) {
      this.checkin = '今日已签'
      this.isActive = true
    } else {
      this.checkin = '今日签到'
      this.isActive = false
    }
    // 修复: this.admin → this.member
    this.memberPhone = this.member.memberPhone

    // 监听当前路由
    this.updateCurrentRoute()
  },
  watch: {
    '$route'() {
      this.updateCurrentRoute()
    }
  },
  methods: {
    updateCurrentRoute() {
      const path = this.$route.path
      const parts = path.split('/')
      this.currentRoute = parts[parts.length - 1]
    },
    toContent(url) {
      this.$router.push({ path: url })
    },
    checkIn() {
      if (this.today != this.checkTime) {
        window.localStorage.setItem('access-checkTime', this.today)
        this.isActive = true
        this.checkin = '今日已签'
        addCheckIn({
          memberNo: this.member.memberNo  // 修复: this.admin → this.member
        }).then(res => {
          this.$message.success(res.data.message || '签到成功')
          updateMemberIntegral({
            memberNo: this.member.memberNo,  // 修复: this.admin → this.member
            price: -1
          })
          this.getMemberIntegral()
        }).catch(err => {
          this.$message.error(err.data?.message || '签到失败')
        })
      }
    },
    getMemberIntegral() {
      getMemberIntegral({
        memberNo: this.member.memberNo  // 修复: this.admin → this.member
      }).then(res => {
        this.MemberIntegral = res.data
      }).catch(err => {
        console.log(err.message)
      })
    }
  }
}
</script>

<style scoped>
/* ========== 基础重置 ========== */
li {
  list-style: none;
}

a {
  text-decoration: none;
}

/* ========== 侧边栏容器 ========== */
.member-aside {
  float: left;
  width: 240px;
  min-height: calc(100vh - 88px);
  background: transparent;
  position: relative;
  z-index: 10;
  border-right: 1px solid rgba(255, 255, 255, 0.08);
}

/* ========== 用户信息卡片 ========== */
.user-card {
  padding: 28px 20px 20px;
  text-align: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  background: transparent;
}

.avatar-wrapper {
  position: relative;
  display: inline-block;
  margin-bottom: 12px;
}

.avatar-default {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.1);
  border: 3px solid rgba(255, 255, 255, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 12px rgba(236, 101, 8, 0.2);
}

.avatar-default i {
  font-size: 30px;
  color: rgba(255, 255, 255, 0.5);
}

.user-phone {
  font-size: 15px;
  font-weight: 600;
  color: #ddd;
  margin-bottom: 14px;
}

/* ========== 签到区域 ========== */
.checkin-area {
  padding-top: 0;
}

.checkin-btn {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  background: linear-gradient(135deg, #ff6b35, #f7931e);
  color: #fff;
  border: none;
  border-radius: 20px;
  padding: 6px 20px;
  font-size: 13px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 3px 10px rgba(255, 107, 53, 0.25);
}

.checkin-btn:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 5px 16px rgba(255, 107, 53, 0.35);
}

.checkin-btn:active:not(:disabled) {
  transform: translateY(0);
}

.checkin-btn.checked {
  background: #555;
  box-shadow: none;
  cursor: not-allowed;
}

.checkin-hint {
  font-size: 11px;
  color: #888;
  margin-top: 8px;
}

/* ========== 导航菜单 ========== */
.nav-menu {
  padding: 8px 0;
}

.nav-item {
  display: flex;
  align-items: center;
  padding: 12px 20px;
  margin: 2px 12px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  color: #aaa;
  transition: all 0.2s ease;
  position: relative;
}

.nav-item i:first-child {
  margin-right: 10px;
  font-size: 16px;
  width: 20px;
  text-align: center;
  color: #888;
  transition: color 0.2s ease;
}

.nav-item span {
  flex: 1;
}

.nav-arrow {
  font-size: 12px;
  color: #666;
  transition: all 0.2s ease;
  opacity: 0;
  transform: translateX(-4px);
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #ec6508;
}

.nav-item:hover i:first-child {
  color: #ec6508;
}

.nav-item:hover .nav-arrow {
  opacity: 1;
  transform: translateX(0);
  color: #ec6508;
}

.nav-item:active {
  transform: scale(0.98);
}

/* 活跃状态 */
.nav-item-active {
  background: rgba(236, 101, 8, 0.15) !important;
  color: #ec6508 !important;
  font-weight: 600;
}

.nav-item-active i:first-child {
  color: #ec6508 !important;
}

.nav-item-active .nav-arrow {
  opacity: 1;
  transform: translateX(0);
  color: #ec6508;
}

/* 活跃指示器 */
.nav-item-active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 3px;
  height: 18px;
  background: #ec6508;
  border-radius: 0 2px 2px 0;
}
</style>
