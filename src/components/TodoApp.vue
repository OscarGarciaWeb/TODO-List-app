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

  <div class="d-flex p-2 mt-2">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td style="width: 350px;">
            <span :class="{'finished': task.status === 'Finished'}">{{task.name}}</span>
          </td>
          <td style="width: 200px;">
            <span @click="changeStatus(index)" class="pointer" 
              :class="
                {
                  'todo': task.status === 'To-do',
                  'inprogress': task.status === 'In-Progress',
                  'end': task.status === 'Finished'                  
                }"
              > 
                {{task.status}} 
              </span>
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
      return{
        task:'',
        editItem:null,
        avaliableStatus: ['To-do','In-Progress','Finished'],

        tasks: 
        [
          {
          name: 'comprar platanos',
          status: 'To-do'
          },
          {
          name: 'comprar Pan',
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
      changeStatus(index: any){
        let newIndex = this.avaliableStatus.indexOf(this.tasks[index].status);
        if (++newIndex >2) newIndex = 0;
        this.tasks[index].status = this.avaliableStatus[newIndex];
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
