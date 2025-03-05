<script setup>
import { TrashIcon } from "@heroicons/vue/24/outline";

const props = defineProps({
  post: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["delete"]);

function emitDelete() {
  emit("delete", props.post.id);
}
</script>

<template>
  <article class="card">
    <header>
      <img
        :src="post.author.avatarURL"
        :alt="post.author.username"
        width="36"
        height="36"
        class="avatar"
      />
      <!--RENDU DECLARATIF-->
      <a>{{ post.author.username }}</a>
    </header>
    <!--RENDU DECLARATIF-->
    <p>{{ post.content }}</p>

    <footer>
      <button class="btn-icon" @click="emitDelete">
        <TrashIcon />
      </button>
    </footer>
  </article>
</template>
