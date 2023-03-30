<template>
  <div id="app">
    <element-modal
      :showModal="showModal"
      @close="showModal = false"
      @add-to-cart="addToCart"
      :product="selectedItem"
      :cartItems="cartItems"
    ></element-modal>
    <block-header>
      <template #menu>
        <list-menu :routes="menuRoutes"></list-menu>
      </template>
      <template #search>
        <input-search @search="search"></input-search>
      </template>
    </block-header>
    <block-content>
      <h1 class="content-page-title">Картины эпохи Возрождения</h1>
      <div class="content-page-cards">
        <element-card
          v-for="product in searchedAuctionItems"
          :key="product.id"
          :product="product"
          :cartItems="cartItems"
          @add-to-cart="addToCart"
          @show-modal="handleShowModal"
        ></element-card>
      </div>
    </block-content>
    <block-footer>
      <template #menu>
        <list-menu :routes="menuRoutes"></list-menu>
      </template>
      <template #phone>
        <p class="f-text phone">
          <span class="icon">
            <icon-phone></icon-phone>
          </span>
          {{ sitePhone }}
        </p>
      </template>
      <template #address>
        <p class="f-text address">
          <span class="icon">
            <icon-address></icon-address>
          </span>
          {{ siteAddress }}
        </p>
      </template>
    </block-footer>
  </div>
</template>

<script>
import BlockHeader from "@/components/BlockHeader.vue";
import BlockContent from "@/components/BlockContent.vue";
import BlockFooter from "@/components/BlockFooter.vue";

import ListMenu from "@/components/ListMenu.vue";

import IconPhone from "@/components/icons/IconPhone.vue";
import IconAddress from "@/components/icons/IconAddress.vue";
import InputSearch from "@/components/InputSearch.vue";

import ElementCard from "@/components/ElementCard.vue";
import ElementModal from "@/components/ElementModal.vue";

export default {
  name: "App",
  components: {
    BlockHeader,
    BlockContent,
    BlockFooter,
    ListMenu,
    IconPhone,
    IconAddress,
    InputSearch,
    ElementCard,
    ElementModal,
  },
  data() {
    return {
      menuRoutes: [
        { name: "Каталог", link: "#catalog" },
        { name: "Доставка", link: "#delivery" },
        { name: "Оплата", link: "#payment" },
        { name: "Контакты", link: "#contacts" },
        { name: "О компании", link: "#about" },
      ],
      auctionItems: [
        {
          id: 1,
          title: "«Рождение Венеры» Сандро Боттичелли",
          priceOld: 2000000,
          price: 1000000,
          saled: false,
          image: "paint-1",
          allImages: ["paint-2", "paint-3", "paint-1", "paint-4"],
          description:
            "«Рождение Венеры» (итал. Nascita di Venere) — картина итальянского художника тосканской школы Сандро Боттичелли. Представляет собой живопись темперой на холсте размером 172,5 x 278,5 см. В настоящее время хранится в галерее Уффици, Флоренция.",
        },
        {
          id: 2,
          title: "«Тайная вечеря»  Леонардо да Винчи",
          priceOld: 0,
          price: 3000000,
          saled: false,
          image: "paint-2",
          allImages: ["paint-3", "paint-2", "paint-1", "paint-4"],
          description:
            "Фреска «Тайная вечеря» Леонардо да Винчи — одна из самых знаменитых и загадочных картин в мире. Роспись гениального художника уже более 500 лет вызывает восхищение, очаровывает и удивляет. Великолепное произведение искусства можно увидеть в трапезной главной церкви монастыря доминиканцев Санта-Мария-делле-Грацие. Монастырь находится в западной части Милана.",
        },
        {
          id: 3,
          title: "«Сотворение Адама» Микеланджело",
          priceOld: 6000000,
          price: 5000000,
          saled: false,
          image: "paint-3",
          allImages: ["paint-2", "paint-3", "paint-1", "paint-4"],
          description:
            "«Сотворение Адама» — фреска итальянского художника Микеланджело, которая является частью потолка Сикстинской капеллы, написанная ок. 1508–1512 гг. Он иллюстрирует библейское повествование о сотворении из Книги Бытия, в котором Бог дает жизнь Адаму, первому человеку.",
        },
        {
          id: 4,
          title: "«Урок анатомии»  Рембрандт",
          priceOld: 2000000,
          price: 1000000,
          saled: true,
          image: "paint-4",
          allImages: ["paint-2", "paint-3", "paint-1", "paint-4"],
          description:
            "Урок анатомии доктора Николаса Тулпа - это картина Рембрандта, написанная маслом на холсте 1632 года и хранящаяся в музее Маурицхёйс в Гааге, Нидерланды. Первоначально он был создан для демонстрации Гильдией хирургов в их конференц-зале. Картина считается одним из ранних шедевров Рембрандта.",
        },
      ],
      cartItems: [],
      sitePhone: "+7 (812) 555-55-55",
      siteAddress: "г. Санкт-Петербург, ул. Ефимова, 3",
      searchFilter: "",
      showModal: false,
      selectedItem: {},
    };
  },
  computed: {
    searchedAuctionItems() {
      return this.auctionItems.filter((item) =>
        item.title.toLowerCase().includes(this.searchFilter.toLocaleLowerCase())
      );
    },
  },
  methods: {
    search(searchedText) {
      this.searchFilter = searchedText;
    },
    addToCart(productId) {
      const product = this.auctionItems.find((item) => item.id == productId);
      this.cartItems = [...this.cartItems, product];

      localStorage.setItem("cart", JSON.stringify(this.cartItems));
    },
    handleShowModal(productId) {
      this.selectedItem = this.auctionItems.find(
        (item) => item.id == productId
      );
      this.showModal = true;
    },
  },
  mounted() {
    const cart = localStorage.getItem("cart");
    if (cart) {
      this.cartItems = JSON.parse(cart);
    }
  },
};
</script>

<style lang="scss">
@import "assets/styles/app.scss";
</style>
