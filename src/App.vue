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

        <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <br />
        <small>Total Todo : {{ totalTodo }}</small>
      </div>
    </div>

  </div>
</template>

<script>
import List from './components/ListComponent.vue'
import { computed, onMounted, reactive, ref, toRefs } from 'vue'

export default {
  components: { List },
  setup() {
    const todo = ref('')
    const todos = reactive({
      list: []
    })

    onMounted(() => {
      todos.list = JSON.parse(localStorage.getItem('todos') ?? '[]') // ambil data dari local storage, kalau data kosong kasih aja array kosong, sebagai string karena json berupa string
    })

    const totalTodo = computed(() => todos.list.length)

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      })

      todo.value = ''

      saveToLocalStorage()
    }

    const deleteTodo = todoIndex => {
      todos.list = todos.list.filter((todo, index) => {
        if (index != todoIndex) {
          return todo
        }
      })

      saveToLocalStorage()
    }

    const doneTodo = todoIndex => {
      todos.list = todos.list.filter((todo, index) => {
        if (index == todoIndex) {
          todo.isDone = true
        }
        return todo
      })

      saveToLocalStorage()
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('todos', JSON.stringify(todos.list))
    }

    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      add,
      deleteTodo,
      doneTodo
    }
  },
}

</script>
