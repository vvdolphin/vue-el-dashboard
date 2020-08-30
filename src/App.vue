<template>
  <div id="app">
    <el-container>
      <el-container>
        <el-aside width="200px">
          <navmenu></navmenu>
        </el-aside>
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import NavMenu from '@/components/NavMenu'
// import Header from '@/components/Header'

import login from "./components/login";

export default {
  name: 'app',
  components: {
    // 'login':login
    'navmenu': NavMenu,
  }
}
</script>

<style>
.header {
  background-color: #409EFF;
  color: #fff;
  line-height: 60px;
}
</style>
