<template>
  <div class="app-wrapper">
    <NavBar />
    <div class="inner-wrapper">
 <div class="container mt-5">
    <form @submit.prevent="addTodos()"> 
      <div class="row ">
        <div class="col-md-6">
          <input
            
            type="text"
            v-model="newTodo"
            class="form-control m-0"
            placeholder="what do you want to do ?"
          />
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-success">Add TODO</button>
        </div>
      </div>
    </form>
       <div id="todoList">    
      <div
        :class="{ completed: todo.complete }"
        v-for="(todo, index) in todos"
        :key="index"
        @click="completedTodo(todo)"
         
      >
        <div class="row mt-4">
          <button class="btn btn-success col-md-6">{{ todo.text }}</button>
  
      
        </div>
      </div>
    </div>
    <p v-if="todos.length === 0" class="mt-3">No Task</p>
    <button
      class="btn btn-danger mt-4"
      v-if="todos.length !== 0"
      @click="removeTodos"
    >
      Remove All Todos
    </button>
    </div>
    </div>
   
  </div>
</template>

<script>
import NavBar from "./components/NavBar.vue"
import { ref } from "vue";

export default {
  components: {
    NavBar
  },
  name: "App",
  setup() {
    const newTodo = ref("");
    const initialLoadData = [
      {
        complete: false,
        text: "Create todo list functionality",
      },
    ];
    let stroedTodos;
    localStorage.getItem("todos")
      ? (stroedTodos = JSON.parse(localStorage.getItem("todos")))
      : (stroedTodos = initialLoadData);
    const todos = ref(stroedTodos);
    function addTodos() {
      if (newTodo.value !== "") {
        todos.value.push({
          complete: false,
          text: newTodo.value,
        });
        newTodo.value = "";
        updateStorage();
      }
    }
    function removeTodos() {
      todos.value.splice(0, todos.value.length);
      updateStorage();
    }
    function completedTodo(todo) {
      todo.complete = !todo.complete;
      updateStorage();
    }
    function updateStorage() {
      localStorage.setItem("todos", JSON.stringify(todos.value));
    }
    return {
      completedTodo,
      removeTodos,
      addTodos,
      todos,
      newTodo,
    };
  },
};
</script>
<style>
.inner-wrapper {
  display: grid;
  place-items: center;
}
.card-content:hover {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 2);
}
#todoList div.completed {
  opacity: 0.5;
}
#appContainer {
  padding: 2%;
}
</style>
