<template>
  <div id="app">
    <div class="top">
      <router-link to="/">累计确诊</router-link>|
      <router-link to="/about">疫情动态</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.top > a {
  text-decoration: none;
  color: black;
  font-size: 20px;
}
.top > a:hover,
.top > a.active {
  color: blue;
}
</style>
