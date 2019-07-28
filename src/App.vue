<template>
  <div id="app">
    <div :class="[{flexStart: step === 1}, 'wrapper']" class="wrapper">
      <transition name="slide">
        <img src="./assets/logo.svg" class="logo" v-if="step === 1" />
      </transition>
      <transition name="fade">
        <BackgroundImage v-if="step === 0" />
      </transition>
      <Claim v-if="step === 0" />
      <SearchInput :dark="step === 1" v-model="searchValue" @input="handleInput" />
      <div class="results" v-if="results && !loading && step === 1">
        <Item
          v-for="item in results"
          :item="item"
          :key="item.data[0].nasa_id"
          @click.native="handleModalOpen(item)"
        />
      </div>
    </div>
    <div class="loader" v-if="step === 1 && loading" />
    <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = false" />
  </div>
</template>

<script>
import axios from "axios";
import debounce from "lodash.debounce";
import Claim from "@/components/Claim";
import BackgroundImage from "@/components/BackgroundImage";
import SearchInput from "@/components/SearchInput";
import Item from "@/components/Item";
import Modal from "@/components/Modal";
const API = "https://images-api.nasa.gov/search";
export default {
  name: "Search",
  components: { Claim, SearchInput, BackgroundImage, Item, Modal },
  data() {
    return {
      modalOpen: false,
      modalItem: null,
      loading: false,
      step: 0,
      searchValue: "",
      results: []
    };
  },
  methods: {
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },
    handleInput: debounce(function() {
      this.loading = true;
      console.log(this.searchValue);
      axios
        .get(`${API}?q=${this.searchValue}&media_type=image`)
        .then(response => {
          console.log(response.data.collection.items);
          this.results = response.data.collection.items;
          this.loading = false;
          this.step = 1;
        })
        .catch(err => {
          console.log(err);
        });
    }, 500)
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap&subset=latin-ext");
* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  font-family: Montserrat, sans-serif;
  margin: 0;
  padding: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.slide-enter-active,
.slide-leave-active {
  transition: margin-top 0.3s ease;
}
.slide-enter,
.slide-leave-to {
  margin-top: -50px;
}
.wrapper {
  display: flex;
  position: relative;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100vh;
  min-height: 100vh;
  margin: 0;
  padding: 30px;

  &.flexStart {
    justify-content: flex-start;
  }
}
.loader {
  margin-top: 100px;
  display: inline-block;
  width: 64px;
  height: 64px;
  @media (min-width: 768px) {
    width: 90px;
    height: 90px;
  }
}
.loader:after {
  content: " ";
  display: block;
  width: 46px;
  height: 46px;
  margin: 1px;
  border-radius: 50%;
  border: 5px solid #1e3d4a;
  border-color: #1e3d4a transparent #1e3d4a transparent;
  animation: loading 1.2s linear infinite;
  @media (min-width: 768px) {
    width: 90px;
    height: 90px;
  }
}
@keyframes loading {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.logo {
  position: absolute;
  top: 30px;
}
.results {
  display: grid;
  margin-top: 50px;
  grid-gap: 20px;
  grid-template-columns: 1fr 1fr;
  @media (min-width: 768px) {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
</style>
