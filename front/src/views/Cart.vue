<template>
    <div id="cartView">
        <div class="container cartContainer">
            <div class="row cartCompClass" v-for="item in infos" :key="item.key">
                <MyCart class="indexMyCart" v-bind:infoObj="item"/>
            </div>
            <div class="buttonTotBuy">
                <button type="button" class="btn btn-danger me-2 buttonObj">Total : {{ total }} €</button>
                <button type="button" class="btn btn-danger me-2 buttonObj">Acheter</button>
            </div>
        </div>
    </div>
</template>

<script>

import MyCart from '../components/CartComp.vue';

export default {
  name: 'Cart',

  components: {
    MyCart,
  },

  data() {
    return {
      infos: [],
      total: 0,
    };
  },
  methods: {

  },
  async created() {
    let info;
    await fetch('http://localhost:3000/cart')
      .then((response) => response.json())
      .then((data) => {
        info = data;
      })
      .catch((error) => {
        console.log(error);
      });
    Object.entries(info).forEach(([key, value]) => {
      this.infos.push({ key, value });
    });
    for (let i = 0; i < this.infos.length; i += 1) {
      this.total = this.infos[i].value.qty * this.infos[i].value.price.base.amount + this.total;
    }
  },
};
</script>

<style>
.cartView, .cartContainer, .indexMyCart{
  z-index: 0;
}

.cartCompClass{
  z-index: 0;
  padding-top:3%;
}

.buttonTotBuy{
    margin-top:2%;
}
</style>
