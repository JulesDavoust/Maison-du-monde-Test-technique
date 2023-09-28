<template>
    <div id="cartView">
        <div class="container">
            <div class="row">
                <div class="col">
                    test 1
                </div>
                <div class="col">
                    test 2
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Cart',
  methods: {

  },
  async created() {
    console.log('created');
    await fetch('http://localhost:3000/cart')
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<style>
    .container{
        border:2px solid black;
    }
    .col{
        border:2px solid black;
    }
    .row{
        border:2px solid black;
    }
</style>
