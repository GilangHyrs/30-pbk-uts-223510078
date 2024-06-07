<template>
  <div class="body">
    <section class="create-todo">
      <h3 style="color: white;">APA YANG KAMU INGIN LAKUKAN HARI INI</h3>
      <form @submit.prevent="addTodo">
        <h4>MASUKAN KEGIATAN</h4>
        <input type="text" placeholder="Masukkan kegiatan" v-model="inputContent" />
        <h4>Pilih kategori:</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="home" v-model="inputCategory" />
            <span class="bubble home"></span>
            <div>Rumah</div>
          </label>
          <label>
            <input type="radio" name="category" value="assignment" v-model="inputCategory" />
            <span class="bubble task"></span>
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
      <div class="todo-list">
        <table>
          <thead>
            <tr>
              <th>Kegiatan</th>
              <th>Kategori</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="todo in todosAsc" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
              <td>{{ todo.content }}</td>
              <td>{{ todo.category }}</td>
              <td>
                <button class="delete" @click="removeTodo(todo)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

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

<style>
.create-todo h3 {
  font-size: 30px;
  font-weight: bold;
  font-family: 'Oswald', sans-serif;
}


.create-todo, .todo-list {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
  padding: 20px;
  font-family: 'Oswald', sans-serif;
  
}

.create-todo form {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgb(255, 251, 251);
  margin-bottom: 20px;
}

.create-todo .options {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}

.create-todo .options label {
  margin: 0 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.create-todo .options input[type="radio"] {
  display: none;
}

.create-todo .options .bubble {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  margin-right: 10px;
  border: 2px solid transparent;
  transition: all 0.3s;
}

.create-todo .options input[type="radio"]:checked + .bubble {
  border-color: #d61414;
}

.create-todo .options .home { background: #ffcccb; }
.create-todo .options .task { background: #ccffcc; }
.create-todo .options .work { background: #ccccff; }

button[type="submit"] {
  padding: 10px 20px;
  border: none;
  background: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s;
  font-family: 'Oswald', sans-serif;
  
}

button[type="submit"]:hover {
  background: #0056b3;
}

.todo-list table {
  width: 100%;
  border-collapse: collapse;
  font-family: 'Oswald', sans-serif;
  background-color: #fff; 
}

.todo-list th, .todo-list td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  color: #333; 
}

.todo-list th {
  background-color: #f2f2f2;
}

.todo-list tr:hover {
  background-color: #f5f5f5;
}

.todo-list .todo-item.done {
  background: #e0ffe0;
}

.todo-list .todo-item .actions .delete {
  background: transparent;
  border: none;
  color: #dc3545;
  cursor: pointer;
  transition: color 0.3s;
}

.todo-list .todo-item .actions .delete:hover {
  color: #c82333;
}

.todo-list .todo-item.done {
  background: #e0ffe0;
}

.todo-list .todo-item .actions .delete {
  background: transparent;
  border: none;
  color: #fcfcfc;
  cursor: pointer;
  transition: color 0.3s;
}

.todo-list .todo-item .actions .delete:hover {
  color: #f8f8f8;
}
.body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url('https://fiverr-res.cloudinary.com/images/q_auto,f_auto/gigs/129325364/original/afaddcb9d7dfaaf5bff7ef04101935814665ac16/design-an-attractive-background-for-your-website.png');
  background-size: cover;
  background-position: center;
}
</style>
