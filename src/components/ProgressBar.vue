<template>
    <div class="progress-bar-wrapper" v-if="props.weather != null">
        <div class="zero-label"> {{ 0 + units }}</div>

        <div class="thermometer">
            <div class="temperature-level" :style="{height: `${temperaturePercent}%`}"></div>

            <div class="temperature-label">{{ temperature + units}}</div>
        </div>
    </div>
</template>
  
<script setup>
  import { computed } from 'vue';

  const props = defineProps({
    weather: {
      type: Object,
      required: true
    },
    isButtonOneActive: {
        type: Boolean,
        required: true 
    }
  });
    
  const temperature = computed(() => {
    if(props.weather != null && props.weather.main && props.weather.main.temp != null) {
        return props.weather.main.temp;
    }
    return 0
  });
  
  const minCTemp = -50;

  const maxCTemp = 50;

  const minFTemp = -122;

  const maxFTemp = 122;

  const temperaturePercent = computed(() => {
    if(props.isButtonOneActive == true) {
        return (100 - Math.min(Math.max((temperature.value - minCTemp) / (maxCTemp - minCTemp) * 100, 0), 100));
    } else {
        return (100 - Math.min(Math.max((temperature.value - minFTemp) / (maxFTemp - minFTemp) * 100, 0), 100));
    }
    });

  const units = computed(() => { 
    return props.isButtonOneActive ? "°C" : "°F";
})
</script>
  
<style scoped>
.progress-bar-wrapper {
    grid-area: content1;
    display: flex;
    align-items: center;
    position: relative;
}
.zero-label {
    font-size: 14px;
    top: 50%;
    transform: translateY(-50%);
    position: absolute;
    left: 65px;
}
 .thermometer {
    width: 60px;
    height: 300px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    position: relative;
    background: linear-gradient(180deg, rgba(249,87,46,1) 0%, rgba(247,164,86,1) 33%, rgba(244,184,126,1) 45%, rgba(163,181,184,1) 55%, rgba(98,170,228,1) 67%, rgba(0,116,255,1) 100%);
}
.temperature-level {
    width: 100%;
    position: absolute;
    border-radius: 10px 10px 0 0;
    top: 0;
    background: #FDF5E6;
}
.temperature-label {
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 14px;
    color: #333;
}
</style>