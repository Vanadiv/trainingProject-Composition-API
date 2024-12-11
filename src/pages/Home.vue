<template>
<div class="wrap"> 
    <div class="city-input">  
      <h1> Погодное приложение</h1>
      <input type="text" v-model="city" placeholder="Введите название города" @keydown.enter="checkInput">
      <button :disabled="city ==''" @click="checkInput"> {{ buttonName }} </button>
      <button v-if="weather != null" @click="clearWeatherInfo"> Сбросить данные </button>
    </div>
    <div class="city-name">
      <template v-if="weather != null">
          <img src="@/assets/map.svg" />
          <h1> {{ showCityName }}</h1>
      </template>
      <p v-else> {{ error }}</p>
    </div>
  
  <Temperature :temp="weather" :getWeatherInfo="getWeatherInfo" :getFTemp="getFTemp" :isButtonOneActive="isButtonOneActive" />
  
  <Sun :weather="weather" />
  
  <Condition :weather="weather"/>

  <MapCity :weather="weather"/>
</div> 
</template>

<script>
import axios from 'axios';
import Temperature from '../components/Temperature.vue';
import Sun from '../components/Sun.vue';
import Condition from '../components/Condition.vue';
import MapCity from '../components/MapCity.vue';
import { ref, computed, onMounted  } from 'vue';

export default {
  components: {
    Temperature,
    Sun,
    Condition,
    MapCity
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
    const positionUser = ref({lat: null, lon: null});

    onMounted(() => {
      const savedCity = localStorage.getItem('city');
      if (savedCity != null) {
        city.value = savedCity;
      }
      const savedButtonState = localStorage.getItem('isButtonOneActive');
      if (savedButtonState != null) {
        isButtonOneActive.value = JSON.parse(savedButtonState); 
      }
      const savedWeatherInfo = localStorage.getItem('weatherInfo');
      if (savedWeatherInfo != null) {
        weather.value = JSON.parse(savedWeatherInfo);
      } else {
        navigator.geolocation.getCurrentPosition((position) => {
        positionUser.value.lat = position.coords.latitude;
        positionUser.value.lon = position.coords.longitude;
        getWeatherFromGeo();
      }, (error) => {
        error.value = 'Не удалось определить местоположение';
      })
      }
    });

    const getWeatherFromGeo = () => {
        axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${positionUser.value.lat}&lon=${positionUser.value.lon}&lang=ru&units=metric&appid=9bc72beca7fd1dafc25b81e5fb32b98e`)
        .then(res => {
          weather.value = res.data;
        })
        .catch(() => {
            error.value = "Произошла ошибĸа. Введите название города в поле поиска.";
            weather.value = null
        });
      }

    const checkInput = () => {
      if (city.value.trim().length > 1 ) {
        getWeatherInfo();
        localStorage.setItem('city', city.value);
      } else  if (city.value.trim().length == 1) {
        error.value = "Введите более одного символа";
        weather.value = null;
      }
    }

    const getWeatherInfo = () => {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&lang=ru&units=metric&appid=9bc72beca7fd1dafc25b81e5fb32b98e`)
        .then(res => {
          weather.value = res.data;
          localStorage.setItem('weatherInfo', JSON.stringify(res.data));
        })
        .catch(err => {
          if (err.response && err.response.status === 404) {
            error.value = "Такого города нет";
            weather.value = null;
            return;
            } 
            error.value = "Произошла ошибĸа. Попробуйте снова.";
            weather.value = null
        });
      isButtonOneActive.value = true;
      localStorage.setItem('isButtonOneActive', JSON.stringify(isButtonOneActive.value));
    };

    const getFTemp = () => {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&lang=ru&units=imperial&appid=9bc72beca7fd1dafc25b81e5fb32b98e`)
        .then(res => {
          weather.value = res.data;
          localStorage.setItem('weatherInfo', JSON.stringify(res.data));
        });
      isButtonOneActive.value = false;
      localStorage.setItem('isButtonOneActive', JSON.stringify(isButtonOneActive.value));
    };
    
    const showCityName = computed(() => {
      return weather.value ? weather.value.name : ''
    });

    const buttonName = computed(() => {
      return city.value =='' ? 'Введите название' : 'Узнать погоду'
    })

    const clearWeatherInfo = () => {
      localStorage.removeItem('city');
      localStorage.removeItem('weatherInfo');
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
      getWeatherInfo,
      getFTemp,
      showCityName,
      clearWeatherInfo,
      checkInput,
      buttonName,
      positionUser
    };
  }
};
</script>

<style scoped>

.wrap {
    display: grid;
    gap: 30px 30px;
    grid-template-areas: "header header header" "city city city" "content1 content2 content5" "content3 content4 content5";
    grid-template-columns: 0.8fr 0.6fr 0.7fr;
    grid-template-rows: auto auto 1fr 1fr;
    width: 100vw;
    color: #292733;
    text-align: center;
    align-content: center;
}
.city-name {
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
.city-input {
    grid-area: header;
}
</style>