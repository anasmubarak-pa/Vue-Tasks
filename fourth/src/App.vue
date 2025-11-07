<script setup>
import { onMounted, ref } from 'vue';
import InputArea from './components/inputArea.vue';
import TaskList from './components/taskList.vue';

const tasks = ref([])
const status = ref([])
const disable = ref(false)
let lastIndex = 0
let stateFunc

const saveData = ()=>{
  localStorage.setItem('taskList',JSON.stringify(new Array(tasks.value,status.value)))
}

onMounted(()=>{
  if(localStorage.getItem('taskList'))
  {
    [tasks.value,status.value] = JSON.parse(localStorage.getItem('taskList'))
  }
})

const addTask = (input,stateVal=1)=>{
  if(input.trim()!='')
  {
    tasks.value.push(input)
    status.value.push(stateVal)
  }
  saveData()
}
const confirm = (input)=>{
  if(input.trim()!='')
  {
    tasks.value[lastIndex] =  input
    discrad()
  }
}

const discrad = ()=>{
  disable.value = !disable.value
  saveData()
  stateFunc('')
}

const markDone = (index)=>{
  status.value[index] = !status.value[index]
  let statusState = status.value[index]
  let taskState = tasks.value[index]
  removeTask(index)
  addTask(taskState,statusState)
}

const editTask = (index)=>{
  disable.value = !disable.value
  stateFunc(tasks.value[index])
  lastIndex = index
  saveData()
}

const removeTask = (index)=>{
  tasks.value.splice(index,1)
  status.value.splice(index,1)
  saveData()
}

const updateVal = (func)=>{
  stateFunc = func
}
</script>

<template>
  <div class="ml-7 mt-4 flex-col justify-center items-center">
    <h1 class="mt-2 text-blue-100 text-4xl font-bold">Enter some tasks and press the button</h1>
    <InputArea @addTask="addTask" @confirm="confirm" @discrad="discrad" @inputAssign="updateVal" :disable="disable"/>
    <TaskList @markDone="markDone" @editTask="editTask" @removeTask="removeTask" :disable="disable" :tasks="tasks" :status="status"/>
  </div> 
</template>

<style scoped>
</style>