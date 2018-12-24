<template>
  <div id="app" class="d-flex align-items-stretch">
    <div id="app-navbar"><app-navbar /></div>
    <div id="server-canvas"><server-canvas /></div>
  </div>
</template>

<script>
import appNavbar from "./components/AppNavbar.vue";
import serverCanvas from "./components/ServerCancas.vue";

export default {
  name: "homepage",
  components: {
    appNavbar,
    serverCanvas
  }
};
</script>

<style lang="less" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  padding: 0;
}

#app-navbar {
  background: #242424;
  min-width: 250px;
  max-width: 250px;
}
#server-canvas{
  flex: 1;
  background: #000;
  padding: 25px 20px;
}
</style>
