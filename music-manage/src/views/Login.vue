<template>
  <div class="login-container">

   <div class="login">
     <div class="imgk" style="flex: 1">
       <img src="../assets/images/455.png" style="width: 100%;height: 100%">
     </div>
     <div style="width: 50%;">
       <h1 style="margin-top: 18%;margin-bottom: 20px;text-align: center"> 音乐管理系统</h1>
       <el-form :model="ruleForm" :rules="rules">

         <el-form-item prop="username" style="width: 300px;margin-left:  20%;height: 50px">
           <el-input v-model="ruleForm.username" placeholder="请输入用户名" ></el-input>
         </el-form-item>
         <el-form-item prop="password" style="width: 300px;margin-left:  20%;height: 50px">
           <el-input type="password" placeholder="请输入密码"  v-model="ruleForm.password" @keyup.enter="submitForm" ></el-input>
         </el-form-item>
         <el-form-item>
           <el-button class="login-btn" type="primary" @click="submitForm" style="width: 300px;margin-left:  20%;">登录</el-button>
         </el-form-item>
       </el-form>
     </div>
   </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, getCurrentInstance, ref, reactive } from "vue";
import mixin from "@/mixins/mixin";
import { HttpManager } from "@/api/index";
import { RouterName, MUSICNAME } from "@/enums";

export default defineComponent({
  setup() {
    const { proxy } = getCurrentInstance();
    const { routerManager } = mixin();

    const nusicName = ref(MUSICNAME);
    const ruleForm = reactive({
      username: "admin",
      password: "123",
    });
    const rules = reactive({
      // username: [{ required: true, message: "请输入用户名", trigger: "blur" }],
      // password: [{ required: true, message: "请输入密码", trigger: "blur" }],
    });
    async function submitForm() {
      let username = ruleForm.username;
      let password = ruleForm.password;
      const result = (await HttpManager.getLoginStatus({username,password})) as ResponseBody;
      (proxy as any).$message({
        message: result.message,
        type: result.type,
      });

      if (result.success) routerManager(RouterName.Info, { path: RouterName.Info });
    }
    return {
      nusicName,
      ruleForm,
      rules,
      submitForm,
    };
  },
});
</script>

<style scoped>
.login-container {
  position: relative;
  background: url("../assets/images/9999.jpg");
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
  width: 100%;
  height: 100%;
}

.title {
  position: absolute;
  top: 50%;
  width: 100%;
  margin-top: -230px;
  text-align: center;
  font-size: 30px;
  font-weight: 600;
  color: #fff;
}

.login {
  position: absolute;
  left: 31%;
  top: 43%;
  width: 60%;
  height: 50%;
  margin: -150px 0 0 -190px;
  /*border-radius: 5px;*/
  background: #fff;
  display: flex;
  /*justify-content: flex-start;*/
}
.imgk{
  width: 50%;
  height: 100%;

}
.login-btn {
  width: 100%;
}
</style>
