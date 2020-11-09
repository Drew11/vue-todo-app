<template>
  <div id="app" class="container">
    <img class="logo" alt="Vue logo" src="./assets/logo.png">
    <todo-list>

    </todo-list>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 40px;
  font-size: 24px;
}

.logo {
  display: block;
  margin: 0 auto 10px;
  height: 75px;
}

</style>

