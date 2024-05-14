<template>
  <div class="app">
    <header>
      <button @click="currentView = 'todos'" :class="{ active: currentView === 'todos' }">Todos</button>
      <button @click="currentView = 'posts'" :class="{ active: currentView === 'posts' }">Posts</button>
    </header>

    <div v-if="currentView === 'todos'" class="todo-app">
      <h1>List Tugas</h1>
      <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" placeholder="Tambah tugas" />
        <button type="submit" class="primary">Tambah</button>
      </form>
      <h2>Daftar Tugas</h2>
      <div class="filter-section">
        <button @click="filterTodos('all')" class="filter-btn">Semua</button>
        <button @click="filterTodos('active')" class="filter-btn">Belum Selesai</button>
        <button @click="filterTodos('completed')" class="filter-btn">Selesai</button>
      </div>
      <table>
        <thead>
          <tr>
            <th style="width: 60%">Tugas</th>
            <th style="width: 20%">Status</th>
            <th style="width: 20%">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(todo, index) in filteredTodos" :key="index">
            <td>
              <input type="checkbox" v-model="todo.done" />
              <span :class="{ 'done': todo.done }">{{ todo.text }}</span>
            </td>
            <td :class="{ completed: todo.done }">{{ todo.done ? 'Completed' : 'Not Completed' }}</td>
            <td>
              <button @click="removeTodo(index)" class="danger">Remove</button>
            </td>
          </tr>
        </tbody>
      </table>
      <footer>
        <p>Semoga dapat membantu harimu!</p>
      </footer>
    </div>

    <div v-if="currentView === 'posts'" class="post-app">
      <h1>Daftar Postingan</h1>
      <div class="user-select">
        <label for="user">Pilih User:</label>
        <select v-model="selectedUserId" @change="fetchPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <h2>Postingan</h2>
      <table>
        <thead>
          <tr>
            <th style="width: 60%">Judul</th>
            <th style="width: 40%">Isi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="post in posts" :key="post.id">
            <td>{{ post.title }}</td>
            <td>{{ post.body }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
      currentView: 'todos',
      users: [],
      selectedUserId: null,
      posts: []
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }
      this.todos.push({
        text: this.newTodo,
        done: false,
      });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    filterTodos(filterType) {
      this.filter = filterType;
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
          if (this.users.length > 0) {
            this.selectedUserId = this.users[0].id;
            this.fetchPosts();
          }
        });
    },
    fetchPosts() {
      if (this.selectedUserId) {
        fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
          .then(response => response.json())
          .then(data => {
            this.posts = data;
          });
      }
    }
  },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'active':
          return this.todos.filter(todo => !todo.done);
        case 'completed':
          return this.todos.filter(todo => todo.done);
        default:
          return this.todos;
      }
    }
  },
  created() {
    this.fetchUsers();
  }
};
</script>
<style>
/* General Styles */
body {
  background-color: #10234e;
  color: #f5f5f5;
  font-family: sans-serif;
  margin: 0;
  background-image: url(/src/assets/bck.jpg);
  background: cover;
  background-size: cover;
  background-repeat: no-repeat;
}

/* Header */
header {
  display: flex;
  justify-content: center;
  padding: 10px;
  background-color: #232440;
}

header button {
  margin: 0 10px;
  padding: 10px 20px;
  background-color: transparent;
  border: none;
  color: #fff;
  cursor: pointer;
  font-size: 1rem;
}

header button.active {
  background-color: #3a7bca;
  border-radius: 5px;
}

/* Todo List Container */
.todo-app, .post-app {
  max-width: 600px;
  margin: 40px auto;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(135, 64, 64, 0.1);
  background-color: #232440;
}

/* Heading */
.todo-app h1, .post-app h1 {
  text-align: center;
  color: #fff;
  font-size: 1.8rem;
  margin-bottom: 20px;
}

/* Input Form */
.todo-app form {
  display: flex;
  margin-bottom: 20px;
}

.todo-app input[type=text] {
  flex: 1;
  padding: 15px 20px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  color: #333;
}

.todo-app button[type=submit] {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #3a7bca;
  color: #1d1616;
  border: none;
}

/* Filter Buttons */
.filter-section {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 5px 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
  color: #fff;
}

.filter-btn.active {
  background-color: #2196f3;
}

/* Todo List */
table {
  width: 100%;
  border-collapse: collapse;
}

th {
  padding: 10px;
  background-color: #2196f3;
  color: #fff;
  text-align: left;
}

td {
  padding: 10px;
  position: relative;
}

/* Strikethrough style for completed tasks */
td.done span {
  text-decoration: line-through;
  color: #aaa;
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  transform: translateY(-50%);
  background-image: linear-gradient(to right, #ccc 50%, transparent 50%);
}

.done {
  text-decoration: line-through;
  color: #aaa;
}

/* Post Section */
.post-app .user-select {
  margin-bottom: 20px;
  text-align: center;
}

.post-app select {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.post-app table {
  width: 100%;
  border-collapse: collapse;
}

.post-app th {
  padding: 10px;
  background-color: #2196f3;
  color: #fff;
  text-align: left;
}

.post-app td {
  padding: 10px;
}
</style>
