<script setup lang="ts">
import { ref } from "vue";

// массив задач
const tasks = ref([
  "Сходить в кино",
  "Погулять с собакой",
  "Встретится с Шерлоком на Бэйкер Стрит"
]);

// функция удаления задачи
const onClickDeleteTask = (i) => {
  tasks.value.splice(i, 1);
};

// переменная для хранения значения инпута
const inputValue = ref("");

// функция добавления задачи
const onInputClick = () => {
  // выполнить код если инпут не пустой
  if (inputValue.value !== "") {
    // добавить задачу в массив
    tasks.value.push(inputValue.value);
    // очистить значение инпута
    inputValue.value = "";
  }
};
</script>

<template>
  <div class="app">
    <h1>To do list</h1>

    <div class="app__create-task">
      <input
          type="text"
          name="todo"
          placeholder="Напишите задачу.."
          v-model="inputValue"
      />
      <a @click.prevent="onInputClick">Добавить задачу</a>
    </div>

    <ul class="app__list">
      <li class="task" v-for="(task, i) in tasks" :key="i">
        <input type="checkbox" name="task" />
        <span>{{ task }}</span>

        <a @click.prevent="onClickDeleteTask(i)">Удалить задачу</a>
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped >
.app {
  padding: 20px;
  width: 500px;
  border: 2px solid red;
  border-radius: 20px;

  &__list {
    margin: 30px 0 0 20px;
    padding: 0;
    list-style: none;
  }

  &__create-task {
    display: flex;
    gap: 5px;
  }

  a {
    border: 1px solid #000;
    padding: 0 5px;
    border-radius: 3px;
    cursor: pointer;
  }
}

.task {
  display: flex;
  align-items: center;
  min-height: 30px;
  margin-bottom: 20px;

  &:last-child {
    margin-bottom: 0;
  }

  input:checked {
    ~ span {
      text-decoration: line-through;
    }

    ~ a {
      display: none;
    }
  }

  span {
    margin-right: auto;
  }
}
</style>
