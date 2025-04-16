<script setup> 
import { onUpdated, ref, defineProps } from 'vue';
import { MagnifyingGlassIcon } from '@heroicons/vue/24/solid'
import axios from 'axios'
import Searchresult from './Searchresult.vue';

let place = ref(null)
const placeSuggestion = ref(null)



const searchlocation = async ()=>{placeSuggestion.value=null
   let { data }  = await axios.get(`https://maps.googleapis.com/maps/api/place/textsearch/json?query=${place.value}&key=${import.meta.env.VITE_GOOGLE_MAPS_API_KEY}`)
   placeSuggestion.value = data.results
}

</script>

<template>
    <div class="flex flex-col items-center ">
        <h1 class="text-3xl text-center p-5 pt-5 font-mono bg-blue-400 w-full"><img class="absolute pl-5 w-[100px] h-[50px]" src="../assets/SydneyMet.png" >Weather App </h1>
        <div class=" relative flex border rounded-3xl mt-3 p-4 focus-within:border-blue-600 bg-white" >
            <input  placeholder="Search City or place" class="rounded-lg min-w-[500px] focus:outline-none" v-model="place" @keydown="searchlocation">
            <MagnifyingGlassIcon class=" w-6 text-black-500 bg-white p-1"></MagnifyingGlassIcon>

            <Searchresult v-show="placeSuggestion" :data="placeSuggestion" @place-data="(e)=>$emit('place-data',e)" ></Searchresult>
            
        </div>
    </div>

</template>