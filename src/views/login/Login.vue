<template>
  <div class="login-wrap">
    <img src="../../assets/img/login-bg.svg"
         alt="">
    <div class="ms-login">
      <div class="ms-title">博客管理系统</div>
      <el-form :model="param"
               :rules="rules"
               ref="login"
               label-width="0px"
               class="ms-content">
        <el-form-item prop="name">
          <el-input v-model="param.name"
                    placeholder="name">
            <template #prepend>
              <el-button icon="el-icon-user"></el-button>
            </template>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input type="password"
                    placeholder="password"
                    v-model="param.password"
                    @keyup.enter="submitForm()">
            <template #prepend>
              <el-button icon="el-icon-lock"></el-button>
            </template>
          </el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary"
                     @click="submitForm()">登录</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import { ElMessage } from "element-plus";

export default {
  setup() {
    const router = useRouter();
    const store = useStore();
    store.commit("clearTags");
    const param = reactive({
      name: "",
      password: "",
    });

    const rules = {
      name: [
        {
          required: true,
          message: "请输入用户名",
          trigger: "blur",
        },
      ],
      password: [
        { required: true, message: "请输入密码", trigger: "blur" },
      ],
    };
    const login = ref(null);
    const submitForm = () => {
      login.value.validate((valid) => {
        if (valid) {
          store.dispatch('login/loginAction', param)
          // router.push("/");
        } else {
          ElMessage.error("登录失败");
          return false;
        }
      });
    };

    return {
      param,
      rules,
      login,
      submitForm,
    };
  },
};
</script>

<style scoped lang="less">
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #3e557a;
  img {
    width: 500px;
    height: 400px;
    position: absolute;
    top: 50%;
    left: 10%;
    transform: translateY(-50%);
  }
}
.ms-title {
  width: 100%;
  letter-spacing: 3px;
  line-height: 50px;
  text-align: center;
  font-size: 22px;
  color: #fff;
  border-bottom: 1px solid #ddd;
}
.ms-login {
  position: absolute;
  left: 70%;
  top: 50%;
  width: 350px;
  margin: -150px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}
.ms-content {
  padding: 30px 30px;
}
.login-btn {
  text-align: center;
}
.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}
.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: #fff;
}
</style>