<template>
  <div>
    <Weather />
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

export default {
    name: 'App',
    components: {
      Weather
    }
}
</script>

