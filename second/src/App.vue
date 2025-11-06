<script>
export default{
  data(){
    return{
      name:'Programmer',
      status:true,
      active:'active',
      tasks: ['Task 1','Task 2','Task 3','Task 4'],
      link:'https://vuejs.org/'
    }
  },
  methods:{
    printHi(){
      console.log('hi')
    }
  }
}
</script>

<template>
  <h1>Hello Vue!</h1>
  <p>
    hi i am {{ name }}
  </p>
  <p v-if="status">True</p>
  <p v-else="status">True</p>
  <p v-if="active=='active'">Active</p>
  <p v-else-if="active"></p>
  <h3>Tasks:</h3>
  <ul>
    <li v-for="task in tasks":key="task" @click="printHi">{{ task }}</li>
  </ul>
  <a v-bind:href="link">Vue1</a>
  <a :href="link">Vue2</a>
  <p v-on:click="printHi"></p>
</template>

<style scoped></style>