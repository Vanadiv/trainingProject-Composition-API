<template>
<div v-if="props.weather != null" class="sun-info">
    <div class="center-header">
        <h2> Солнце </h2>
    </div>

    <div class="center-text">
        <h2 v-if="props.weather.sys.sunset != 0 && props.weather.sys.sunrise != 0"> Восход  — {{ timeSunrise }} </h2>
        <h2 v-if="props.weather.sys.sunset != 0 && props.weather.sys.sunrise != 0"> Заход  — {{ timeSunset }} </h2>
        <h2 v-if="props.weather.sys.sunset == 0 && props.weather.sys.sunrise == 0 && props.weather.coord.lat > 0"> Полярная ночь </h2>
        <h2 v-if="props.weather.sys.sunset == 0 && props.weather.sys.sunrise == 0 && props.weather.coord.lat < 0"> Полярный день </h2>
    </div>   

    <!-- <div class="sun-img">
        <img src="@/assets/sun2.png" class="scale-img">
    </div>  -->

    <SunChart v-if="props.weather.sys.sunrise != 0 && props.weather.sys.sunset != 0" :weather = "weather"/>
    <div v-else style="align-content: center;"> ——— </div>

</div>  
</template>

<script setup>
import { computed } from 'vue';
import SunChart from './SunChart.vue';

const props = defineProps ({
    weather: {
        type: Object,
        required: true
    },
})


const timeSunset = computed(() => {
        const sunset = new Date((props.weather.sys.sunset + props.weather.timezone) * 1000);
        return sunset.toLocaleString('ru-RU', {
            hour: '2-digit',
            minute: '2-digit',
            timeZone: 'UTC'
        })
    })

const timeSunrise = computed(() => {
           const sunrise = new Date((props.weather.sys.sunrise + props.weather.timezone) * 1000);
           return sunrise.toLocaleString('ru-RU', {
                hour: '2-digit',
                minute: '2-digit',
                timeZone: 'UTC'
            })
        })
</script>

<style scoped>
.scale-img {
    width: 60%; 
    height: auto;
}
.sun-info {
    grid-area: content2; 
    display: grid;
    box-shadow: 0px 2px 40px #00000014;
    border-radius: 10px;
    grid-template-areas: "header header" "text chart";
    grid-template-rows: auto 1fr;
    grid-template-columns: auto 1fr;
    gap: 15px;
    background: rgba(255, 255, 255, 0.2);
    padding: 30px;
}
.sun-img {
    grid-area: imgSun;
    display: flex;
    flex-direction: column;
    align-items: center; 
    justify-content: center;
    height: 100%;
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