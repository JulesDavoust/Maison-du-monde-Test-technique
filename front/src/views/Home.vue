<template>
  <div class="home">
    <div class="container containerProducts">
      <div v-if="products.length > 2">
        <div class="row" v-for="group in groupedProducts" :key="group[0].key">
          <div class="col-4 d-flex justify-content-center align-items-center colProductComp" v-for="item in group" :key="item.key">
            <ProductComp :infoProdObj="item"/>
          </div>
        </div>
      </div>
      <div v-if="products.length < 3">
        <div class="row">
        <div class="col d-flex justify-content-center align-items-center colProductComp" v-for="item in products" :key="item.key">
          <ProductComp :infoProdObj="item"/>
        </div>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProductComp from '../components/ProductComp.vue';

export default {
  name: 'Home',
  components: {
    ProductComp,
  },
  data() {
    return {
      products: [],
    };
  },
  computed: {
    groupedProducts() {
      const groups = [];
      for (let i = 0; i < this.products.length; i += 3) {
        groups.push(this.products.slice(i, i + 3));
      }
      return groups;
    },
  },
  methods: {
  },
  async created() {
    let product;
    await fetch('http://localhost:3000/products')
      .then((response) => response.json())
      .then((data) => {
        product = data;
      })
      .catch((error) => {
        console.log(error);
      });
    Object.entries(product).forEach(([key, value]) => {
      this.products.push({ key, value });
    });
  },
};
</script>

<style scoped>
.icone{
  width: 100%;
  font-size:50%;
  height:20vh;
  border:2px solid black;
}

.containerProducts{
  margin-top:3%;
}
</style>
