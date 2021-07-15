<template>
  <v-app>
    <v-main>
      <v-container>
        <v-card elevation="2" class="mx-auto" max-width="344" tile>
          <v-list dense>
            <v-list-item-group>
              <!-- Pour chaque task dans mon tableau de tasks[] // On l'a appelé task, mais on aurait pu l'appeler tache-->
              <v-list-item v-for="(task, index) in tasks" :key="index">
                <v-list-item-content>
                  <v-list-item-title
                    >{{ task }}
                    <v-list-item-icon>
                      <v-icon color="red" @click="removeTask(index)"
                        >mdi-delete</v-icon
                      >
                    </v-list-item-icon>
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
          <v-card-actions>
            <v-btn rounded color="light-green" dark @click.stop="dialog = true">
              Ajouter une tâche
            </v-btn>
          </v-card-actions>
          <v-dialog v-model="dialog" max-width="290">
            <v-card>
              <v-card-title>Mon formulaire</v-card-title>
              <v-form>
                <v-container>
                  <v-row>
                    <v-col cols="12">
                      <v-text-field
                        v-model="taskName"
                        label="Nom de la tâche"
                        :counter="100"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
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
              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn color="green darken-1" text @click="dialog = false">
                  Annuler
                </v-btn>

                <v-btn color="green darken-1" text @click="addTask">
                  Enregistrer
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "indexTodoList",

  data() {
    return {
      dialog: false,
      taskName: "",
      taskDescription: "",
      tasks: [],
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

      this.tasks.push(this.taskName);
      this.dialog = false;
      this.taskName = "";
    },

    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>
