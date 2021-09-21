<template>
  <v-app>
    <v-main>
      <v-container class="mt-10">
        <v-card elevation="2" class="mx-auto" max-width="344" shaped>
          <v-card-title>Ma To Do List</v-card-title>
          <v-card-subtitle v-if="tasks.length === 0"
            >Votre liste est vide</v-card-subtitle
          >
          <v-list dense>
            <v-list-item-group>
              <v-list-item v-for="(task, index) in tasks" :key="index">
                <v-list-item-content>
                  <v-list-item-title class="font-weight-black"
                    >{{ task.name }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{
                    task.description
                  }}</v-list-item-subtitle>
                </v-list-item-content>
                <v-card-actions>
                  <v-checkbox
                    v-model="task.isArchive"
                    color="success"
                    class="mt-5"
                    @change="archiveTask(task, index)"
                  ></v-checkbox>
                  <v-btn
                    icon
                    rounded
                    dark
                    @click="openEditorDialog(task, index)"
                  >
                    <v-icon color="orange" right>mdi-pencil</v-icon>
                  </v-btn>
                </v-card-actions>
                <v-icon color="red" right @click="removeTask(index)"
                  >mdi-delete</v-icon
                >
              </v-list-item>
            </v-list-item-group>
          </v-list>
          <v-card-actions class="mt-10">
            <v-row justify="center">
              <v-btn
                rounded
                color="light-green"
                dark
                @click.stop="dialog = true"
              >
                Ajouter une tâche
              </v-btn>
            </v-row>
          </v-card-actions>

          <v-dialog v-model="dialog" max-width="400">
            <Form
                  title="Créer une tâche"
                  @save="addTask"
            ></Form>
          </v-dialog>

          <v-dialog v-model="edit" max-width="400">
            <Form
              title="Modifier mon formulaire"
              :task="editingTask"
              @save="editTask"
            ></Form>
          </v-dialog>

          <v-snackbar v-model="saveMsg" :timeout="timeout">
            Votre tâche est bien enregistrée
             <Snackbar>

            </Snackbar>
          </v-snackbar>

          <v-snackbar v-model="deleteMsg" :timeout="timeout">
            Votre tâche est bien supprimée
            <template v-slot:action="{ attrs }">
              <v-btn
                color="blue"
                text
                v-bind="attrs"
                @click="deleteMsg = false"
              >
                Fermer
              </v-btn>
            </template>
          </v-snackbar>

          <v-snackbar v-model="editMsg" :timeout="timeout">
            Votre tâche est bien modifiée
            <template v-slot:action="{ attrs }">
              <v-btn color="blue" text v-bind="attrs" @click="editeMsg = false">
                Fermer
              </v-btn>
            </template>
          </v-snackbar>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Form from "~/components/Form.vue";
import Snackbar from "~/components/Snackbar.vue";

export default {
  name: "indexTodoList",

  components: { Form, Snackbar },

  data() {
    return {
      dialog: false,
      taskName: "",
      taskDescription: "",
      tasks: [],
      editMsg: false,
      deletetMsg: false,
      saveMsg: false,
      edit: false,
      editingTask: {},
      indexEditingTask: undefined,
      tasksArchive: [],
      timeout: 2000,
    };
  },

  methods: {
    addTask(completeTask) {

      this.tasks.push({...completeTask, isArchive: false});
      this.dialog = false;
      this.taskName = "";
      this.taskDescription = "";
      this.saveMsg = true;
    },

    openEditorDialog(task, index) {
      this.editingTask = task;
      this.indexEditingTask = index;
      this.edit = true;
    },

    editTask(task) {
      this.tasks.splice(this.indexEditingTask, 1, {...task});
      this.editMsg = true;
      this.edit = false;
      this.editingTask = {};
      this.indexEditingTask = undefined;
    },

    removeTask(index) {
      this.tasks.splice(index, 1);
      this.deleteMsg = true;
    },

    archiveTask(task) {
      if (task.isArchive === true) {
        this.tasksArchive.push(task);
      } else {
        const indexArchive = this.tasksArchive.indexOf(task);
        this.tasksArchive.splice(indexArchive, 1);
      }
    }
  }
};
</script>
