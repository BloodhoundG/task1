<script setup lang="ts">
import {
  onBeforeMount,
  onMounted,
  onBeforeUnmount,
  onUnmounted,
} from "vue";

    onBeforeMount(() =>
        console.log(
            "Компонент MyComponent: onBeforeMount отработал ДО монтирования в DOM"
        )
    );
    onMounted(() =>
        console.log(
            "Компонент MyComponent: onMounted отработал ПОСЛЕ монтирования в DOM"
        )
    );
    onBeforeUnmount(() =>
        console.log(
            "Компонент MyComponent: onBeforeUnmounted отработал ДО размонтирования"
        )
    );
    onUnmounted(() =>
        console.log(
            "Компонент MyComponent: onUnmounted отработал ПОСЛЕ размонтирования"
        )
    );
</script>

<template>
<div>Lifecycle-first-component. Нажми чтобы сгенерировать ошибку</div>
</template>

<style scoped>

</style>
