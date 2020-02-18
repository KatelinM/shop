<template class="product">
  <div>
    <div class="tooltip row">
      <i class="fa fa-exclamation-circle"></i>
      <div>
        <p>Продается {{ unitPlural }}:</p>
        <p>1 {{ unit }} = {{ unitRatio + unitAlt }}</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'tooltip',
    props: ["unit", "unitRatio", "unitAlt", "unitPlural"],
  }
  import p from './tooltip.less'
</script>
