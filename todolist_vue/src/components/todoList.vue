<template>
  <ul class="todo-main">
    <transition-group appear 
      name="animate__animated animate__bounce"
      enter-active-class="animate__fadeInDown"
      leave-active-class="animate__fadeOutUp"
    >
      <listItem v-for="todoObj in todos" :key="todoObj.id" :todoObj="todoObj"/>
      </transition-group>
  </ul>
 
</template>

<script>
import listItem from './todoItem.vue'
import 'animate.css'
export default {
    name: 'todoList',
    components: {listItem},
    props:['todos']
}
</script>

<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}

.animate__animated, .animate__bounce {
  --animate-duration: .3s;
}
</style>