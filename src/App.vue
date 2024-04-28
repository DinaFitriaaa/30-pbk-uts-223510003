<template>
  <div class="todo-app">
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
</template>




<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
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
    },
  },
};


</script>

<style>
/* General Styles */
body {
  background-color: #10234e;
  color: #f5f5f5; /* Light background for better contrast */
  font-family: sans-serif; /* Simpler, more neutral font */
  margin: 0;
  background-image: url(/src/assets/bck.jpg); 
  background: cover;
background-size: cover;
background-repeat: no-repeat;
}

/* Todo List Container */
.todo-app {
  max-width: 600px;
  margin: 40px auto;
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(135, 64, 64, 0.1); /* Subtle shadow for depth */
  background-color: #232440; /* Darker background color */
}

/* Heading */
.todo-app h1 {
  text-align: center;
  color: #fff; /* White heading color */
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
  padding: 15px 20px; /* Increase padding for a more spacious feel */
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1); /* Subtle inset shadow for a touch of depth */
  background-color: #fff; /* Light background for better input visibility */
  /* Added line: Set placeholder color for better visibility */
  color: #333; /* Darker text color for input field */
  /* Added line: Set placeholder color to a lighter shade of the background */
  ::placeholder { /* Targets the placeholder text */
    color: #aaa;
  }
}

.todo-app button[type=submit] {
  margin-left: 10px;
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #3a7bca; /* Green button color */
  color: #1d1616; /* White button text */
  border: none; /* Remove border for better strikethrough visibility */
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
  color: #fff; /* White text for filter buttons */
}

.filter-btn.active {
  background-color: #2196f3; /* Blue background for active filter */
}

/* Todo List */
table {
  width: 100%;
  border-collapse: collapse; /* Remove borders between table cells */
}

th {
  padding: 10px; /* Added padding for better spacing in table headers */
  background-color: #2196f3; /* Blue background for table headers */
  color: #fff; /* White text color for table headers */
  text-align: left; /* Left-align table headers for better readability */
}

td {
  padding: 10px; /* Added padding for better spacing in table cells */
  position: relative; /* Added for strikethrough positioning */
}

/* Strikethrough style for completed tasks */
td.done span {
  text-decoration: line-through; /* Strikethrough line */
  color: #aaa; /* Gray color for the strikethrough line */
  position: absolute; /* Absolute positioning for strikethrough */
  top: 50%; /* Center the strikethrough line vertically */
  left: 0;
  width: 100%;
  transform: translateY(-50%);
  background-image: linear-gradient(to right, #ccc 50%, transparent 50%);
}

.done {
  text-decoration: line-through;
  color: #aaa;
}


</style>