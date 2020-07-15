<template>
  <div>
    <md-card class="main-card">
      <md-card-header>
        <div class="main-title">ToDo or not ToDo</div>
        <div class="main-subtitle">The Perfect ToDo List</div>
        </md-card-header>
        <md-card-content>
    <md-field :class="{'md-invalid': errorMessage}">
      <md-input 
        class="todo-input"
        v-model="currentTodo" 
        @keydown.enter="addTodo()" 
        placeholder="Add todo">
        </md-input>
        <span class="error">Don't be lazy! Find something to do!</span>
      </md-field>
    <md-list class="todos">
      <draggable v-model="todos" group="list-items" :disabled="!dragAllowed" @start="drag=true" @end="drag=false">
      <md-list-item 
        v-for="todo in todos" 
        v-if="todo.edit===false" 
        :key="todo.id" 
        @dblclick="editTodo(todo, true)" 
        :md-ripple="false"
        class="todo-item"
        >
        <div class="todo-div">
          <md-checkbox v-model="todo.completed"></md-checkbox>
          <div class="todo-label" :class="{ completeTodo: todo.completed }">{{ todo.label }}</div>
        </div>
        <div class="todo-actions">
          <md-button class="edit-button" @click="editTodo(todo, true)">
            <md-icon class="edit-img">Edit</md-icon>
          </md-button>
        </div>
      </md-list-item>
      <md-list-item v-else>
        <md-field>
          <md-input
            class="edit-todo"
            v-model="todo.label"
            v-focus
            @keydown.enter="editTodo(todo, false)"
            @focusout="editTodo(todo, false)">
          </md-input>
        </md-field>
      </md-list-item>
      </draggable>
    </md-list>
  </md-card-content>
  </md-card>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  components: {
    draggable
  },
  data() {
    return {
      todos: [],
      currentTodo: '',
      errorMessage: false,
      dragAllowed: true
    };
  },
  methods: {
    addTodo() {
      if (this.currentTodo.length === 0){
        this.errorMessage = true;
        }
        else {
          this.errorMessage = false;
          this.todos.push({
            id: this.todos.length, 
            label: this.currentTodo, 
            completed: false,
            edit: false
          });
        }
      this.currentTodo = '';
    },

    removeTodo(todo) {
      this.errorMessage = false;
      todo.edit = false;
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },

    editTodo(todo, toggle){
      this.errorMessage = false;
      if (toggle){
        todo.edit = true;
        this.dragAllowed = false;
      }
      else {
        todo.edit = false;
        this.dragAllowed = true;
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