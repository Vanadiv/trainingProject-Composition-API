<template>
    <div id="map" v-if="props.weather != null" class="wrap"> 
    </div> 
</template>
    
<script setup>
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'
import { onMounted, watchEffect, ref, nextTick  } from 'vue'

const props = defineProps({
  weather: {
    type: Object,
    required: true
  }
})

const map = ref(null);
const positionCity = ref({lat: null, lon: null});

const initMap = () => {
  if (props.weather != null && props.weather.coord != null) {
    positionCity.value.lat = props.weather.coord.lat;
    positionCity.value.lon = props.weather.coord.lon;

    nextTick(() => {
        if (map.value != null) {
            map.value.remove();  
        }
    
        map.value = L.map('map', { attributionControl: false }).setView([positionCity.value.lat, positionCity.value.lon], 13);

        L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }).addTo(map.value);

        L.marker([positionCity.value.lat, positionCity.value.lon]).addTo(map.value)
            .bindPopup(`<b>${props.weather.name}</b><br/>${props.weather.weather[0].description}`).openPopup();
        })
    }
}

onMounted(() => {
  initMap(); 
});

watchEffect(() => {
  if (props.weather && props.weather.coord) {
    initMap();
  }
});
</script>
    
<style scoped>
#map {
    width: 40vw;
    height: 100%;
    border-radius: 10px;
}
.wrap {
    margin-right: 50px;
    grid-area: content5;
    grid-row: span 2;
}
</style>