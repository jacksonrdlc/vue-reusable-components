<template>
  <div id="app">
    <img src="./assets/logo.png">
    <p>
      {{ selectedFruit }}
    </p>
    <p>
      <dropdown :options='fruitOptions' v-model='selectedFruit'></dropdown>
    </p>
    <p>
      <autocomplete-dropdown :options="fruitOptions" v-model="selectedFruit"></autocomplete-dropdown>
    </p>
    <todo-list v-bind:todos="todos"></todo-list>
    <create-todo v-on:create-todo="addTodo"></create-todo>
  </div>
</template>

<script>
  import TodoList from "./components/TodoList";
  import CreateTodo from "./components/CreateTodo";
  import Dropdown from "./components/Dropdown";
  import AutocompleteDropdown from "./components/AutocompleteDropdown";
  
  export default {
    name: "App",
    components: {
      TodoList,
      CreateTodo,
      Dropdown,
      'autocomplete-dropdown': AutocompleteDropdown,
    },
    // data function avails data to the template
    data() {
      return {
        todos: [{
            title: "Todo A",
            project: "Project A",
            done: false
          },
          {
            title: "Todo B",
            project: "Project B",
            done: true
          },
          {
            title: "Todo C",
            project: "Project C",
            done: false
          },
          {
            title: "Todo D",
            project: "Project D",
            done: false
          }
        ],
        selectedFruit: "Banana",
        fruitOptions: {
          Apple: "Apple",
          Banana: "Banana",
          Blueberry: "Blueberry",
          Kiwi: "Kiwi",
          Pear: "Pear",
          Pineapple: "Pineapple",
          Watermelon: "Watermelon"
        }
      };
    },
    methods: {
      addTodo(card) {
        let nc = card.data
        this.todos.push(nc);
      }
    }
  };
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
