<template>
<li>
  <label>
    <input type="checkbox" :checked="todoObj.done" @change="handleCheckbox(todoObj.id)"/>
    <span v-show="!todoObj.isEdit">{{todoObj.title}}</span>
    <input 
      type="text" 
      v-show="todoObj.isEdit" 
      :value="todoObj.title" 
      @blur="handleBlur(todoObj, $event)" 
      @keyup.enter="handleBlur(todoObj, $event)" 
      ref="inputTitle"
    >
    <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
    <button 
      class="btn btn-edit" 
      v-show="!todoObj.isEdit" 
      @click="handleEdit(todoObj)"
    >编辑</button>
  </label>
</li>
</template>

<script>
export default {
    name: 'todoItem',
    props:['todoObj'],
    methods: {
      handleCheckbox(id){
        this.$bus.$emit('checkTodo', id)
      },
      handleDelete(id){
        if(confirm('确定删除?')){
          this.$bus.$emit('deleteTodo',id)
        }
      },
      handleEdit(todoObj){
        if(todoObj.hasOwnProperty.call(todoObj,'isEdit')){
          todoObj.isEdit = true
        } else {
          this.$set(todoObj,'isEdit',true)
        }
        this.$nextTick(function(){
          this.$refs.inputTitle.focus()
        })
      },
      handleBlur(todoObj, e){
        todoObj.isEdit = false
        if(!e.target.value.trim()) return alert('输入不能为空')
        this.$bus.$emit('updateTodo', todoObj.id, e.target.value)
      }
    },
}
</script>

<style scoped>
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  /* float: left; */
  cursor: pointer;
}

li label, li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li label button {
  float: right;
  margin-right: 6px;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>