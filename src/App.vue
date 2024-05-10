<template>
  <div class="container">
    <h1>JADWAL BULAN RAMADHAN</h1>
    <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="kegiatan">
    <div v-for="(todo, index) in todos" :key="index" class="todo-item" :class="{ 'completed': todo.completed }">
      <input type="checkbox" v-model="todo.completed"> 
      <template v-if="!todo.editing">
        <span>{{ todo.text }}</span>
        <button @click="editTodo(index)">Edit</button>
        <button @click="markAsDone(index)">Mark as Done</button>
      </template>
      <template v-else>
        <input type="text" v-model="editedTodoText" ref="editedTodoInput" @keyup.enter="saveTodo(index)" @blur="saveTodo(index)">
        <button @click="saveTodo(index)" class="save-btn">Save</button>
      </template>
      <button @click="cancelTodo(index)">Delete</button>
    </div>
    <button @click="filterTodos">Filter Belum Selesai</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: []
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, completed: false, editing: false });
        this.newTodo = '';
      }
    },
    cancelTodo(index) {
      this.todos.splice(index, 1);
    },
    filterTodos() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    editTodo(index) {
      this.todos[index].editing = true;
      this.editedTodoText = this.todos[index].text;
      this.$nextTick(() => {
        this.$refs.editedTodoInput[index].focus();
      });
    },
    saveTodo(index) {
      this.todos[index].text = this.editedTodoText;
      this.todos[index].editing = false;
    },
    markAsDone(index) {
      this.todos[index].completed = true;
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 20px auto;
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  color: white;
  background-image: url("./assets/background.jpeg");
  background-size: cover;
  background-repeat: no-repeat;
}

.todo-item {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  color: black;
  display: flex;
  align-items: center;
}

.completed {
  text-decoration: line-through;
  color: #999;
}

input[type="checkbox"] {
  margin-right: 5px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 16px;
}

button {
  margin-left: 5px;
  padding: 8px 12px;
  cursor: pointer;
}

button.save-btn {
  background-color: green;
  color: white;
}

input[type=submit] {
  background-color: crimson;
  border-radius: 18px;
  color: white;
}
</style>
