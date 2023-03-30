<template>
  <div class="search">
    <div class="input-control">
      <input
        @keypress.enter="handleChange"
        @change="handleChange"
        v-model="searchedText"
        type="search"
        placeholder="Поиск по названию картины"
      />
    </div>
    <button class="default-btn serch-btn" @click="handleChange">Найти</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchedText: "",
    };
  },
  methods: {
    handleChange() {
      this.$emit("search", this.searchedText);
    },
  },
};
</script>

<style></style>
