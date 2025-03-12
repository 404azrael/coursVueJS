<script setup>
import { TrashIcon, HeartIcon } from "@heroicons/vue/24/outline";
import { HeartIcon as iconLiked } from "@heroicons/vue/24/solid";
import { useRouter } from "vue-router";

const router = useRouter();

const props = defineProps({
  post: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["delete", "like"]);

function emitDelete() {
  emit("delete", props.post.id);
}

function emitLike() {
  emit("like", props.post.id);
}

function goToUserProfile(){
  router.push({name: "user", params: {username: props.post.author.username}})
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
      <a @click="goToUserProfile">{{ post.author.username }}</a>
    </header>
    <!--RENDU DECLARATIF-->
    <p>{{ post.content }}</p>

    <footer>
      <button class="btn-icon" @click="emitLike">
        <!--<HeartIcon v-if="!post.liked" />
        <iconLiked v-else class="active"/>-->
        <component :is="post.liked ? iconLiked : HeartIcon" :class="{active: post.liked}"/>
      </button>
      <button class="btn-icon" @click="emitDelete">
        <TrashIcon />
      </button>
    </footer>
  </article>
</template>
