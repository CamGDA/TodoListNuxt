<template>
     <v-card>
        <v-card-title>{{title}}</v-card-title>
        <v-form>
        <v-container>
            <v-row justify="center">
            <v-col cols="10">
                <v-text-field
                @input="taskToSave.name=$event"
                label="Nom de la tâche"
                :value="task.name"
                :counter="100"
                required
                ></v-text-field>
            </v-col>
            <v-col cols="10" justify="center">
                <v-text-field
                @input="taskToSave.description=$event"
                label="Description de la tâche"
                :value="task.description"
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

        <v-btn color="green darken-1" text @click="$emit('save',taskToSave)">
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
        task: {},
    },

    data() {
        return {
            taskToSave: {},
        }
    }
}
</script>