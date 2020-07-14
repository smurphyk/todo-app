<template>
  <div>
    <md-field>
      <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add todo"></md-input>
      </md-field>
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id" @dblclick="editTodo(todo, true)" :class="{ completedTodo: todo.completed }">
        {{ todo.label }} <input type="checkbox" @click="completeTodo(todo)"/><button @click="removeTodo(todo)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: ''
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length, 
        label: this.currentTodo, 
        completed: false,
        edit: false
      });
      this.currentTodo = '';
    },
    editTodo(todo, toggle){
      if (toggle){
        todo.edit = true;
        todo.completed = false;
      }
      else {
        todo.edit = false;
      }
    },
    completeTodo(todo) {
      if (event.target.checked) {
        todo.completed = true
      }
      else {
        todo.completed = false;
      }
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
};
</script>

<style>
</style>