<script setup lang="ts">
import TodoApp from './components/TodoApp.vue'
</script>

<template>
  <header>
    <img alt="TODO logo" class="logo" src="@/assets/to-do-list.png" width="125" height="125" />
    <div class="wrapper">
      <TodoApp msg="You did it!" />
    </div>
  </header>

</template>

<style scoped>
</style>
