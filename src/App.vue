<template>
  <div class="container todo-app">
      <h1 class="title">Todo List</h1>

      <div class="todo-app__main">
        <ul class="todo-list">
          <li :class="{'todo-list__item': true, 'todo-list__item--completed': todo.completed}" v-for="todo in todos" :key="todo.id">
            <ToDoListItem :task="todo" @completeTask="todo.completed = true" @removeTask="removeTodo(todo.id)"/>
          </li>
        </ul>
        <div class="todo-list__empty" v-if="!todos.length">
          <p>Список задач пуст</p>
        </div>
      </div>

      <div class="todo-app__footer" v-show="todos.length > 0">
        <p class="todo-app__footer-text">Осталось {{ unfullfiledTasks }} задания(й)</p>
        <button class="btn btn--clear" @click="clearCompleted">Удалить завершенные</button>
        <button class="btn btn--clear" @click="clearAll">Очистить список</button>
      </div>
    </div>
</template>
<script setup>
import { computed, reactive} from 'vue';
import ToDoListItem from './components/ToDoListItem.vue';


  const todos = reactive(
  [
    {
      id: 1,
      text: "Пример задачи",
      completed: false
    },
    {
      id: 2,
      text: "Изучить компоненты Vue.js",
      completed: false
    },
    {
      id: 3,
      text: "Создать TodoList приложение",
      completed: false
    },
    {
      id: 4,
      text: "Похвалить себя за отличную работу",
      completed: false
    },
  ])

  function removeTodo(id) {
    let idx = null;
    todos.forEach((todo, index) => {
      if (todo.id === id) {
        idx = index;
      }
    })
    todos.splice(idx, 1)
  }

  const unfullfiledTasks = computed(()=> {
    const unfullfiledTasks = todos.filter(todo => !todo.completed);
    return unfullfiledTasks.length;
  })

  function clearCompleted() {
    const arr = todos.filter(todo => !todo.completed);
    todos.splice(0);
    todos.push(...arr);
  }

  function clearAll() {
    todos.splice(0);
  }

</script>
<style src="./App.css"></style>
