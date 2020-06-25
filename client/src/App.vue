<template>
  <div id="app">
    <!--<img src="./assets/logo.png">-->
    <navbar></navbar>
    <transition name="router-animation" enter-active-class="animated fadeIn" leave-active-class="animated fadeOut" mode="out-in">
      <router-view />
    </transition>
  </div>
</template>

<script>
import Navbar from './components/Navbar'

export default {
  name: 'App',
  components: {
    'Navbar': Navbar
  }
}
</script>

<style>
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");

  .page-wrapper {
    animation-duration: 0.2s;
  }
</style>
