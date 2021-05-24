<template>
   <v-card flat  width="518" class="ml-4">
  <v-form v-model="valid">
    <v-text-field  label="Número da bolsa" outlined></v-text-field>
    <v-text-field label="Código do produto" outlined></v-text-field>
    <v-text-field label="Sequencial de separação" outlined></v-text-field>
  <v-btn
            depressed
            width="275"

      color="teal accent-4 py-4 text-center text-uppercase"
      dark
            @click="console.log('hi')"
    >
      VERIFICAR INTEGRIDADE
    </v-btn>
  </v-form>
  </v-card>
</template>