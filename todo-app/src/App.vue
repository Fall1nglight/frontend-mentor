<template>
  <div class="container-sm p-5">
    <!-- Header -->
    <!-- TODO d-flex és mind2 egy sorba -->
    <div class="row justify-content-center">
      <div class="heading h1 col-5 col-md-4 col-lg-3">
        Todo
      </div>
      <div class="toggler-icon col-5 col-md-4 col-lg-3 text-end">
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

    <!-- Form -->
    <div class="row justify-content-center mt-4">
      <div
        :class="[
          'col-sm-10 col-md-8 col-lg-6 p-3 main-form',
          darkMode ? 'form-dark' : 'form-light',
        ]"
      >
        <form @submit.prevent="addTodo">
          <div class="row g-0 align-items-center">
            <div class="col-2 circle ms-2 me-3"></div>
            <div class="col-10">
              <input
                v-model="currentTodo"
                type="text"
                class="form-control"
                placeholder="Create a new todo..."
              />
            </div>
          </div>
        </form>
      </div>
    </div>

    <!-- List -->
    <div class="row justify-content-center mt-4">
      <div class="col-sm-10 col-md-8 col-lg-6 p-0">
        <ul class="list-group list-group-flush">
          <li
            v-for="(todo, index) in todos"
            :key="index"
            :class="[
              darkMode ? 'list-dark' : 'list-light',
              'list-group-item p-3',
            ]"
          >
            {{ todo }}
          </li>
        </ul>
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
    const todos = ref([]);
    const currentTodo = ref('');

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

    const addTodo = () => {
      if (!currentTodo.value) return;
      todos.value.push(currentTodo.value);
    };

    return {
      darkMode,
      togglerIcon,
      togglerIconCaption,
      toggleDarkMode,
      addTodo,
      todos,
      currentTodo,
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
  text-transform: uppercase;
  font-weight: 700;
  color: white;
}

/* Form & Input */
.main-form {
  border-radius: 5px;
}

.circle {
  width: 1.25rem;
  height: 1.25rem;
  border: 1px solid white;
  border-radius: 100%;
}

.form-dark,
.form-dark {
  background-color: hsl(235, 24%, 19%);
}

.form-light {
  background-color: hsl(236, 33%, 92%);
}

.form-dark .form-control,
.form-dark .form-control::placeholder {
  color: hsl(234, 39%, 85%);
}

.form-light .form-control,
.form-light .form-control::placeholder {
  color: hsl(236, 9%, 61%);
}

.form-control {
  background-color: transparent !important;
  border: none;
}

.form-control:focus {
  outline: none !important;
  box-shadow: none !important;
}

.form-control::placeholder {
  font-weight: 700;
}

/* List */

/* TODO | Last item in the list should get border to cut down edges */
/* TODO | Move all styles inside a main dark and light class */
.list-dark {
  color: hsl(234, 39%, 85%);
  background-color: hsl(235, 24%, 19%);
  border-bottom: 1px solid hsl(234, 39%, 85%) !important;
}

.list-light {
  color: hsl(236, 9%, 61%);
  background-color: hsl(236, 33%, 92%);
  border-color: hsl(236, 9%, 61%);
}

.list-group {
  border-radius: 5px;
}
</style>
