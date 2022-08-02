<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <todoHeader @addtodo="addtodo"/>
        <todo-List :todos="todos"/>
        <todoFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllSelected="clearAllSelected"/>
      </div>
    </div>
  </div>
</template>

<script>
import todoHeader from './components/todoHeader.vue'
import todoList from './components/todoList.vue'
import todoFooter from './components/todoFooter.vue'

export default {
  name: 'App',
  components: {
    todoHeader,
    todoList,
    todoFooter
  },
  data(){
    return {
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    addtodo(todoObj){
      this.todos.unshift(todoObj)
    },
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.done = !todo.done
      })
    },
    deleteTodo(id){
      this.todos = this.todos.filter((todo)=>{
        return todo.id !== id
      })
    },
    checkAllTodo(done){
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    clearAllSelected(){
      this.todos = this.todos.filter((todo)=>{
        return todo.done === false
      })
    },
    updateTodo(id,title){
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.title = title
      })
    }
  },
  watch:{
    todos: {
      deep:true,
      handler(value){
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  },
  mounted(){
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('deleteTodo',this.deleteTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
  },
  // 组件不再使用时解绑指定事件
  beforeDestroy(){
    // header
    this.$off('addtodo')
    // list->item
    this.$off('checkTodo')
    this.$off('deleteTodo')
    this.$off('updateTodo')
    // footer
    this.$off('checkAllTodo')
    this.$off('clearAllSelected')
  }
}
</script>

<style>
body {
  background-color: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.btn-edit {
  color: #fff;
  background-color: #61dd6b;
  border: 1px solid #3e9540;
}

.btn-edit:hover {
  color: #fff;
  background-color: #3e9540;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
