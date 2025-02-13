<script setup>
import SearchInput from './components/SearchInput.vue';
import { ref } from 'vue';
import WeatherCard from './components/WeatherCard.vue';

// On va utilser ses deux variables pour afficher le resultat cliquée
const places = ref([])   

const addPlace = (data) => {
    places.value.push(data)
}

const deletePlace = (name) => {
    // console.log(name)
    places.value = places.value.filter(p => p.location.name !== name)
}

</script>

<template>
    <main>
        <!-- Date -->
         <div class="text-center mb-6">
            {{ 
                new Date().toLocaleDateString('en-us',{
                    weekday: 'long',
                    year: 'numeric',
                    month: 'long',
                    day: 'numeric'
                })
            }}
         </div>

         <!-- Barre de recherche -->
          <div >
            <SearchInput @place-data="addPlace"/>
          </div>

          <!-- Carte Meteo -->
           <!-- Pour chaque place dans les places, on veut les idx comme key -->
            <div class="grid grid-cols-2 gap-4">
                <div v-for=" (place,idx) in places" :key="idx">
            <!-- On loop les places -->
                <WeatherCard :place="place" @delete-place="deletePlace" />
           </div>
            </div>
           

    </main>
</template>