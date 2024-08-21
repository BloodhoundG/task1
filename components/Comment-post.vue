<script setup lang="ts">
const props = defineProps({
  post: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['onCloseEvent']);
</script>

<template>
  <li class="comment">
    <p>ID: {{ props.post.id }}</p>
    <p>Post ID: {{ props.post.postId }}</p>
    <p>Name: {{ props.post.name }}</p>
    <p>Email: {{ props.post.email }}</p>
    <p>Body: {{ props.post.body }}</p>
    <p
        class="comment__button"
        @click="emit('onCloseEvent', props.post.id)"
    >
      X
    </p>
  </li>
</template>

<style>

</style>
