<template>
  <el-form :model="ruleForm2" :rules="rules2" ref="ruleForm2" label-position="left" label-width="0px" class="demo-ruleForm login-container">
    <h3 class="title">咖啡预定系统登录</h3>
    <el-form-item prop="account">
      <el-input type="text" v-model="ruleForm2.account" auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item prop="checkPass">
      <el-input type="password" v-model="ruleForm2.checkPass" auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <!--<el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>-->
    <el-form-item style="width:100%;">
      <el-button type="primary" style="width:100%;" @click.native.prevent="handleSubmit2" :loading="logining">登录</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  export default {
    data() {
      return {
        logining: false,
        ruleForm2: {
          account: '',
          checkPass: ''
        },
        rules2: {
          account: [
            { required: true, message: '请输入账号', trigger: 'blur' },
          ],
          checkPass: [
            { required: true, message: '请输入密码', trigger: 'blur' },
          ]
        },
        checked: true
      };
    },
    methods: {
      handleSubmit2() {
          if(this.ruleForm2.account===null||this.ruleForm2.account===''){
              this.$message({
                message: "请先输入账号",
                type: 'error'
              });
              return false;
          }
          if(this.ruleForm2.checkPass===null||this.ruleForm2.checkPass===''){
              this.$message({
                  message: "请先输入密码",
                  type: 'error'
              });
              return false;
          }

          this.$http.post('/api/administer/login.do', {
              administerName: this.ruleForm2.account,
              administerPassword: this.ruleForm2.checkPass
              },
          ).then((res)=>{
              if(res.data.success){
                  sessionStorage.setItem('user', JSON.stringify(res.data.data));
                  this.$router.push({ path: '/order-list' });
                  // this.userList = res.data.data;
                  // this.total = parseInt(res.data.msg);
              }else{
                  console.log(res.data.msg)
              }
          }).catch(error =>{
              console.log(error)
          });


          // var _this = this;
        // this.$refs.ruleForm2.validate((valid) => {
        //   if (valid) {
        //     //_this.$router.replace('/table');
        //     this.logining = true;
        //     //NProgress.start();
        //     var loginParams = { username: this.ruleForm2.account, password: this.ruleForm2.checkPass };
        //     requestLogin(loginParams).then(data => {
        //       this.logining = false;
        //       //NProgress.done();
        //       let { msg, code, user } = data;
        //       if (code !== 200) {
        //         this.$message({
        //           message: msg,
        //           type: 'error'
        //         });
        //       } else {
        //         sessionStorage.setItem('user', JSON.stringify(user));
        //         this.$router.push({ path: '/order-list' });
        //       }
        //     });
        //   } else {
        //     console.log('error submit!!');
        //     return false;
        //   }
        // });
      }
    }
  }

</script>

<style lang="scss" scoped>
  .login-container {
    /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
    .title {
      margin: 0px auto 40px auto;
      text-align: center;
      color: #505458;
    }
    .remember {
      margin: 0px 0px 35px 0px;
    }
  }
</style>