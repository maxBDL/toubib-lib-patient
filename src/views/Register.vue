<script setup>
import { createUserWithEmailAndPassword } from "firebase/auth";
import { doc, setDoc } from "firebase/firestore";
import { ref } from "vue";
import { auth, db } from "../firebase/index.js";
import {RouterView, RouterLink, useRouter} from 'vue-router'

const email = ref(null);
const password = ref(null);

const lastname = ref(null);
const firstname = ref(null);

const hasError = ref(false);

const user = ref(null);
const route = useRouter();


const onRegister = async () => {
  try {
    const resp = await createUserWithEmailAndPassword(
      auth,
      email.value,
      password.value
    );
    if (resp.user) {
      user.value = resp.user.uid;
      window.navigator.vibrate(200);

      const result = await setDoc(doc(db, "patients", user.value), {
        firstname: firstname.value,
        lastname: lastname.value
      });
      route.push('/');
    }
  } catch (e) {
    window.navigator.vibrate(2000);
    hasError.value = true;
    console.log(e);
  }
};
</script>

<template>
  <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
    <div class="w-full max-w-md space-y-8">
      <div>
        <img class="mx-auto h-12 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600"
          alt="Your Company" />
        <h2 class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900">Register in the app</h2>
        <p class="mt-2 text-center text-sm text-gray-600">
          Or
          <RouterLink to="/login" class="font-medium text-indigo-600 hover:text-indigo-500"> Login </RouterLink>
        </p>
      </div>
      <form class="mt-8 space-y-6" @submit.prevent="onRegister">
        <input type="hidden" name="remember" value="true" />
        <div class="-space-y-px rounded-md shadow-sm">
          <div>
            <label for="email-address" class="sr-only">Email address</label>
            <input v-model="email" id="email-address" name="email" type="email" autocomplete="email" required
              class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="Email address" />
          </div>

          <div>
            <label for="lastname" class="sr-only">Nom</label>
            <input v-model="lastname" id="lastname" name="lastname" type="text" autocomplete="lastname" required
              class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="lastname" />
          </div>
          <div>
            <label for="firstname" class="sr-only">Prenoms</label>
            <input v-model="firstname" id="firstname" name="firstname" type="text" autocomplete="firstname" required
              class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="firstname" />
          </div>
          <div>
            <label for="password" class="sr-only">Password</label>
            <input v-model="password" id="password" name="password" type="password" autocomplete="current-password"
              required
              class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
              placeholder="Password" />
          </div>
        </div>

        <div>
          <button type="submit"
            class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
            Sign in
          </button>
        </div>
      </form>
    </div>
  </div>
</template>
  