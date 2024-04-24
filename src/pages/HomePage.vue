<template>
  <h1 class="font-bold text-center text-[42px]">Welcome to Home page</h1>
</template>

<script setup>
import { useQuasar } from "quasar";
import { ref, onBeforeMount } from "vue";
import { useRouter } from "vue-router";
defineOptions({
  name: "HomePage",
});

const router = useRouter();
const $q = useQuasar();

const checkToken = () => {
  const token = $q.cookies.has("token_cookie");
  token ? router.push("/home") : router.push("/");
};
onBeforeMount(() => {
  checkToken();
});
</script>
