<template>
  <div class="text-center">
    <v-btn color="primary" class="my-16" @click="add_dialog = true"> Add Todo </v-btn>
    <v-card class="mx-auto my-4" width="400px">
      <v-table>
        <tbody>
          <tr v-for="todo in todos" :key="todo.text">
            <td>{{ todo.text }}</td>
            <v-row align="center" justify="center">
            <v-col cols="auto">
            <v-btn icon="mdi-text-box-edit-outline" size="x-small"></v-btn>
            <v-btn icon="mdi-trash-can-outline" size="x-small"></v-btn>
          </v-col>
        </v-row>
          </tr>
        </tbody>
      </v-table>
    </v-card>
  </div>
  <v-dialog v-model="add_dialog" width="150px">
    <v-card>
      <v-card-title class="text-h5">
          Add Todo
        </v-card-title>
        <v-text-field v-model="todo_text"
                  required
                ></v-text-field>
      <v-card-actions>
        <v-btn color="primary" @click="add_todo">Add</v-btn>
        <v-btn color="primary" @click="add_dialog = false">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      add_dialog: false,
    };
  },
  methods: {
    add_todo(todo) {
      this.todos.push({
        id: Date.now(),
        text: this.todo_text,
        done: false,
      });
    },
  },
};
</script>

<style>
body {
  background-color: #f5f5f5;
}
</style>
