<script setup lang="ts">
  defineProps<{msg: string}>()
</script>

<template>
  <h1>TODO List</h1>
  <h4>{{ msg }}</h4>

  <div class="d-flex p-2">
     <input v-model="task" type="text" placeholder="Introduzca tarea" class="form-control mx-2">
     <button @click="submitTask" class="btn btn-warning">Enviar</button> 
  </div>

  <!-- <div>
    <v-data-table headers="headers" items="tasks">
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editTask(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deleteTask(item)"
        >
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
  </div>  -->


  

  <div class="d-flex p-2 mt-2">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td style="width: 350px;">
          <input :class="{'finished': task.status === 'Finished'}" v-model="task.name" type="text" class="form-control ml-2 mr-4">
          </td>
          <td>
              <div class="dropdown">
                <button class="btn dropdown-toggle" :id=index.toString() @click="openMenu(index)" 
                  :class="{ 
                    'todo': tasks[index].status === 'To-do',
                    'inprogress': tasks[index].status === 'In-Progress',
                    'end': tasks[index].status === 'Finished'                  
                  }"
                >
                  {{ task.status }}
                </button>
                <ul class="dropdown-menu" :class="{ show: menu === index }">
                  <li><a class="dropdown-item" @click="changeStatus(index, value = 'todo')">To-do</a></li>
                  <li><a class="dropdown-item" @click="changeStatus(index, value = 'inprogress')">In-progress</a></li>
                  <li><a class="dropdown-item" @click="changeStatus(index, value = 'finished')">Finished</a></li>
                </ul>
              </div>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen" color="blue"> </span>  
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash" color="red"> </span>  
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script lang="ts">
  export default{
    data(){
      return {
        task:'',
        menu: -1,
        value: '',
        editItem:null,
        avaliableStatus: ['To-do','In-Progress','Finished'],
        showMenu: false,
        tasks: 
        [
          {
          name: 'Comprar platanos',
          status: 'To-do'
          },
          {
          name: 'Comprar Pan',
          status: 'In-Progress'
          },
        ]
      }
    },
    methods: {
      submitTask(){
        if (this.task.length === 0) return;
        if (this.editItem === null) {
          this.tasks.push({
            name: this.task,
            status: 'To-do'
          })          
        }
        else{
          this.tasks[this.editItem].name = this.task;
          this.editItem = null;
        }
        this.task = '';
      },
      deleteTask(index: any) {
        this.tasks.splice(index,1);
      },
      editTask(index: any) {
        this.task = this.tasks[index].name;
        this.editItem = index;
      },
      changeStatus(index: any, value: string){
        if (value === 'todo') {
          this.tasks[index].status = 'To-do';
          this.menu = -1;
        } else if (value === 'inprogress') {
          this.tasks[index].status = 'In-Progress';
          this.menu = -1;
        }
        else{
          this.tasks[index].status = 'Finished';
          this.menu = -1;
        }
      },   
      openMenu(index: any) {
        if (index === this.menu) {
          this.menu = -1;
        }
        else{
          this.menu = index;
        }
      },
    }
  }
</script>
<style>
  @media (prefers-color-scheme: dark) {
      body 
      {
          background: #fff !important;
          color: black !important;
      }
  }
  .pointer
  {
    cursor: pointer;
  }
  .finished
  {
    font-weight: bolder;
    text-decoration: line-through;
  }
  .todo
  {
    font-weight: bolder;
    color:red;
  }  
  .inprogress
  {
    font-weight: bolder;
    color:orange;
  }  
  .end
  {
    font-weight: bolder;
    color:greenyellow;
  }
</style>
