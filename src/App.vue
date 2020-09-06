<template>
  <div class="container">
    <h1>ToDo app</h1>
    <md-field>
      <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add a todo"></md-input>
      <md-button class="md-icon-button md-raised" @click="addTodo()">
        <md-icon>add</md-icon>
      </md-button>
    </md-field>
    <ul class="todos">
      <li v-for="todo in filteredTodos" :key="todo.id">
        <div class="display">
          <input type="checkbox" v-on:change="completeTask(todo)" v-bind:checked="todo.completed" />
          <p
            class="title"
            contenteditable="true"
            v-on:keydown.enter="updateTask($event, todo)"
            v-on:blur="updateTask($event, todo)"
            v-bind:class="{completed: todo.completed}"
          >{{ todo.label }}</p>
          <div>
            <md-icon>delete</md-icon>
            <span class="md-list-item-text"></span>
          </div>
        </div>
      </li>
    </ul>
    <div class="filter">
      <md-button class="md-dense md-raised md-primary allToDos" @click="allToDos()">All</md-button>
      <md-button class="md-dense md-raised md-primary activeToDos" @click="activeToDos()">Active</md-button>
      <md-button
        class="md-dense md-raised md-primary completedToDos"
        @click="completedToDos()"
      >Completed</md-button>
    </div>
  </div>
</template>

<script>
export default {
  name: "RegularToolbar",
  data() {
    return {
      title: "todo",
      todos: [],
      currentTodo: "",
      selectedFilter: "all",
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false,
      });
      this.currentTodo = "";
    },
    removeTodo(e, todo) {
      e.preventDefault();
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    updateTask: function (e, todo) {
      e.preventDefault();
      todo.label = e.target.innerText;
      e.target.blur();
    },
    completeTask: function (todo) {
      todo.completed = !todo.completed;
    },
    allToDos: function () {
      this.selectedFilter = "all";
    },
    activeToDos: function () {
      this.selectedFilter = "active";
    },
    completedToDos: function () {
      this.selectedFilter = "completed";
    },
  },
  computed: {
    filteredTodos: function () {
      if (this.selectedFilter === "active") {
        return this.todos.filter((todo) => {
          return !todo.completed;
        });
      } else if (this.selectedFilter === "completed") {
        return this.todos.filter((todo) => {
          return todo.completed;
        });
      }
      return this.todos;
    },
  },
};
</script>

<style>
.container {
  margin: auto;
  width: 50%;
  padding-top: 50px;
}

h1 {
  font-size: 50px;
  font-family: "Berkshire Swash", cursive;
}

.md-field {
  border: 2px solid royalblue;
  border-radius: 5px;
  font-family: "Niconne", cursive;
  padding: 0 !important;
}

.md-input {
  height: 40px !important;
}

.display {
  margin-bottom: 30px;
  display: flex;
  height: 25px;
  align-items: center;
}

.title {
  font-size: 25px;
  margin-right: 5px;
  text-transform: capitalize;
}

p {
  font-family: "Dancing Script", cursive;
}

.completed {
  text-decoration: line-through;
}

.md-checkbox {
  margin-right: 10px !important;
}

@media all and (max-width: 600px) {
  .container {
    width: 90%;
  }
}
</style>