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

/*function addPost() {
  const newPost = {
    id: Math.random().toString(36).substring(2), //UUID
    content: trimmedText.value,
    createdAt: new Date(),
    liked: false,
    author: {
      username: "404azrael",
      avatarUrl: "https://media1.tenor.com/m/NUzXmsZeSy8AAAAd/meme-edit.gif", //https://avatars.githubusercontent.com/u/130103824?v=4
    },
  };
  posts.value.push(newPost);
  text.value = "";
}*/

function addPost() {
  const userData = JSON.parse(localStorage.getItem("user"));
  const token = userData.token;
  fetch("https://posts-crud-api.vercel.app/posts", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify({ content: trimmedText.value }),
  })
    .then((response) => response.json())
    .then((data) => {
      apiPosts.value.unshift({
        id: data.id,
        content: data.content,
        createdAt: data.createdAt,
        author: {
          id: userData.user.id,
          username: userData.user.username,
          avatarUrl: userData.user.avatarUrl,
        },
      });
      text.value = "";
    });
}

function deletePost(postId) {
  posts.value = posts.value.filter((post) => post.id !== postId); //ne garde que les posts qui ne sont pas celui sélectionné
}

function likePost(postId) {
  let liked = posts.value.find((post) => post.id == postId).liked;
  posts.value.find((post) => post.id == postId).liked = !liked;

  /*const postToUpdate = posts.value.find((post) => post.id == id);
  postToUpdate.liked = !postToUpdate.liked*/
}

const apiPosts = ref([]);
const loading = ref(false);
function fetchPosts() {
  loading.value = true;
  const result = fetch("https://posts-crud-api.vercel.app/posts");
  result
    .then((response) => response.json())
    .then((data) => {
      apiPosts.value = data;
      loading.value = false;
    });
}
fetchPosts();
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

      <h2 v-if="loading">Chargement...</h2>
      <h2 v-else-if="!apiPosts.length">Il n'y a rien par ici...</h2>

      <PostCard
        v-for="(post, index) in apiPosts"
        :key="index"
        :post="post"
        @delete="deletePost"
        @like="likePost"
      />
    </div>
  </main>
</template>
