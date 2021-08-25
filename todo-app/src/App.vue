<template>
  <div class="container-sm p-5 mt-5">
    <div class="row justify-content-center">
      <div class="heading col-sm-5 col-md-4 col-lg-3">
        <h1>Todo</h1>
      </div>
      <div class="toggler-icon col-sm-5 col-md-4 col-lg-3 text-end">
        <button class="btn">
          <img
            :src="togglerIcon"
            :alt="togglerIconCaption"
            :hover="togglerIconCaption"
            @click="toggleDarkMode"
          />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref } from '@vue/reactivity';
import { watch } from '@vue/runtime-core';
import iconSun from './assets/icon-sun.svg';
import iconMoon from './assets/icon-moon.svg';

export default {
  name: 'App',
  setup() {
    //refs
    const darkMode = ref(true);

    //watches
    watch(darkMode, () => {
      if (darkMode.value)
        return (document.body.classList.value = 'background-dark');
      document.body.classList.value = 'background-light';
    });

    //computed properties
    const togglerIcon = computed(() => (darkMode.value ? iconSun : iconMoon));

    const togglerIconCaption = computed(() =>
      darkMode.value ? 'Toggle light mode' : 'Toggle dark mode'
    );

    //functions
    const toggleDarkMode = () => {
      darkMode.value = !darkMode.value;
    };
    return {
      darkMode,
      togglerIcon,
      togglerIconCaption,
      toggleDarkMode,
    };
  },
};
</script>

<style>
* {
  font-family: 'Josefin Sans', sans-serif;
}

/* Background */
.background-dark {
  background: url('./assets/bg-mobile-dark.jpg') hsl(235, 21%, 11%);
  background-repeat: no-repeat;
  background-position: top center;
  background-size: cover;
}

.background-light {
  background: url('./assets/bg-mobile-light.jpg') hsl(0, 0%, 98%);
  background-repeat: no-repeat;
  background-position: top center;
  background-size: cover;
}

@media (min-width: 576px) {
  .background-dark {
    background: url('./assets/bg-desktop-dark.jpg') hsl(235, 21%, 11%);
    background-repeat: no-repeat;
    background-position: top center;
    background-size: cover;
  }

  .background-light {
    background: url('./assets/bg-desktop-light.jpg') hsl(0, 0%, 98%);
    background-repeat: no-repeat;
    background-position: top center;
    background-size: cover;
  }
}

/* Heading */
.heading {
  text-align: start;
}

.heading h1 {
  text-transform: uppercase;
  font-weight: 700;
  color: white;
}
</style>
