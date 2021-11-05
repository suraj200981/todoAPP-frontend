  <template>
  <v-card  color="basil" style="width:50%;">
    <v-card-title class="text-center justify-center py-6">
      <h2 class="font-weight-bold text-h3 basil--text">
        What would you like to do?
      </h2>
    </v-card-title>

    <v-tabs
      v-model="tab"
      background-color="transparent"
      color="basil"
      grow
      style="height:300px!important;"
    >
      <v-tab>
        View all todos
      </v-tab>
 

      <v-tab-item style="overflow-y:scroll; overflow-x: hidden; /* Hide vertical scrollbar */">

        <v-row>
          <v-col>
          <v-card-text height="300px" v-for="todo in todoList" :key="todo"><b>Task: </b>{{todo.name}} <br>
          <b>Date:</b> {{todo.date}} <br>
          <b>Status:</b> <span v-if="todo.status=='Done'"  style="color: green; ">{{todo.status}}</span> <span v-if="todo.status=='ND'"  style="color: red; ">{{todo.status}}</span> <br>
          </v-card-text>
          </v-col>
          <v-col>
        <v-card-text v-for="todo in todoList" :key="todo">

          <v-btn elevation="50" x-small color="green" @click="getCurrentVals(todo.id)" v-bind="attrs" v-on="on"><v-icon>mdi-check</v-icon></v-btn><br>

          <v-btn elevation="2" x-small color="red" @click="deleteTodo(todo.id)"><v-icon>mdi-delete-empty-outline</v-icon></v-btn>
          </v-card-text>
        </v-col>
        </v-row>
      </v-tab-item>


 <v-tab>
        create
      </v-tab>
      <v-tab-item>
        <br>
          <v-form @submit="createTodo" method="post">
            <h4>Enter task:</h4>
            <v-text-field type="text" name="taskTodo" v-model="create.name"></v-text-field>
            <h4>Enter date:</h4>
            <v-text-field type="text" name="taskDate" v-model="create.date"></v-text-field>
            <h4>Status:</h4>
            <v-text-field type="text" name="taskStatus" v-model="create.status"></v-text-field>
             <v-btn
      color="success"
      type="submit"
    >
      Create todo
    </v-btn>
          </v-form>
      </v-tab-item>

       </v-tabs>
       <br>
  </v-card>
</template>


<script>

import Vue from 'vue';
import axios from 'axios';
import VueAxios from 'vue-axios';

Vue.use(VueAxios,axios)

  export default {
    name: 'mainForms',

    data ()  {
      return {
              dialog: false,

        //get all
        todoList: Array,
        //update todoList
        updateLits: Array,
        //create
        create:{
          id:null,
          name:null,
          date:null,
          status:null
        },
        //update
         currentTodo:{
          id:null,
          name:null,
          date:null,
          status:null       
           },
        items: [
          'View all todos', 'Create',
        ],
        componentKey: 0,

      }
    },
      //READ
       mounted(){
        Vue.axios.get('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/')
        .then((resp)=>{

          this.todoList = resp.data;
            console.warn(resp.data);
        })
        .catch((err)=>{
          console.error(err);
        });

        
    },

    methods:{
      //READ BY ID
      getTodoId(e){
          Vue.axios.get('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/'+e)
          .then((resp)=>{ 

              this.updateList = resp.data;
                console.warn("Get by ID successfull");
                console.warn(resp);

            })            
          },

        //CREATE
        createTodo(e) {
              var temp = this.todoList.slice(-1)[0];
              var newID = parseInt(temp.id)+1;

              this.create.id = newID;
              
              Vue.axios.post('https://todoapisurajsharmaappservice.azurewebsites.net/create/', this.create)
              .then((resp)=>{

                  console.warn("Post successfull");
                  console.warn(resp);
              })
    location.reload()

            e.preventDefault();
          
        },

        //DELETE
        deleteTodo(e){
          Vue.axios.delete('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/'+e)
          .then((resp)=>{

              console.warn("Delete successfull");
              console.warn(resp);
          })

           location.reload();
        },
        //UPDATE
        updateTodo(t){
          Vue.axios.put('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/'+t,{
            
           id: this.currentTodo.id,
           name: this.currentTodo.name,
           date: this.currentTodo.date,
            status: "Done"
          })
          .then((resp)=>{
                        
                console.warn("Update successfull");
                console.warn(resp);

                location.reload();
            })          

        },  

         //UPDATE
        getCurrentVals(t){

          Vue.axios.get('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/'+t)
          .then((resp)=>{
                        
              this.currentTodo.id = resp.data[0].id;
              this.currentTodo.name = resp.data[0].name; 
              this.currentTodo.date = resp.data[0].date;
              this.currentTodo.status = resp.data[0].status;

                console.warn("get by id succesfull");
                            this.updateTodo(t);

            })          


        }, 

    }
  }
   
</script>
<style scoped>
.v-btn:not(.v-btn--round).v-size--default {

    margin-top: 10px!important;

} 

div.v-window__container {
   
    height: 300px!important;
}
.col{
  height: 400px!important;
}

@media only screen and (max-width: 600px) {
  .v-card{
    width: 100%!important;
  }
 .v-application .text-h3 {
    font-size: 30px!important;
 }
}
.v-btn{
  min-width: 0px!important;
    width: 62px!important;
    height: 32px!important;
}

</style>
