<template>
<div>
  <br/>
  <br/>
<div class="box">
<el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="createBox">
  
  <el-form-item label="任务类型" prop="type">
    <el-col :span="11">
    <el-select v-model="ruleForm.type" placeholder="请选任务类型">
      <el-option label="取快递" value="2"></el-option>
      <el-option label="陪运动" value="3"></el-option>
      <el-option label="学习业务" value="4"></el-option>
      <el-option label="求夸业务" value="5"></el-option>
      <el-option label="其他业务" value="6"></el-option>
    </el-select>
    </el-col>
  </el-form-item>
  
  <el-form-item label="任务简介" prop="descript">
    <el-input type="textarea" v-model="ruleForm.descript" placeholder="请简单描述任务">
    </el-input>
  </el-form-item>

  <el-form-item label="任务详情" 
                prop="detail"  
                 style="width:60%">
    <el-input type="textarea" v-model="ruleForm.detail" style="height:100%">
    </el-input>
  </el-form-item>
  
  <el-form-item label="截止时间" required>
    <el-col :span="8">
      <el-form-item prop="deadline">
        <el-date-picker 
           type="date" 
            placeholder="选择日期" 
            v-model="ruleForm.deadline" 
            format="yyyy-MM-dd" 
            value-format="yyyy-MM-dd"
            style="width: 100%;"></el-date-picker>
      </el-form-item>
    </el-col>
    
    
  </el-form-item>

  <el-form-item label="联系方式" prop="phone_num">
    <el-col :span="11">
    <el-input placeholder="手机号" 
             v-model="ruleForm.phone_num"
             style="width:100%"></el-input>
    </el-col>
  </el-form-item>

    <el-form-item prop="weixin">
      <el-col :span="11">
      <el-input placeholder="微信号" 
                v-model="ruleForm.weixin"
                style="width:100%"></el-input>
      </el-col>
  </el-form-item>

  <el-form-item label="需求份数" prop="need">
    <el-col :span="5">
      <el-input  
                v-model="ruleForm.need"
                style="width:100%"></el-input>
      </el-col>
      
      <el-col :span="11">
        <el-form-item label="赏金" prop="price">
      <el-input 
                v-model="ruleForm.price"
                style="width:100%"></el-input>
        </el-form-item>
      </el-col>
  </el-form-item>
  
  <el-form-item>
    <el-button @click="goBack">返回上一级</el-button>
    <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
  </el-form-item>
</el-form>
</div>
</div>
</template>
  


<script>
  export default {
    data() {
      return {
        ruleForm: {
          type: '',
          descript:'',
          detail:'',
          deadline: '',
          phone_num:'',
          weixin:'',
           need:'',
           price:''
        },
        rules: {
          type: [
            { required: true, message: '请选择任务类型', trigger: 'change' },
          ],
          descript: [
            { required: true, message: '请简单描述任务', trigger: 'blur' }
          ],
          deadline: [
            { required: true, message: '请确认任务截止日期', trigger: 'blur' }
          ],
          phone_num: [
            { required: true, message: '必填', trigger: 'blur' }
          ],
          weixin: [
            { required: true, message: '必填', trigger: 'blur' }
          ],
          need: [
            { required: true, message: '必填', trigger: 'blur' }
          ],
           price: [
            { required: true, message: '必填', trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
            this.updateTask(this);
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      //返回上一级
      goBack:function() {
        this.$router.go(-1);
      },
      updateTask: function(vm) {
        var jsonData = {
          type: parseInt(this.ruleForm.type),
          description: this.ruleForm.descript,
          detail: this.ruleForm.detail,
          deadline: this.ruleForm.deadline,
          phone_num: this.ruleForm.phone_num,
          wechat: this.ruleForm.weixin,
          quantity:parseInt(this.ruleForm.need),
          reward:parseFloat(this.ruleForm.price)
        };
        var axios={method: "post",url: "/module/user/create_task",widthCredentials: false,data: jsonData};
        this.$http(axios).then(function(res) {
          if (res.status == 200) {
            alert(res.data.msg);
            if (res.data.msg=='successful'){
              vm.$router.push('/User/Part/Putjob/TodoTask');
            }
          } else {
            alert("Failed");
          }
        }).catch(function(err) {
          console.log(err);
        });
    },
    }
  }
</script>

<style scoped>
    .box{
      position: relative;
     border: 1.5px solid #eee;
      left: 30%;
      width: 600px;
      height: 700px;
    }
</style>