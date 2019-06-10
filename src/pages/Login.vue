<template>
<div class="form-wrapper">
  <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">

    <el-form-item label="账号" prop="username">
      <el-input v-model="ruleForm.username"></el-input>
    </el-form-item>

    <el-form-item label="密码" prop="password">
      <el-input type="password" v-model="ruleForm.password"></el-input>
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
      <el-button @click="resetForm('ruleForm')">取消</el-button>
    </el-form-item>
  </el-form>
</div> 
</template>
<script>
  export default {
    data() {
      return {
        ruleForm: {
          username:"",
          password:""
        },
        rules: {
          username:[
            //required 必须输入   message提示信息     trigger blur失去焦点时触发
            {required: true , message:"请输入用户名",trigger:'blur'},
          ],
          password:[
            {required: true , message:"请输入密码",trigger:'blur'},
          ],
        }
      }
    },
    methods:{
      //登录事件，提交账号密码到服务器
      onSubmit(){
        //提交到后台的数据
        const data = {
          uname: this.username,
          upwd: this.password
        }

        //如果表单的验证不通过，不提交表单
        this.$refs.form.validate((valid) => {
            //验证通过执行
            if(valid) {

              //调用axios
              this.$axios({
                //请求地址
                url:"http://localhost:8899/admin/account/login",
                //请求方式
                method:"POST",
                //数据
                data,
                withCredentials:true
              }).then(res =>{
                //解构并且赋值
                const {message,status} = res.data;

                //登录成功
                if(status === 0){
                  this.$router.push("/");
                }

                //登录失败
                if(status === 1){
                  this.$message.error(message);
                }
              })
            }   
        });
      }
    }
  }
</script>

<style scopes>
    .form-wrapper{
      width: 100%;
      background-color: #999;
      position: absolute;
      top:0;
      bottom: 0;
    }
    .demo-ruleForm{
      width: 500px;
      position: absolute;
      left: 50%;
      margin-left:-250px;
      top:50%;
      margin-top: -95px;
    }
</style>
