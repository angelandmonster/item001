<template>
  <div class="login_container">
    <div class="login_box">
      <!--图片区域-->
      <div class="logo_box">
        <img src="../assets/11.png" title="logo" />
      </div>
      <el-form ref="loginFormRef" :model="Login" :rules ="LoginRules" label-width="0px" class="el_box">
        <!--文本框区域-->
        <el-form-item prop="username">
          <el-input prefix-icon="el-icon-user-solid" v-model="Login.username"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input prefix-icon="el-icon-lock" v-model="Login.password" type="password"></el-input>
        </el-form-item>
        <!--按钮区域-->
        <el-form-item class="btns">
          <el-button type="primary" @click="LoginForm">登录</el-button>
          <el-button type="info" @click="resetForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
    data(){
        return{
            //数据的双向绑定
            Login:{
                username:'admin',
                password:'123456',
            },
            LoginRules:{
                //表单验证id和密码
                username:[
                    { required: true, message: '请输入登录id', trigger: 'blur' },
                    { min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        resetForm(){
            this.$refs.loginFormRef.resetFields();
        },
        LoginForm(){
            this.$refs.loginFormRef.validate(async valid =>{
                if(!valid) return;
                const {data:res} =await this.$http.post('login',this.Login);
                if(res.meta.status !==200) return this.$message.error("登录失败");
                this.$message.success("登录成功");

                //保存token到sessionstorge中
                window.sessionStorage.setItem('token',res.data.token);
                this.$router.push("home");
            })
        }
    }
};
</script>
<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
  width: 100%;
}
.login_box {
  background-color: #fff;
  width: 500px;
  height: 350px;
  border-radius: 15px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  .logo_box {
    width: 130px;
    height: 130px;
    border-radius: 50%;
    border: 1px #bfa solid;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
      border: 1px #bfa solid;
    }
  }
}
.el_box{
    position: absolute;
    bottom:0;
    padding:15px;
    width:100%;
    box-sizing: border-box;
}
.btns{
    display: flex;
    justify-content: flex-end;
}
</style>