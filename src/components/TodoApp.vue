<template>
  <h2>TODO List</h2>
  <!-- START Modal -->
    <div>
      <button class="btn btn-primary mx-2 my-2" @click="showModal = true">Add Task</button>
      <BasicModal v-if="showModal" @closeModal="closeModal">
        <template v-slot:header>
          <h2>Create task</h2>
        </template>
        <template v-slot:body>          
          <div class="d-flex">
            <input v-model="task" type="text" placeholder="Write Task" class="form-control mx-2">
            <td>
              <div class="dropdown">
                <button class="btn btn-primary dropdown-toggle ml-2" @click="show = true">{{ stat }}</button>      
                <ul class="dropdown-menu" :class="{ show: show }">
                  <li><a class="dropdown-item" @click="stat = 'To-do', show = false">To-do</a></li>
                  <li><a class="dropdown-item" @click="stat = 'In-Progress', show = false">In-progress</a></li>
                  <li><a class="dropdown-item" @click="stat = 'Finished', show = false">Finished</a></li>
                </ul>
              </div>
            </td>
            <button @click="submitTask" class="btn btn-success mx-2">Save</button> 
          </div>
        </template>
      </BasicModal>
    </div>
  <!-- END Modal -->
  <!-- START Table with Status, Delete and Update -->
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
  <!-- END Table with Status, Delete and Update -->
</template>
<script lang="ts">

  import BasicModal from "./BasicModal.vue";
  export default{
  components: { BasicModal },
    data(){
      return {
        menu: -1,
        value: '',
        task:'',
        stat: 'To-do',
        editItem: null,
        show: false,
        dialog: false,
        showModal: false,
        tasks: [
          {
            name: '',
            status: ''
          }
        ],
      }
    },  
    mounted() {
      // Limpiamos el localStorage
      localStorage.clear();
      // Grabamos los datos del listado
      this.saveInitialData();
    },
    methods: {
      saveInitialData(){
        const taskData = 
        [
          {
            name: 'Leer libros',
            status: 'To-do'
          },
          {
            name: 'Aprender Ts y Vue Js',
            status: 'In-Progress'
          },
          {
            name: 'Hacer yoga',
            status: 'Finished'
          },
        ];
        const storedTasks = localStorage.getItem('tasks');
        this.tasks = storedTasks ? JSON.parse(storedTasks) : taskData;
      },
      openMenu(index: any) {
        if (index === this.menu) {
          this.menu = -1;
        }
        else{
          this.menu = index;
        }
      },
      closeMenu(){
        this.dialog = false;
        this.showModal = false;
        this.clearData();
      },
      closeModal(){
        this.dialog = false;
        this.showModal = false;
        this.clearData();
      },
      clearData(){
        this.task = '';
        this.stat = 'To-do';
      },
      submitTask(){
        if (this.task.length === 0) return;
        if (this.editItem === null) {
          this.tasks.push({
            name: this.task,
            status: this.stat
          })
        }
        else{
          this.tasks[this.editItem].name = this.task;
          this.tasks[this.editItem].status = this.stat;
          this.editItem = null;
        }

        // Almacenar en localStorage
        localStorage.setItem('tasks', JSON.stringify(this.tasks));

        this.dialog = false;
        this.showModal = false;
        this.clearData();
      },
      editTask(index: any) {
        this.showModal = true;
        this.dialog = true;
        this.task = this.tasks[index].name;
        this.stat = this.tasks[index].status;
        this.editItem = index;
      },
      deleteTask(index: any) {
        this.tasks.splice(index,1);        

        // Actualizar localStorage
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
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
      }
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
