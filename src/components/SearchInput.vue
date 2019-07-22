<template>
  <div class="searchWrapper">
    <div class="search">
      <input id="search" name="search" v-model="searchValue" @input="handleInput" />
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchInput"
};
</script>

<style lang="scss" scoped>
.searchWrapper {
  margin-top: 50px;
  display: flex;
  flex-direction: column;
  width: 250px;
}

input {
  background: none;
  height: 30px;
  width: 100%;
  border: 0;
  border-bottom: 1px solid black;
}
</style>
