<script setup>
import axios from 'axios';
import { ref, computed, watch, onMounted } from 'vue';
import Weathercard from './Weathercard.vue';




const props = defineProps({
  location: {
    type: Object,
    default: { lat: 33.86, lng: 151.20 }
  },

  PlaceName: {
    type: String 
  }
});

const location = computed(() => {
  if (props.location.lat == null && props.location.lng == null) {
    return { lat: 33.86, lng: 151.20  };
  } else {
    return { lat: props.location.lat, lng: props.location.lng };
  }
});

const weatherData = ref()

onMounted(() => {
  fetchWeatherData();
})

const fetchWeatherData = async () => {
  try {
    console.log(location.value)
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${location.value.lat}&lon=${location.value.lng}&appid=e8c8546ac1cc12d6390bdd7fa53027a5&units=metric`);
    weatherData.value = response.data;
    console.log(weatherData.value)
  } catch (error) {
    console.error('Error fetching weather data:', error);
  }
};

watch(location, fetchWeatherData, { deep: true })


</script>

<template>
  <div class="flex flex-col min-w-full items-center mt-10">
    <Weathercard v-if='weatherData' :weatherdata="weatherData" :PlaceName="PlaceName"></Weathercard>
    <h1 v-else>Loading weather data...</h1>
  </div>

</template>