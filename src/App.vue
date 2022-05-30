<script>

export default {

  components: {
  }
  }
</script> 

<template>
  <router-view v-if="!$route.meta?.keepAlive" />
  <router-view v-slot="{ Component }">
      <keep-alive>
        <component v-if="$route.meta?.keepAlive" :is="Component" />
      </keep-alive>
  </router-view>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  font-family: "Microsoft JhengHei", "Heiti TC", "sans-serif";
}
img {
  display: block;
}
html,
body {
  width: 100%;
  height: 100%;
  background-color: #FBF8F6;
}
</style>
