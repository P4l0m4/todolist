<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav> -->
    <!-- <router-view /> -->
    <TodoComponent />
  </div>
</template>

<script>
import TodoComponent from "./components/TodoComponent.vue";

export default {
  components: {
    TodoComponent,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  font-family: "Poppins", sans-serif;
  border-radius: 8px;
}
</style>
