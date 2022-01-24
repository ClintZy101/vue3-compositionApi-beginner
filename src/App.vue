<template>
  <h1>Vue 3 Todo App</h1>
  <!-- @submit.prevent === preventDefault() -->
  <form action="" @submit.prevent="addNewTodo">
    <label for="">New Todo</label>
    <input type="text" v-model="newTodo" name="newTodo" />
    <button>Add New todo</button>
  </form>

  <button @click="markAllDone">Mark All Done</button>
  <button @click="removeAllTodos">Remove All Todos</button>

  <h1>Todos:</h1>
  <ul>
    <!-- in v-for you need to pass in the arguments (todo, index) in order to access it in button or child components-->
    <li v-for="(todo, index) in todos" :key="todo.id">
      <h3 :class="{ done: todo.done }"  @click="toggleDone(todo)" >{{ todo.content }}</h3>
      <button @click="removeTodo(index)">Remove Todo</button>
    </li>
  </ul>

</template>

<script>
import { ref } from "vue";
import { v4 as uuidv4 } from "uuid";

export default {
  // composition api (for vue3) vs options api (for vue2)
  setup() {
    const newTodo = ref("");
    const todos = ref([]);

    //sample: some unique id ==> Math.floor(Math.random() * 100)

    function addNewTodo() {
      //  console.log(newTodo.value)
      todos.value.push({
        id: uuidv4(),
        done: false,
        content: newTodo.value,
      });
      // clear input after submission of input value/data
      newTodo.value = '';
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(index) {
      todos.value.splice(index, 1)
    }
// for convenience sake when user wants to unmark all again, you just use the !todo.done instead of true. Just find a way to change the text: Mark all done to 'Unmark All'. Perhaps this conditional i achieved through vuex...

    function markAllDone() {
      todos.value.forEach(todo => todo.done = !todo.done);
    }

// just be careful of this function cause it might be an inconvenience to users when accidentally clicked. Put yourself in the users' shoes
    function removeAllTodos() {
      todos.value = [];
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAllTodos
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  display: grid;
  column-gap: 1rem;
}
form > input {
  margin: 10px auto;
}
form > button {
  width: max-content;
  margin: auto;
}

ul >li {
  width: 100px;
  /* border: solid; */
  cursor: pointer;
  margin: auto
}
.done {
  text-decoration: line-through;
}
</style>
