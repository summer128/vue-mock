<template>
  <div class="healthBase">
    <!--  基本信息-->
    <div class="user_block">
      <div class="user_msg_bar">
        <img src="@/assets/images/patientList/userLabel.png" alt="" />
        <p>基本信息</p>
        <el-button plain @click="edit">编辑</el-button>
      </div>
      <div class="base_msg_container">
        <p><span>姓名：</span>{{ healthBase.name }}</p>
        <p><span>籍贯：</span>{{ healthBase.birthplace }}</p>
        <p><span>性别：</span>{{ healthBase.sex }}</p>
        <p><span>文化程度：</span>{{ healthBase.education }}</p>
        <p><span>身份证号：</span>{{ healthBase.idCard }}</p>
        <p><span>民族：</span>{{ healthBase.nation }}</p>
        <p><span>手机号：</span>{{ healthBase.phoneNumber }}</p>
        <p><span>职业：</span>{{ healthBase.profession }}</p>
        <p><span>出生日期：</span>{{ healthBase.birthday }}</p>
        <p><span> 现住址：</span>{{ healthBase.presentAddr }}</p>
        <p><span> 通讯地址：</span>{{ healthBase.correspondenceAddr }}</p>
        <p><span> 国籍：</span>{{ healthBase.nationality }}</p>
        <p><span>工作单位：</span>{{ healthBase.company }}</p>
        <p><span>婚姻状况：</span>{{ healthBase.marriage }}</p>
        <p><span>定点医疗单位：</span>{{ healthBase.fixMedicalUnit }}</p>
        <div class="fill_more">
          <p>特殊类型人群：</p>
          <div class="fill_more_type">
            <p>{{ healthBase.specialPeople }}</p>
          </div>
        </div>
        <div class="fill_more">
          <p>医疗费用类型：</p>
          <div class="fill_more_type">
            <p>{{ healthBase.expenseType }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { get,post } from '../../../../request/http'

export default {
  data() {
    return {
      healthBase: null,
      userAge:'',
    };
  },
  activated() {
    //基本信息
    get(`/health/healthBase/${localStorage.getItem('userId')}`).then((res) => {
      this.healthBase = JSON.parse(JSON.stringify(res.data.data));
      this.getAge(this.healthBase.birthday)
      let visitedData = {
        name:this.healthBase.name,
        sex:this.healthBase.sex,
        age:this.userAge,
        phoneNumber:this.healthBase.phoneNumber,
      }
      window.localStorage.setItem('visitedData',JSON.stringify(visitedData))
    })
  },
  methods:{
    edit(){
      this.$router.push({ name: 'PatientEdit', query: {editIndex:'2',editData:JSON.parse(JSON.stringify(this.healthBase))}})
    },
    // 根据出生日期获取年龄
     getAge(userBirthday){
        let birthdays = new Date(userBirthday.replace(/-/g, "/"));
        let d = new Date();
        let age =
          d.getFullYear() -
          birthdays.getFullYear() -
          (d.getMonth() < birthdays.getMonth() ||
          (d.getMonth() == birthdays.getMonth() &&
          d.getDate() < birthdays.getDate())
            ? 1
            : 0);
        this.userAge = age;
      }
  }
};
</script>

<style lang="less" scoped>
.healthBase {
  
  // 基本信息--板块
  .base_msg_container {
    padding: 3%;
    background: white;
    p {
      width: 50%;
      line-height: 42px;
      float: left;
    }
    .fill_more {
      width: 100%;
      display: flex;
      p {
        width: 12%;
      }
      .fill_more_type {
        flex: 1;
        p {
          width: auto;
          margin: 0 1%;
        }
      }
    }
  }
}
</style>