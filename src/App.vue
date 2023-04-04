<template>
  <div>
    <HeaderComp></HeaderComp>
    <HomeComp></HomeComp>
    <FooterComp></FooterComp>
  </div>
</template>

<script>
import HomeComp from './components/HomeComp.vue'
import HeaderComp from './components/HeaderComp.vue';
import FooterComp from './components/FooterComp.vue';

export default {
  name: 'App',
  components: {
    HomeComp,
    HeaderComp,
    FooterComp
  }
}
</script>

<style>
body{
  margin: 0px;
  padding: 0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
