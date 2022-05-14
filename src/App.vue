<template>
  <div class="flex justify-center items-center  text-dark">
    <div class="w-full max-w-lg bg-light px-5 py-6 rounded-lg mt-4 min-h-screen">
      <Todo />
    </div>
  </div>
</template>

<script>
import Todo from './components/Todo.vue'

export default {
  name: 'App',

  components: {Todo}
}
</script>

<style>
</style>
