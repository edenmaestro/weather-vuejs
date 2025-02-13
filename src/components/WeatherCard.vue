<script setup>
import { ref } from 'vue';
import Borderline from './Borderline.vue';
import WeatherForecarstDay from './WeatherForecarstDay.vue';
import WeatherInfo from './WeatherInfo.vue';

// On fait un props pour appeler les infos dans le json
defineProps({
    place: Object
})

const showDetail = ref(false)

</script>

<template>
  <div class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden bg-blue-500">
     <!-- Pour le temps et l'endroit -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-location-dot"></i>
        <h1 class="text-3xl">{{ place.location.name }}</h1>
      </div>
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">{{ new Date(place.location.localtime).getHours() }}:{{ new Date(place.location.localtime).getMinutes() }}</h1>
      </div>
    </div>

     <!-- Pour voir la temperature courante -->
    <div class="text-center flex-1">
      <img :src="place.current.condition.icon" alt="icon" width="200" class="mx-auto -mb-10" />
      <h1 class="text-9xl mb-2 mr-4">{{ Math.round(place.current.temp_c)}}&deg;</h1>
      <p class="text-2xl">{{place.current.condition.text}}</p>
    </div>


    <Borderline />


     <!-- Pour voir le autres jours  -->
    <div v-for="(day, idx) in place.forecast.forecastday" key="idx">
      <WeatherForecarstDay :day="day" />
    </div>

    <!-- info -->
    <div v-show="showDetail">
        <WeatherInfo :place="place"  @close-info="showDetail = false"/>
    </div>

    <!-- forecast btn -->
    <div class="flex justify-end items-center gap-1 mt-10">
      <button @click="showDetail = true">More <i class="fa-solid fa-arrow-right text-sm -mb-px"></i></button>
    </div>
  </div>
</template>