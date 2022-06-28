<template>
  <div class="login-container">
    <el-form
      class="login-form"
      :model="loginForm"
      :rules="LoginRuleForm"
      ref="loginFormRef"
    >
      <div class="title-container">
        <h3 class="title">用户登录</h3>
        <svg-icon className="svg-language" icon="language"></svg-icon>
      </div>
      <!-- 用户名 -->
      <el-form-item prop="username">
        <span class="svg-container">
          <el-icon>
            <svg-icon icon="user"></svg-icon>
          </el-icon>
        </span>
        <el-input
          placeholder="username"
          name="username"
          type="text"
          v-model="loginForm.username"
        ></el-input>
      </el-form-item>
      <!-- 密码框 -->
      <el-form-item prop="password">
        <!-- 锁图标 -->
        <span class="svg-container">
          <el-icon>
            <svg-icon icon="password"></svg-icon>
          </el-icon>
        </span>
        <!-- on-input眼睛图标 -->
        <el-input
          placeholder="password"
          name="password"
          v-model="loginForm.password"
          :type="inputType"
        />
        <span class="show-pwd" @click="handlePasswordEdit">
          <el-icon>
            <svg-icon
              :icon="inputType === 'password' ? 'eye' : 'eye-open'"
            ></svg-icon>
          </el-icon>
        </span>
      </el-form-item>
      <el-button
        type="primary"
        style="width: 100%; margin-bottom: 30px"
        @click="handleLoginSubmit"
        >登录</el-button
      >
    </el-form>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { validatePassword } from './rule'
const inputType = ref('password')
const loginFormRef = ref()
const loginForm = reactive({
  username: '',
  password: ''
})
/*
 * 表单验证
 */
const LoginRuleForm = reactive({
  username: [
    {
      required: true,
      trigger: 'blur',
      message: '用户名必填'
    }
  ],
  password: [
    {
      required: true,
      trigger: 'blur',
      validator: validatePassword
    }
  ]
})
/**
 * 密码框铭文密文
 */
const handlePasswordEdit = () => {
  inputType.value = inputType.value === 'password' ? 'text' : 'password'
}
/**
 * 登录
 */
const handleLoginSubmit = async () => {
  if (!loginFormRef.value) return
  await loginFormRef.value.validate((valid) => {
    if (valid) {
      alert('登录')
    }
  })
}
</script>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;
$cursor: #fff;

.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  overflow: hidden;
  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
    ::v-deep(.el-form-item) {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }
    ::v-deep(.el-input) {
      display: inline-block;
      height: 47px;
      width: 85%;
      .el-input__wrapper {
        background-color: transparent !important;
        box-shadow: none;
        vertical-align: middle;
        height: 100%;
        width: 100%;
      }
      .el-input__wrapper.is-focus {
        box-shadow: none;
      }
      input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        color: $light_gray;
        height: 47px;
        caret-color: $cursor;
      }
    }
  }
  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    display: inline-block;
  }
  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
    ::v-deep(.svg-language) {
      position: absolute;
      top: 4px;
      right: 0;
      background-color: #fff;
      font-size: 22px;
      padding: 4px;
      border-radius: 4px;
      cursor: pointer;
    }
  }
  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
