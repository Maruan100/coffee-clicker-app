<template>
  <div class="container">
    <div class="box">
      <h1>Welcome</h1>

      <div class="box_form">
        <input v-model="user" type="text" class="input-fild" placeholder=" " />
        <label class="input-label">Name</label>
      </div>
      <button :disabled="v$.$invalid" type="submit" class="btn" @click="logIn">
        Play
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import { useRouter } from "vue-router";

const router = useRouter();
const user = ref("");

const logIn = () => {
  router.push({ path: "/game", query: { user: user.value } });
};

const v$ = useVuelidate({ user: required }, user);
</script>
