<template>
  <div class="login-wrap">
    <el-button type="primary" @click="submitForm()">登录</el-button>
  </div>
</template>

// index.html 页面 通过 script 标签引入 jsencrypt.js
// 本来是想说这里通过require引入的
// 但是require是webpack的写法，vite不支持
// 看vite官方文档，说用 import.meta.glob('./jsencrypt.js'),
// 但是也没用，最后只好放在通过script标签引入

<script>
import { ref, reactive } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import { ElMessage } from "element-plus";
import axios from 'axios';

// const JSEncrypt = import.meta.globEager('../common/bin/jsencrypt.js');

const temp = {hello: 'world'};

// import JSEncrypt from '../common/bin/jsencrypt.js';

export default {
  setup() {
    const router = useRouter();
    const param = reactive({
      username: "admin",
      password: "123456",
    });

    const login = ref(null);
    const submitForm = () => {
      login.value.validate(async (valid) => {
        if (valid) {
          let obj = {};
          
          // 加密算法的key
          const encrypt = new JSEncrypt();
          encrypt.setPublicKey(
            "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDAfA20cRn7tWmU3R9rEA6UnH+1KzqoZAu0JJdDis+wgZTp8JKnkFBBMa8TsaI0muA0/J0dsY2wC6m3d5sT6lwXeLk9KOxasjtM3Y2lISLks6VRG6qE9CYKEynbMTf2WjygI08pxjiCDLTC5RRJ53fomE9JE0iXMFyYy9kKi1JcxwIDAQAB/8914nmEgq6wbH5Iuw5Ra6805doP14SEMbKEITlyloyc6b3l1EjRWe1HuwlGaHwlW2vBAOAKbu1EeBDaLjehmmH7gOB3Tr0OvR7smQIDAQAB"
          );

          // 对账号密码进行加密
          obj.username = encrypt.encrypt(param.username);
          obj.password = encrypt.encrypt(param.password);


          const res = await axios.post(
            '/animal/web/login',
            `username=${obj.username}&password=${obj.password}`,
          );

          console.log('登录接口返回 res', res);
          if (res.data.success) {
            // 存放登录通过证明
            localStorage.setItem("ms_username", param.name);
            router.push("/");
          } else {
            ElMessage.error(res.data.message);
          }
        } else {
          ElMessage.error("请输入账号密码");
          return false;
        }
      });
    };

    const store = useStore();
    store.commit("clearTags");



    // 这下面是路由导航首位判断，可以放在路由文件里面
    // 也可以放在main.ts 文件里面，看你自己喜欢
    const router = createRouter({
      history: createWebHashHistory(),
      routes
    });

    router.beforeEach((to, from, next) => {
      document.title = `${to.meta.title} | vue-manage-system`;
      const role = localStorage.getItem('ms_username');
      if (!role && to.path !== '/login') {
        next('/login');
      } else if (to.meta.permission) {
        // 如果是管理员权限则可进入，这里只是简单的模拟管理员权限而已
        role === 'admin'
          ? next()
          : next('/403');
      } else {
        next();
      }
    });

    return {
      param,
      rules,
      login,
      submitForm,
    };
  },
};
</script>

<style scoped>
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../assets/img/login-bg.jpg);
  background-size: 100%;
}
.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 20px;
  color: #fff;
  border-bottom: 1px solid #ddd;
}
.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
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