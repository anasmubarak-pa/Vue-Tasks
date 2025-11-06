<script setup>
  import InputText from 'primevue/inputtext';
  import FloatLabel from 'primevue/floatlabel';
  import Button from 'primevue/button';
  import { computed, defineProps } from 'vue';
  import { defineEmits } from 'vue';
  import { ref } from 'vue';
  const taskInputC = ref('')
  let props = defineProps({disable:{type:Boolean},
  changeInput:Function
})

const printc=()=>{
  console.log(props.taskInput)
}
  defineEmits(['addTask','confirm','discrad'])
  // console.log(props.tasks,props.status)
</script>
<template>
    <div class="input-and-button flex items-baseline">
      <FloatLabel class="mt-7">
        <InputText v-model="taskInputC" size="large"/>
        <label for="new-task">Enter a task and press submit</label> 
      </FloatLabel>
      <Button v-if="!props.disable" label="Add Task" icon="pi pi-plus" @click="$emit('addTask',taskInputC),taskInputC='',printc()" class="ml-2"/>
      <div v-else class="flex">
        <Button label="Confirm" icon="pi pi-check" @click="$emit('confirm')" class="ml-2"/>
        <Button label="Discrad" icon="pi pi-times" @click="$emit('discrad')" class="ml-2" severity="danger"/>
      </div>
    </div>
</template>