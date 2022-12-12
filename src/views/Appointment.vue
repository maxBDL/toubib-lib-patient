<script setup>
import { ref, onMounted } from 'vue'
import { RouterView, RouterLink, useRouter } from 'vue-router'
import { doc, getDocs, collection, addDoc } from "firebase/firestore";
import { auth, db } from "../firebase/index.js";


defineProps({
  msg: String
})

const route = useRouter();

const count = ref(0)
const lastname = ref(null);
const name = ref(null);
const doctor_id = ref(null);
const description = ref(null);
const user_id = ref("1");
const doctors = ref(null);
const date = ref(null);

onMounted(async () => {
  const result = await getDocs(collection(db, "doctors"));
  const docs = [];
  result.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());

    const doctor = {
      'id': doc.id,
      'name': doc.data().name,
      'lastname': doc.data().lastname,
    };
    docs.push(doctor);
  });


  doctors.value = docs;
})

const createAppointment = async () => {
  const result = await addDoc(collection(db, "appointments"), {
    name: name.value,
    lastname: lastname.value,
    doctor_id: doctor_id.value,
    user_id: user_id.value,
    description: description.value,
    date: date.value
  });
  route.push('/');
}
</script>

<template>
  <nav>
    <RouterLink to="/"> Home </RouterLink> |
    <RouterLink to="/appointments"> Nouveau </RouterLink>
  </nav>
  <div class="mt-10 sm:mt-0">
    <div class="md:grid md:grid-cols-3 md:gap-6">
      <div class="mt-5 md:col-span-2 md:mt-0">
        <form @submit.prevent="createAppointment">
          <div class="overflow-hidden shadow sm:rounded-md">
            <div class="bg-white px-4 py-5 sm:p-6">
              <div class="grid grid-cols-6 gap-6">

                <div class="col-span-6 sm:col-span-3">
                  <label for="first-name" class="block text-sm font-medium text-gray-700">First name</label>
                  <input required v-model="name" type="text" name="name" id="first-name" autocomplete="given-name"
                    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>

                <div class="col-span-6 sm:col-span-3">
                  <label for="last-name" class="block text-sm font-medium text-gray-700">Last name</label>
                  <input required v-model="lastname" type="text" name="lastname" id="last-name" autocomplete="family-name"
                    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
                </div>

                <div class="col-span-6 sm:col-span-3">
                  <label for="country" class="block text-sm font-medium text-gray-700">Doctor</label>
                  <select required v-model="doctor_id" id="country" name="country" autocomplete="country-name"
                    class="mt-1 block w-full rounded-md border border-gray-300 bg-white py-2 px-3 shadow-sm focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm">
                    <option v-for="doctor in doctors" :key="index" :value="doctor.id">{{ doctor.name }}
                      {{ doctor.lastname }}</option>
                  </select>
                </div>

                <div class="col-span-6 sm:col-span-3">
                  <label for="about" class="block text-sm font-medium text-gray-700">Description</label>
                  <textarea required v-model="description" id="about" name="description" rows="3"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"/>
                </div>

                <div  class="col-span-6 sm:col-span-3">
                  <div class="datepicker relative form-floating mb-3 xl:w-96">
                    <label for="date" class="block text-sm font-medium text-gray-700">Select a date</label>
                    <input required type="date" v-model="date" name="date"
                    class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
                      placeholder="Select a date" />
                  </div>
                </div>

              </div>
            </div>
            <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
              <button type="submit"
                class="inline-flex justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">Save</button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
