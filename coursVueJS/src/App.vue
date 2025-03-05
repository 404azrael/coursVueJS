<script setup>
import { ref, computed } from "vue";
import PostCard from "@/components/PostCard.vue";

const text = ref("");
const trimmedText = computed(() => text.value.trim()); // à chaque fois que "text" est modifiée, la fonction fléchée ()=>text.value.trim() va être utilisée

// function updateText(event) {
//   text.value = event.target.value;
// }

const posts = ref([]);
const sortedPosts = computed(() =>
  [...posts.value].sort((post1, post2) => post2.createdAt - post1.createdAt),
);

function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2), //UUID
    content: trimmedText.value,
    createdAt: new Date(),
    author: {
      username: "404azrael",
      avatarURL: "https://media1.tenor.com/m/NUzXmsZeSy8AAAAd/meme-edit.gif", //https://avatars.githubusercontent.com/u/130103824?v=4
    },
  };
  posts.value.push(newPost);
  text.value = "";
}

function deletePost(postId) {
  posts.value = posts.value.filter((post) => post.id !== postId); //ne garde que les posts qui ne sont pas celui sélectionné
}
</script>

<template>
  <!--<h1>£( ° [______] ° )£</h1>-->
  <main>
    <div class="container">
      <form class="card" @submit.prevent="addPost">
        <textarea name="post" id="post" placeholder="Quoi de neuf ?" v-model="text"></textarea>
        <!--DIRECTIVES avec leur équivalent en "sucre syntaxique"
          - v-bind:{paramHTML} = lie le paramètre à une variable ou constante Javascript    (ex : v-bind:value)   =>  :
          - v-on:{typeEvent} = écoute un événement sur une balise HTML    (ex : @input, @click)  =>  @
          - v-model = binding bi-latéral (v-bind et v-on en même temps)
          - v-for = permet de faire des foreach et des for
        -->
        <button type="submit" :disabled="!trimmedText">Poster</button>
      </form>

      <h2 v-show="!posts.length">Il n'y a rien par ici...</h2>

      <PostCard
        v-for="(post, index) in sortedPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
      />
    </div>
  </main>
</template>
