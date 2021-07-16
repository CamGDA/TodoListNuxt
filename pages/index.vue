<template>
  <v-app>
    <v-main>
      <v-container class="mt-10">
        <v-card elevation="2" class="mx-auto" max-width="344" shaped>
          <v-card-title>Ma To Do List</v-card-title>
          <v-card-subtitle v-if="tasks.length === 0">Votre liste est vide</v-card-subtitle>
          <v-list dense>
            <v-list-item-group>
              <!-- Pour chaque task dans mon tableau de tasks[] // On l'a appelé task, mais on aurait pu l'appeler tache-->
              <v-list-item v-for="(task, index) in tasks" :key="index">
                <v-list-item-content>
                  <v-list-item-title class="font-weight-black">{{ task.name }} </v-list-item-title>
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
                  <v-btn icon
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

          <!-- Création tâches-->
          <v-dialog v-model="dialog" max-width="400">
            <v-card>
              <v-card-title>Mon formulaire</v-card-title>
              <v-form>
                <v-container>
                  <v-row justify="center">
                    <v-col cols="10">
                      <v-text-field
                        v-model="taskName"
                        label="Nom de la tâche"
                        :counter="100"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="10" justify="center">
                      <v-text-field
                        v-model="taskDescription"
                        label="Description de la tâche"
                        :counter="200"
                        required
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-form>
              <v-card-actions class="mt-10">
                <v-spacer></v-spacer>

                <v-btn color="red darken-1" text @click="dialog = false">
                  Annuler
                </v-btn>

                <v-btn color="green darken-1" text @click="addTask">
                  Enregistrer
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>

          <!-- Modification tâches-->
          <v-dialog v-model="edit" max-width="400">
            <Form title="Modifier mon formulaire" :task="editingTask" @save="editTask"></Form>
          </v-dialog>

          <!-- Message tâche enregistrée-->
          <v-snackbar v-model="saveMsg" :timeout="timeout">
            Votre tâche est bien enregistrée
            <template v-slot:action="{ attrs }">
              <v-btn color="blue" text v-bind="attrs" @click="saveMsg = false">
                Fermer
              </v-btn>
            </template>
          </v-snackbar>

          <!-- Message tâche supprimée-->
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

          <!-- Message tâche modifiée-->
          <v-snackbar v-model="editMsg" :timeout="timeout">
            Votre tâche est bien modifiée
            <template v-slot:action="{ attrs }">
              <v-btn
                color="blue"
                text
                v-bind="attrs"
                @click="editeMsg = false"
              >
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

import Form from '~/components/Form.vue'

export default {
  name: "indexTodoList",

  components : {Form},

  data() {
    return {
      dialog: false,
      taskName: "",
      taskDescription: "",
      tasks: [],
      saveMsg: false,
      deleteMsg: false,
      editMsg: false,
      edit: false,
      editingTask: {},
      indexEditingTask: undefined,
      tasksArchive: [],
    };
  },

  methods: {
    addTask() {
      // On veut prendre le contenu de taskName (qui se rempli grace au v-model dans la dialog)
      // Pour le mettre dans notre tableau de tasks qui est initialement vide, apres avoir cliqué sur enregistré
      // Notre taskname est => this.taskName

      // Un tableau d'objet :
      // tasks[
      // 0: {
      //    id: 0
      //    name: 'coucou',
      //    description: 'description'
      // }
      // 1:
      // ]

      const completeTask = {
        name: this.taskName,
        description: this.taskDescription,
        isArchive: false,
      };

      this.tasks.push(completeTask);
      this.dialog = false;
      this.taskName = "";
      this.taskDescription = "";
      this.saveMsg = true;
    },

    openEditorDialog(task, index) {
      this.editingTask = task;
      this.indexEditingTask = index
      this.edit = true;
    },

    editTask(task) {
      //this.tasks.splice(this.indexEditingTask, 1, task);
      //this.tasks[this.indexEditingTask]=task;
      //this.$set(this.tasks[this.indexEditingTask])

      const index = this.tasks.indexOf(this.editingTask);

      this.tasks.splice(index, 1, task);

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
        const indexArchive = this.tasksArchive.indexOf(task)
        this.tasksArchive.splice(indexArchive, 1)
      }
      
    }
  },
};
</script>
