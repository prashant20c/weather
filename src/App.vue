<script setup>
import { ref } from 'vue';
import navigation from './components/navigation.vue'
import container from './components/container.vue'
import info from './components/info.vue';



let lat = ref(null)
let lng = ref(null)
let PlaceName = ref()
let error = ref(null)




// function getLongLat() {
//   return new Promise((res, rej) => {
//     navigator.geolocation.getCurrentPosition(data => {
//     res({lat: data.coords?.latitude, lng: data.coords?.longitude});
//   }, () => {
//     error.value = 'location access failed !!!'
//     rej(error.value);
//   })
//   })    
// }

const selectedPlace = (e) => {

  console.log(e)
  lat.value = e.geometry.location.lat
  lng.value = e.geometry.location.lng
  PlaceName.value = e.formatted_address
}


</script>

<template>
  <div  class="bg-sky-200 w-screen h-screen">
    <navigation @place-data="selectedPlace"></navigation>
    <container v-if="error == null" :location="{ lat: lat, lng: lng }" :PlaceName="PlaceName" ></container>
    <div v-else class="text-3xl text-center p-5 text-red-400">{{ error }}</div>
    <info class="absolute bottom-0"> </info>
  </div>
  
</template>