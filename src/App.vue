<template>
  <div id="app">
    <div class="nav">
      <router-link to="/">Spacer</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800&display=swap&subset=latin-ext');
*{
  box-sizing: border-box;

}
body{
  font-family: Montserrat,sans-serif;
  margin: 0;
  padding: 0;
}
</style>
