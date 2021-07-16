<template>
  <v-card>
    <v-card-title>{{ title }}</v-card-title>
    <v-form ref="form">
      <v-container>
        <v-row justify="center">
          <v-col cols="10">
            <!-- @input c'est l'evenement que mon champ ecoute. Par exemple quand j'appui sur la lettre A.  -->
            <!-- Avec @input on modifie notre object taskToSave (qui a la toute base etait pareil que task) -->
            <!-- :value c'est la valeur par defaut du champ quand il est prerempli (notamment pour la modification) -->
            <v-text-field
              v-model="taskToSave.name"
              label="Nom de la tâche"
              :counter="100"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="10" justify="center">
            <v-text-field
              v-model="taskToSave.description"
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

      <v-btn color="red darken-1" text @click="edit = false">
        Annuler
      </v-btn>

      <v-btn color="green darken-1" text @click="save">
        Enregistrer
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "Form",

  // Ce dont le composant a besoin pour marcher
  props: {
    title: "",
    task: {}
  },

  data() {
    return {
      taskToSave: {}
    };
  },
  // Watcher
  // Surveille this.task et si ca bouge, tu me gardes la nouvelle valeur (val) et on la met dans taskToSave
  watch: {
    task: function(val, oldval) {
      this.taskToSave = { ...val };
    }
  },
  // Ne se declenche qu'en chargement de la page donc 1 fois. (donc si on rechange de tache, ca ne marche pas)
  created() {
    Object.assign(this.taskToSave, this.task || {});
  },
  methods: {
    save() {
      console.log("emit");
      this.$emit("save", this.taskToSave);
      this.$refs.form.reset();
    }
  }
};
</script>
