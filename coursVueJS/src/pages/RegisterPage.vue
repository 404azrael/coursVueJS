<!-- RegisterPage.vue -->
<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const username = ref("");
const email = ref("");
const password = ref("");

const isFormValid = computed(() => {
  return !!username.value.trim() && !!email.value.trim() && !!password.value.trim();
});

const isLoading = ref(false);

function register() {
  isLoading.value = true;
  console.log({
    username: username.value,
    email: email.value,
    password: password.value,
  });
  setTimeout(() => {
    router.push("/");
  }, 1000);
}
</script>

<template>
  <form @submit.prevent="register">
    <h1 style="margin-bottom: 1rem">Créer un compte</h1>
    <input
      type="text"
      id="username"
      name="username"
      required
      v-model="username"
      placeholder="Nom d'utilisateur"
    />
    <input
      type="email"
      id="email"
      name="email"
      autocomplete="email"
      required
      v-model="email"
      placeholder="Email"
    />
    <input
      type="password"
      id="password"
      name="password"
      autocomplete="new-password"
      required
      v-model="password"
      placeholder="Mot de passe"
    />
    <button type="submit" :disabled="!isFormValid || isLoading" :class="{ loading: isLoading }">
      Créer un compte
    </button>
  </form>
</template>

<style scoped>
input {
  background: var(--color-bg-tertiary);
  border: none;
  border-radius: 10px;
  color: var(--color-text-primary);
  outline: none;
  padding: 1rem;
  margin-bottom: 1rem;
}
button {
  width: 100%;
}
</style>
