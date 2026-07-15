[README(1).md](https://github.com/user-attachments/files/30031073/README.1.md)
# 健身房管理系统

## 项目概述

健身房管理系统是一个基于前后端分离架构的综合性健身管理平台，旨在为健身房提供完整的数字化管理解决方案。系统包含管理员和会员两种角色，支持会员管理、课程预约、员工管理、器材管理、充值消费等核心业务功能。

### 核心功能

- **管理员端**：会员管理、员工管理、课程管理、器材管理、报名管理、数据可视化统计
- **会员端**：课程浏览与报名、在线充值、余额消费、积分兑换、个人信息管理、签到打卡
- **特色功能**：Excel批量导入导出会员数据、JWT身份认证、VIP等级体系、积分奖励机制

## 技术栈

### 前端
- Vue.js 2.x + Vue Router + Vuex
- Element UI 组件库
- Axios HTTP客户端
- ECharts 数据可视化
- Moment.js 时间处理

### 后端
- Spring Boot 框架
- MyBatis ORM框架
- MySQL 数据库
- JWT (JSON Web Token) 身份认证
- Hutool 工具库
- Apache POI Excel处理
- Lombok 代码简化

### 开发工具
- Maven 依赖管理
- IDEA 集成开发环境
- Navicat 数据库管理

## 项目架构

### 整体架构设计

系统采用经典的前后端分离架构，通过RESTful API进行数据交互：

```
前端(Vue) <--HTTP/JSON--> 后端(Spring Boot) <--MyBatis--> 数据库(MySQL)
```

### 模块间关系

**后端分层架构**：
- Controller层：接收HTTP请求，参数校验，返回响应
- Service层：业务逻辑处理，事务控制
- Mapper层：数据持久化，SQL映射
- Entity层：数据模型定义
- Utils层：工具类（JWT认证等）

**前端模块划分**：
- views：页面视图组件（管理员视图、会员视图）
- components：可复用组件（头部、侧边栏）
- router：路由配置与权限控制
- store：全局状态管理
- api：API接口封装
- utils：工具函数（axios封装）

### 数据流向

1. 用户在前端界面触发操作
2. 前端调用api模块中的接口方法
3. Axios发送HTTP请求到后端Controller
4. Controller调用Service进行业务处理
5. Service通过Mapper访问数据库
6. 数据沿原路返回至前端渲染展示

### 认证机制

系统采用JWT Token进行身份验证：
- 登录成功后生成Token并返回给前端
- 前端将Token存储在localStorage中
- 后续请求在请求头中携带Token
- 路由守卫拦截未授权访问并验证Token有效性
- Token有效期为24小时

## 目录结构

```
代码文件/
├── gym-backend/                          # 后端项目
│   ├── src/main/java/com/gym/
│   │   ├── controller/                   # 控制器层 - 处理HTTP请求
│   │   │   ├── AdminController.java      # 管理员登录与Token验证
│   │   │   ├── MemberController.java     # 会员CRUD、Excel导入导出
│   │   │   ├── EmployeeController.java   # 员工管理
│   │   │   ├── CourseController.java     # 课程管理
│   │   │   ├── EquipmentController.java  # 器材管理
│   │   │   ├── RegisterController.java   # 课程报名管理
│   │   │   ├── RechargeController.java   # 充值记录管理
│   │   │   └── CheckInController.java    # 签到功能
│   │   ├── service/                      # 服务层 - 业务逻辑处理
│   │   │   ├── AdminService.java
│   │   │   ├── MemberService.java        # 会员业务逻辑（含批量导入、积分计算）
│   │   │   ├── EmployeeService.java
│   │   │   ├── CourseService.java
│   │   │   ├── EquipmentService.java
│   │   │   ├── RegisterService.java
│   │   │   ├── RechargeService.java
│   │   │   └── CheckInService.java
│   │   ├── mapper/                       # 数据访问层 - MyBatis接口
│   │   │   ├── AdminMapper.java
│   │   │   ├── MemberMapper.java
│   │   │   ├── EmployeeMapper.java
│   │   │   ├── CourseMapper.java
│   │   │   ├── EquipmentMapper.java
│   │   │   ├── RegisterMapper.java
│   │   │   ├── RechargeMapper.java
│   │   │   └── CheckInMapper.java
│   │   ├── entity/                       # 实体层 - 数据模型
│   │   │   ├── Admin.java                # 管理员实体
│   │   │   ├── Member.java               # 会员实体（含VIP等级、积分、余额）
│   │   │   ├── Employee.java             # 员工实体（教练、前台、保洁、经理）
│   │   │   ├── Course.java               # 课程实体
│   │   │   ├── Equipment.java            # 器材实体
│   │   │   ├── Register.java             # 报名记录实体
│   │   │   ├── Recharge.java             # 充值记录实体
│   │   │   ├── CheckIn.java              # 签到记录实体
│   │   │   └── Common.java               # 通用统计实体
│   │   ├── utils/                        # 工具类
│   │   │   └── JwtUtil.java              # JWT Token生成与验证
│   │   └── BackendApplication.java       # Spring Boot启动类
│   ├── src/main/resources/
│   │   ├── com/gym/mapper/               # MyBatis XML映射文件
│   │   │   ├── AdminMapper.xml
│   │   │   ├── MemberMapper.xml          # 会员SQL映射（含批量插入、模糊查询）
│   │   │   ├── EmployeeMapper.xml
│   │   │   ├── CourseMapper.xml
│   │   │   ├── EquipmentMapper.xml
│   │   │   ├── RegisterMapper.xml
│   │   │   ├── RechargeMapper.xml
│   │   │   └── CheckInMapper.xml
│   │   ├── application.yml               # Spring Boot配置文件（端口、数据库）
│   │   └── banner.txt                    # 启动横幅
│   ├── doc/
│   │   └── db_gym.sql                    # 数据库建表脚本
│   └── pom.xml                           # Maven依赖配置
│
├── gym-frontend/                         # 前端项目
│   ├── src/
│   │   ├── views/                        # 页面视图
│   │   │   ├── admin/                    # 管理员页面
│   │   │   │   ├── Index.vue             # 首页仪表盘
│   │   │   │   ├── MemberManage.vue      # 会员管理（增删改查、搜索）
│   │   │   │   ├── AddMembers.vue        # 添加会员
│   │   │   │   ├── EmployeeManage.vue    # 员工管理
│   │   │   │   ├── AddEmployee.vue       # 添加员工
│   │   │   │   ├── CourseManage.vue      # 课程管理
│   │   │   │   ├── EquipmentManage.vue   # 器材管理
│   │   │   │   ├── RegisterManage.vue    # 报名管理
│   │   │   │   └── Echarts.vue           # 数据可视化统计
│   │   │   ├── member/                   # 会员页面
│   │   │   │   ├── allCourse.vue         # 所有课程浏览与报名
│   │   │   │   ├── VipCard.vue           # VIP卡购买与积分兑换
│   │   │   │   ├── OnlinePay.vue         # 在线充值
│   │   │   │   ├── RechargeRecord.vue    # 充值记录查询
│   │   │   │   ├── BuyRecord.vue         # 购买记录查询
│   │   │   │   ├── MyProfile.vue         # 个人资料管理
│   │   │   │   └── ChangePassword.vue    # 修改密码
│   │   │   └── Login.vue                 # 登录页面（管理员/会员）
│   │   ├── components/                   # 可复用组件
│   │   │   ├── admin/
│   │   │   │   ├── Header.vue            # 管理员端头部导航
│   │   │   │   └── Aside.vue             # 管理员端侧边栏菜单
│   │   │   ├── member/
│   │   │   │   ├── MemberHeader.vue      # 会员端头部导航
│   │   │   │   └── MemberAside.vue       # 会员端侧边栏菜单
│   │   │   └── Error.vue                 # 错误页面
│   │   ├── layout/                       # 布局组件
│   │   │   ├── Layout.vue                # 管理员布局（Header + Aside + 内容区）
│   │   │   └── MemberLayout.vue          # 会员布局
│   │   ├── router/
│   │   │   └── index.js                  # Vue Router路由配置（含路由守卫）
│   │   ├── store/
│   │   │   └── index.js                  # Vuex状态管理
│   │   ├── api/
│   │   │   └── allApi.js                 # 所有API接口封装（50+接口）
│   │   ├── utils/
│   │   │   ├── request.js                # Axios实例配置（代理、超时）
│   │   │   └── valid.js                  # 表单验证工具
│   │   ├── assets/                       # 静态资源
│   │   │   ├── css/                      # 全局样式
│   │   │   └── images/                   # 图片资源
│   │   ├── App.vue                       # 根组件
│   │   └── main.js                       # 入口文件（Vue实例初始化）
│   ├── public/
│   │   ├── index.html                    # HTML模板
│   │   └── favicon.ico                   # 网站图标
│   ├── package.json                      # 前端依赖配置
│   ├── vue.config.js                     # Vue CLI配置（代理设置）
│   └── babel.config.js                   # Babel转译配置
│
└── 数据库/
    └── db_gym.sql                        # 完整数据库结构与初始数据
```

## 核心文件说明

### 项目入口文件和配置文件

**后端入口**：`gym-backend/src/main/java/com/gym/BackendApplication.java`
- Spring Boot应用启动类
- 使用@SpringBootApplication注解启用自动配置
- 内置Tomcat服务器监听9090端口

**前端入口**：`gym-frontend/src/main.js`
- Vue实例初始化
- 注册Element UI组件库
- 挂载路由和状态管理

**配置文件**：
- `application.yml`：后端配置，包括数据库连接（MySQL）、服务端口（9090）、MyBatis日志
- `vue.config.js`：前端开发服务器配置，API代理设置（/api -> http://localhost:9090）
- `package.json`：前端依赖管理和npm脚本

### 核心业务逻辑实现

**会员管理服务**：`MemberService.java`
- 会员CRUD操作（增删改查）
- 批量导入：从Excel读取数据并批量插入数据库
- 登录认证：验证手机号和密码，生成JWT Token
- 积分管理：购买课程扣减积分，充值增加积分
- 余额管理：在线充值、课程消费扣款
- VIP等级：根据积分兑换不同等级的VIP权限
- 模糊查询：支持按姓名或手机号搜索会员

**课程管理服务**：`CourseService.java`
- 课程信息发布（名称、时间、时长、价格、教练）
- 课程报名：会员选择课程并扣除相应课时
- 课程查询：分页查询、关键词搜索

**员工管理服务**：`EmployeeService.java`
- 员工信息管理（姓名、年龄、性别、电话、职位）
- 职位分类：教练(1)、前台(2)、保洁(3)、经理(4)
- 经理关联：manager表关联employee表建立经理关系

**认证与授权**：`JwtUtil.java`
- createToken()：生成管理员Token
- createTokenToMember()：生成会员Token
- checkToken()：验证Token有效性和完整性
- Token包含用户名、角色信息，有效期24小时

### 数据模型和API接口

**核心数据模型**：

`Member.java` - 会员实体
- 基本信息：编号、用户名、密码、姓名、年龄、性别、电话
- 会员属性：身高、体重、开卡时间
- 账户信息：购买课时、剩余课时、积分、余额
- VIP体系：cardClass（购买课时）、cardNextClass（剩余课时）、memberPower（VIP等级）
- 个性化：个性签名、用户权限标识

`Employee.java` - 员工实体
- 基本信息：编号、姓名、年龄、性别、电话
- 工作信息：入职时间、职位描述、职位类型

`Course.java` - 课程实体
- 课程信息：编号、名称、时间、时长、价格、描述
- 关联关系：employeeNo（教练）、managerNo（经理）

`Register.java` - 报名记录
- 关联会员和课程的中间表

`Recharge.java` - 充值记录
- 充值信息：日期、方式、状态、金额、会员编号

**API接口设计**：

所有接口遵循RESTful风格，统一返回Map格式：
```json
{
  "code": 200,
  "message": "操作成功",
  "data": {...}
}
```

主要接口分类：
- 认证接口：getAdminPassword、getMemberPassword、checkToken
- 会员接口：getAllMember、addMember、updateMember、deleteMember、getByKeywordMember
- 课程接口：getAllCourse、addCourse、updateCourse、deleteCourse
- 员工接口：getAllEmployee、addEmployee、updateEmployee、deleteEmployee
- 充值接口：addRechargeByMemberNo、getRechargeByMemberNo
- 报名接口：addRegister、deleteRegister、getAllRegister
- 统计接口：totalMember、totalCourse、totalEmployee（返回数据总数用于分页）

### 关键组件和服务模块

**前端路由守卫**：`router/index.js`
- beforeEach全局前置守卫
- 登录状态检查：从localStorage读取access-admin和access-member
- Token验证：调用后端checkToken接口验证合法性
- 路由重定向：未登录跳转登录页，Token失效跳转错误页
- 路由嵌套：Layout布局组件包含子路由

**API封装**：`api/allApi.js`
- 统一管理50+个API接口
- 使用request.js封装的axios实例
- GET请求使用params传参，POST请求使用data传参
- 接口命名规范：动词+名词（如getAllMember、addMember）

**HTTP请求配置**：`utils/request.js`
- Axios实例创建，baseURL设置为/api
- 超时时间3秒
- 配合vue.config.js代理转发到后端9090端口

**数据可视化**：`Echarts.vue`
- 使用ECharts图表库展示统计数据
- 可能包含会员增长趋势、课程热度、收入统计等图表

**Excel导入导出**：`MemberController.java`
- 导出：查询所有会员数据，使用Hutool的ExcelWriter生成xlsx文件
- 导入：读取上传的Excel文件，解析为Member对象列表，批量插入数据库
- 使用@Alias注解映射Excel列名与实体属性

**数据库设计亮点**：
- 外键关联：course表的employeeNo和managerNo关联员工表
- 唯一约束：member表的memberPhone字段唯一
- 索引优化：常用查询字段建立索引
- 字符集：utf8mb4支持完整Unicode字符
- 初始数据：包含测试用的管理员、会员、员工、课程数据

### 技术亮点

1. **JWT无状态认证**：避免Session存储压力，支持分布式部署
2. **Excel批量处理**：提升数据录入效率，支持大规模会员导入
3. **分页查询**：所有列表接口支持分页，优化大数据量性能
4. **模糊搜索**：支持多字段模糊查询，提升用户体验
5. **VIP等级体系**：积分兑换VIP权限，激励会员消费
6. **双重布局**：管理员和会员独立布局，权限隔离清晰
7. **路由守卫**：前端权限控制，防止未授权访问
8. **代理配置**：开发环境解决跨域问题，生产环境可配置Nginx反向代理

### 运行环境要求

- JDK 17+
- Node.js（支持Vue CLI）
- MySQL 8.0+
- Maven 3.6+

### 启动说明

**后端启动**：
1. 导入数据库：执行db_gym.sql创建数据库和表
2. 修改配置：更新application.yml中的数据库账号密码
3. 启动项目：运行BackendApplication.main()方法
4. 访问地址：http://localhost:9090

**前端启动**：
1. 安装依赖：npm install
2. 启动开发服务器：npm run dev
3. 访问地址：http://localhost:8080（默认端口）
4. 打包部署：npm run build

### 默认账号

- 管理员：admin / admin
- 会员：18470558967 / admin（或其他测试会员账号）
