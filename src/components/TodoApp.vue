<template>
  <h2>TODO List</h2>

  <div v-if="!dialog" class="d-flex p-2">
    <button @click="dialog = true" class="btn btn-primary">Add Task</button> 
  </div>

  <div v-if="dialog" class="d-flex p-2">
    <input v-model="task" type="text" placeholder="Write Task" class="form-control mr-2">
    <input v-model="stat" type="text" placeholder="Select Status" class="form-control mx-2" readonly>    
    <div class="dropdown">
      <button class="btn btn-primary mx-2 dropdown-toggle" @click="show = true">Status</button>
      <ul class="dropdown-menu" :class="{ show: show }">
        <li><a class="dropdown-item" @click="stat = 'To-do', show = false">To-do</a></li>
        <li><a class="dropdown-item" @click="stat = 'In-Progress', show = false">In-progress</a></li>
        <li><a class="dropdown-item" @click="stat = 'Finished', show = false">Finished</a></li>
      </ul>
    </div>
    <button @click="submitTask" class="btn btn-success">Save</button> 
    <button @click="dialog = false" class="btn btn-danger mx-2">Close</button> 
  </div>

  <div class="d-flex p-2 mt-2">
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col"><b>Task</b></th>
          <th scope="col"><b>Status</b></th>
          <th scope="col" class="text-center"><b>Edit</b></th>
          <th scope="col" class="text-center"><b>Delete</b></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td style="width: 500px;">
            <input :class="{'finished': task.status === 'Finished'}" v-model="task.name" type="text" class="form-control ml-2 mr-4">
          </td>
          <td style="width: 200px;">
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
          <td style="width: 100px;">
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen" color="blue"> </span>  
            </div>
          </td>
          <td style="width: 100px;">
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
        stat:'',
        taskname:'',
        menu: -1,
        value: '',
        dialog: false,
        show: false,
        editItem: null,
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
      submitTask(){
        if (this.task.length === 0) return;
        if (this.editItem === null) {
          this.tasks.push({
            name: this.task,
            status: this.stat
          })          
          this.dialog = false;
        }
        else{
          this.tasks[this.editItem].name = this.task;
          this.editItem = null;
        }
        this.task = '';
        this.stat = '';
      },
      deleteTask(index: any) {
        this.tasks.splice(index,1);
      },
      editTask(index: any) {
        this.taskname = this.tasks[index].name;
        this.task = this.tasks[index].name;
        this.editItem = index;
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
    color:rgb(37, 224, 0);
  }
</style>
