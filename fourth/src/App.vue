<script setup>
import { ref } from 'vue';
import inputArea from './components/inputArea.vue';
import Button from 'primevue/button';

const taskInput = ref('')
const tasks = ref([])
const status = ref([])
const disable = ref(false)
let lastIndex = 0

  const addTask = (input)=>{
    // console.log('hello',tasks.value,input)
    if(input.trim()!='')
    {
      tasks.value.push(input)
      status.value.push(1)
      taskInput.value=''
      // console.log(status.value)
    }
}
  const confirm = (input)=>{
  if(taskInput.value.trim()!='')
  {
    disable.value = !disable.value
    tasks.value[lastIndex] =  input
    taskInput.value=''
  }
}

const discrad = ()=>{
  disable.value = !disable.value
  taskInput.value=''
}
const markDone = (index)=>{
  // console.log(e,index)
  status.value[index] = !status.value[index]
  let statusState = status.value[index]
  let taskState = tasks.value[index]
  tasks.value.splice(index,1)
  status.value.splice(index,1)
  // console.log(tasks.value)
  status.value.push(statusState)
  tasks.value.push(taskState)
  // console.log(e.target.parentElement.parentElement)

}



const editTask = (index)=>{
  disable.value = !disable.value
  // taskInput.value = tasks.value[index]
  lastIndex = index
  // console.log(e)
}

const removeTask = (index)=>{
  // console.log(e)
  // console.log(index)
  tasks.value.splice(index,1)
  status.value.splice(index,1)
}
</script>

<template>
  <div class="ml-7 mt-4 flex-col justify-center items-center">
    <h1 class="mt-2 text-blue-100 text-4xl font-bold">Enter some tasks and press the button</h1>
    <inputArea @addTask="addTask" @confirm="confirm" @discrad="discrad" :disable="disable" :task-input="taskInput"/>
    <ul class="mt-2">
      <li v-for="(task,index) in tasks":key=index  class="flex items-center text-4xl font-bold cursor-pointer">
        <span v-if="status[index]">{{ task }}</span>
        <span v-else class="opacity-50">{{ task }}</span>
        <div>
          <Button v-if="status[index]" icon="pi pi-check" @click="markDone(index)" class=" ml-1" severity="primary" :disabled="disable" rounded/>
          <Button v-else icon="pi pi-undo" @click="markDone(index)" class=" ml-1" severity="primary" :disabled="disable" rounded/>
          <Button icon="pi pi-pencil" v-if="status[index]" @click="editTask(index)" class=" ml-1" :disabled="disable" severity="contrast" rounded/>
          <Button icon="pi pi-trash" @click="removeTask(index)" class=" ml-1" :disabled="disable" severity="danger" rounded/>
        </div>
      </li>
    </ul>
  </div> 
</template>

<style scoped>
</style>
