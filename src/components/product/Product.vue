<template>
  <div class="product">
    <div class="product__photo">
      <a href="#">
        <img :src="'https://' + product.primaryImageUrl"/>
      </a>
    </div>
    <div class="product__info">
      <div class="row">
        <p class="product_code">Код: {{ product.code }}</p>
        <p v-bind:class="[product.hasAlternateUnit ? 'text-green' : 'text-red', 'availability']">
          {{ product.hasAlternateUnit ? 'Наличие' : 'Нет в наличии' }}
        </p>
      </div>
      <a href="#" class="product_title">{{ product.title }}</a>
      <p class="description m-b-10">{{ product.description }}</p>
      <div v-if="product.assocProducts" class="product_tags">
        <p>Могут понадобиться:</p>
        <span v-for="p in product.assocProducts.split(';')">
          <a href="#" >{{ p.replace('\n', ', ') }}</a>
        </span>
      </div>
    </div>
    <div class="product__buy">
      <div class="row product_price_club_card">
        <div class="product_price_club_card_text">По карте <br> клуба</div>
        <div class="price-gold">
          {{ altMode ? product.priceGoldAlt : product.priceGold }}
          <span class="rouble__i black__i">
             <svg version="1.0" xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 50 50">
                <path fill="#000" d="M28.109 29.658a19.104 19.104 0 0014.355-4.312A15.973 15.973 0 0044.963 8.1a11.95 11.95 0 00-3.92-4.899A14.706 14.706 0 0035.017.898a44.821 44.821 0 00-7.447-.49H9.05V23.68H3.562v5.781H9.05v5.977H3.562v5.781H9.05v8.378h6.467v-8.133h20.088v-5.781H15.517v-6.026H28.109zM15.566 6.386h12.543a27.6 27.6 0 016.026.441 7.255 7.255 0 014.116 2.891 8.87 8.87 0 011.568 5.242 8.33 8.33 0 01-2.646 6.565 13.429 13.429 0 01-8.917 2.352H15.517V6.386h.049z"/>
              </svg>
          </span>
        </div>
      </div>

      <p class="price-default">
        {{ altMode ? product.priceRetailAlt  : product.priceRetail}}
        <span class="rouble__i black__i">
             <svg version="1.0" xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 50 50">
                <path fill="#000" d="M28.109 29.658a19.104 19.104 0 0014.355-4.312A15.973 15.973 0 0044.963 8.1a11.95 11.95 0 00-3.92-4.899A14.706 14.706 0 0035.017.898a44.821 44.821 0 00-7.447-.49H9.05V23.68H3.562v5.781H9.05v5.977H3.562v5.781H9.05v8.378h6.467v-8.133h20.088v-5.781H15.517v-6.026H28.109zM15.566 6.386h12.543a27.6 27.6 0 016.026.441 7.255 7.255 0 014.116 2.891 8.87 8.87 0 011.568 5.242 8.33 8.33 0 01-2.646 6.565 13.429 13.429 0 01-8.917 2.352H15.517V6.386h.049z"/>
              </svg>
          </span>
      </p>
      <div class="product_price_points" v-if="product.bonusAmount">
        <p>Можно купить за {{ product.bonusAmount }} балла</p>
      </div>
      <div class="product_units row">
        <div v-bind:class="{ 'unit--active': altMode }"
             v-on:click="setAltMode(true)"
             v-if="!product.hasAlternateUnit"
             class="unit--select">
          <p>За {{ setSingle(product.unitAlt) }}</p>
        </div>
        <div v-bind:class="{ 'unit--active': !altMode }"
             v-on:click="setAltMode(false)"
             class="unit--select">
          <p>За {{ setSingle(product.unit) }}</p>
        </div>
      </div>
      <div class="row cart tooltip-box">
        <div class="quantity">
          {{ product.quantity }}
          <tooltip v-if="hasRatio(product.unit, product.unitAlt) && product.unit !== product.unitAlt"
                   v-bind:unit="product.unit"
                   v-bind:unitAlt="product.unitAlt"
                   v-bind:unitPlural="setPlural(product.unit, product.unitAlt)"
                   v-bind:unitRatio="product.unitRatio">
          </tooltip>
        </div>
        <counter v-bind:product="product"></counter>
        <button class="btn btn_cart" v-bind:data-product-id="product.productId">
          <i class="fa fa-shopping-cart"></i>В Корзину
        </button>
      </div>
    </div>
  </div>
</template>

<script>
  import p from './product.less'
  import Tooltip from '../tooltip/Tooltip.vue'
  import Counter from '../counter/Counter.vue'

  export default {
    name: 'product',
    props: ["product", "msg"],
    components: {
      Tooltip,
      Counter
    },
    data () {
      return {
        altMode: this.altMode
      }
    },
    methods: {
      setAltMode: function(mode){
        this.altMode = mode
      },
      hasRatio: function (unit, unitAlt) {
        const unitList = ["упак.", "шт.", "компл"];
        return unitList.some((u) => u === unit) || unitList.some((u) => u === unitAlt);
      },
      setSingle: function (unit) {
        switch (unit) {
          case "упак.":
            return "упаковку";
          case "шт.":
            return "штуку";
          case "компл":
            return "комплект";
          default:
            return unit;
        }
      },
      setPlural: function (unit, unitAlt) {
        if (unit === "упак." || unitAlt === "упак.") {
          return "упаковками";
        } else if (unit === "шт." || unitAlt === "шт.") {
          return "поштучно";
        } else if (unit === "компл" || unitAlt === "компл") {
          return "комплектами";
        }
      },
    },
  }
</script>