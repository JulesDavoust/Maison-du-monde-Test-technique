<template>
    <div>
        <div class="card indexCard">
            <img :src="infoMinicartObj['value'].images[0].xsmall" class="card-img-top cardImage" alt="...">
            <div class="card-body cardBody">
                <h6>{{ infoMinicartObj['value'].name }}</h6>
                <b>Référence :</b> {{infoMinicartObj['value'].reference}}
                <div class="d-flex justify-content-between align-items-center">
                    <div class="d-flex justify-content-between align-items-center counterMiniCart">
                        <button class="btn d-flex justify-content-center align-items-center btnCount" @click="decrement">-</button>
                        <div class="h4 count d-flex justify-content-center align-items-center">{{ count }}</div>
                        <button class="btn d-flex justify-content-center align-items-center btnCount" @click="increment">+</button>
                    </div>
                </div>
                <div class="d-flex justify-content-between align-items-center">
                    <div>
                        <button type="button" class="btn btn-light me-2 buttonObj">Prix : {{ price }}€</button>
                        <button type="button" class="btn btn-danger me-2 buttonObj" @click="delItemCart">Supprimer</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'MiniCartComp',
  props: {
    infoMinicartObj: Object,
  },
  data() {
    return {
      count: this.infoMinicartObj.value.qty,
      price: 0,
      verifSuppr: false,
      showModal: false,
    };
  },
  methods: {
    async delItemCart() {
      console.log('delete');
      await fetch(`http://localhost:3000/cart/${this.infoMinicartObj.value.id}`, {
        method: 'DELETE',
      })
        .then((response) => {
          if (response.ok) {
            console.log('Elément supprimé avec succès !');
          } else console.log("Erreur lors de la suppression de l'élément.");
        })
        .catch((error) => {
          console.error('Erreur lors de la mise à jour de la quantité:', error);
        });
      // eslint-disable-next-line no-restricted-globals
      location.reload();
    },
    increment() {
      this.count += 1;
      const newData = { qty: this.count };
      this.price = this.count * this.infoMinicartObj.value.price.base.amount;
      console.log(this.price);
      fetch(`http://localhost:3000/cart/${this.infoMinicartObj.value.id}`, {
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
    },
    decrement() {
      if (this.count > 0) {
        this.count -= 1;
        if (this.count === 0) this.delItemCart();
        else {
          const newData = { qty: this.count };
          this.price = this.count * this.infoMinicartObj.value.price.base.amount;
          fetch(`http://localhost:3000/cart/${this.infoMinicartObj.value.id}`, {
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
        }
      } else {
        this.delItemCart();
      }
    },
  },
  created() {
    this.price = this.count * this.infoMinicartObj.value.price.base.amount;
  },
};
</script>

<style>
.indexCard{
    z-index: 9999;
    width: 18rem;
}

.counterMiniCart{
    background-color: white;
    border:1px solid black;
    border-radius: 7px;
    width:140px;
    height:4vh;
    margin-bottom: 2%;
}

@media(max-width: 768px){
    .indexCard{
        z-index: 9999;
        width: 15rem;
        margin-bottom: 5%;
    }
}
</style>
