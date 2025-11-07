<script setup>
import ProgressSpinner from 'primevue/progressspinner';
import Button from "primevue/button"
import InputText from 'primevue/inputtext';
import { ref } from 'vue'

const KEY  = import.meta.env.VITE_VAL
const inputLocation = ref('')
const weather = ref('load')
const weatherC = ref(0)
const day = ref('')
const emit = defineEmits(['update'])
const loading = ref(false)
let preval = ''

const changeLocation = ()=>{
    if(inputLocation.value.trim()!=''&&inputLocation.value!=preval)
    {
        preval= inputLocation.value
        loading.value = true
      fetchWeather()
    }
  }
  async function fetchWeather() {
  try {
    let response = await fetch(`https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/${inputLocation.value.replaceAll(' ','%20')}?key=${KEY}`)
    if(response.ok)
    {
        response = await response.json()
        console.log(response.currentConditions)  
        weatherC.value = Math.round((response.currentConditions.temp-32)*5/9)
        getWeather(response.currentConditions.icon)
        loading.value = false
    }
  } catch (error) {
    console.log('error while fetching',error)
  }
}
const getWeather = (icon)=>{
  weather.value = icon.includes('partly')?icon.split('-')[1]:icon.split('-')[0]
  day.value = icon.includes('partly')?icon.split('-')[2]:icon.split('-')[1]
  emit('update',weather.value,weatherC.value,day.value)
}
</script>
<template>
     <div class="flex justify-center  items-center">
      <span class="mr-4 text-xl font-sans text-amber-100 font-bold">Enter a place and press the button :-</span><InputText type="text" v-model="inputLocation" size="large"/>
      <ProgressSpinner v-if="loading" stroke-width="7" style="width: 43px; height: 43px; margin-left: 8px;"/>
      <Button v-else label="Submit" @click="changeLocation" class="ml-2" size="large"/>
    </div>
</template>