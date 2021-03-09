<template>
  <div id="app">
    <div class="container">
      <img alt="Vue logo" src="./assets/logo.png" />
      <HelloWorld msg="My Todo App" />
      <todo-list></todo-list>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    HelloWorld,
    TodoList
  }
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}
</style>
