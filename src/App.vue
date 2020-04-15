<template>
  <div id="app">
    <todo-list></todo-list>
<!--    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />-->
  </div>
</template>
<script lang="ts">
  import {Component, Vue} from 'vue-property-decorator';
  import TodoList from "@/components/TodoList.vue";

  @Component({
    components: {
      TodoList
    }
  })
  export default class App extends Vue{}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
