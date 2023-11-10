<script setup>
    import map from '../assets/map.svg';
    import marker from '../assets/marker.svg';
</script>

<template>
    <div id="map">
        <div>
            <img :src="map">
            <img :src="marker" class="marker" id="marker-1">
            <img :src="marker" class="marker" id="marker-2">
            <img :src="marker" class="marker" id="marker-3">
            <img :src="marker" class="marker" id="marker-4">
            <img :src="marker" class="marker" id="marker-5">
            <img :src="marker" class="marker" id="marker-6">
            <img :src="marker" class="marker" id="marker-7">
        </div>
    </div>
</template>

<style scoped>
#map {
    background-color: #A8D0E7;
    width: 23.65%;
    height: 23.4%;
    position: absolute;
    left: 46.6%;
    top: 33%;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: -1;
}
#map > div {
    position: relative;
}
#map > div > img:first-child {
    width: 90%;
    height: 90%;
}
.marker {
    position: absolute;
    height: 1.5vw;
}
#marker-1 {
    left: 14%;
    top: 49%;
}
#marker-2 {
    left: 23.5%;
    top: 21%;
}
#marker-3 {
    left: 39.6%;
    top: 31%;
}
#marker-4 {
    left: 67%;
    top: 19%;
}
#marker-5 {
    left: 72.5%;
    top: 42%;
}
#marker-6 {
    left: 52%;
    top: 68%;
}
#marker-7 {
    left: 33%;
    top: 67%;
}
</style>