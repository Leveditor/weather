<template>
  <div>
    <Weather />
    <Footer />
  </div>
</template>

<style>
  body {
    background-image: url('./assets/sunny.jpg');
    background-repeat: no-repeat;
    background-size: cover;
  }
</style>

<script>
import './assets/tailwind.css';
import Weather from './components/Weather.vue';
import Footer from './components/Footer.vue';

export default {
    name: 'App',
    components: {
      Weather,
      Footer
    }
}
</script>

