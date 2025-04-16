<script setup>
import { reactive, ref, watch, watchEffect } from 'vue';

let props = defineProps({
  weatherdata: {
    type: Object,
    require: true
  },

  PlaceName: {
    type: String,
    default: 'Australia'
  }
}
)

let date = ref()

setInterval(() => {
  date.value = new Intl.DateTimeFormat('en-GB', {
    dateStyle: 'full',
    timeStyle: 'short',
    hourCycle:'h12'
  }).format(new Date())
}, 1000)

console.log(props.weatherdata)

let weatherStatus = ref(props.weatherdata.weather[0].main);
let png = ref(props.weatherdata.weather[0].icon)
console.log(png.value)
const backgroundImageUrl = ref(`url('/src/assets/${weatherStatus.value}.png')`);
const weatherPng = ref(`url(https://openweathermap.org/img/wn/${weatherStatus.value})`)

watch(() => props.weatherdata, (newVal, oldVal) => {
  if (newVal && newVal.weather && newVal.weather.length > 0) {
    weatherStatus.value = newVal.weather[0].main;
    backgroundImageUrl.value = `url('/src/assets/${weatherStatus.value}.png')`;
    console.log(weatherStatus.value, backgroundImageUrl.value)
  }
});



</script>

<template>
  <div :style="{ backgroundImage: backgroundImageUrl }"
    class="text-center font-bold text-gray-800 border-2 border-none rounded-lg  min-w-[400px] min-h-[300px] ">
    <div class="mb-4">
      <h2>Current Weather</h2>
      <h1>{{ PlaceName }}</h1>
      <h1> {{ date }}</h1>

    </div>
    <div class="font-light ">
      <div  :style="{backgroundImage : weatherPng}" class="max-h-[20px] max-w-[20px]"> </div>
      <div> Now:{{ weatherdata.main.temp }}&degC</div>
      <div>feels like : {{ weatherdata.main.feels_like }}&degC</div>
      <div>clouds : {{ weatherdata.weather[0].main }}</div>
  
    </div>

  </div>

</template>