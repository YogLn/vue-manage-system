<template>
  <div class="header">
    <!-- 折叠按钮 -->
    <div class="collapse-btn"
         @click="collapseChage">
      <i v-if="!collapse"
         class="el-icon-s-fold"></i>
      <i v-else
         class="el-icon-s-unfold"></i>
    </div>
    <div class="logo">后台管理系统</div>
    <div class="header-right">
      <div class="header-user-con">
        <div class="full-screen"
             @click="hadleScreenClick">
          <img src="../assets/img/fullscreen.png"
               alt="" />
        </div>
        <!-- 用户头像 -->
        <div class="user-avator">
          <img :src="userInfo.avatar_url" />
        </div>
        <!-- 用户名下拉菜单 -->
        <el-dropdown class="user-name"
                     trigger="click"
                     @command="handleCommand">
          <span class="el-dropdown-link">
            {{userInfo.name}}
            <i class="el-icon-caret-bottom"></i>
          </span>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item command="user">个人中心</el-dropdown-item>
              <el-dropdown-item divided
                                command="loginout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
    </div>
  </div>
</template>
<script>
import { computed, onMounted } from "vue";
import { useStore } from "@/store";
import { useRouter } from "vue-router";
import screenfull from 'screenfull'
import LocalCache from '@/utils/cache';
export default {
  setup() {
    const router = useRouter();
    const store = useStore();

    const userInfo = computed(() => store.state.login.userInfo)

    const collapse = computed(() => store.state.collapse);
    // 侧边栏折叠
    const collapseChage = () => {
      store.commit("handleCollapse", !collapse.value);
    };

    onMounted(() => {
      if (document.body.clientWidth < 1500) {
        collapseChage();
      }
    });

    // 用户名下拉菜单选择事件
    const handleCommand = (command) => {
      if (command == "loginout") {
        LocalCache.clear()
        router.push("/login");
      } else if (command == "user") {
      }
    };

    const hadleScreenClick = () => {
      if (screenfull.isEnabled) {
        screenfull.toggle()
      }
    }

    return {
      collapse,
      userInfo,
      collapseChage,
      handleCommand,
      hadleScreenClick
    };
  },
};
</script>
<style scoped>
.header {
  position: relative;
  box-sizing: border-box;
  width: 100%;
  height: 70px;
  font-size: 22px;
  color: #fff;
}
.collapse-btn {
  float: left;
  padding: 0 21px;
  cursor: pointer;
  line-height: 70px;
}
.header .logo {
  float: left;
  width: 250px;
  line-height: 70px;
}
.header-right {
  float: right;
  padding-right: 15px;
}
.header-user-con {
  display: flex;
  height: 70px;
  align-items: center;
}
.btn-bell-badge {
  position: absolute;
  right: 0;
  top: -2px;
  width: 8px;
  height: 8px;
  border-radius: 4px;
  background: #f56c6c;
  color: #fff;
}
.btn-bell .el-icon-bell {
  color: #fff;
}
.user-name {
  margin-left: 10px;
}
.user-avator {
  margin: 0 10px;
}
.user-avator img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.el-dropdown-link {
  color: #fff;
  cursor: pointer;
}
.el-dropdown-menu__item {
  text-align: center;
}
.full-screen {
  cursor: pointer;
}
.full-screen img {
  width: 30px;
  line-height: 30px;
}
</style>
