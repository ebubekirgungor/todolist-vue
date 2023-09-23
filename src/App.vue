<template>
  <div class="text-center">
    <v-btn color="primary" class="my-16" @click="add_dialog = true">
      Add Todo
    </v-btn>
    <v-card class="text-left mx-auto" width="390px" >
      <v-table>
        <tbody>
          <tr style="height: 10px" v-for="todo in todos" :key="todo.id">
            <td>
              <v-checkbox
                @click="checkbox_toggle()"
                class="checkbox"
                v-model="todo.done"
              ></v-checkbox>
            </td>
            <td>{{ todo.text }}</td>
            <td style="width: 0px">
              <v-btn
                @click="open_edit_dialog(todo.id)"
                icon="mdi-text-box-edit-outline"
                size="small"
              ></v-btn>
            </td>
            <td style="width: 0px">
              <v-btn
                @click="delete_todo(todo.id)"
                icon="mdi-trash-can-outline"
                size="small"
              ></v-btn>
            </td>
          </tr>
        </tbody>
      </v-table>
    </v-card>
  </div>
  
  <v-dialog v-model="add_dialog" width="400px">
    <v-card>
      <v-card-title class="text-h5 card-title"> Add Todo </v-card-title>
      <v-row justify="center" class="ma-7">
        <v-text-field
          class="text-field"
          v-model="todo_text"
          required
        ></v-text-field>
      </v-row>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" @click="add_todo">Add</v-btn>
        <v-btn color="primary" @click="add_dialog = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

  <v-dialog v-model="edit_dialog" width="400px">
    <v-card>
      <v-card-title class="text-h5 card-title"> Edit Todo </v-card-title>
      <v-row justify="center" class="ma-7">
        <v-text-field
          class="text-field"
          v-model="update_text"
          required
        ></v-text-field>
      </v-row>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" @click="edit_todo(edit_id)">Edit</v-btn>
        <v-btn color="primary" @click="edit_dialog = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  data() {
    return {
      todos: JSON.parse(localStorage.getItem("todos")) || [],
      add_dialog: false,
      edit_dialog: false,
      edit_id: 0,
    };
  },
  methods: {
    update_localstorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    add_todo() {
      if (this.todo_text === "") {
        return;
      }
      this.todos.push({
        id: Date.now(),
        text: this.todo_text,
        done: false,
      });
      this.update_localstorage();
      this.todo_text = "";
    },
    edit_todo(id) {
      if (this.update_text === "") {
        return;
      }
      this.todos.find((todo) => todo.id === id).text = this.update_text;
      this.edit_dialog = false;
      this.update_localstorage();
    },
    delete_todo(id) {
      this.todos.splice(
        this.todos.findIndex((todo) => todo.id === id),
        1
      );
      this.update_localstorage();
    },
    checkbox_toggle() {
      setTimeout(() => this.update_localstorage(), 200);
    },
    open_edit_dialog(id) {
      this.edit_dialog = true;
      this.update_text = this.todos.find((todo) => todo.id === id).text;
      this.edit_id = id;
    },
  },
};
</script>
<style>
html {
  overflow: hidden !important;
}
body {
  background-color: #f5f5f5;
}
.v-table__wrapper {
  overflow: hidden !important;
}
.checkbox {
  margin-right: -35px;
  margin-bottom: -21px;
}
.card-title {
  margin-top: 20px;
  pointer-events: none;
  user-select: none;
  text-align: center;
}
.text-field {
  width: 300px;
}
</style>
