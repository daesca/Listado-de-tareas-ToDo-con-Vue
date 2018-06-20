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
          <app-task v-for="(task, index) in tasks" :tasks="tasks" :task="task" :index="index" :key="index + 1"></app-task>
       </ul>
      </div>
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
      ]
    }
  },
  methods: {
    newTaskisEmpty(){
      if(!this.newTask == ''){
        return this.addToList();
      }
      return M.toast({html: 'No se permiten campos vacios', classes:'toast-down-right'});
     },
     addToList(){
      this.tasks.push({nameTask:this.newTask, stateComplete: false});
      this.newTask = '';
     },    
  }
}
</script>

<style lang="scss">
body{
  font-family:"Julius Sans One";
}
.toast-down-right{
  position:absolute;
  background-color: tomato;
  top:0;
  color: white;
  bottom: 50px !important;
  right: 20px !important;
  height: fit-content;
  padding: 1em;
  width: 400px;
  margin:0 auto;
  text-align:center;
}
  .main-container{
    display: flex;
    flex-direction: column;
    margin:0 auto;
    min-height: 400px;
    width: 500px;

    ul{
      list-style: none;
      padding-left: 0;
    }

    input[type="text"]{
      position: relative;
      width: 82%;
      vertical-align: middle;
      font-size: 16px;
      overflow: hidden;
      text-transform: uppercase;
    }

    button{
      background-color: #44BA81;
      border: none;
      padding: .3em 1em;
      color: white;
      font-weight: bold;
    }
    .group-tasks{
      background-color: #F2F2F2;
    }

  }
</style>