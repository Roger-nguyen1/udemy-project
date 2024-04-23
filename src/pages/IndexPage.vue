<template>
  <div class="q-pa-md" style="max-width: 400px">
    <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
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

      <div>
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
<script>
import { useQuasar } from "quasar";
import { ref } from "vue";

export default {
  setup() {
    const $q = useQuasar();

    const username = ref(null);
    const email = ref(null);
    const password = ref(null);
    const passwordAA = ref(null);
    const accept = ref(false);

    return {
      username,
      email,
      password,
      passwordAA,
      accept,

      onSubmit() {
        if (accept.value !== true) {
          $q.notify({
            color: "red-5",
            textColor: "white",
            icon: "warning",
            message: "You need to accept the license and terms first",
          });
        } else {
          $q.notify({
            color: "green-4",
            textColor: "white",
            icon: "cloud_done",
            message: "Submitted",
          });
        }
      },

      onReset() {
        username.value = null;
        email.value = null;
        password.value = null;
        passwordAA.value = null;
        accept.value = false;
      },
    };
  },
};
</script>
