<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const todos = ref([])
const inputContent = ref('')
const inputCategory = ref(null)

const todosAsc = computed(() => todos.value.slice().sort((a, b) => b.createdAt - a.createdAt))

const addTodo = () => {
  if (inputContent.value.trim() === '' || inputCategory.value === null) {
    return
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    createdAt: new Date().getTime()
  })

  inputContent.value = ''
  inputCategory.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
  <section class="create-todo">
    <h3>APA YANG KAMU INGIN LAKUKAN HARI INI</h3>
    <form @submit.prevent="addTodo">
      <h4>Masukkan kegiatan atau tugas yang ingin Anda lakukan:</h4>
      <input type="text" placeholder="Masukkan kegiatan atau tugas" v-model="inputContent" />
      <h4>Pilih kategori:</h4>
      <div class="options">
        <label>
          <input type="radio" name="category" value="home" v-model="inputCategory" />
          <span class="bubble business"></span>
          <div>Rumah</div>
        </label>
        <label>
          <input type="radio" name="category" value="task" v-model="inputCategory" />
          <span class="bubble"></span>
          <div>Tugas</div>
        </label>
        <label>
          <input type="radio" name="category" value="work" v-model="inputCategory" />
          <span class="bubble work"></span>
          <div>Kerja</div>
        </label>
      </div>
      <button type="submit">Submit</button>
    </form>
    <section class="todo-list">
      <h3>HASIL YANG SUDAH ANDA LAKUKAN</h3>
      <div class="list">
        <div v-for="todo in todosAsc" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">delete</button>
          </div>
        </div>
      </div>
    </section>
  </section>
</template>

<style>
.create-todo, .todo-list {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}

.create-todo form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.create-todo .options {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.create-todo .options label {
  margin: 0 10px;
}

.todo-list .list {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.todo-list .todo-item {
  width: 100%;
  max-width: 500px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.todo-list .todo-item.done .todo-content {
  text-decoration: line-through;
}
</style>
