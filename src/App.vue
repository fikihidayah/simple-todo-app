<template>
  <div class="container mt-5">

    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-md-10">
            <input type="text" v-model="todo" class="form-control" @keyup.enter="add">
          </div>
          <div class="col-md-2">
            <button class="btn btn-success w-100" @click="add">ADD</button>
          </div>
        </div>

        <list :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <br />
        <small>Total Todo : {{ totalTodo }}</small>
      </div>
    </div>

  </div>
</template>

<script>
import List from './components/ListComponent.vue'

export default {
  components: { List },
  data() {
    return {
      todo: '',
      todos: [],
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todos')) // ambil data dari local storage
  },
  computed: {
    totalTodo: function () {
      return this.todos.length;
    }
  },
  methods: {
    add() {
      this.todos.unshift({
        activity: this.todo,
        isDone: false,
      })

      this.todo = ''
      this.saveToLocalStorage()
    },
    deleteTodo(todoIndex) {
      this.todos = this.todos.filter((todo, index) => {
        if (index != todoIndex) {
          return todo
        }
      })

      this.saveToLocalStorage()
    },
    doneTodo(todoIndex) {
      this.todos = this.todos.filter((todo, index) => {
        if (index == todoIndex) {
          todo.isDone = true
        }
      })

      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
}

</script>
