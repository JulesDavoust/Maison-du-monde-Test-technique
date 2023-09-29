<template>
  <div id="myApp">
    <navBar :infosMiniCart="infos"/>
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
  data() {
    return {
      infos: [],
    };
  },
  async created() {
    let info;
    console.log('created');
    await fetch('http://localhost:3000/cart')
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        info = data;
      })
      .catch((error) => {
        console.log(error);
      });
    Object.entries(info).forEach(([key, value]) => {
      this.infos.push({ key, value });
    });
    console.log(this.infos);
  },
};
</script>

<style>
</style>
