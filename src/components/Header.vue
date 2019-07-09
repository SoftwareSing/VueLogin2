<template>
  <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" :router = 'true' >
    <el-menu-item index="/dashboard">儀錶板</el-menu-item>
    <el-menu-item index="/workstation">工作台</el-menu-item>
    <el-menu-item index="/history" >歷史資料</el-menu-item>
    <el-menu-item index="4" @click="logout">登出</el-menu-item>
  </el-menu>
</template>
<script>
  import {Menu,Submenu,MenuItem} from 'element-ui';
  export default {
    data() {
      return {
        activeIndex: '1'
      };
    },
    methods: {
      logout(){
        localStorage.removeItem('token');
        this.$router.push('/login');
      },
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      }
    },
    components: {
      'el-menu':Menu,
      'el-menu-item':MenuItem,
      'el-submenu':Submenu,
    }
  }
</script>
<style scoped>
  @import url("element-ui/lib/theme-chalk/index.css");
  @import url("element-ui/lib/theme-chalk/display.css");
</style>