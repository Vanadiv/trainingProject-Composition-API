<script>
import axios from 'axios';
import Temperature from '../components/Temperature.vue';
import Sun from '../components/Sun.vue';
import Condition from '../components/Condition.vue';
import { ref, computed, onMounted  } from 'vue';

export default {
  components: {
    Temperature,
    Sun,
    Condition
  },
  setup() {
    const city = ref("");
    const error = ref("");
    const weather = ref(null);
    const backgroundColor = ref("");
    const isButtonOneActive = ref(true);
    const conditions = ref("");
    const sunset = ref("");
    const sunrise = ref("");

    onMounted(() => {
      const savedCity = localStorage.getItem('city');
      if (savedCity) {
        city.value = savedCity;
      }
      const savedWeatherData = localStorage.getItem('weatherData');
      const savedButtonState = localStorage.getItem('isButtonOneActive');
      if (savedWeatherData) {
        weather.value = JSON.parse(savedWeatherData);
      }
      if (savedButtonState !== null) {
        isButtonOneActive.value = JSON.parse(savedButtonState); 
      }
    });

    const checkInput = () => {
      if (city.value.trim().length > 1 ) {
        get_data();
        localStorage.setItem('city', city.value);
      } else  if (city.value.trim().length == 1) {
        error.value = "Введите более одного символа";
        weather.value = null;
      }
    }

    const get_data = () => {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&lang=ru&units=metric&appid=9bc72beca7fd1dafc25b81e5fb32b98e`)
        .then(res => {
          weather.value = res.data;
          localStorage.setItem('weatherData', JSON.stringify(res.data));
        })
        .catch(err => {
          if (err.response && err.response.status === 404) {
            error.value = "Такого города нет";
            weather.value = null;
          } else {
            error.value = "Произошла ошибка. Попробуйте снова.";
            weather.value = null;
          }
        });
      isButtonOneActive.value = true;
      localStorage.setItem('isButtonOneActive', JSON.stringify(isButtonOneActive.value));
    };

    const getFtemp = () => {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&lang=ru&appid=9bc72beca7fd1dafc25b81e5fb32b98e`)
        .then(res => {
          weather.value = res.data;
          localStorage.setItem('weatherData', JSON.stringify(res.data));
        });
      isButtonOneActive.value = false;
      localStorage.setItem('isButtonOneActive', JSON.stringify(isButtonOneActive.value));
    };
    
    const showCityName = computed(() => {
      return weather.value ? weather.value.name : '';
    });

    const clearWeatherData = () => {
      localStorage.removeItem('city');
      localStorage.removeItem('weatherData');
      localStorage.removeItem('isButtonOneActive');
      weather.value = null;  
      error.value = "";  
      city.value = "";  
      isButtonOneActive.value = true;
    };

    return {
      city,
      error,
      weather,
      backgroundColor,
      isButtonOneActive,
      conditions,
      sunset,
      sunrise,
      get_data,
      getFtemp,
      showCityName,
      clearWeatherData,
      checkInput
    };
  }
};

</script>

<template>

<div class = "wrap"> 
    <div style = "grid-area: header;">  
      <h1> Погодное приложение</h1>
      <input type = "text" v-model = "city" placeholder="Введите название города" @keydown.enter="checkInput">
      <button disabled v-if = "city == ''"> Введите название</button>
      <button v-else @click = "checkInput"> Узнать погоду</button>
      <button v-if="weather" @click="clearWeatherData"> Сбросить данные </button>
    </div>

    <div class = "infoCity">
      <template v-if = "weather">
          <img src = "@/assets/map.svg" />
          <h1> {{ showCityName }}</h1>
      </template>
      <p v-else> {{ error }}</p>
    </div>

  <Temperature :temp = "weather" :get_data = "get_data" :getFtemp = "getFtemp" :isButtonOneActive = "isButtonOneActive" />

  <Sun :weather = "weather" />

  <Condition :weather = "weather"/>
</div>

</template>

<style scoped>

.wrap {
    display: grid;
    gap: 30px 30px;
    grid-template-areas: "header header" "city city" "content1 content2" "content3 content4";
    grid-template-columns: 0.5fr 0.5fr;
    grid-template-rows: auto auto 1fr 1fr;
    width: 100vw;
    background: #FDF5E6;
    color: #292733;
    text-align: center;
    align-content: center;
}
.infoCity {
    display: flex;
    grid-area: city;  
    align-items: center; 
    justify-content: center;
    border-radius: 10px;
}
input {
    margin-top: 30px;
    padding: 5px 30px 5px 8px;
    font-size: 14px;
    background: transparent;
    border: 0;
    border-bottom: 1px solid #292733;
    outline: none;
}
input:focus {
    border-bottom: 2px solid #292733;
}
button:disabled {
    border-radius: 10px;
    border: 2px solid #a0a0a5;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: default;
    background: #a0a0a5;
    color: #fff;
    transform: none !important;
}
button {
    border-radius: 10px;
    border: 2px solid #0d0b87;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transfor, 500ms ease;
    background: #0d0b87;
    color: #fff;
}
button:hover {
    transform: scale(1.1) translateY(-5px);
}
</style>