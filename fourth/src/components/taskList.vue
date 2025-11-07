<script setup>
  import Button from 'primevue/button';
  import { computed, defineProps,defineEmits } from 'vue';
  defineEmits(['markDone','editTask','removeTask'])
  let props = defineProps({disable:{type:Boolean},tasks:{type:Array},status:{type:Array}})
</script>
<template>
    <ul class="mt-2">
      <li v-for="(task,index) in tasks":key=index  class="flex items-center text-4xl font-bold cursor-pointer">
        <span v-if="status[index]">{{ task }}</span>
        <span v-else class="opacity-50">{{ task }}</span>
        <div>
          <Button v-if="status[index]" icon="pi pi-check" @click="$emit('markDone',index)" class=" ml-1" severity="primary" :disabled="disable" rounded/>
          <Button v-else icon="pi pi-undo" @click="$emit('markDone',index)" class=" ml-1" severity="primary" :disabled="disable" rounded/>
          <Button icon="pi pi-pencil" v-if="status[index]" @click="$emit('editTask',index)" class=" ml-1" :disabled="disable" severity="contrast" rounded/>
          <Button icon="pi pi-trash" @click="$emit('removeTask',index)" class=" ml-1" :disabled="disable" severity="danger" rounded/>
        </div>
      </li>
    </ul>
</template>