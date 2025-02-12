<script setup>
import SearchInput from './components/SearchInput.vue';
import { ref } from 'vue';

// On va utilser ses deux variables pour afficher le resultat cliquée
const places = ref([])   

const addPlace = (data) => {
    places.value.push(data)
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
           <div>
                <p v-for="place in places">
                    {{ place.location.name }}
                </p>
           </div>

    </main>
</template>