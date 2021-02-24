<!--
 * @Project: 
 * @Remark: 
 * @Author: 邹新许
 * @Date: 2021-02-24 11:02:25
 * @LastEditors: 邹新许
 * @LastEditTime: 2021-02-24 18:17:34
 * @FilePath: /tauri-demo/src/App.vue
-->
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js 33App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
