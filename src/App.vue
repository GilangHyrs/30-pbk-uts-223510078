<script setup>
import { ref, onMounted, watch } from 'vue'
import TodoList from './components/TodoList.vue'

const name = ref('')
const activeMenu = ref('todos')

watch(name, newVal => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

const selectedUser = ref(null)
const users = ref([])
const posts = ref([])
const selectedUserName = ref('')

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await response.json()
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const fetchPosts = async () => {
  if (!selectedUser.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`)
    posts.value = await response.json()
    selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
  } catch (error) {
    console.error('Error fetching posts:', error)
  }
}

onMounted(fetchUsers)
</script>

<template>
  <main class="app">
    <header class="header">
      <nav class="menu">
        <ul>
          <li @click="activeMenu = 'post'"><a href="#">Post</a></li>
          <li @click="activeMenu = 'todos'"><a href="#">Todos</a></li>
        </ul>
      </nav>
    </header>
    <section class="greeting">
      <h2 class="title">
        WELCOME TO, <input class="text" type="text" placeholder="MY FIRST TODO LIST APP" v-model="name" />
      </h2>
    </section>
    <TodoList v-if="activeMenu === 'todos'" />
    <section v-if="activeMenu === 'post'" class="post-section">
      <h2>Postingan Pengguna</h2>
      <div class="select-user">
        <label>Pilih Pengguna:</label>
        <select v-model="selectedUser" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div v-if="posts.length > 0" class="user-posts">
        <h3>Postingan Pengguna: {{ selectedUserName }}</h3>
        <ul>
          <li v-for="post in posts" :key="post.id">
            <h4>{{ post.title }}</h4>
            <p>{{ post.body }}</p>
          </li>
        </ul>
      </div>
    </section>
  </main>
</template>

<style>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 20px;
  box-sizing: border-box;
}

.header .menu ul {
  display: flex;
  justify-content: center;
  list-style: none;
  padding: 0;
  margin: 0;
}

.header .menu ul li {
  margin: 0 10px;
}

.header .menu ul li a {
  text-decoration: none;
  color: #000;
}

.greeting {
  text-align: center;
  margin-bottom: 20px;
}

.post-section .select-user {
  margin-bottom: 20px;
}

.user-posts ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
</style>
