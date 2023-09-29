<template>
    <div class="card cardSizeProduct" style="width: 25rem;">
        <div @mouseenter="nextPic=true" @mouseleave="nextPic=false">
            <div v-if="nextPic==false"><img :src="infoProdObj['value'].images[0].medium" class="card-img-top" alt="..."></div>
            <div v-if="nextPic==true"><img :src="infoProdObj['value'].images[1].medium" class="card-img-top" alt="..."></div>
        </div>
        <div class="card-body">
            <h6>{{ infoProdObj['value'].name }}</h6>
            <b>Référence :</b> {{infoProdObj['value'].reference}}
            <div class="d-flex justify-content-between align-items-center">
                <div>
                    <button type="button" class="btn btn-light me-2 buttonObj">Prix : {{ price }}€</button>
                    <button type="button" class="btn btn-primary me-2 buttonObj" @click="AddItemCart">Ajouter</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'ProductComp',
  props: {
    infoProdObj: Object,
  },
  data() {
    return {
      count: this.infoProdObj.value.qty,
      price: 0,
      nextPic: false,
    };
  },
  methods: {
    async AddItemCart() {
      console.log(this.infoProdObj.value.id);
      const response1 = await fetch('http://localhost:3000/cart');
      console.log('test2');
      if (!response1.ok) throw new Error('Failed to fetch cart');
      const cart = await response1.json();

      const existingProduct = cart.find((item) => item.id === this.infoProdObj.value.id);
      console.log('test1');
      if (existingProduct) {
        existingProduct.qty += 1;
        const newData = { qty: existingProduct.qty };
        console.log(newData);
        console.log();
        fetch(`http://localhost:3000/cart/${this.infoProdObj.value.id}`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(newData),
        })
          .then((response) => response.json())
          .then((data) => {
            console.log(data);
          })
          .catch((error) => {
            console.error('Erreur lors de la mise à jour de la quantité:', error);
          });
        window.location.reload();
      } else {
        console.log(this.infoProdObj.value.id);
        const productResponse = await fetch(`http://localhost:3000/products/${this.infoProdObj.value.id}`);
        if (!productResponse.ok) throw new Error('Failed to fetch product');
        const product = await productResponse.json();
        product.qty = 1;
        console.log(product);
        await fetch('http://localhost:3000/cart', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(product),
        });
        window.location.reload();
      }
    },
  },
  created() {
    this.price = this.infoProdObj.value.price.base.amount;
  },
};
</script>

<style>
.indexCard{
    z-index: 9999;
}

.counterMiniCart{
    background-color: white;
    border:1px solid black;
    border-radius: 7px;
    width:140px;
    height:4vh;
    margin-bottom: 2%;
}

.cardSizeProduct{
    width: 70%;
}
</style>
