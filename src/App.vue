<template>
  <div id="app" class="main-container">  
  <h1 class="text-center">Lista de Tareas</h1>
    <form class="text-center">
      <input v-model="newTask" type="text">
      <button @click.prevent="newTaskisEmpty">Agregar</button>
      <!-- <p v-show="empty" class="text-red">No se permite enviar tareas vacías</p> -->
    </form>
    <div v-if="tasks.length != 0">
      <h2 class="text-center">Tareas ingresadas</h2>
      <div id="actual-tasks" class="group-tasks">
        <ul>
          <app-task v-for="(task, index) in tasks" :tasks="tasks" :task="task" :index="index" :key="index + 1" @error="showError"></app-task>
       </ul>
      </div>
    </div>
    <div class="error-message" :class=" error ? 'active-error': ''">
        <p>{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
import Task from './Task.vue';
export default {
  components:{
    'app-task': Task
  },
  data () {
    return {
      newTask:'',
      tasks:[
          /*
          Esquema de una tarea

          {
           nameTask: "",
           stateComplete: false,
           stateEdit: false
          }
          */
      ],
      error: false,
      errorMessage: ''
    }
  },
  methods: {
    newTaskisEmpty(){
      if(!this.newTask == ''){
        return this.addToList();
      }
      return this.showError('El campo no puede estar vacio');
     },
     addToList(){
      this.tasks.push({nameTask:this.newTask, stateComplete: false});
      this.newTask = '';
     },
     showError(message){
      this.errorMessage = message;
      this.error = true;
      setTimeout(() => {
        this.error = false;
      }, 3000);
     }
  }
}
</script>

<style lang="scss">
body{
  font-family:"Julius Sans One";
  position: relative;
}
  .main-container{
    display: flex;
    flex-direction: column;
    margin:0 auto;
    min-height: 400px;

    @media(min-width: 768px){
        width: 500px;  
    }

    ul{
      list-style: none;
      padding-left: 0;
    }

    input[type="text"]{
      position: relative;
      width: 78%;
      vertical-align: middle;
      font-size: 16px;
      overflow: hidden;
      text-transform: uppercase;
      @media (max-width: 1024px){
        width: 65%;
      }
    
    }

    button{
      background-color: #44BA81;
      border: none;
      padding: .3em 1em;
      color: white;
      font-weight: bold;
      cursor: pointer;
      text-transform: uppercase;
    }
    .group-tasks{
      background-color: #F2F2F2;
    }
    .error-message{
      background-color: tomato;
      color: white;
      text-align: center;
      margin-top: 5em;
      transition: opacity .5s;
      opacity: 0;
      
    }
    .active-error{
      opacity: 1;
    }

  }
</style>