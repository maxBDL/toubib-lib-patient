<script setup>
import { signInWithEmailAndPassword } from "firebase/auth";
import { ref } from "vue";
import { auth } from "../firebase/index.js";
import {RouterView, RouterLink, useRouter} from 'vue-router'


const email = ref(null);
const password = ref(null);
const hasError = ref(false);
const route = useRouter();

const onLogin = async () => {
  try {
    const r = await signInWithEmailAndPassword(
      auth,
      email.value,
      password.value
    );
    if (r.user) {
      console.log("connected");
      window.navigator.vibrate(200);
      route.push('/');
    }
  } catch (e) {
    window.navigator.vibrate(2000);
    hasError.value = true;
  }
};
</script>

<template>
    <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8">
      <div class="w-full max-w-md space-y-8">
        <div>
          <img class="mx-auto h-12 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600" alt="Your Company" />
          <h2 class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900">Login to your account</h2>
          <p class="mt-2 text-center text-sm text-gray-600">
          Or
          <RouterLink to="/register" class="font-medium text-indigo-600 hover:text-indigo-500"> Register </RouterLink>
        </p>
        </div>
        <form class="mt-8 space-y-6" @submit.prevent="onLogin">
          <input type="hidden" name="remember" value="true" />
          <div class="-space-y-px rounded-md shadow-sm">
            <div>
              <label for="email-address" class="sr-only">Email address</label>
              <input v-model="email" id="email-address" name="email" type="email" autocomplete="email" required="" class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm" placeholder="Email address" />
            </div>
            <div>
              <label for="password" class="sr-only">Password</label>
              <input  v-model="password" id="password" name="password" type="password" autocomplete="current-password" required="" class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm" placeholder="Password" />
            </div>
          </div>
  
          <div>
            <button type="submit" class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
              Sign in
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  