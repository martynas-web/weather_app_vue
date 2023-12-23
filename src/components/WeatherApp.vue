<script>
import {ref} from 'vue';
import axios from 'axios';

export default {
  setup() {
    // Define reactive state
    const isVisible = ref(true);
    const inputText = ref('');
    let temperature = ref('');

    // Define methods
    const getTemperature = (e) => {
      e.preventDefault();

      const country = inputText.value;

      if (country === '') return;

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${country}&APPID=757f823421ff0618ace66ded0b3c2f5a&units=metric`)
          .then(response => {
            if (response.data.main) {
              temperature.value = response.data.main.temp;
            }
      })
    };

    // Return values that can be used in the template
    return {
      isVisible,
      inputText,
      temperature,
      getTemperature,
    };
  },
};
</script>

<template>
  <div class="flex items-center flex-col justify-center h-full">
    <h1>Weather in: <span class="capitalize">{{ inputText }}</span></h1>
    <form class="flex flex-row w-full gap-x-4 justify-center mt-4" action="">
      <input v-model="inputText" type="text" placeholder="Type here"
             class="input input-bordered input-primary w-full max-w-xs"/>
      <button @click="getTemperature" class="btn btn-primary">
        Search
      </button>
    </form>
    <p class="text-4xl mt-4 text-secondary">
      <span>{{ temperature }}</span> °C
    </p>
  </div>
</template>