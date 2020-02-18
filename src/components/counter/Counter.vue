<template>
  <div class="arrows m-r-10">
    <div class="arrow arrow-top" v-on:click="plusQuantity(product)">
      <i class="fa fa-angle-up"></i>
    </div>
    <div class="arrow arrow-bottom" v-on:click="minusQuantity(product)">
      <i class="fa fa-angle-down"></i>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Counter',
    props: ["product"],
    methods: {
      plusQuantity: function(product){
        product.quantity += 1;
      },
      minusQuantity: function(product){
        product.quantity -= 1;
        if (product.quantity < 0){
          product.quantity = 0;
        }
      },
    },
  }
  import p from './counter.less'
</script>
