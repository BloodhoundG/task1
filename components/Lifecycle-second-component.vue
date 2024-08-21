<script setup lang="ts">
import {onActivated, onDeactivated, onMounted, onUnmounted} from "vue";


    onMounted(() =>
        console.log(
            "Компонент MySecondComponent: onMounted отработает единожды при загрузке страницы"
        )
    );

    // данный код никогда не отрабатывает, так как компонент не размонтируется, а кэшируется
    onUnmounted(() => console.log("Меня никогда не покажут в консоли :( "));

    onActivated(() =>
        console.log(
            "Компонент MySecondComponent: onActivated отработает после вставки компонента в DOM как закэшированного"
        )
    );
    onDeactivated(() =>
        console.log(
            "Компонент MySecondComponent: onDeactivated отработал после того как компонента был деактивирован"
        )
    );
</script>

<template>
<div>Lifecycle-second-component. Мой второй компонент</div>
</template>

<style scoped>

</style>
