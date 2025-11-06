<script setup>
import { onMounted, ref } from 'vue';
    const name = ref('Programmer')
    const status = ref('active')
    const tasks = ref(['task1','task2'])
    const newTask = ref('The task')

    const tf = ()=>{
      console.log('hi')
      if(status.value=='active')
      {
        status.value = 'working'
      }
      else if(status.value=='working')
      {
        status.value = 'active'
      } 
    }

    const addTask = ()=>{
      console.log('hi')
      if(newTask.value.trim()!=='')
      {
        tasks.value.push(newTask.value)
        newTask.value = ''
      }
    }

    const deleteTask = (index)=>{
      tasks.value.splice(index,1)
    }

    onMounted(async ()=>{
      try{
        const responce = await fetch('https://jsonplaceholder.typicode.com/todos')
        const data = await responce.json()
        tasks.value = data.map((task)=>task.title
      )
      }
      catch(error)
      {
        console.log(error)
      }
    })
</script>

<template>
  <h1>Hello Vue!</h1>
  <p>I am {{ name }}</p>
  <p @click="tf">I am currently {{ status }}</p>
  <form @submit.prevent="addTask">
    <label for="newTask">Add task</label>
    <input type="text" name="newTask" id="newTask" v-model="newTask"/>
    <button type="submit">Submit</button>
  </form>
  <ul>
    <li v-for="(task,index) in tasks":key="task">
      <span>{{ task }}</span><button @click="deleteTask(index)">X</button>
    </li>
  </ul>
</template>

<style scoped></style>
