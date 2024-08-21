<script setup lang="ts">
import { Form, Field, ErrorMessage, defineRule, configure } from "vee-validate";

const getClass = (meta) => {
  if (!meta.dirty && !meta.validated) {
    return "";
  } else if (meta.valid) {
    return "valid";
  } else {
    return "invalid";
  }
};

// при форма валидна в консоль выведутся данные заполненной формы
const onSubmit = (value) => {
  console.log(value);
};

// глобальная установка параметра валидации текущего поля при каждом его изменении
configure({ validateOnInput: true });

// глобально определяю правило для обязательных полей
defineRule("required", (value) => {
  if (!value || !value.length) {
    return "Обязательное поле";
  }
  return true;
});

// глобально определяю правило для поля с номером телефона
defineRule("phone", (value) => {
  const regular = /^(\+?7|8)(\d{3}|\(\d{3}\))[-]?\d{3}[-]?\d{2}[-]?\d{2}$/;
  return !regular.test(value) ? "Неправильный номер телефона" : true;
});

// глобально определяю правило для email
defineRule("email", (value) => {
  const regular =
    /^((([0-9A-Za-z]{1}[-0-9A-z\.]{1,}[0-9A-Za-z]{1})|([0-9А-Яа-я]{1}[-0-9А-я\.]{1,}[0-9А-Яа-я]{1}))@([-A-Za-z]{1,}\.){1,2}[-A-Za-z]{2,})$/;
  return !regular.test(value) ? "Неправильный e-mail" : true;
});

// глобально определяю правило для чекбокса
defineRule("agree", (value) => {
  if (!value) {
    return "Чтобы продолжить необходимо дать согласие";
  }
  return true;
});
</script>

<template>
  <div class="app">
    <Form class="form" @submit="onSubmit">
      <Field name="name" v-slot="{ field, meta }" :rules="'required'">
        <input
          :class="{ [getClass(meta)]: true }"
          v-bind="field"
          type="text"
          placeholder="ФИО"
        />
        <ErrorMessage class="color-red" name="name" />
      </Field>

      <Field name="phone" v-slot="{ field, meta }" :rules="'required|phone'">
        <input
          :class="{ [getClass(meta)]: true }"
          v-bind="field"
          type="tel"
          placeholder="Телефон"
        />
        <ErrorMessage class="color-red" name="phone" />
      </Field>

      <Field name="email" v-slot="{ field, meta }" :rules="'required|email'">
        <input
          :class="{ [getClass(meta)]: true }"
          v-bind="field"
          type="email"
          placeholder="Email"
        />

        <ErrorMessage class="color-red" name="email" />
      </Field>

      <Field name="note" v-slot="{ field }">
        <input type="text" v-bind="field" placeholder="Примечание" />

        <!-- тут сообщение ошибки не нужно так как поле не обязательное -->
        <!-- <ErrorMessage class="color-red" name="note" /> -->
      </Field>

      <div class="checkbox-wrapper">
        <Field
          name="agreement"
          type="checkbox"
          value="true"
          :rules="'agree|required'"
        />
        Пользовательское соглашение
      </div>

      <ErrorMessage class="color-red" name="agreement" />

      <button type="submit">Заполнить</button>
    </Form>
  </div>
</template>

<style scoped>
.app {
  padding: 20px;
}

input {
  height: 43px;
  border: 1px solid #afafaf;
  margin: 0;
  padding: 0 15px;
}

button {
  min-height: 45px;
  min-width: 135px;
  width: fit-content;
  border-radius: 30px;
  border: none;
  color: #fff;
  background: #ff5a7d;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.checkbox-wrapper {
  display: flex;
  align-items: center;
  gap: 15px;
  text-decoration: underline;
}

.valid {
  background: #00800050;
}

.invalid {
  background: #ff000050;
}

.color-red {
  color: #ff0000;
}
</style>
