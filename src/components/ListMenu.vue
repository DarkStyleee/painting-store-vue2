<template>
  <menu class="menu">
    <!-- Пример с vue-router -->
    <!-- <router-link
      v-for="(route, idx) in routes"
      :key="idx"
      :to="{ name: route.link }"
      >{{ route.name }}</router-link
    > -->
    <a
      class="menu__link"
      v-for="(route, idx) in routes"
      :key="idx"
      :href="route.link"
      >{{ route.name }}</a
    >
  </menu>
</template>

<script>
export default {
  props: {
    routes: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style></style>
