<script>
import {ref, onMounted} from 'vue';
import axios from 'axios';
import IconSun from "@/components/icons/IconSun.vue";
import IconWind from "@/components/icons/IconWind.vue";
import IconMoisture from "@/components/icons/IconMoisture.vue";

export default {
  components: {IconSun, IconWind, IconMoisture},
  setup() {
    // Define reactive state
    const isVisible = ref(true);
    const inputText = ref('');
    let temperature = ref('0');
    let wind = ref('0');
    let moisture = ref('0');


    // Define methods
    const passInputTo = (e) => {
      e.preventDefault();

      const country = inputText.value;

      getTemperature(country)
    }
    const getTemperature = (country) => {

      if (country === '') return;

      localStorage.setItem('country', country);

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${country}&APPID=757f823421ff0618ace66ded0b3c2f5a&units=metric`)
          .then(response => {
            if (response.data.main) {
              temperature.value = response.data.main.temp;
              moisture.value = response.data.main.humidity;
              wind.value = response.data.wind.speed;
            }
          })
    };

    // Perform actions when the component is mounted
    onMounted(() => {
      if (localStorage.getItem('country')) {
        const country = localStorage.getItem('country');

        inputText.value = country;

        getTemperature(country);
      }
    });

    return {
      isVisible,
      inputText,
      temperature,
      wind,
      moisture,
      getTemperature,
      passInputTo
    };
  },
};
</script>

<template>
  <div class="flex items-center flex-col justify-center h-full">
    <h1>Weather in: <span class="capitalize">{{ inputText }}</span></h1>
    <form class="flex flex-row w-full gap-x-4 justify-center mt-5" action="">
      <input v-model="inputText" type="text" placeholder="Type here"
             class="input input-bordered input-primary w-full max-w-xs"/>
      <button @click="passInputTo" class="btn btn-primary">
        Search
      </button>
    </form>
    <div class="flex flex-col md:flex-row gap-2 md:gap-5 mt-5 w-full items-start md:justify-center md:items-center">
      <p class="text-4xl flex flex-row gap-x-3 text-secondary">
        <span>{{ temperature }}</span>
        <IconSun/>
      </p>
      <p class="text-4xl flex flex-row gap-x-3 text-accent">
        <span>{{ wind }}</span>
        <IconWind/>
      </p>
      <p class="text-4xl flex flex-row gap-x-3 text-success">
        <span>{{ moisture }}</span>
        <IconMoisture/>
      </p>
    </div>
  </div>
</template>