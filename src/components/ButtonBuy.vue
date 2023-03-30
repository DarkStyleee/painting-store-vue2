<template>
  <button
    class="buy-btn"
    @click="addToCart"
    :disabled="findedItem || isLoading"
    :class="{
      'buy-btn--inCart': findedItem,
      'buy-btn--isLoading': isLoading,
    }"
  >
    <template v-if="isLoading">
      <icon-loader class="icon icon-loader"></icon-loader>
      <span>Обработка...</span>
    </template>
    <template v-else-if="!isLoading && findedItem">
      <icon-check class="icon"></icon-check>
      <span>В корзине</span>
    </template>
    <template v-else>
      <span>Купить</span>
    </template>
  </button>
</template>

<script>
import IconLoader from "@/components/icons/IconLoader.vue";
import IconCheck from "@/components/icons/IconCheck.vue";

export default {
  components: {
    IconLoader,
    IconCheck,
  },
  props: {
    isLoading: {
      type: Boolean,
      default: false,
    },
    productId: {
      type: Number,
      required: true,
    },
    cartItems: {
      type: Array,
      required: true,
    },
  },
  computed: {
    findedItem() {
      return this.cartItems.find((item) => item.id === this.productId);
    },
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.productId);
    },
  },
};
</script>
