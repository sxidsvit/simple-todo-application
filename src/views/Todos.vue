<template>
  <div>
    <router-link to="/">Вернуться на главную страницу</router-link>
    <hr />
    <AddTodo @add-todo="addTodo" />
    <select v-model="filter">
      <option value="all" class="text-primary">Все задачи</option>
      <option value="completed" class="text-success">Выполненные задачи</option>
      <option value="not-completed" class="text-danger">Не выполненные задачи</option>
    </select>
    <hr class="mt-5" />
    <Loader v-if="loading" />
    <TodoList
      v-else-if="filteredTodos.length"
      v-bind:todos="filteredTodos"
      @remove-todo="removeTodo"
    />
    <p v-else>Нет задач!</p>
  </div>
</template>

<script>
import AddTodo from "@/components/AddTodo";
import Loader from "@/components/Loader";
import TodoList from "@/components/TodoList";
export default {
  name: "app",
  data() {
    return {
      todos: [],
      loading: true,
      filter: "all"
    };
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=3")
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {
          this.todos = json;
          this.loading = false;
        }, 1000);
      });
  },
  // watch: {
  //   filter(value) {
  //     console.log(value)
  //   }
  // },
  computed: {
    filteredTodos() {
      if (this.filter === "all") {
        return this.todos;
      }

      if (this.filter === "completed") {
        return this.todos.filter(t => t.completed);
      }

      if (this.filter === "not-completed") {
        return this.todos.filter(t => !t.completed);
      }
    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    }
  },
  components: {
    TodoList,
    AddTodo,
    Loader
  }
};
</script>