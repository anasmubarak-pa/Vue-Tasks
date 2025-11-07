<script setup>
import InputField from './components/InputField.vue'
import { ref } from 'vue'

const weather = ref('load')
const weatherC = ref(0)
const day = ref('')
const getbg = (weather)=>{
  let url = new URL('./assets/video/',import.meta.url).href
  return `${url}/${weather}.mp4`
}
const ackUpdates = (w,wc,d)=>{
  weather.value = w
  weatherC.value = wc
  day.value = d
}
</script>

<template>
  <div class="flex flex-col w-full h-screen overflow-hidden justify-center  items-center" >
    <video autoplay muted loop class="absolute -z-10 h-screen" :key="weather">
        <source :src="getbg(weather)" type="video/mp4">
    </video>
    <InputField @update="ackUpdates"/>
    <div class="mt-7 font-bold text-7xl">{{weatherC}}°C</div>
    <div v-if="weather!='load'" class="mt-7 font-bold text-7xl">{{weather}} - {{day}}</div>
  </div>
</template>

<style scoped></style>