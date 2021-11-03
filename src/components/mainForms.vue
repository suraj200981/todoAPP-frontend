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
      <v-tab
        v-for="item in items"
        :key="item"
      >
        {{ item }}
      </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item
        v-for="item in items"
        :key="item"
      >
        <v-card
          color="basil"
          flat
        >
          <v-card-text v-for="todo in todoList" :key="todo"><b>Task: </b>{{todo.name}} <br>
          <b>Date:</b> {{todo.date}} <br>
          <b>Status:</b> <span v-if="todo.status=='Done'"  style="color: green; ">{{todo.status}}</span> <span v-if="todo.status=='ND'"  style="color: red; ">{{todo.status}}</span> <br>
          </v-card-text>
        </v-card>
      </v-tab-item>
    </v-tabs-items>
  </v-card>
</template>


<script>

import Vue from 'vue';
import axios from 'axios';
import VueAxios from 'vue-axios';

Vue.use(VueAxios,axios)

  export default {
    name: 'mainForms',
    mounted(){
        Vue.axios.get('https://todoapisurajsharmaappservice.azurewebsites.net/api/todos/')
        .then((resp)=>{

          this.todoList = resp.data;
            console.warn(resp.data);
        })
    },

    data ()  {
      return {
        todoList: Array,
        tab: null,
        items: [
          'View all todos', 'Create',
        ],
        text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
      }
    }
  }
   
</script>
