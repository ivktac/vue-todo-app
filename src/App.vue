<template>
  <div class="container">
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id" class="todo">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.title }}</span>
        <button @click="editTodo(todo.id)"> <i class="fas fa-edit"></i> </button>
        <button @click="removeTodo(todo.id)"> <i class="fas fa-trash"></i> </button>
      </li>
      <label class="todo" for="todo">
        <input type="text" id="todo" v-model="todo" @keydown.enter="addTodo" />
        <button @click="addTodo">Add</button>
      </label>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Todo } from "./types";

const localTodos = localStorage.getItem("todos") ? JSON.parse(localStorage.getItem("todos")!) : [
  {
    id: 1,
    title: "Learn Vue 3",
    done: false,
  }
];

const todos = ref<Todo[]>(localTodos);

const todo = ref("");

const addTodo = () => {
  todos.value.push({
    id: todos.value.length + 1,
    title: todo.value,
    done: false,
  });

  todo.value = "";

  save()
};

const removeTodo = (id: number) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);

  save()
};

const editTodo = (id: number) => {
  const index = todos.value.findIndex((todo) => todo.id === id);
  const oldTitle = todos.value[index].title;
  todos.value[index].title = prompt("Edit todo", oldTitle) ?? oldTitle;

  save()
};

const save = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 15px;
}

.todos {
  list-style: none;
  padding: 0;
}

.todo {
  display: flex;
  align-items: center;
  gap: 15px;
  justify-content: flex-start;
  font-size: 2rem;
  margin-bottom: 20px;
}

.todo input[type="checkbox"] {
  width: 30px;
  height: 30px;
}

.todo input {
  font-size: 2rem;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: #fff;
}

.todo button {
  font-size: 2rem;
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  background-color: #fff;
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}
</style>
