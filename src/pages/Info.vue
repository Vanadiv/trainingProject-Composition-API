<template>
<div class="wrap">
    <h1> Дополнительная информация о погоде</h1>
    <router-link to="/">
        <button> Назад </button>
    </router-link>

    <div id="map">


    </div>
</div>
</template>

<script setup>
// import L from 'leaflet'
// import 'leaflet/dist/leaflet.css'
// import { onMounted  } from 'vue';

// const props = defineProps ({
//     temp: {
//         type: Object,
//         required: true 
//     }
// })

// const initMap = () => {
//     const map = L.map('map').setView([`${props.temp.coord.lon},${props.temp.coord.lat}`], 13);
//     L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
//     maxZoom: 19,
//     attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
// }).addTo(map);
//     const marker = L.marker([51.5, -0.09]).addTo(map);
// }

// onMounted(() => {
//   initMap();
// });

</script>

<style scoped>
/* #map {
    width: 50vw;
    height: 50vh;
} */
.wrap {
    display: flex;
    justify-content: center;
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