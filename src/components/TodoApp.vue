<template>
  <div class="TodoApp">
    <h1>todo app</h1>
    <div>
      <input v-model="title" @keyup.enter="addTodo(title)" />
      <button @click="addTodo(title)">add todo</button>
    </div>
    <div>
      <ul>
        <li v-for="todo in fixedTodos" :key="todo.id">
          <!-- 是否已完成 -->
          <input type="checkbox" v-model="todo.completed" />
          <input v-if="todo === currentEditTodo" v-model="oldEditTodo.title" />
          <span v-else>{{ todo.id }} | {{ todo.title }}</span>

          <template v-if="todo === currentEditTodo">
            <button @click="cancelEditTodo()">cancel</button>
            <button @click="confirmEditTodo()">confirm</button>
          </template>
          <button v-else @click="startEditTodo(todo)">edit</button>

          <button @click="deleteTodo(todo)">delete</button>
        </li>
      </ul>
      <div>
        <button @click="completeAll()">complete all</button>
        <button @click="clearAllComplete()">clearAllComplete</button>
        <button @click="show('all')">all</button>
        <button @click="show('completed')">complete</button>
        <button @click="show('uncompleted')">un complete</button>
      </div>
    </div>
    <div>
      <pre>{{ todos }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  data() {
    return {
      title: '',
      currentEditTodo: null,
      oldEditTodo: null,
      todos: [
        {
          id: 1,
          title: 'learn html',
          completed: false,
        },
      ],
      filterType: 'all', // all | completed | uncompleted
    };
  },
  computed: {
    fixedTodos () {
      return this.todos.filter(item => {
        if (this.filterType === 'all') {
          return true
        } else if (this.filterType === 'completed') {
          return item.completed
        } else {
          return !item.completed
        }
      })
    },
  },
  methods: {
    addTodo(title) {
      if (title.trim()) {
        const maxId = Math.max(...this.todos.map((todo) => todo.id), 0);
        this.todos.push({
          id: maxId + 1,
          title: title.trim(),
          completed: false,
        });
        this.title = '';
      }
    },
    deleteTodo(todo) {
      const index = this.todos.findIndex((item) => item === todo);
      this.todos.splice(index, 1);
    },
    startEditTodo(todo) {
      this.currentEditTodo = todo;
      this.oldEditTodo = { ...todo };
    },
    cancelEditTodo() {
      this.currentEditTodo = null;
      this.oldEditTodo = null;
    },
    confirmEditTodo() {
      this.currentEditTodo = null;
      this.todos = this.todos.map((todo) => {
        if (todo.id === this.oldEditTodo.id) {
          return { ...this.oldEditTodo };
        } else {
          return todo;
        }
      });
    },
    completeAll() {
      this.todos = this.todos.map((item) => {
        item.completed = true;
        return item;
      });
    },
    clearAllComplete() {
      this.todos = this.todos.filter((item) => !item.completed);
    },
    show (type) {
      this.filterType = type
    },
  },
};
</script>

<style scoped></style>
