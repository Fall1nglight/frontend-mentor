<template>
  <Navbar />
  <Calculator />
</template>

<script>
import Navbar from './components/Navbar.vue';
import Calculator from './components/Calculator.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    Calculator,
  },
};
</script>

<style>
* {
  font-family: 'Space Mono', monospace;
}

body {
  background: hsl(185, 41%, 84%);
}
</style>
