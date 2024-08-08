<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import PassengerService from '@/services/PassengerService';
import type { Passenger } from '@/types';

const passenger = ref<Passenger | null>(null);
const route = useRoute();

onMounted(() => {
  const id = parseInt(route.params.id as string, 10);
  if (!isNaN(id)) {
    PassengerService.getPassengerById(id)
      .then(response => {
        passenger.value = response.data;
      })
      .catch(error => {
        console.error('Failed to fetch passenger details:', error)
      });
  }
});
</script>

<template>
  <div v-if="passenger">
    <h1>{{ passenger.name }}</h1>
    <p>Country: {{ passenger.country }}</p>
    <p>Logo: <img :src="passenger.logo" alt="Airline Logo"></p>
    <p>Slogan: {{ passenger.slogan }}</p>
    <p>Head Quarters: {{ passenger.head_quaters }}</p>
    <p>Website: <a :href="passenger.website">{{ passenger.website }}</a></p>
    <p>Established: {{ passenger.established }}</p>
  </div>
</template>
