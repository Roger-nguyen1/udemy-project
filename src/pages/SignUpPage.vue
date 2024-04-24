<template>
  <div class="flex justify-center items-center">
    <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
      <h1 class="font-bold text-center text-[42px]">Sign UP</h1>
      <q-input
        filled
        v-model="username"
        label="Your username *"
        hint="Username"
        lazy-rules
        :rules="[(val) => (val && val.length > 0) || 'Please type something']"
      />

      <q-input
        filled
        type="email"
        v-model="email"
        label="Your email *"
        :rules="[
          (val) => (val !== null && val !== '') || 'Please type your email',
        ]"
      />
      <q-input
        filled
        type="password"
        v-model="password"
        label="Your password *"
        hint="More than 8 character"
        :rules="[
          (val) => (val !== null && val !== '') || 'Please type a password',
          (val) => val.length > 8 || 'Please type more than 8 character',
        ]"
      />
      <q-input
        filled
        type="password"
        v-model="passwordAA"
        label="Your passwordAA *"
        hint="More than 8 character"
        :rules="[
          (val) => (val !== null && val !== '') || 'Please type a password',
          (val) => val.length > 8 || 'Please type more than 8 character',
        ]"
      />

      <q-toggle v-model="accept" label="I accept the license and terms" />

      <div
        v-if="isLoading"
        class="flex flex-col justify-center items-center"
        role="status"
      >
        <svg
          aria-hidden="true"
          class="w-8 h-8 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
          viewBox="0 0 100 101"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
            fill="currentColor"
          />
          <path
            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
            fill="currentFill"
          />
        </svg>
        <span class="m-2">Loading...</span>
      </div>

      <div class="flex justify-center items-center mb-10">
        <q-btn label="Submit" type="submit" color="primary" />
        <q-btn
          label="Reset"
          type="reset"
          color="primary"
          flat
          class="q-ml-sm"
        />
      </div>
    </q-form>
  </div>
</template>
<script setup>
import { useQuasar } from "quasar";
import { ref, onBeforeMount } from "vue";
import { useRouter } from "vue-router";

defineOptions({
  name: "SignUpPage",
});

const router = useRouter();
const $q = useQuasar();
const username = ref(null);
const email = ref(null);
const password = ref(null);
const passwordAA = ref(null);
const accept = ref(false);
const isLoading = ref(false);

const checkToken = () => {
  const token = $q.cookies.has("token_cookie");
  token ? router.push("/home") : router.push("/signup");
};
onBeforeMount(() => {
  checkToken();
});

const setToken = (token) => {
  $q.cookies.set("token_cookie", token, { expires: "1d", sameSite: "Strict" });
};

//fetch data for Signup
const fetchSignup = () => {
  isLoading.value = true;
  fetch(`https://gestioncab-back.vercel.app/users/signup`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      username: username.value,
      email: email.value,
      password: password.value,
      passwordAA: passwordAA.value,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.result) {
        setToken(data.token);
        $q.notify({
          color: "green-4",
          textColor: "white",
          icon: "cloud_done",
          message: "Registration successful!",
        });

        const verifyToken = $q.cookies.has("token_cookie");
        verifyToken && router.push("/home");
      }
    })
    .catch((error) => {
      console.error("Signup Error :", error);
      $q.notify({
        color: "red-5",
        textColor: "white",
        icon: "warning",
        message: "An error occurred during registration. Please retry",
      });
    })
    .finally(() => {
      setTimeout(() => {
        isLoading.value = false;
      }, 1000);
    });
};
//onSubmit function
const onSubmit = () => {
  if (accept.value !== true) {
    $q.notify({
      color: "red-5",
      textColor: "white",
      icon: "warning",
      message: "You need to accept the license and terms first",
    });
    isLoading.value = false;
  } else {
    $q.notify({
      color: "green-4",
      textColor: "white",
      icon: "cloud_done",
      message: "Submitted",
    });
    fetchSignup();
  }
};
//onReset function
const onReset = () => {
  username.value = null;
  email.value = null;
  password.value = null;
  passwordAA.value = null;
  accept.value = false;
};
</script>
