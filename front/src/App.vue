<template>
  <div id="myApp">
    <navBar/>
    <router-view/>
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue';

export default {
  name: 'MyApp',
  components: {
    navBar: NavBar,
  },
};
</script>

<style>
</style>
