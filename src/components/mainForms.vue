  <template>
  <v-card color="basil" style="width:50%">
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
    >
      <v-tab>
        View all todos
      </v-tab>
 

      <v-tab-item>
          <v-card-text v-for="todo in todoList" :key="todo"><b>Task: </b>{{todo.name}} <br>
          <b>Date:</b> {{todo.date}} <br>
          <b>Status:</b> <span v-if="todo.status=='Done'"  style="color: green; ">{{todo.status}}</span> <span v-if="todo.status=='ND'"  style="color: red; ">{{todo.status}}</span> <br>
          </v-card-text>
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
        //get all
        todoList: Array,
        //create
        create:{
          id:null,
          name:null,
          date:null,
          status:null
        },
        items: [
          'View all todos', 'Create',
        ],
      }
    },

       mounted(){
        Vue.axios.get('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/')
        .then((resp)=>{

          this.todoList = resp.data;
            console.warn(resp.data);
        })
    },

    methods:{

        createTodo(e) {
              var temp = this.todoList.slice(-1)[0];
              var newID = parseInt(temp.id)+1;

              this.create.id = newID;
              
              Vue.axios.post('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/create/', this.create)
              .then((resp)=>{

                  console.warn("Post successfull");
                  console.warn(resp);
              })

            e.preventDefault();
        }

    }
  }
   
</script>
