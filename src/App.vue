<template>
  <div id="app">
    <todolist></todolist>
  </div>
</template>

<script>
import TodoList from './components/TodoList';
export default {
  name: 'App',
  components: {
    'todolist':TodoList,
  }
}

</script>

<style>
</style>
