<template>
  <v-app>
    <v-app-bar
      app
      color="success"
      dark
    >
    <v-app-bar-title>
      Welcome to Tiv Sovithyea friendlist!
    </v-app-bar-title>
    </v-app-bar>

    <v-main>
      <v-row>
        <v-col class="col-md-4 co-sm-8">
          <v-card class="ma-5">
            <v-img
              src="https://scontent.fpnh4-1.fna.fbcdn.net/v/t39.30808-6/295743918_3324038374494643_8632169940825302980_n.jpg?_nc_cat=104&ccb=1-7&_nc_sid=09cbfe&_nc_eui2=AeEL6HIMa2d5KSVplP7PeqzK382XWcXDgtPfzZdZxcOC090jx6zxkm_32r_uOxpfgrbC1vdpGgvoXgHo5l37w4KQ&_nc_ohc=c_KGBP-hKcoAX-xq9Nq&_nc_ht=scontent.fpnh4-1.fna&oh=00_AT9myNUsfc1VtuYTD3aZOPTWzPdvB6zwyLucuy1ESKRXzQ&oe=62F5B9DC"
              height="450px"
            />
          </v-card>
        </v-col>
        <v-col class="col-sm-12 col-md-8">
          <v-card class="ma-5 shadow-6">
            <v-simple-table>
            <thead>
              <tr>
                <th>Name</th>
                <th>Edit</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i)  in todoList" :key="i">
                <td>{{item.name}}</td>
                <td>
                  <v-btn class="green accent-3" @click="handleEdit(item.id)">Edit</v-btn>
                </td>
                <td>
                  <v-btn color="red" @click="handleDelete(item.id)">Delete</v-btn>
                </td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <td class="col-md-8 col-sm-8">
                  <v-text-field
                    v-model="todoItem.name"
                    label= "Your Friend Name"
                    required
                  ></v-text-field>
                </td>
                <td class="col-md-4 col-sm-4">
                  <v-btn color="primary"
                   mx-1
                  @click="handleTodoItem"
                  >
                    {{editMode ? "edit" : "Add"}}
                  </v-btn>
                  <v-btn
                  v-if="editMode"
                  sm
                  class="mx-2"
                  color="red"
                  @click="handleCancel"
                  >
                  Cancel
                  </v-btn>
                </td>
              </tr>
            </tfoot>
          </v-simple-table>
          </v-card>
        </v-col>
      </v-row>      
    </v-main>
  </v-app>
</template>

<script>

import Axios from "axios";

const todoUrl = "http://localhost:3500/todo";

export default {
  name: 'App',

  data: () => ({
    todoList: [
    
    ],

    todoItem: {

    },

    editMode: false
  }),

  methods: {
    handleEdit(id) {
      this.editMode = true;
      this.todoItem = this.todoList.find((item) => item.id == id);
    },

    handleCancel() {
      this.editMode = false;
      this.todoItem = ""
    },

    async handleTodoItem() {
      const id = this.todoItem.id;
      if(this.editMode) {
        await Axios.put(`${todoUrl}/${id}`, this.todoItem);
        this.editMode = false;
        this.todoItem.name = "";
      } else {
        await Axios.post(todoUrl, this.todoItem);
        this.todoItem.name = "";
      }

      Axios.get(todoUrl).then(
          (response) => (this.todoList = response.data)
        );
    },
    async handleDelete(id) {
      await Axios.delete(`${todoUrl}/${id}`);
      Axios.get(todoUrl).then(
        (response) => (this.todoList = response.data)
      )
    }
  },
  
  created() {
    Axios.get(todoUrl).then((response) => (this.todoList = response.data));
  }
};
</script>
