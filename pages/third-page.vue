<script setup lang="ts">
import { ref } from "vue";

const onClickCloseTask = (i) => {
  commentsArray.value = commentsArray.value.filter((item) => item.id !== i);
};

let commentsArray = ref([]);

// функция запроса на сервер
const fetchComments = async () => {
  // попытка выполнить код
  try {
    // производится ассинхронный запрос
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/posts/1/comments"
    );
    // выполнить код если результат запроса положительный
    if (response.ok) {
      commentsArray.value = await response.json();
      // выполнить код если результат запроса отрицательный
    } else {
      throw new Error("Ошибка запроса");
    }
    // если в коде происходит ошибка выполняется блок catch
  } catch (error) {
    console.log(error);
  }
};

fetchComments();
</script>

<template>
  <div class="app">
    <div class="comments">
      <ul class="comments__list">
        <!-- передаю пропсы внутрь дочернего компонента -->
        <CommentPost
          v-for="o in commentsArray"
          :key="o.id"
          :post="o"
          @on-close-event="onClickCloseTask"
        />
      </ul>
    </div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}

.comments__list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

p {
  margin: 0 0 10px;
}

.comment {
  position: relative;
  padding: 15px 70px 15px 15px;
  background-color: #0f5405;
  color: #fff;
  border-radius: 30px;
}

.comment__button {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  right: 15px;
  top: 15px;
  background: #000;
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  color: #fff;
  cursor: pointer;
}
</style>
