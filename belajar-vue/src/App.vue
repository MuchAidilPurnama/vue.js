<script setup>
import { ref } from 'vue'

let message = "Hello World"
let rawHtml = "semuanya musingkeun" 
let a = 10
let b = 32
let c = a + b
let objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
let count = ref(5)
const text = ref('')

function onInput(e) {
  text.value = e.target.value
}
let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

fetchData()

</script>

<template>
  <table>
  <div>
    <span style="color: aqua;"><h1>{{ message }}</h1></span>
    <li><span v-html ="rawHtml"></span></li>
  </div>
  bil 1 = {{ a }} <br>
  bil 2 = {{ b }} <br>
  hasil = {{ c }} <br>
  <div v-bind="objectOfAttrs">
    <span style="color: aqua;"><h1>{{ message }}</h1></span>
    <li><span v-html ="rawHtml"></span></li>
  </div>
  <br>
  <button>hitungan : {{ count }}</button>
  <br>
  <div>
  <input :value="text" @input="onInput" placeholder="Type here">
  <p>{{ text }}</p>
  </div>
  <br>
  <div>
    <div>
      <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button></div></div><br>
  <div>
    <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++">selanjutnya</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
  <br>
  </div>
  </table>
</template>

<style>
.done {
  text-decoration: line-through;
}

</style>