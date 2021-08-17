<template>
  <div class="login-container">
    <div class="login_box">
      <div class="avatr_box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        label-width="80px"
        :rules="loginFormRoules"
      >
        <el-form-item label="用户名" prop="username">
          <el-input
            prefix-icon="el-icon-user-solid"
            v-model="loginForm.username"
          >
          </el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input
            prefix-icon="el-icon-lock"
            v-model="loginForm.password"
            type="password"
          >
          </el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRoules: {
        username: [
          { required: true, message: '请输入正确用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在3~10', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入正确密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在6-15个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) {
          return this.$message.error('登录失败')
        } else {
          this.$message.success('登录成功')
          // 将登录后的token保存到客户端 sessionStorage
          window.sessionStorage.setItem('token', res.data.token)
          this.$router.push('/home')
        }
      })
    }
  }
}
</script>

<style Lang="less" scoped>
.login-container {
  background: #93b1d0;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background: #e3f1ff57;
  border: 1px solid #e2f0ff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  margin-bottom: 50%;
}
.avatr_box {
  width: 130px;
  height: 130px;
  border: 1px solid #e4e4e4;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0px 1px 5px #c0cfde;
  background: #a5d1ff;
  position: absolute;
  left: 0;
  right: 0;
  top: -20%;
  margin: auto;
}
img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: #fff;
}
form {
  margin-top: 25%;
  padding-right: 15px;
}
</style>
