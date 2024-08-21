<script setup lang="ts">
import {
  ref,
  onBeforeUpdate,
  onUpdated,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered,
} from "vue";

const isShow = ref(false); // переменная-индикатор показа/скрытия первого компонента
const isShowSecond = ref(false); // переменная-индикатор показа/скрытия второго компонента
let anyData = ref(0); // реактивная переменная для счётчика

// функция для генерации ошибки
const onClickError = () => {
  throw new Error("Генерирую ошибку");
};

onBeforeUpdate(() =>
    console.log(
        "Корневой компонент App: onBeforeUpdate отработал ДО обновления реактивных данных компонента"
    )
);

onUpdated(() =>
    console.log(
        "Корневой компонент APP: onUpdate отработал ПОСЛЕ обновления реактивных данных компонента"
    )
);

// функция отработает при возникновении в компоненте ошибки, нужно это для возможности остановить её распространение вверх
onErrorCaptured((error) =>
    console.log(
        "Компонент MyComponent: onErrorCaptured отработал когда в компоненте случилась ошибка",
        error
    )
);

// функция отслеживающая реактивные данные компонента
onRenderTracked((e) => {
  console.log("Корневой компонент APP: onRenderTracked", e);
  // в объекте нету информации о том какая именно переменная отслеживается, поэтому нужно использовать дебагер
  // debugger;
});

// функция отслеживающая изменение реактивных данных компонента
onRenderTriggered((e) => {
  console.log("Корневой компонент APP: onRenderTriggered", e);
  // в объекте нету информации о том какая именно переменная была изменена, поэтому нужно использовать дебагер
  // в объекте можно посмотреть старое значение и новое
  // debugger;
});
</script>

<template>
  <div>
    <!-- установка обработчика клика который меняет булево значение isShow при каждом клике -->
    <a @click="isShow = !isShow">
      Смонтировать/размонтировать первый компонент
    </a>
    <!-- условный рендеринг компонента в зависимости от isShow -->
    <!-- при клике на компонент отработает жизненный цикл onErrorCaptured -->
    <LifecycleFirstComponent class="component" v-if="isShow" @click="onClickError" />
    <!-- при клике на кнопку увеличивает anyData на единицу -->
    <a @click="anyData += 1">
      При изменении переменной данные обновятся: {{ anyData }}
    </a>
    <!-- установка обработчика клика который меняет булево значение при каждом клике -->
    <a @click="isShowSecond = !isShowSecond">
      показать/спрятать второй компонент
    </a>
    <!-- в от личии от первого компонента этот компонент при скрытии кешируется -->
    <!-- т.е. он смонтируется только единожды и далее не будет монтироваться/размонтироваться -->
    <KeepAlive>
      <LifecycleSecondComponent v-if="isShowSecond" class="component" />
    </KeepAlive>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

a {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 50px;
  background: #1a630f;
  color: #fff;
  border: 2px solid #000;
  cursor: pointer;
}

.component {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 50px;
  background: #235da6;
  color: #fff;
  cursor: pointer;
}
</style>
