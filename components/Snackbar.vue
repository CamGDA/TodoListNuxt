<template v-slot:action="{ attrs }">
    <v-btn color="blue" text v-bind="attrs" @click="msg = false">
    Fermer
    </v-btn>
</template>

<script>
export default {
    name: "Snackbar",
}


</script>