<template>
<div class="fullscreen">
  <div class="register-box">
    <div style="text-align: center">
      <img src="static/img/logo.png" alt="" class="logo">
    </div>
    <p class="text-tips">你好，欢迎注册</p>
    <form action="" class="login-form">
      <div class="m-list-group">
        <div class="m-list-group-item">
          <input type="text" v-model="mobile" placeholder="手机号" class="m-input">
        </div>
        <div class="m-list-group-item">
          <input type="username" v-model="userName" placeholder="姓名" class="m-input">
        </div>
        <div class="m-list-group-item">
          <input type="password" v-model="password" placeholder="密码" class="m-input">
        </div>

      </div>
      <div class="m-list-group-item" style="margin-bottom: 15px;">
        <span style="float: left;line-height: 40px;margin-right: 20px;">用户类别</span>
        <el-select v-model="userType" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </div>

      <button class="m-btn sub select-none" @click.prevent="register">注册</button>
      <p class="text-tips">已经有账号？</p>
      <a href="/login" class="m-btn m-btn-text">登录</a>
    </form>
  </div>
</div>
</template>
<script>
export default {
  name: 'register',
  data () {
    return {
      options: [{
        value: '1',
        label: '学生'
      }, {
        value: '2',
        label: '教师'
      }],
      userType: '1',
      userName: '',
      password: '',
      mobile: ''
    }
  },
  methods: {
    register () {
      if (this.mobile === '') {
        this.$message.warning('手机号为必填项')
        return
      } else {
        if (!/^((13[0-9])|(14[5,7,9])|(15[^4])|(18[0-9])|(17[0,1,3,5,6,7,8]))\d{8}$/.test(this.mobile)) {
          this.$message.warning('手机号格式不正确！')
          return
        }
      }
      if (this.userName === '') {
        this.$message.warning('姓名为必填项')
        return
      } else {
        if (!/^[\u4e00-\u9fa5]{0,}$/.test(this.userName)) {
          this.$message.warning('姓名需要为中文')
          return
        }
      }
      if (this.password === '') {
        this.$message.warning('密码为必填项')
        return
      } else {
        if (!/^[A-Za-z0-9]+$/.test(this.password)) {
          this.$message.warning('密码为数字和字母组合！')
          return
        }
      }
      this.$http.post('/user/register', {}, {
        params: {
          userType: this.userType,
          userName: this.userName,
          mobile: this.mobile,
          password: this.password
        }
      }).then(res => {
        if (res.result === 1) {
          this.$message.success('注册成功~')
          // this.isLoging = true
          console.log(res.data)
          sessionStorage.setItem('token', res.data.accessToken)
          // this.user.id = 1
          // this.user.username = res.data.username
          sessionStorage.setItem('user', JSON.stringify(res.data))
          this.$router.push({name: 'home'})
        } else {
          this.$message.error(res.message)
        }
      })
    }
  }
}
</script>
<style type="text/css">
  .m-list-group{
    border-radius: 3px;
    padding: 0;
    margin: 0 0 20px;
  }
  .m-list-group .m-list-group-item{
    position: relative;
    display: block;
    padding: 6px 10px;
    margin-bottom: -1px;
    background-color: #fff;
    border: 1px solid #e7ecee;
  }
  .m-list-group .m-list-group-item:first-child{
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
  }
  .fullscreen{
    position: absolute;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .register-box{
    position: relative;
    width: 330px;
    margin: 0 auto;
    padding: 0px 15px;
  }
  .register-box .logo{
    max-width: 100%;
    margin-bottom: 30px;
  }
  .register-box .text-tips{
    text-align: center;
    color: #909DB7;
  }
  .register-box .m-input{
    width: 100%;
    padding: 10px;
    border: none;
    outline: none;
    box-sizing: border-box;
  }
  .register-box .m-btn{
    font-size: 18px;
    width: 100%;
    color: #fff;
    background-color: #36c1fa;
    display: inline-block;
    padding: 5px 5px;
    margin-bottom: 5px;
    line-height: 1.42857143;
    text-align: center;
    cursor: pointer;
    outline: none;
    border-radius: 2px;
    border: 1px solid #36c1fa;
    box-sizing: border-box;
    text-decoration: none;
  }
  .register-box .m-btn.m-btn-text{
    background: #fff;
    color: #36c1fa;
  }
  .register-box .m-btn:hover{
    background-color: #2DB7F5;
  }
  .register-box .m-btn.m-btn-text:hover{
    color: #fff;
    background-color: #36c1fa;
  }
  .register-box .m-btn:active{
    opacity: 0.8;
  }
  @media (max-width: 768px) {
    .register-box{
      width: auto;
    }
  }
</style>
