<template>
  <div class="vipCard">
    <div class="charge-header">
      <div class="charge-header-left">
        <b class="number">{{MemberIntegral}}</b> 积分
        <p>可用积分</p>
      </div>
      <div class="charge-header-middle">
        <template>
          <b class="number" v-if="this.memberPower == '1'">体验VIP</b>
          <b class="number" v-else-if="this.memberPower == '2'">包月VIP</b>
          <b class="number" v-else-if="this.memberPower == '3'">包季VIP</b>
          <b class="number" v-else-if="this.memberPower == '4'">包年VIP</b>
          <b class="number" v-else>普通用户</b>
        </template>
        <p>当前权限</p>
      </div>
      <div class="charge-header-right">
        <b class="number">无</b>
        <p>剩余VIP免费训练次数</p>
      </div>
    </div>

    <div class="vip-items">
      <div class="item" v-for="item in vipCard">
        <div class="title" :style="{backgroundColor:item.color}">{{item.name}}</div>
        <div class="price">
          {{item.price}}
          <span>积分</span>
        </div>
        <div class="time">
          {{item.time}}
        </div>
          <ul>
            <li> {{item.li1}}</li>
            <li> {{item.li2}}</li>
            <li> {{item.li3}}</li>
          </ul>
        <a href="javascript:;" class="btn" :style="{backgroundColor:item.color}" @click="upgradeClick(item.price,item.name)">立即升级</a>
      </div>
    </div>

    <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="30%"
        :before-close="handleClose">
      <span>确定要使用<b style="color: #309975">{{price}}积分</b>开通<b style="color: #309975">{{name}}</b>吗？</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="upgradeVIP()">确 定</el-button>
      </span>
    </el-dialog>
  </div>




</template>


<script>
import {
  getMemberChange,
  getMemberIntegral,
  getMemberPower,
  updateMemberIntegral,
  updateMemberPower
} from "@/api/allApi";

export default {
  name: "VipCard",
  data(){
    return {
      vipCard:[
        {name:'体验vip',price:'15',time:'1天',li1:'消耗15个积分',li2:'全场 9 折',li3:'1 次免费训练（一天有效）',color:'#e16969'},
        {name:'包月vip',price:'60',time:'1个月',li1:'消耗 60 个积分',li2:'全场 8 折',li3:'5 次免费训练（一月有效）',color:'#0ec0e6'},
        {name:'包季vip',price:'120',time:'3个月',li1:'消耗 120 个积分',li2:'全场 7 折',li3:'10 次免费训练（一季有效）',color:'#514e9f'},
        {name:'包年vip',price:'300',time:'1年',li1:'消耗 299 个积分',li2:'全场 6 折',li3:'20 次免费训练（一年有效）',color:'#6f0ee6'},
      ],
      admin:{

      },
      MemberIntegral:'',
      dialogVisible: false,
      price:'',
      name:'',
      memberPower:''
    }
  },
  created() {
    this.admin = JSON.parse(window.localStorage.getItem('access-member'))
  },
  mounted() {
    this.getMemberIntegral();
    this.getMemberPower();
  },
  methods:{

    upgradeClick(price,name) {
      this.dialogVisible= true
      this.price = price
      this.name = name
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
    //点击升级VIP
    upgradeVIP() {
      let _this = this
      updateMemberIntegral({
        price: this.price,
        memberNo: _this.admin.memberNo,
      }).then(res => {
        if (this.price == 15) {
          updateMemberPower({
            memberPower: 1,
            memberNo: _this.admin.memberNo
          })
        } else if (this.price == 60) {
          updateMemberPower({
            memberPower: 2,
            memberNo: _this.admin.memberNo
          })
        } else if (this.price == 120) {
          updateMemberPower({
            memberPower: 3,
            memberNo: _this.admin.memberNo
          })
        } else if (this.price == 300) {
          updateMemberPower({
            memberPower: 4,
            memberNo: _this.admin.memberNo
          })
        }
      }).catch(err => {
        console.log(err.message)
      })
      this.getMemberIntegral()
      this.dialogVisible = false
    },
    getMemberIntegral(){
      let _this = this
      getMemberIntegral({
        memberNo: _this.admin.memberNo
      }).then(res=>{
        this.MemberIntegral = res.data
      }).catch(err=>{
        console.log(err.message)
      })
    },
    getMemberPower(){
      let _this = this
      getMemberPower({
        memberNo: _this.admin.memberNo
      }).then(res=>{
        this.memberPower = res.data

      }).catch(err=>{
        console.log(err.message)
      })
    }

  }
}

</script>

<style scoped>
li {
  list-style: none;
}

a {
  text-decoration: none;
}

.vipCard {
  padding: 40px;
}

/* ===== 顶部统计卡片：三列 flex ===== */
.charge-header {
  display: flex;
  gap: 20px;
  margin-bottom: 40px;
}

.charge-header-left,
.charge-header-middle,
.charge-header-right {
  flex: 1;
  height: 125px;
  border-radius: 8px;
  padding: 20px 24px;
}

.charge-header-left p,
.charge-header-middle p,
.charge-header-right p {
  margin-top: 12px;
  font-size: 14px;
}

/* ===== VIP 卡片：四列 grid ===== */
.vip-items {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  text-align: center;
}

.vip-items .item {
  border-radius: 8px;
  padding-bottom: 30px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.vip-items .item:hover {
  transform: translateY(-6px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
}

.vip-items .item .title {
  padding: 14px 0;
  font-size: 18px;
  color: #fff;
  margin-bottom: 16px;
  border-radius: 8px 8px 0 0;
}

.vip-items .item .price {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 6px;
}

.vip-items .item .price span {
  font-size: 12px;
  font-weight: 400;
}

.vip-items .item .time {
  margin-bottom: 12px;
  font-size: 13px;
}

.vip-items .item ul {
  margin: 0 12px 16px;
  font-size: 12px;
  padding-top: 10px;
}

.vip-items .item ul li {
  margin-bottom: 4px;
}

.vip-items .item .btn {
  border-radius: 50px;
  display: inline-block;
  margin-bottom: 0;
  color: #fff;
  padding: 8px 20px;
  font-size: 13px;
  line-height: 1;
  transition: opacity 0.2s;
}

.vip-items .item .btn:hover {
  opacity: 0.85;
}

.number {
  font-size: 32px;
}
</style>