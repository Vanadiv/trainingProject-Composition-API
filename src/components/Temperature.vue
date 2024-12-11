<template>
<div v-if="props.temp != null" class="temp">
    <div class = "btn-change"> 
        <button class="btn-temp" @click="buttonEventName"> {{ buttonName }} </button>
        <router-link to="/info"> 
        <button class="btn-temp-info" > Дополнительная информация </button>
        </router-link>
    </div>

    <div class="temp-info"> 
        <h2> {{ showTemp }} </h2>
        <h2> {{ showFeelsLike }} </h2>
        <h2> {{ showMinTemp }} </h2>
        <h2> {{ showMaxTemp }} </h2>
    </div>

    <ProgressBar :weather = "temp" :isButtonOneActive="isButtonOneActive"/>
</div> 
</template>

<script setup>
import { computed } from 'vue';
import ProgressBar from './ProgressBar.vue';

const props = defineProps ({
    temp: {
        type: Object,
        required: true 
    },
    getWeatherInfo:{
        type: Function,
        required: true 
    },
    getFTemp: {
        type: Function,
        required: true 
    },
    isButtonOneActive: {
        type: Boolean,
        required: true 
    }
})

const showTemp = computed(()  => { 
    return "Температура: " + props.temp.main.temp 
})
const showFeelsLike = computed(() => { 
    return "Ощущается как: " + props.temp.main.feels_like 
})
const showMinTemp = computed(() => { 
    return "Минимальная температура: " + props.temp.main.temp_min 
})
const showMaxTemp = computed(() => { 
    return "Максимальная температура: " + props.temp.main.temp_max 
})
const buttonName = computed(() => { 
    return props.isButtonOneActive ? "Перевести в °F" : "Перевести в °C" 
})
const buttonEventName = () => { 
    props.isButtonOneActive ? props.getFTemp() : props.getWeatherInfo() 
}
</script>

<style scoped>
.temp {
    grid-area: content1; 
    display: grid;
    grid-template-areas: "header header" "content1 content2";
    grid-template-columns: 0.3fr 1fr;
    grid-template-rows: auto 1fr;
    box-shadow: 0px 2px 40px #00000014;
    font-weight: 700;
    font-size: 14px;
    border-radius: 10px;
    grid-row: span 2;
    padding: 30px;
    margin-left: 50px;
    background: rgba(255, 255, 255, 0.2);
}
.temp-info {
    grid-area: content2;
    align-content: center; 
    justify-content: center;
}
.btn-temp {
    border-radius: 10px;
    border: 2px solid #0d0b87;
    padding: 10px 15px;
    cursor: pointer;
    background: #0d0b87;
    color: #fff;
    transform: none !important;
}
.btn-temp-info {
    border-radius: 10px;
    border: 2px solid #0d0b87;
    padding: 10px 15px;
    cursor: pointer;
    background: #0d0b87;
    color: #fff;
    transform: none !important;
    margin-left: 20px;
}
.btn-change {
    display: flex;
    padding: 0;
    grid-area: header;
    grid-column-start: span 2;
    margin: 0;
    justify-content: start;
}
</style>