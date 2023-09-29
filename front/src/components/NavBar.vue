<template>
    <div>
        <div id="Navbar" class="d-flex justify-content-center align-items-center">
            <nav class="navbar nb">
                <a class="nav-link active" aria-current="page" href="/"><div class="accueil d-flex justify-content-center align-items-center">Accueil</div></a>
                <div class="caddieContainer" >
                    <div class="d-flex justify-content-center align-items-center nbrItem" style="flex-direction: column;" @mouseenter="showDiv2" @mouseleave="hideDiv2">
                        {{infosMiniCart.length}}
                        <span>produits</span>
                        <div class="divMiniCart" v-show="isDiv2Visible">
                            <div class="row rowMiniCart" v-for="item in infosMiniCart" :key="item.key">
                                <MiniCart class="indexRowMiniCart" :infoMinicartObj="item"/>
                            </div>
                        </div>
                    </div>
                    <div class="d-flex justify-content-center align-items-center divCaddie">
                        <a class="nav-link active" aria-current="page" href="/cart">
                            <img class="imgCaddie" src="https://www.icone-png.com/png/14/13561.png">
                        </a>
                    </div>
                </div>
            </nav>
        </div>
    </div>
</template>

<script>
import MiniCartComp from './MiniCartComp.vue';

export default {
  name: 'navBar',
  props: {
    infosMiniCart: Array,
  },
  data() {
    return {
      isDiv2Visible: false,
    };
  },
  components: {
    MiniCart: MiniCartComp,
  },
  methods: {
    showDiv2() {
      this.isDiv2Visible = true;
    },
    hideDiv2() {
      this.isDiv2Visible = false;
    },
  },
};
</script>

<style>
.caddieContainer {
    display: flex;
    align-items: center;
    position: relative;
    justify-content: center;
    width:200px;
    margin-right: 3%;
    height: 9vh;
}

.nbrItem {
    border:2px solid black;
    height: 9vh;
    width: 90px;
    border-radius: 10px;
    background-color: bisque;
    margin-right: 10px;
}

@media(min-width: 768px) {
    .nbrItem:hover{
        box-shadow: 0px 0px 10px black;
        transform: scale(1.05);
        transition: 0.3s ease-in-out;
    }
}

.divCaddie {
    background-color: bisque;
    border:2px solid black;
    border-radius: 10px;
    width: 90px;
    height: 9vh;
    padding-left: 5%;
}

.divMiniCart {
    border: 2px solid black;
    border-radius: 7px;
    position: absolute;
    top: 100%;
    right: 0%;
    color: #fff;
    z-index: 1;
    box-shadow: 0px 6px 10px black;
}

#Navbar{
    background-color: burlywood;
    height: 12vh;
}

.nb {
    z-index: 9999;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
}
/* Bouton Accueil */
.accueil{
    font-size:150%;
    margin-left:15%;
    width:150%;
    height:9vh;
    background-color:bisque;
    border:2px solid black;
    border-radius: 10px;
}

.accueil:hover{
    box-shadow: 0px 0px 10px black;
    transform: scale(1.05);
    transition: 0.3s ease-in-out;
}

/* Bouton Caddie */
.imgCaddie{
    width:80%;
}

.divCaddie:hover{
    box-shadow: 0px 0px 10px black;
    transform: scale(1.05);
    transition: 0.3s ease-in-out;
}

@media(max-width: 425px) {
    .imgCaddie{
        z-index: 9999;
        width:80%;
    }

    .caddieContainer {
        display: flex;
        align-items: center;
        position: relative;
        justify-content: center;
        width:150px;
        margin-right: 3%;
        height: 9vh;
    }

    .nbrItem {
        border:2px solid black;
        height: 9vh;
        width: 90px;
        border-radius: 10px;
        background-color: bisque;
        margin-right: 10px;
    }

    .divCaddie {
        background-color: bisque;
        border:2px solid black;
        border-radius: 10px;
        width: 90px;
        height: 9vh;
        padding-left: 5%;
    }

    .divMiniCart {
        border: 2px solid black;
        position: absolute;
        top: 100%;
        right: 0%;
        color: #fff;
        z-index: 1;
        box-shadow: 0px 6px 10px black;
    }

    .accueil{
        z-index: 9999;
        font-size:100%;
        margin-left:15%;
    }
}
</style>
