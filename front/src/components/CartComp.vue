<template>
  <div id="CartComp">
    <div class="card cardCart">
        <div class="card-body cardBodyCart">
            <h5 class="card-title cardGame">{{ infoObj["value"].name }}</h5>
            <div class="d-flex justify-content-between align-items-center">
                <div class="d-flex justify-content-between align-items-center counter">
                    <button class="btn d-flex justify-content-center align-items-center btnCount" @click="decrement">-</button>
                    <div class="h4 count d-flex justify-content-center align-items-center">{{ count }}</div>
                    <button class="btn d-flex justify-content-center align-items-center btnCount" @click="increment">+</button>
                </div>
            </div>
            <div class="d-flex justify-content-between align-items-center">
                <div>
                    <button type="button" class="btn btn-light me-2 buttonObj">Prix : {{ price }}€</button>
                    <button type="button" class="btn btn-danger me-2 buttonObj" @click="delItemCart">Supprimer</button>
                    <button class="btn btn-info dropdown-toggle buttonObj" @click="clickInfo" type="button" data-bs-toggle="dropdown" aria-expanded="false">
                        Images
                    </button>
                </div>
            </div>
        </div>
        <div v-if="buttonInfoVar == true">
            <div class="container">
                <div class="row">
                    <div class="col d-flex justify-content-center align-items-center">
                        <div id="carouselXsmall" class="carousel carousel-dark slide">
                            <div class="carousel-inner">
                                <div v-if="showImageXsmall == false">
                                    <img :src="infoObj['value'].images[0].large" class="d-block w-100" alt="xsmall">
                                </div>
                                <div  v-if="showImageXsmall == true">
                                    <img :src="infoObj['value'].images[1].large" class="d-block w-100" alt="xsmall">
                                </div>
                            </div>
                            <button class="carousel-control-prev" type="button" data-bs-target="#carouselXsmall" data-bs-slide="prev" @click="nextXsmall">
                                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                            </button>
                            <button class="carousel-control-next" type="button" data-bs-target="#carouselXsmall" data-bs-slide="next" @click="nextXsmall">
                                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            <b>Référence :</b> {{infoObj['value'].reference}}
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyCart',
  props: {
    infoObj: Object,
  },
  data() {
    return {
      count: this.infoObj.value.qty,
      buttonInfoVar: false,
      showImageXsmall: false,
      price: 0,
    };
  },
  methods: {
    async delItemCart() {
      console.log('delete');
      await fetch(`http://localhost:3000/cart/${this.infoObj.value.id}`, {
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
    nextXsmall() {
      if (this.showImageXsmall === false) {
        this.showImageXsmall = true;
      } else {
        this.showImageXsmall = false;
      }
    },
    clickInfo() {
      if (this.buttonInfoVar === false) {
        this.buttonInfoVar = true;
      } else {
        this.buttonInfoVar = false;
      }
    },
    async increment() {
      let product;
      await fetch(`http://localhost:3000/products/${this.infoObj.value.id}`)
        .then((response) => response.json())
        .then((data) => {
          product = data;
        })
        .catch((error) => {
          console.log(error);
        });
      console.log(product.qty);
      if (this.count < product.qty) {
        this.count += 1;
        const newData = { qty: this.count };
        this.price = this.count * this.infoObj.value.price.base.amount;
        console.log(this.price);
        fetch(`http://localhost:3000/cart/${this.infoObj.value.id}`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(newData),
        })
          .then((response) => response.json())
          .then((data) => {
            console.log(data); // Loggez les données mises à jour
          })
          .catch((error) => {
            console.error('Erreur lors de la mise à jour de la quantité:', error);
          });
        window.location.reload();
      }
    },
    decrement() {
      if (this.count > 0) {
        this.count -= 1;
        if (this.count === 0) this.delItemCart();
        else {
          const newData = { qty: this.count };
          this.price = this.count * this.infoObj.value.price.base.amount;
          fetch(`http://localhost:3000/cart/${this.infoObj.value.id}`, {
            method: 'PATCH',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(newData),
          })
            .then((response) => response.json())
            .then((data) => {
              console.log(data); // Loggez les données mises à jour
            })
            .catch((error) => {
              console.error('Erreur lors de la mise à jour de la quantité:', error);
            });
          window.location.reload();
        }
      } else {
        this.delItemCart();
      }
    },
  },
  created() {
    this.price = this.count * this.infoObj.value.price.base.amount;
  },
};
</script>

<style>

.CartComp, .cardCart, .cardBodyCart{
  z-index: 0;
}

#carouselXsmall{
  z-index: 0;
  width:70%;
}

.card-body{
  z-index: 0;
  background-color:bisque;
}

.count{
  padding-top:7%;
  width: 20px;
  text-align: center;
}

.counter{
  background-color: white;
  border:1px solid black;
  border-radius: 7px;
  width:100px;
  height:6vh;
  margin-bottom: 2%;
}

.btnCount{
  background-color: bisque;
  height: 3vh;;
}

.buttonObj{
  margin-bottom: 2%;
}
</style>
