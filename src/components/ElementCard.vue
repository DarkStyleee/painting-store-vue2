<template>
  <div class="card" :class="product.saled ? 'card--disable' : ''">
    <img
      @click="showModal"
      class="card__image"
      :src="require(`@/assets/images/${product.image}.png`)"
      :alt="product.title"
    />
    <div class="card__wrapper">
      <h2 @click="showModal" class="card__title">{{ product.title }}</h2>
      <div class="card-details">
        <template v-if="!product.saled">
          <div class="card-details__prices">
            <h6
              class="card-details__old-price"
              v-if="product?.priceOld && product?.priceOld > 0"
            >
              {{ formattedNumber(product.priceOld) }} $
            </h6>
            <h3 class="card-details__price">
              {{ formattedNumber(product.price) }} $
            </h3>
          </div>
          <button-buy
            class="default-btn buy-btn"
            :product-id="product.id"
            :cart-items="cartItems"
            @add-to-cart="addToCart"
            :isLoading="isLoading"
          ></button-buy>
        </template>
        <template v-else>
          <h3>Продана на аукционе</h3>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import ButtonBuy from "@/components/ButtonBuy.vue";

import formatNumber from "@/utils/formattedNumber.js";

export default {
  components: {
    ButtonBuy,
  },
  props: {
    product: {
      type: Object,
      required: true,
    },
    cartItems: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
    addToCart(productId) {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
        this.$emit("add-to-cart", productId);
      }, 2000);
    },
    showModal() {
      this.$emit("show-modal", this.product.id);
    },
    formattedNumber(num) {
      return formatNumber(num);
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  &__image,
  &__title {
    cursor: pointer;
  }

  &__title {
    &:hover {
      text-decoration: underline;
    }
  }
}
</style>
