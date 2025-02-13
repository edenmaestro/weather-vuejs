<script setup>
import { reactive } from 'vue'

// On créé notre propre event listener
const emit = defineEmits(['place-data'])


// On créée une variable qu'on va appelé searchTerm et on va faire passer l'objet 
const searchTerm = reactive({
    query: '',
    timeout: null,
    results: null,
})


// On créé une fonction, une sorte de lazy loading pour rechercher le lieu dans l'API 
const handleSearch = () => {
    console.log(searchTerm.query)
    clearTimeout(searchTerm.timeout)
    searchTerm.timeout = setTimeout( async() => {
        // Conditions
        if (searchTerm.query !== '') {
            // Ici on va fetch les data de l'api en créant une variable
            // Si l'endroit de recherche n'est pas vide, alors on fais tourner notre variable RES
            const res = await fetch(`http://api.weatherapi.com/v1/search.json?key=7a48f37894564769a7b90724251202&q=${searchTerm.query}`)

            // On prend les infos dans l'API, on le fait reagir en tant que DATA json (comme en ajax)
            const data = await res.json()

            // on lie notre variable searchTerm avec les DATA
            searchTerm.results = data

            // On affiche les resultats dans le DOM
            console.log(searchTerm.results)
        } else {
            // Et si il n'y a rien, on remet à zero les resultats
            searchTerm.results = null
        }
        // console.log(searchTerm.query)
    }, 500)
}



// Ici on créé une variable pour prendre les infos d'un seul endroit 
const getWeather = async (id) => {
    // On a changé London par l'id dans l'API id:{id}
    const res = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=7a48f37894564769a7b90724251202&q=id:${id}&days=3&aqi=no&alerts=no
    `) 
    // Ici on remplace nos data dans un res.json pour les montrer dans notre barre de recherche 
    const data = await res.json()
    // console.log(data);
    // On envoie les données du event listener dans la data
    emit('place-data', data)

    // Montrer l'info cliqué et fermé les recherches
    searchTerm.query = ''
    searchTerm.results = null
}

</script> 

<template>
  <div>
    <!-- {{ searchTerm.query }} -->
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <!-- Dans le v-model, on appelle la variable et chaque caractere qu'on met est pris (query)  -->
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="handleSearch"
        />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
        <!-- v-for ressemble à une class mais pour Vue, on utilise place comme le debut d'un mapping pour déclarer chaque élément dans l'API -->
         <div v-if="searchTerm.results !== null">
            <div v-for="place in searchTerm.results" :key="place.id">
                <!-- @click pour selectionner une info dans la liste de resultats -->
                <button @click="getWeather(place.id)" class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left">
                {{ place.name }}, {{ place.region }}, {{ place.country }}
                </button>
            </div>
         </div>
      
    </div>
  </div>
</template>