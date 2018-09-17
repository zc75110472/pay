<template>
  <div id="app">
    <left-nav></left-nav>
    <div class='main'>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import leftNav from './components/common/leftNav'
export default {
  name: 'App',
  components: {
    leftNav
  }
}
</script>

<style lang='scss'>
#app {
  .main {
    float: left;
    width: calc(100% - 60px);
    height: 100%;
    background: #eff2f7;
  }
}
</style>
