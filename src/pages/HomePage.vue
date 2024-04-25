<template>
  <div class="flex flex-col justify-center items-center">
    <h1 class="font-bold text-center text-[42px]">Welcome to the Home page</h1>
    <div class="w-[500px] m-5">
      <p class="text-[30px] font-bold text-center">You are connected</p>
      <p>
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
        veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
        commodo consequat. Duis aute irure dolor in reprehenderit in voluptate
        velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt
        mollit anim id est laborum."
      </p>
    </div>
  </div>
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
