<template>
    <div id="Navbar" class="d-flex justify-content-center align-items-center">
        <nav class="navbar nb">
            <a class="nav-link active" aria-current="page" href="/"><div class="accueil">Accueil</div></a>
            <a class="nav-link active" aria-current="page" href="/cart"><div class="caddie">Mon panier</div></a>
        </nav>
    </div>
</template>

<script>
export default {
  name: 'navBar',
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

.accueil, .caddie{
    font-size:150%;
}

.caddie {
    margin-left:-15%;
}
.accueil {
    margin-left:15%;
}
</style>
