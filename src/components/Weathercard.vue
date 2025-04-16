<script setup>
import { reactive, ref, watch, watchEffect } from 'vue';

const props = defineProps({
  weatherdata: {
    type: Object,
    required: true
  },
  PlaceName: {
    type: String,
    default: 'Australia'
  }
});

let date = ref();

setInterval(() => {
  date.value = new Intl.DateTimeFormat('en-GB', {
    dateStyle: 'full',
    timeStyle: 'short',
    hourCycle: 'h12'
  }).format(new Date());
}, 1000);

let weatherStatus = ref(props.weatherdata.weather[0].main);
let png = ref(props.weatherdata.weather[0].icon);

//  Dynamically import weather background images
const images = import.meta.glob('../assets/*.png', { eager: true });
const backgroundImageUrl = ref('');

const updateBackgroundImage = () => {
  for (const path in images) {
    if (path.endsWith(`${weatherStatus.value}.png`)) {
      backgroundImageUrl.value = `url(${images[path].default})`;
    }
  }
};

// Weather icon from OpenWeatherMap
const weatherPng = ref(`url(https://openweathermap.org/img/wn/${png.value}@2x.png)`);

// Initial background setup
updateBackgroundImage();

watch(() => props.weatherdata, (newVal) => {
  if (newVal?.weather?.length > 0) {
    weatherStatus.value = newVal.weather[0].main;
    png.value = newVal.weather[0].icon;
    weatherPng.value = `url(https://openweathermap.org/img/wn/${png.value}@2x.png)`;
    updateBackgroundImage();
  }
});
</script>


<template>
  <div
    :style="{ backgroundImage: backgroundImageUrl }"
    class="text-center font-bold text-gray-800 border-2 border-none rounded-lg min-w-[400px] min-h-[300px] bg-cover bg-center"
  >
    <div class="mb-4">
      <h2>Current Weather</h2>
      <h1>{{ PlaceName }}</h1>
      <h1>{{ date }}</h1>
    </div>
    
    <div class="font-light">
      <div
        :style="{ backgroundImage: weatherPng }"
        class="w-16 h-16 bg-contain bg-no-repeat mx-auto"
      ></div>
      <div>Now: {{ weatherdata.main.temp }}&deg;C</div>
      <div>Feels like: {{ weatherdata.main.feels_like }}&deg;C</div>
      <div>Clouds: {{ weatherdata.weather[0].main }}</div>
    </div>
  </div>
</template>
