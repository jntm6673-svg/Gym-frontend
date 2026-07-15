<template>
  <div class="all-course">
    <h2 class="page-title">
      <i class="el-icon-notebook-2"></i> 全部课程
      <span class="page-subtitle">选择你喜欢的课程开始训练吧</span>
    </h2>

    <!-- 课程卡片网格 -->
    <div class="course-grid" v-loading="loading">
      <div
        class="course-card"
        v-for="(course, index) in tableData"
        :key="course.courseNo"
        :style="{ '--card-color': cardColors[index % cardColors.length] }"
      >
        <!-- 卡片顶部图片 -->
        <div class="card-image" @click="showDetail(course)">
          <div class="card-image-bg" v-if="getCourseImage(course.courseName)">
            <img :src="getCourseImage(course.courseName)" :alt="course.courseName" class="card-img">
            <span class="card-badge">{{ course.courseDuration }}分钟</span>
          </div>
          <div class="card-image-bg card-image-fallback" v-else :style="{ background: getFallbackGradient(index) }">
            <i :class="cardIcons[index % cardIcons.length]" class="card-icon"></i>
            <span class="card-badge">{{ course.courseDuration }}分钟</span>
          </div>
        </div>

        <!-- 卡片内容 -->
        <div class="card-body">
          <h3 class="course-name" @click="showDetail(course)">{{ course.courseName }}</h3>
          <p class="course-desc">{{ course.courseDesc || '暂无描述' }}</p>

          <div class="course-meta">
            <span class="meta-item">
              <i class="el-icon-user"></i> {{ course.employeeNameCoach }}
            </span>
            <span class="meta-item">
              <i class="el-icon-date"></i> {{ course.courseTime }}
            </span>
          </div>

          <div class="card-footer">
            <div class="price-group">
              <span class="price-current">¥{{ course.discountPrice || course.coursePrice }}</span>
              <span class="price-original" v-if="course.discountPrice && course.discountPrice < course.coursePrice">
                ¥{{ course.coursePrice }}
              </span>
            </div>
            <button class="buy-btn" @click="buyCourse(course)">
              <i class="el-icon-shopping-cart-2"></i> 购买
            </button>
          </div>
        </div>
      </div>

      <!-- 空状态 -->
      <div class="empty-state" v-if="!loading && tableData.length === 0">
        <i class="el-icon-document"></i>
        <p>暂无课程数据</p>
      </div>
    </div>

    <!-- 课程详情弹窗 -->
    <el-dialog
      :title="detailCourse.courseName"
      :visible.sync="detailVisible"
      width="520px"
      top="10vh"
      class="course-dialog"
    >
      <div class="detail-content" v-if="detailCourse.courseNo">
        <div class="detail-header">
          <i :class="detailIcon" class="detail-icon"></i>
          <div>
            <h3>{{ detailCourse.courseName }}</h3>
            <p class="detail-price">
              <span class="price-current">¥{{ detailCourse.discountPrice || detailCourse.coursePrice }}</span>
              <span class="price-original" v-if="detailCourse.discountPrice && detailCourse.discountPrice < detailCourse.coursePrice">
                ¥{{ detailCourse.coursePrice }}
              </span>
            </p>
          </div>
        </div>

        <el-divider></el-divider>

        <div class="detail-info">
          <div class="info-row">
            <span class="info-label"><i class="el-icon-alarm-clock"></i> 课程时长</span>
            <span class="info-value">{{ detailCourse.courseDuration }} 分钟</span>
          </div>
          <div class="info-row">
            <span class="info-label"><i class="el-icon-date"></i> 开课时间</span>
            <span class="info-value">{{ detailCourse.courseTime }}</span>
          </div>
          <div class="info-row">
            <span class="info-label"><i class="el-icon-user"></i> 授课教练</span>
            <span class="info-value">{{ detailCourse.employeeNameCoach }}</span>
          </div>
          <div class="info-row">
            <span class="info-label"><i class="el-icon-phone"></i> 联系方式</span>
            <span class="info-value">{{ detailCourse.employeePhoneCoach }}</span>
          </div>
          <div class="info-row">
            <span class="info-label"><i class="el-icon-star-off"></i> 课程积分</span>
            <span class="info-value">{{ detailCourse.courseIntegral }} 积分</span>
          </div>
        </div>

        <el-divider></el-divider>

        <div class="detail-desc">
          <h4>课程描述</h4>
          <p>{{ detailCourse.courseDesc || '暂无描述' }}</p>
        </div>
      </div>

      <span slot="footer" class="dialog-footer">
        <el-button @click="detailVisible = false">关 闭</el-button>
        <el-button type="primary" @click="detailVisible = false; buyCourse(detailCourse)">
          <i class="el-icon-shopping-cart-2"></i> 立即购买
        </el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import moment from "moment";
import {
  addRegister,
  getAllCourse,
  getMemberPower,
  updateMemberChangeByMemberNo,
  updateMemberIntegral
} from "@/api/allApi";

// 导入课程图片
const courseImages = {
  yoga: require("@/assets/瑜伽.png"),
  swim: require("@/assets/游泳.png"),
  run: require("@/assets/跑步训练.png"),
  dumbbell: require("@/assets/哑铃训练.png"),
};

// 课程名称关键词 → 图片映射
const keywordMap = [
  { keys: ['瑜伽', 'yoga', 'YOGA'], img: 'yoga' },
  { keys: ['游泳', '泳', 'swim', 'SWIM'], img: 'swim' },
  { keys: ['跑步', '跑', 'run', 'RUN', '慢跑', '冲刺'], img: 'run' },
  { keys: ['哑铃', '力量', '器械', '举重', '杠铃', '深蹲', '硬拉'], img: 'dumbbell' },
];

export default {
  data() {
    return {
      tableData: [],
      admin: {},
      memberPower: '',
      coursePrice: '',
      loading: false,
      detailVisible: false,
      detailCourse: {},
      cardColors: [
        '#ec6508',
        '#2196f3',
        '#26a69a',
        '#7c4dff',
        '#ef5350',
        '#ff9800',
        '#009688',
        '#5c6bc0',
      ],
      cardIcons: [
        'el-icon-star-off',
        'el-icon-s-flag',
        'el-icon-medal',
        'el-icon-s-promotion',
        'el-icon-trophy',
        'el-icon-s-unfold',
        'el-icon-s-marketing',
        'el-icon-s-opportunity',
      ],
    }
  },
  computed: {
    indexMethod() {
      return (this.currentPage - 1) * 10 + 1
    }
  },
  filters: {
    dataFormat(value) {
      return moment(value).format("YYYY-MM-DD")
    }
  },
  created() {
    this.admin = JSON.parse(window.localStorage.getItem('access-member'))
  },
  mounted() {
    this.getMemberPower()
    this.getAllCourse()
  },
  methods: {
    // 根据课程名称自动匹配图片
    getCourseImage(courseName) {
      if (!courseName) return null
      for (const item of keywordMap) {
        for (const keyword of item.keys) {
          if (courseName.includes(keyword)) {
            return courseImages[item.img]
          }
        }
      }
      return null // 未匹配到，使用渐变兜底
    },
    // 未匹配到图片时的渐变色
    getFallbackGradient(index) {
      const color = this.cardColors[index % this.cardColors.length]
      return `linear-gradient(135deg, ${color}, ${color}dd)`
    },
    showDetail(course) {
      this.detailCourse = course
      this.detailVisible = true
    },
    buyCourse(course) {
      let _this = this
      addRegister({
        courseNo: course.courseNo,
        memberPhone: _this.admin.memberPhone,
        memberNo: _this.admin.memberNo,
      }).then(res => {
        if (res.data.code === 200) {
          updateMemberChangeByMemberNo({
            memberNo: this.admin.memberNo,
            coursePrice: course.coursePrice
          })
          updateMemberIntegral({
            memberNo: this.admin.memberNo,
            price: -course.courseIntegral
          })
          this.$message.success('购买成功！')
        } else {
          this.$message.error(res.data.message)
        }
      }).catch(err => {
        console.log(err.message)
      })
    },
    getMemberPower() {
      let _this = this
      getMemberPower({
        memberNo: _this.admin.memberNo
      }).then(res => {
        this.memberPower = res.data
      }).catch(err => {
        console.log(err.message)
      })
    },
    getAllCourse() {
      this.loading = true
      getAllCourse({
        page: 0,
        size: 50
      }).then(res => {
        this.tableData = res.data
      }).catch(err => {
        console.log(err.message)
      }).finally(() => {
        this.loading = false
      })
    }
  }
}
</script>

<style scoped>
/* ========== 页面容器 ========== */
.all-course {
  padding: 10px 10px 40px;
}

/* ========== 页面标题 ========== */
.page-title {
  font-size: 20px;
  color: #fff;
  margin-bottom: 4px;
  font-weight: 600;
}

.page-title i {
  color: #ec6508;
  margin-right: 6px;
}

.page-subtitle {
  font-size: 13px;
  color: #888;
  font-weight: 400;
  margin-left: 12px;
}

/* ========== 卡片网格 ========== */
.course-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: 20px;
}

/* ========== 课程卡片 ========== */
.course-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s ease;
  cursor: default;
}

.course-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
  border-color: var(--card-color);
}

/* ========== 卡片图片区域 ========== */
.card-image {
  height: 140px;
  cursor: pointer;
  overflow: hidden;
  position: relative;
}

.card-image-bg {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.3s ease;
  position: relative;
  overflow: hidden;
}

.card-image-fallback {
  opacity: 0.85;
}

.card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.course-card:hover .card-image-bg {
  transform: scale(1.05);
}

.card-icon {
  font-size: 48px;
  color: rgba(255, 255, 255, 0.7);
}

.card-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: rgba(0, 0, 0, 0.35);
  color: #fff;
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 10px;
  backdrop-filter: blur(4px);
}

/* ========== 卡片内容 ========== */
.card-body {
  padding: 16px;
}

.course-name {
  font-size: 16px;
  font-weight: 600;
  color: #eee;
  margin-bottom: 6px;
  cursor: pointer;
  transition: color 0.2s;
}

.course-name:hover {
  color: #ec6508;
}

.course-desc {
  font-size: 12px;
  color: #999;
  line-height: 1.5;
  height: 36px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  margin-bottom: 10px;
}

/* ========== 课程元信息 ========== */
.course-meta {
  display: flex;
  gap: 14px;
  margin-bottom: 14px;
  font-size: 12px;
  color: #888;
}

.meta-item {
  display: flex;
  align-items: center;
  gap: 4px;
}

.meta-item i {
  font-size: 13px;
}

/* ========== 卡片底部 ========== */
.card-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.price-group {
  display: flex;
  align-items: baseline;
  gap: 6px;
}

.price-current {
  font-size: 20px;
  font-weight: 700;
  color: #ec6508;
}

.price-original {
  font-size: 12px;
  color: #666;
  text-decoration: line-through;
}

.buy-btn {
  display: flex;
  align-items: center;
  gap: 4px;
  padding: 7px 16px;
  border: none;
  border-radius: 6px;
  font-size: 13px;
  cursor: pointer;
  background: linear-gradient(135deg, #ec6508, #f0883e);
  color: #fff;
  transition: all 0.2s ease;
}

.buy-btn:hover {
  box-shadow: 0 4px 14px rgba(236, 101, 8, 0.4);
  transform: translateY(-1px);
}

.buy-btn:active {
  transform: translateY(0);
}

/* ========== 空状态 ========== */
.empty-state {
  grid-column: 1 / -1;
  text-align: center;
  padding: 60px 0;
  color: #666;
}

.empty-state i {
  font-size: 48px;
  margin-bottom: 12px;
  display: block;
}

/* ========== 详情弹窗 ========== */
.detail-header {
  display: flex;
  align-items: center;
  gap: 16px;
}

.detail-icon {
  font-size: 40px;
  color: #ec6508;
}

.detail-header h3 {
  font-size: 18px;
  color: #eee;
}

.detail-price {
  margin-top: 4px;
}

.detail-price .price-current {
  font-size: 18px;
}

.detail-price .price-original {
  margin-left: 8px;
}

.detail-info .info-row {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  font-size: 14px;
}

.info-label {
  color: #999;
}

.info-label i {
  margin-right: 6px;
  color: #ec6508;
}

.info-value {
  color: #ddd;
}

.detail-desc h4 {
  color: #ccc;
  margin-bottom: 8px;
  font-size: 14px;
}

.detail-desc p {
  color: #999;
  font-size: 13px;
  line-height: 1.6;
}
</style>
