<template>
    <div id="Navbar" class="d-flex justify-content-center align-items-center">
        <nav class="navbar nb">
            <a class="nav-link active" aria-current="page" href="/"><div class="accueil d-flex justify-content-center align-items-center">Accueil</div></a>
            <a class="nav-link active" @mouseenter="mouseIn = true" @mouseleave="mouseIn = false" aria-current="page" href="/cart">
                <div class="d-flex justify-content-center align-items-center divCaddie">
                    <img class="imgCaddie" src="https://www.icone-png.com/png/14/13561.png">
                </div>
                <div v-if="mouseIn == true">
                    <MiniCart/>
                </div>
            </a>
        </nav>
    </div>
</template>

<script>
import MiniCartComp from './MiniCartComp.vue';

export default {
  name: 'navBar',
  data() {
    return {
      mouseIn: false,
    };
  },
  components: {
    MiniCart: MiniCartComp,
  },
};
</script>

<style>
#Navbar{
    background-color: burlywood;
    height: 10vh;
}

.nb {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
}
/* Bouton Accueil */
.accueil{
    font-size:150%;
    margin-left:15%;
    width:105%;
    height:7vh;
    background-color:bisque;
    border:2px solid black;
    border-radius: 10px;
}

.accueil:hover{
    box-shadow: 0px 0px 10px black;
    transform: scale(1.1);
    transition: 0.2s ease-in-out;
}

/* Bouton Caddie */
.imgCaddie{
    width:65%;
}

.divCaddie{
    background-color:bisque;
    border:2px solid black;
    border-radius: 10px;
    width:30%;
    height:7vh;
    margin-left:60%;
}

.divCaddie:hover{
    box-shadow: 0px 0px 10px black;
    transform: scale(1.1);
    transition: 0.2s ease-in-out;
}

@media(max-width: 335px) {
    .imgCaddie{
        width:65%;
    }

    .divCaddie{
        background-color:bisque;
        border:2px solid black;
        border-radius: 10px;
        width:30%;
        height:7vh;
        margin-left:85%;
        margin-top:-15%;
    }

    .accueil{
        font-size:150%;
        margin-left:15%;
        margin-top:5%;
    }
}
</style>
