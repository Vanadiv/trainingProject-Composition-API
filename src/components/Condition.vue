<template>
<div v-if="props.weather != null" class="conditions-info" :style="showImage">
    <div class="center-header"> 
        <h2> Осадки </h2>
    </div>
    <div class="center-text">
        <h2> {{ showConditions }} </h2>
    </div>
</div> 
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps ({
    weather: {
        type: Object, 
        required: true 
    },
})

const showConditions = computed(() => {
    if(props.weather != null && props.weather.rain != null) {
        return `Дождь - в среднем ${props.weather.rain?.['1h']} см дождя за 1 час`
    } 
    if(props.weather != null && props.weather.snow != null) {
        return `Снег - в среднем ${props.weather.snow?.['1h']} см снега за 1 час`
    } 
    return "Осадков нет"
})

const showImage = computed(() => {           
    if(props.weather != null && props.weather.rain != null) {
        return 'background: url(src/assets/09d.png) no-repeat'
    }
    if(props.weather != null && props.weather.snow != null) {
        return  'background: url(src/assets/13d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '01d') {
        return  'background: url(src/assets/01d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '02d') {
        return  'background: url(src/assets/02d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '03d') {
        return  'background: url(src/assets/03d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '04d') {
        return  'background: url(src/assets/04d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '09d') {
        return  'background: url(src/assets/09d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '10d') {
        return  'background: url(src/assets/10d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '11d') {
        return  'background: url(src/assets/11d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '13d') {
        return  'background: url(src/assets/13d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '50d') {
        return  'background: url(src/assets/50d.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '01n') {
        return  'background: url(src/assets/01n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '02n') {
        return  'background: url(src/assets/02n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '03n') {
        return  'background: url(src/assets/03n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '04n') {
        return  'background: url(src/assets/04n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '09n') {
        return  'background: url(src/assets/09n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '10n') {
        return  'background: url(src/assets/10n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '11n') {
        return  'background: url(src/assets/11n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '13n') {
        return  'background: url(src/assets/13n.png) no-repeat';
    }
    if(props.weather != null && props.weather.snow == null && props.weather.rain == null && props.weather.weather[0].icon == '50n') {
        return  'background: url(src/assets/50n.png) no-repeat';
    }
    return 'background: rgba(255, 255, 255, 0.2);'
})
</script>

<style scoped>
.conditions-info {
    display: grid;
    grid-area: content4; 
    box-shadow: 0px 2px 40px #00000014;
    border-radius: 10px;
    padding: 30px;
    grid-template-areas: "header" "text";
    grid-template-columns: 1fr;
    grid-template-rows: auto 1fr;
    gap: 15px;
}
.center-text {
    grid-area: text;
    display: flex;
    flex-direction: column;
    align-items: center; 
    justify-content: center;
    height: 100%;
    font-size: 14px;
}
.center-header {
    grid-area: header;
}
</style>