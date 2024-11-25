<template>
<div v-if="props.temp" class="temp-info">
    <div class = "btn-change"> 
        <button class="btn-temp" @click="buttonEventName"> {{ buttonName }} </button>
        <router-link to="/info"> 
        <button class="btn-temp" > Дополнительная информация </button>
        </router-link>
    </div>

    <div> 
        <h2> {{ showTemp }} </h2>
        <h2> {{ showFeelsLike }} </h2>
        <h2> {{ showMinTemp }} </h2>
        <h2> {{ showMaxTemp }} </h2>
    </div>
</div> 
</template>

<script setup>
import { computed } from 'vue';

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
.temp-info {
    grid-area: content1; 
    display: grid;
    box-shadow: 0px 2px 40px #00000014;
    align-content: center; 
    justify-content: center;
    font-weight: 700;
    border-radius: 10px;
    grid-row: span 2;
    padding: 30px;
    margin-left: 50px;
}
.btn-temp {
    border-radius: 10px;
    border: 2px solid #0d0b87;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    background: #0d0b87;
    color: #fff;
    transform: none !important;
}
.btn-change {
    position: absolute;
}
</style>