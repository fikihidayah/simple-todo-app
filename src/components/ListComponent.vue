<template>
  <div class="mt-3">
    <ul class="list-group">
      <li v-for="(todo, index) in todos" :key="index" class="list-group-item">
        <div class="row">
          <div class="col">
            <!-- jika todonya sudah selesai maka tulisan akan digaris -->
            <span v-if="todo.isDone">
              <del>{{ todo.activity }}</del>
            </span>
            <span v-else>
              {{ todo.activity }}
            </span>
          </div>
          <div class="col-auto">
            <div class="row gx-2">
              <div class="col">
                <button @click="doneTodo(index)" class="btn btn-info">Done</button>
              </div>
              <div class="col">
                <button @click="deleteTodo(index)" class="btn btn-danger">X</button>
              </div>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  props: {
    todos: {
      type: Array,
      default: () => [],
    }
  },
  methods: {
    deleteTodo(index) {
      this.$emit('deleteTodo', index)
    },
    doneTodo(index) {
      this.$emit('doneTodo', index)
    }
  }
}
</script>