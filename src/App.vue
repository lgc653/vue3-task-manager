<template>
  <div id="app">
    <h1>任务管理应用</h1>
    <router-link to="/">
      任务列表
    </router-link>
    <router-link to="/add">
      添加任务
    </router-link>
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
/* 添加样式 */
</style>
