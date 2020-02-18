<template>
  <div class="container">
    <product v-for="product in products" v-bind:product="product"></product>
  </div>
</template>

<script>
import Product from './components/product/Product.vue'
import products from './assets/products.json'
import p from './app.less'

products.forEach(p => {
  p.quantity = 1;
  p.hasAlternateUnit = p.unit === p.unitAlt;
});
/*
Поиск уникальных значений единиц
let data = [];
products.forEach(p => {
  data.push(p.unit)
  data.push(p.unitAlt)
});
var uniqUnits = [...new Set([...data])];
console.log(uniqUnits)

//["упак.", "м. кв.", "шт.", "м/п", "м.кв.", "м.куб.", "компл"]
*/
export default {
  name: 'App',
  data:  function() {
     return {
       products: products
     }
  },
  components: {
    Product
  }
}
</script>