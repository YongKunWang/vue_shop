<template>
    <!--父组件-->
    <div class="login_container">
      <!--子组件-->
      <div class="login_box">
        <!--头像区域開始-->
        <div class="avatar_box">
          <img src="../assets/logo.png" alt="">
        </div>
        <!--form表單開始-->
        <el-form ref="loginFormRef" :model="loginForm" :rules="rules" class="login_form" label-width="0px">
          <el-form-item prop="username" >
            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-users"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input type="password" v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="submitForm">登陆</el-button>
            <el-button type="info" @click="resetLoginForm('loginForm')">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    open2() {
      this.$message({
        message: '登陆成功！',
        type: 'success'
      })
    },

    open3() {
      this.$message({
        message: '登陆失败！',
        type: 'warning'
      })
    },
    submitForm() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.open3()
        this.open2()

        window.sessionStorage.setItem('token', res.data.token)

        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .login_container {
    position: relative;
    height: 100%;
    background-color: #2b4b6b;
  }

  .login_box {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    transform: translate(-50%, -50%);

    .avatar_box {
      position: absolute;
      top: 0px;
      left: 50%;
      width: 130px;
      height: 130px;
      padding: 10px;
      border-radius: 50%;
      border: 1px solid #eee;
      box-shadow: 0 0 10px #ddd;
      background-color: #fff;
      transform: translate(-50%,-50%);

      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }

  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 22px;
    box-sizing: border-box;
  }
  .btns {
    display: flex;
    justify-content: flex-end;
  }

</style>
