<script setup lang="ts">
import { Form, Field, ErrorMessage, defineRule, configure } from "vee-validate";
import { required, email, min, numeric, max } from "@vee-validate/rules";
import { localize } from '@vee-validate/i18n';

// глобальные настройки валидации
configure({
  validateOnInput: true,
  generateMessage: localize("ru", {
    messages: {
      required: "Обязательное поле. ",
      email: "Невалидный email. ",
      numeric: "Поле должно содержать только цифры. ",
      min: "Минимум 10 цифр. ",
      max: "Максимум 30 символов. ",
      forbiddenSymbolFilter: "Использованы запрещённые символы. ",
    },
  }),
});

// глобальное объявление правил, в данном случае встроенные правила veeValidate
// правила для:
defineRule("required", required); // обязательных полей
defineRule("email", email); // email адресов
defineRule("numeric", numeric); // полей с числовыми значениями
defineRule("min", min); // полей с минимальным количеством символов
defineRule("max", max); // полей с максимальным количеством символов

// правило для фильтрации запрещённых символов
defineRule("forbiddenSymbolFilter", (value) => {
  const regular = /^[^%*#@]*$/; // создание регулярного выражения с запрещёнными значениями

  // выполнить код если значение поля ввода содержит запрещённые символы
  if (!regular.test(value)) {
    const forbiddenSymbols = value
      .split("") // разбитие значения поля на символы
      .filter((item) => !regular.test(item)); // извлечение всех запрещённых символов из поля ввода
    const symolsSet = Array.from(new Set(forbiddenSymbols)); // создание уникальной коллекции запрещённых символов
    return `Символы: ${symolsSet} запрещены`; // отображение невалидных символов пользователю
  } else {
    // если код не содержит запрещённых символов вернуть true
    return true;
  }
});

const showMessageLength = (value) => {
  return value ? value.length : 0;
};

// вывод в консоль данных формы
const onSubmit = (value) => {
  console.log(value);
};
</script>

<template>
  <div class="app">
    <h1>Elegant Contact Form.</h1>
    <div class="info">
      <a href="https://www.grandvincent-marion.fr" target="_blank">
        <p>
          Made with
          <i class="fa fa-heart"></i>
          by Marion Grandvincent
        </p>
      </a>
    </div>

    <Form @submit="onSubmit">
      <h1>
        Should you have any questions, please do not hesitate to contact me :
      </h1>

      <div class="contentform">
        <div id="sendmessage">
          Your message has been sent successfully. Thank you.
        </div>

        <div class="leftcontact">
          <div class="form-group">
            <p>
              Surname
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-male"></i></span>
            <Field
              name="prenom"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="prenom" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Name
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-user"></i></span>
            <Field
              name="nom"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="nom" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              E-mail
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-envelope-o"></i></span>
            <Field
              name="email"
              v-slot="{ field, meta }"
              :rules="{ required: true, email: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="email"
              />
              <ErrorMessage class="error" name="email" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Company
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-home"></i></span>
            <Field
              name="society"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="society" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Company Address
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-location-arrow"></i></span>
            <Field
              name="adresse"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="adresse" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Postcode
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-map-marker"></i></span>
            <Field
              name="postal"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="postal" />
            </Field>
          </div>
        </div>

        <div class="rightcontact">
          <div class="form-group">
            <p>
              City
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-building-o"></i></span>
            <Field
              name="ville"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="ville" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Phone number
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-phone"></i></span>
            <Field
              name="phone"
              v-slot="{ field, meta }"
              :rules="{ required: true, numeric: true, min: 10 }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="phone" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Function
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-info"></i></span>
            <Field
              name="fonction"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="fonction" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Subject
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-comment-o"></i></span>
            <Field
              name="sujet"
              v-slot="{ field, meta }"
              :rules="{ required: true }"
            >
              <input
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                type="text"
              />
              <ErrorMessage class="error" name="sujet" />
            </Field>
          </div>

          <div class="form-group">
            <p>
              Message
              <span>*</span>
            </p>
            <span class="icon-case"><i class="fa fa-comments-o"></i></span>
            <Field
              name="message"
              v-slot="{ field, meta }"
              :rules="{ required: true, max: 30, forbiddenSymbolFilter: true }"
            >
              <textarea
                :class="
                  !meta.dirty && !meta.validated
                    ? ''
                    : meta.valid
                    ? 'valid'
                    : 'invalid'
                "
                v-bind="field"
                rows="14"
              ></textarea>
              <div v-if="showMessageLength(field.value) > 30">
                {{ showMessageLength(field.value) }} / 30
              </div>
              <ErrorMessage class="error" name="message" />
            </Field>
          </div>
        </div>
      </div>
      <button type="submit" class="bouton-contact">Send</button>
    </Form>
  </div>
</template>

<style scoped>
.app {
  background: #eaeaea;
  font-family: "Open Sans", sans-serif;
}
textarea {
  resize: none;
}

.info p {
  text-align: center;
  color: #999;
  text-transform: none;
  font-weight: 600;
  font-size: 15px;
  margin-top: 2px;
}

.info i {
  color: #f6aa93;
}
form h1 {
  font-size: 18px;
  background: #f6aa93 none repeat scroll 0 0;
  color: rgb(255, 255, 255);
  padding: 22px 25px;
  border-radius: 5px 5px 0 0;
  margin: auto;
  text-shadow: none;
  text-align: left;
}

form {
  border-radius: 5px;
  max-width: 700px;
  width: 100%;
  margin: 5% auto;
  background-color: #ffffff;
  overflow: hidden;
}

p span {
  color: #f00;
}

p {
  margin: 0;
  font-weight: 500;
  line-height: 2;
  color: #333;
}

h1 {
  text-align: center;
  color: #666;
  text-shadow: 1px 1px 0 #fff;
  margin: 0;
  padding: 50px 0 0;
}

input {
  border-radius: 0 5px 5px 0;
  border: 1px solid #eee;
  margin-bottom: 15px;
  width: 75%;
  height: 40px;
  float: left;
  padding: 0 15px;
}

a {
  text-decoration: inherit;
}

textarea {
  border-radius: 0 5px 5px 0;
  border: 1px solid #eee;
  margin: 0;
  width: 75%;
  height: 130px;
  float: left;
  padding: 0 15px;
}

.form-group {
  overflow: hidden;
  clear: both;
}

.icon-case {
  width: 35px;
  float: left;
  border-radius: 5px 0 0 5px;
  background: #eeeeee;
  height: 42px;
  position: relative;
  text-align: center;
  line-height: 40px;
}

i {
  color: #555;
}

.contentform {
  padding: 40px 30px;
}

.bouton-contact {
  background-color: #81bda4;
  color: #fff;
  text-align: center;
  width: 100%;
  border: 0;
  padding: 17px 25px;
  border-radius: 0 0 5px 5px;
  cursor: pointer;
  margin-top: 40px;
  font-size: 18px;
}

.leftcontact {
  width: 49.5%;
  float: left;
  border-right: 1px dotted #ccc;
  box-sizing: border-box;
  padding: 0 15px 0 0;
}

.rightcontact {
  width: 49.5%;
  float: right;
  box-sizing: border-box;
  padding: 0 0 0 15px;
}

.validation {
  display: none;
  margin: 0 0 10px;
  font-weight: 400;
  font-size: 13px;
  color: #de5959;
}

.show {
  display: block;
}

.valid {
  background: #00800050;
}

.invalid {
  background: #ff000050;
}

.error {
  color: #ff0000;
}
</style>
