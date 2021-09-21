<template>
  <v-card>
    <v-card-title>{{ title }}</v-card-title>
    <v-form ref="form">
      <v-container>
        <v-row justify="center">
          <v-col cols="10">
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

  props: {
    title: "",
    task: {}
  },

  data() {
    return {
      taskToSave: {},
    };
  },
  watch: {
    task: function(val, oldval) {
      this.taskToSave = { ...val };
    }
  },
  created() {
    Object.assign(this.taskToSave, this.task || {});
  },
  methods: {
    save() {
      this.$emit("save", this.taskToSave);
      this.$refs.form.reset();
    }
  }
};
</script>
