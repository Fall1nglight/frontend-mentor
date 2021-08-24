<template>
  <Calculator />
</template>

<script>
import Calculator from './components/Calculator.vue';

export default {
  name: 'App',
  components: {
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

#app {
  display: flex;
  min-height: 100vh;
  align-items: center;
}
</style>
