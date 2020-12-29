<template>
  <div id="app">
    <Header :siteName="siteName" />
    <div class="container">
      <TodoList 
        :todos="todos" 
        @delete-todo="deleteTodo" 
        @edit-todo="editTodo" 
        @save-todo="saveTodo"
        @cancel-save="cancelSave"
        @done-todo="doneTodo" />
      <AddTodo @add-todo="addTodo" />
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import TodoList from './components/TodoList.vue'
import AddTodo from './components/AddTodo.vue'

export default {
  name: 'App',
  components: {
    Header,
    TodoList,
    AddTodo
  },
  data() {
    return {
      siteName: 'Список моих задач',
      todos: [
        { title: 'Ремонт', descr: 'описание ремонта', done: false, isEditMode: false },
        { title: 'Зарядка', descr: 'чтоб не болела спина', done: true, isEditMode: false },
        { title: 'Заказать воду', descr: 'позвонить курьеру утром', done: false, isEditMode: false },
        { title: 'Купить подарок', descr: 'поискать что подарить родителям', done: false, isEditMode: false }
      ]
    }
  },
  methods: {
    deleteTodo(index) {
      this.todos.splice(index,1)
    },
    editTodo(index) {
      let editedElementIndex = this.todos.findIndex(item => item.isEditMode);
      if (editedElementIndex != -1) {
        this.cancelSave(editedElementIndex);
      }
      this.todos[index].isEditMode = true;
      this.$children[1].updatedTitle = this.todos[index].title;
      this.$children[1].updatedDescr = this.todos[index].descr;
    },
    saveTodo(index, updatedTitle, updatedDescr) {
      this.todos[index].isEditMode = false;
      this.todos[index].title = updatedTitle;
      this.todos[index].descr = updatedDescr;
    },
    doneTodo(index, isDone) {
      this.todos[index].done = isDone;
    },
    cancelSave(index) {
      this.todos[index].isEditMode = false;
    },
    addTodo(todo) {
      this.todos.push(todo)
    }
  }
}
</script>

<style lang="scss">
@mixin box-sizing($property) {
    -webkit-box-sizing: $property;
    -ms-box-sizing: $property;
    box-sizing: $property;
}

* {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 14px;
}
.container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
input {
  @include box-sizing(border-box);
  width: 100%;
  height: 30px;
}
textarea {
  @include box-sizing(border-box);
  width: 100%;
  height: 50px;
}
.error-common {
  margin: 5px 0;
  padding: 5px;
  background-color: rgba(255,0,0, 0.1);
}
</style>
