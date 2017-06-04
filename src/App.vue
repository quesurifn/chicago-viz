<template>
  
  <div id="app">
    <div style="background: #262626;position:absolute;top:0;width:100%;color:white;text-align:center;font-weight:600;height:100px;padding:10px 0 0 0;font-size:26px;">Chicago Crime - Last Week</div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
