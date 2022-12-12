<script setup>
import { ref, onMounted } from 'vue'
import { doc, getDocs, collection, addDoc } from "firebase/firestore";
import { auth, db } from "../firebase/index.js";

defineProps({
  msg: String
})

const appointments = ref(null);
const isLoading = ref(true);

onMounted(async () => {
  const result = await getDocs(collection(db, "appointments"));
  const apts = [];
  result.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());

    const doctor = {
      'id': doc.id,
      'name': doc.data().name,
      'lastname': doc.data().lastname,
      'doctor_id': doc.data().doctor_id,
      'description': doc.data().description,
    };
    apts.push(doctor);
  });
  isLoading.value = false;
  appointments.value = apts;
})
</script>

<template>
  <nav>
    <RouterLink to="/"> Home </RouterLink> |
    <RouterLink to="/appointments"> Nouveau </RouterLink>
  </nav>
  <h1>Vos rdv</h1>
  <div class="flex flex-col">
    <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 inline-block min-w-full sm:px-6 lg:px-8">
        <div class="overflow-hidden">
          <table class="min-w-full">
            <thead class="border-b">
              <tr>
                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                  Date
                </th>
                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                  Doctor
                </th>
                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                  Description
                </th>
              </tr>
            </thead>
            <tbody v-if="!isLoading">
              <tr class="border-b" v-for="appointment in appointments" :key="index">
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                  {{ appointment.date }}
                </td>
                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                  {{ appointment.doctor_id }}
                </td>
                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                  {{ appointment.description }}
                </td>
              </tr>
            </tbody>
            <tbody v-else class="flex items-center justify-center w-100">
              <div class="lds-default"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

.lds-default {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}

.lds-default div {
  position: absolute;
  width: 6px;
  height: 6px;
  background: #f0f;
  border-radius: 50%;
  animation: lds-default 1.2s linear infinite;
}

.lds-default div:nth-child(1) {
  animation-delay: 0s;
  top: 37px;
  left: 66px;
}

.lds-default div:nth-child(2) {
  animation-delay: -0.1s;
  top: 22px;
  left: 62px;
}

.lds-default div:nth-child(3) {
  animation-delay: -0.2s;
  top: 11px;
  left: 52px;
}

.lds-default div:nth-child(4) {
  animation-delay: -0.3s;
  top: 7px;
  left: 37px;
}

.lds-default div:nth-child(5) {
  animation-delay: -0.4s;
  top: 11px;
  left: 22px;
}

.lds-default div:nth-child(6) {
  animation-delay: -0.5s;
  top: 22px;
  left: 11px;
}

.lds-default div:nth-child(7) {
  animation-delay: -0.6s;
  top: 37px;
  left: 7px;
}

.lds-default div:nth-child(8) {
  animation-delay: -0.7s;
  top: 52px;
  left: 11px;
}

.lds-default div:nth-child(9) {
  animation-delay: -0.8s;
  top: 62px;
  left: 22px;
}

.lds-default div:nth-child(10) {
  animation-delay: -0.9s;
  top: 66px;
  left: 37px;
}

.lds-default div:nth-child(11) {
  animation-delay: -1s;
  top: 62px;
  left: 52px;
}

.lds-default div:nth-child(12) {
  animation-delay: -1.1s;
  top: 52px;
  left: 62px;
}

@keyframes lds-default {

  0%,
  20%,
  80%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.5);
  }
}
</style>
