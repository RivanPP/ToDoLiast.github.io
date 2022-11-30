<template>
    <div class="container">
        <h2 class="text-center mt-5">To do List con Vue</h2>
        
      
        <div class="d-flex">
            <input v-model="task" type="text" placeholder="Enter task" class="form-control">
            <button @click="submitTask" class="btn btn-warning rounded-0">Agregar</button>
        </div>

        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                  <th scope="col">Tarea</th>
                  <th scope="col">Estado</th>
                  <th scope="col" class="text-center">#</th>
                  <th scope="col" class="text-center">#</th>
                </tr>
            </thead>
            <tbody>
              <tr v-for="(task,index) in tasks" :key="index">
                  <td>
                      <span :class="{'finished': task.status ==='Finalizado'}">{{task.name}}</span>
                  </td>
                   <td style="width: 120px">
                    <span @click="changeStatus(index)" 
                    class="pointer" :class="{'text-danger':task.status === 'to-do',
                      'text-warning':task.status === 'En progreso',
                      'text-success':task.status === 'Finalizado'
                     }">
                     {{firstCharUpper(task.status)}}
                    </span>
                  </td>
                 <td>
                     <div class="text-center"  @click="editTask(index)">
                        <span class="fa fa-pen"></span>
                     </div>
                 </td>
                 <td>
                    <div class="text-center" @click="deleteTask(index)">
                      <span class="fa fa-trash"></span>
                    </div>
                 </td>
              </tr>            
            </tbody>
         </table>

    </div>
</template>


<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
    data(){
         return{
             task:'Ingresa tarea',
             editedTask: null,
             availableStatuses: ['to-do','En progreso','Finalizado'],
             tasks:[
              {
                name:'Tarea',
                status: 'to-do'
              },
              {
                name:'Tarea 2',
                status: 'En progeso'
              }
            ]
         }
    },
      methods:{
        submitTask(){
           if(this,this.task.length === 0) return;

           if(this.editedTask==null){
           this.tasks.push({
               name: this.task,
               status: 'to-do'
           });
          }else{
            this.tasks[this.editedTask].name=this.task;
            this.editedTask = null;
          }

           this.task ='';
        },
         deleteTask(index){
              this.tasks.splice(index,1);
         },
         editTask(index){
             this.task=this.tasks[index].name;
             this.editedTask = index;
         },
         changeStatus(index){
             let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
             if(++newIndex > 2) newIndex = 0;
             this.tasks[index].status = this.availableStatuses[newIndex];
         },
         firstCharUpper(str){
            return str.charAt(0).toUpperCase() + str.slice(1);
         }
      }
};
</script>

<style scoped>
      .pointer{
        cursor: pointer;
      }
      .finished{
         text-decoration: line-through;
      }
</style>
