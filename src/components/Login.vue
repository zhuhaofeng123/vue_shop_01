<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="..\assets\logo.png" alt="" />
      </div>
      <!-- 登录表单区 -->
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
        label-width="0px"
        class="login_form"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="el-icon-search"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="el-icon-search"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="restLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import qs from 'qs'
export default {
  data() {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'johndoe',
        password: 'secret',
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入注册名称', trigger: 'blur' },
          {
            min: 3,
            max: 10,
            message: '长度在 3 到 10 个字符',
            trigger: 'blur',
          },
        ],
        // 验证用户名是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          {
            min: 6,
            max: 15,
            message: '长度在 6 到 15 个字符',
            trigger: 'blur',
          },
        ],
      },
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    restLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      // 接收的第一个形参是预校验的结果，是一个布尔值
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post(
          'login/token',
          qs.stringify({
            username: this.loginForm.username,
            password: this.loginForm.password,
          })
        )
        if (res.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', res.access_token)
        this.$router.push('/home')
        // const result = this.$http.post('tokne', this.loginForm)
        // console.log(result)
        // .then((rep) => {
        //   if (rep.data.code == 200) {
        //     //  todo 缓存token到浏览器
        //     //  1. 将登录成功之后的token，保存到客户端的sessionStorage中
        //     //  1.1 项目中除了登录之外的其他API接口，必须在登录之后才能访问
        //     //  1.2 token 只应在当前网站打开期间生效，所以将token保存在sessionStorage中
        //     console.log(rep)
        //     window.sessionStorage.setItem('token', rep.data.token)
        //     //  2. 通过编程式导航跳转到后台主页，路由地址是home
        //     alert(rep.data.msg)
        //     this.$router.push('/home')
        //   } else {
        //     alert(rep.data.msg)
        //   }
        // })
        console.log(res)
        // const { data: res } = await this.$http.post('login', this.loginForm)
        // console.log(data)
        // if (res.message !== 'helloworld') return this.$message.error('登录失败')
        // this.$message.success('登录成功')
        // this.$router.push('/home')
      })
      // this.$refs.loginFormRef.validate(async valid => {
      //   if (!valid) return
      //   const { data: res } = await this.$http.post(
      //     '/chapter06/jwt/token',
      //     this.loginForm
      //   )
      //   console.log(res)
      // })
    },
  },
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
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
    }
  }
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>