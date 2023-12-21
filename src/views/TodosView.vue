<script setup>
import { ref } from 'vue';
import { uid } from 'uid'
import { Icon } from '@iconify/vue'
import TodoCreator from '../components/TodoCreator.vue'
import TodoItem from '../components/TodoItem.vue'
const todoList = ref([])

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    // todo: todo
    todo,
    isCompleted: null,
    isEditing: null
  })
}
const toggleTodoComplete = (index) => {
  // Set the value to the opposite of what it currently is 
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted
}
const toggleEditing = (index) => {
  // Set the value to the opposite of what it currently is 
  todoList.value[index].isEditing = !todoList.value[index].isEditing
}
const updateTodo = (index, newTodoText) => {
  todoList.value[index].todo = newTodoText;
  todoList.value[index].isEditing = false
  todoList.value[index].isCompleted = false
}
const deleteTodo = (index) => {
  todoList.value.splice(index, 1)
}

</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <!-- Need to pass props down to the todoItem (:todo="todo") -->
      <TodoItem 
        v-for="(todo, index) in todoList" 
        :key="todo.id"
        :todo="todo" :index="index" 
        @toggle-complete="toggleTodoComplete" 
        @toggle-editing="toggleEditing"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no todo`s to complete! Add one!</span>
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
}

h1 {
  margin-bottom: 16px;
  text-align: center;
}

.todo-list {
  display: flex;
  flex-direction: column;
  list-style: none;
  margin-top: 24px;
  gap: 20px;
}

.todos-msg {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 24px;
}
</style>
