<template>
  <div class="login-index" :style="backgroundDiv">
    <!-- 遮罩层 -->
    <div class="login-overlay"></div>

    <!-- 登录卡片 -->
    <div class="login-wrapper">
      <el-form
        :model="ruleForm"
        :rules="rules"
        status-icon
        ref="ruleForm"
        label-position="left"
        label-width="0px"
        class="login-card">

        <!-- 品牌标题 -->
        <div class="card-header">
          <img src="../assets/logo.png" class="card-logo" alt="logo">
          <h2 class="card-title">健身房管理系统</h2>
          <p class="card-subtitle">Gym Management System</p>
        </div>

        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            type="text"
            v-model="ruleForm.adminAccount"
            auto-complete="off"
            placeholder="请输入用户名"
            prefix-icon="el-icon-user"
          ></el-input>
        </el-form-item>

        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            type="password"
            v-model="ruleForm.adminPassword"
            auto-complete="off"
            placeholder="请输入密码"
            prefix-icon="el-icon-lock"
            @keyup.enter.native="submitForm"
          ></el-input>
        </el-form-item>

        <!-- 身份选择 -->
        <div class="identity-switch">
          <el-radio-group v-model="identity" size="small">
            <el-radio-button label="1">会员登录</el-radio-button>
            <el-radio-button label="2">管理员登录</el-radio-button>
          </el-radio-group>
        </div>

        <!-- 登录按钮 -->
        <el-form-item style="width:100%; margin-top: 10px;">
          <el-button
            type="primary"
            class="login-btn"
            @click="submitForm"
            :loading="loading"
          >登 录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { getMemberPassword, getAdminPassword } from "@/api/allApi";

export default {
  data() {
    return {
      backgroundDiv: {
        backgroundImage: "url(" + require("@/assets/back.png") + ")",
        backgroundRepeat: "no-repeat",
        backgroundSize: "cover",
        backgroundPosition: "center",
      },
      identity: '1',
      loading: false,
      ruleForm: {
        adminAccount: '',
        adminPassword: ''
      },
      rules: {
        adminAccount: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
        adminPassword: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    submitForm() {
      if (this.identity === "2") {
        this.$refs.ruleForm.validate((valid) => {
          if (valid) {
            this.loading = true
            let _this = this
            getAdminPassword(_this.ruleForm).then(res => {
              if (res.data != null) {
                window.localStorage.setItem('access-admin', JSON.stringify(res.data))
                this.$message.success('登录成功')
                _this.$router.replace({ path: '/layout' })
              } else {
                this.$message.error('账号或密码错误')
              }
            }).catch(err => {
              this.$message.error('登录失败，请检查网络连接')
              console.log('登录失败:', err.message)
            }).finally(() => {
              this.loading = false
            })
          } else {
            return false;
          }
        });
      } else {
        this.$refs.ruleForm.validate((valid) => {
          if (valid) {
            this.loading = true
            let _this = this
            let memberPhone = _this.ruleForm.adminAccount
            let memberPassword = _this.ruleForm.adminPassword
            getMemberPassword({
              memberPhone: memberPhone,
              memberPassword: memberPassword
            }).then(res => {
              if (res.data.code === 200) {
                window.localStorage.setItem('access-member', JSON.stringify(res.data))
                this.$message.success('登录成功')
                _this.$router.replace({ path: '/memberLayout' })
              } else {
                this.$message.error('账号或密码错误')
              }
            }).catch(err => {
              this.$message.error('登录失败，请检查网络连接')
              console.log(err.message)
            }).finally(() => {
              this.loading = false
            })
          } else {
            return false;
          }
        });
      }
    },
  }
}
</script>

<style scoped>
.login-index {
  width: 100%;
  height: 100vh;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

/* 遮罩层 - 让背景暗一点突出登录卡片 */
.login-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(0, 0, 0, 0.45) 0%, rgba(0, 0, 0, 0.25) 100%);
  z-index: 0;
}

/* 卡片容器 */
.login-wrapper {
  position: relative;
  z-index: 1;
}

/* 登录卡片 */
.login-card {
  width: 400px;
  padding: 40px 40px 30px;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.4);
  animation: fadeInUp 0.5s ease;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 卡片头部 */
.card-header {
  text-align: center;
  margin-bottom: 30px;
}

.card-logo {
  width: 52px;
  height: 52px;
  margin-bottom: 8px;
}

.card-title {
  font-size: 22px;
  font-weight: 700;
  color: #ec6508;
  letter-spacing: 2px;
  margin-bottom: 4px;
}

.card-subtitle {
  font-size: 12px;
  color: #999;
  letter-spacing: 1px;
}

/* 深色卡片内的输入框 */
::v-deep .login-card .el-input__inner {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.15);
  color: #fff;
}

::v-deep .login-card .el-input__inner::placeholder {
  color: #888;
}

::v-deep .login-card .el-input__inner:focus {
  border-color: #ec6508;
}

::v-deep .login-card .el-input__prefix {
  color: #999;
}

/* 身份切换按钮 */
::v-deep .login-card .el-radio-button__inner {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.15);
  color: #aaa;
}

::v-deep .login-card .el-radio-button__inner:hover {
  color: #ec6508;
}

::v-deep .login-card .el-radio-button.is-active .el-radio-button__inner {
  background: #ec6508;
  border-color: #ec6508;
  color: #fff;
}

/* 身份切换 */
.identity-switch {
  text-align: center;
  margin: 16px 0;
}

/* 登录按钮 */
.login-btn {
  width: 100%;
  height: 44px;
  font-size: 16px;
  letter-spacing: 4px;
  background: linear-gradient(135deg, #ec6508, #f0883e) !important;
  border: none !important;
  border-radius: 6px !important;
  transition: all 0.3s ease;
}

.login-btn:hover {
  box-shadow: 0 4px 16px rgba(236, 101, 8, 0.4);
  transform: translateY(-1px);
}

.login-btn:active {
  transform: translateY(0);
}
</style>
