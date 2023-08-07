<template>
  <div id="app">
    <!-- Display title -->
    <h1>Todo App</h1>

    <!-- Form to add new todos, @submit.prevent prevents the default form submit action -->
    <form @submit.prevent="addTodo">
      <!-- v-model binds input value to newTodo data property, .trim removes leading and trailing spaces -->
      <input type="text" v-model.trim="newTodo" placeholder="Add an item to your to-do list..." />
      <!-- Button to submit form -->
      <button type="submit">Add</button>
    </form>

    <!-- Display error message if exists, v-if conditionally renders element -->
    <p v-if="error" class="error">{{ error }}</p>

    <!-- Transition-group for list animations -->
    <transition-group name="list" tag="ul">
      <!-- v-for renders list of todos -->
      <li v-for="(todo, index) in todos" :key="todo.text" class="list-item">
        <!-- Display individual todo -->
        <div class="todo-item">
          <!-- Checkbox to mark todo as complete or not -->
          <input type="checkbox" v-model="todo.completed" />
          <!-- Display todo text, class binding sets 'completed' class if todo is completed -->
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        </div>
        <!-- Button to remove completed todos -->
        <button v-if="todo.completed" @click="removeTodo(index)">Remove</button>
      </li>
    </transition-group>
    <!-- Display message when todo list is empty -->
    <div v-if="todos.length === 0">No items in the list.</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',  // Data property for new todo input
      todos: [],  // Data property for todo list
      error: '',  // Data property for error message
    };
  },
  methods: {
    // Method to add new todo
    addTodo() {
      if (this.newTodo === '') {  // Check if input is empty
        this.error = 'Item cannot be empty.';  // Set error message
        return;
      }
      // Add new todo to todos array
      this.todos.push({ text: this.newTodo, completed: false });
      this.newTodo = '';  // Clear input field
      this.error = '';  // Clear error message
    },
    // Method to remove a todo
    removeTodo(index) {
      this.todos.splice(index, 1);  // Remove todo from todos array by index
    },
  },
};
</script>

<style scoped>
  /* Styles for various elements in Vue application */
body {
  background-color: #f5f5f5;
  font-family: Arial, sans-serif;
}

#app {
  text-align: center;
  margin-top: 50px;
}

h1 {
  font-family: 'Raleway', sans-serif;
}

ul {
  padding: 0;
}

li {
  background-color: #fff;
  margin: 10px auto;
  width: 50%;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo-item {
  display: flex;
  align-items: center;
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

button {
  background-color: #ff7f7f;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  color: #fff;
  cursor: pointer;
}

form {
  display: flex;
  justify-content: center;
  align-items: center;
}

input[type="text"] {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  width: 300px;
  margin-right: 10px;
}

.error {
  color: red;
}

.completed {
  text-decoration: line-through;
  color: #ccc;
}

.list-item {
  transition: all 0.5s ease;
}

.list-enter-active, .list-leave-active {
  transition: opacity 0.5s, transform 0.5s;
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-leave-active {
  position: absolute;
}

@media screen and (max-width: 600px) {
  li {
    width: 90%;
  }
  form {
    flex-direction: column;
    align-items: flex-start;
  }
  input[type="text"] {
    width: 90%;
    margin-bottom: 10px;
  }
  button {
    width: 75%;
  }
}
</style>
