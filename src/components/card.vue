<template>
  <div class="product product_horizontal">
    <span class="product_code">Код:  {{ productData.code }}</span>
    <div class="product_status_tooltip_container">
      <span class="product_status">Наличие</span>
    </div>
    <div class="product_photo">
      <a href="#" class="url--link product__link">
        <img :src="imageNewUrl">
      </a>
    </div>
    <div class="product_description">
      <a href="#" class="product__link">{{ productData.title }}</a>
    </div>
    <div class="product_tags hidden-sm">
      <p>Могут понадобиться:&nbsp;</p>
      <a
        href="#"
        class="url--link"
        v-for="(product, index) in assocProducts"
        :key="index"
      >
        <span class="url--link_text" v-if="product.length">{{ product }}</span>
      </a>
    </div>
    <div class="product_units">
      <div class="unit--wrapper">
        <div class="unit--select"
          v-for="[label, className] in buttons"
          :key="className"
          v-bind:class="{ active: className === activeClass }"
          v-on:click="setActiveClass(className)">
          <p class="ng-binding">{{ label }}</p>
        </div>
      </div>
    </div>
    <p class="product_price_club_card">
      <span class="product_price_club_card_text">По карте<br>клуба</span>
      <span class="goldPrice">{{ roundGold }}</span>
      <span class="rouble__i black__i">
        <svg version="1.0" id="rouble__b" xmlns="http://www.w3.org/2000/svg" x="0" y="0" width="30px" height="22px" viewBox="0 0 50 50" enable-background="new 0 0 50 50" xml:space="preserve">
          <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_black"></use>
        </svg>
      </span>
    </p>
    <p class="product_price_default">
      <span class="retailPrice">{{ roundRetail }}</span>
      <span class="rouble__i black__i">
        <svg version="1.0" id="rouble__g" xmlns="http://www.w3.org/2000/svg" x="0" y="0" width="30px" height="22px" viewBox="0 0 50 50" enable-background="new 0 0 50 50" xml:space="preserve">
          <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_gray"></use>
        </svg>
      </span>
    </p>
    <div class="product_price_points">
      <p class="ng-binding">Можно купить за 231,75 балла</p>
    </div>
    <div class="list--unit-padd"></div>
    <div class="list--unit-desc">
      <div class="unit--info">
      <div class="unit--desc-i"></div>
        <div class="unit--desc-t">
          <p>
            <span class="ng-binding">Продается упаковками:</span>
            <span class="unit--infoInn">1 упак. = 2.47 м. кв. </span>
          </p>
        </div>
      </div>
    </div>
    <div class="product__wrapper">
      <div class="product_count_wrapper">
        <div class="stepper">
          <input class="product__count stepper-input" type="text" :value="counter">
          <span class="stepper-arrow up" @click="counterPlus"></span>
          <span class="stepper-arrow down" @click="counterMinus"></span>
        </div>
      </div>
      <span class="btn btn_cart" data-url="/cart/" :data-product-id="productData.productId">
        <svg class="ic ic_cart">
          <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#cart"></use>
        </svg>
        <span class="ng-binding">В корзину</span>
      </span>
    </div>
  </div>
</template>

<script>
function splitByIndex (value, size, index) {
  return value.substring(0, index) + size + value.substring(index)
}
export default {

  data () {
    const type = this.productData.unitFull
    if (type === 'упаковка') {
      this.productData.unitFull = 'упаковку'
    } else if (type === 'штука') {
      this.productData.unitFull = 'штуку'
    }
    return {
      activeClass: 'container',
      buttons: [
        [`за ${this.productData.unitAlt}`, 'container'],
        [`за ${this.productData.unitFull}`, 'container-fluid']
      ],
      counter: 1
    }
  },
  props: {
    productData: Object
  },
  computed: {
    imageNewUrl () {
      const baseImgUrl = this.productData.primaryImageUrl
      const size = '_220x220_1'
      const idx = baseImgUrl.lastIndexOf('.')
      const newUrl = splitByIndex(baseImgUrl, size, idx)
      return newUrl
    },
    assocProducts () {
      const products = this.productData.assocProducts
      const productsArr = products.split(';')
      if (productsArr[productsArr.length - 1].length === 0) {
        productsArr.pop()
      }
      return productsArr
    },
    roundGold () {
      if (this.activeClass === 'container') {
        const round = (this.productData.priceGoldAlt * this.counter).toFixed(2)
        return round
      } else {
        const round = (this.productData.priceGold * this.counter).toFixed(2)
        return round
      }
    },
    roundRetail () {
      if (this.activeClass === 'container') {
        const round = (this.productData.priceRetailAlt * this.counter).toFixed(2)
        return round
      } else {
        const round = (this.productData.priceRetail * this.counter).toFixed(2)
        return round
      }
    }
  },
  methods: {
    setActiveClass: function (className) {
      this.activeClass = className
    },
    counterPlus () {
      this.counter++
    },
    counterMinus () {
      if (this.counter > 1) {
        this.counter--
      }
    }
  }
}
</script>
<style lang="scss" scoped>
  .url--link:not(:last-child) {
    .url--link_text:after {
      content: ",";
    }
  }
  .url--link:last-child {
    .url--link_text:after {
      content: ".";
    }
  }
  .product_tags p {
    display: inline;
  }
  .active {
    background: #000;
    p {
      color: #fff !important;
    }
  }
</style>
