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

</script>

<style scoped>
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