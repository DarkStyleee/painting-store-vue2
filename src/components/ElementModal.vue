<template>
  <div class="modal" v-if="showModal">
    <div class="modal-overlay" @click="!isLoading && close()"></div>
    <div class="modal-container">
      <div class="modal-header">
        <h2 class="modal-title">{{ product.title }}</h2>
        <button class="modal-close" @click="!isLoading && close()">
          <icon-close></icon-close>
        </button>
      </div>
      <div class="modal-body">
        <div class="slider modal-slider">
          <ssr-carousel
            show-arrows
            :slides-per-page="1"
            :responsive="[
              {
                minWidth: 768,
                slidesPerPage: 2,
              },
              {
                minWidth: 1024,
                slidesPerPage: 3,
              },
            ]"
          >
            <div
              class="slide"
              v-for="(image, idx) in product.allImages"
              :key="idx"
              :index="idx"
            >
              <img
                class="slider-image"
                :src="require(`@/assets/images/${image}.png`)"
              />
            </div>
          </ssr-carousel>
        </div>
        <div class="modal-description">
          <p>{{ product.description }}</p>
        </div>
      </div>
      <div class="modal-footer">
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
  </div>
</template>

<script>
import ButtonBuy from "@/components/ButtonBuy.vue";
import IconClose from "@/components/icons/IconClose.vue";

import formatNumber from "@/utils/formattedNumber.js";

import SsrCarousel from "vue-ssr-carousel";

// eslint-disable-next-line no-unused-vars
import ssrCarouselCss from "vue-ssr-carousel/index.css";

export default {
  components: {
    ButtonBuy,
    IconClose,
    SsrCarousel,
  },
  props: {
    product: {
      type: Object,
      required: true,
    },
    showModal: {
      type: Boolean,
      default: false,
    },
    cartItems: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedImageIndex: 0,
      isLoading: false,
    };
  },
  methods: {
    close() {
      this.$emit("close");
    },
    addToCart(productId) {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
        this.$emit("add-to-cart", productId);
      }, 2000);
    },
    formattedNumber(num) {
      return formatNumber(num);
    },
  },
};
</script>

<style lang="scss" scoped>
.card-details {
  &__prices {
    margin-right: 10px;
  }
}

.modal-description {
  p {
    margin: 10px 0;
    line-height: 25px;
  }
}

.slider-image {
  width: 100%;
  height: 180px;
}
</style>
