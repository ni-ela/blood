<template>
  <v-card flat width="518" class="ml-4">
    <v-form id="form" ref="form">
      <v-text-field
        name="numero_da_bolsa"
        validate-on-blur
        v-model="numero_da_bolsa"
        label="Número da bolsa"
        outlined
      ></v-text-field>
      <v-text-field
        name="codigo_do_produto"
        validate-on-blur
        v-model="codigo_do_produto"
        label="Código do produto"
        outlined
      ></v-text-field>
      <v-text-field
        name="sequencial_separacao"
        validate-on-blur
        v-model="sequencial_separacao"
        label="Sequencial de separação"
        outlined
      ></v-text-field>
      <v-btn
        depressed
        type="submit"
        form="form"
        width="275"
        color="teal accent-4 py-4 text-center text-uppercase"
        dark
        @click="sendCheckIntegrity()"
      >
        VERIFICAR INTEGRIDADE
      </v-btn>
    </v-form>
  </v-card>
</template>

<script>
import { mapActions } from 'vuex';

export default {
  data() {
    return {
      numero_da_bolsa: '',
      codigo_do_produto: '',
      sequencial_separacao: '',
      visible: true,
      loading: false,
    };
  },
  props: {
    idrequisicao: {
      type: Number,
      required: false,
    },
    id_produto_requisicao: {
      type: Number,
      required: false,
    },
  },

  methods: {
    ...mapActions('modal', ['closeCheckIntegrity']),
    ...mapActions('requisition', ['addCheckIntegrity']),
    close() {
      this.visible = false;
      this.closeCheckIntegrity();
    },
    async sendCheckIntegrity() {
      try {
        if (this.$refs.form.validate()) {
          let product = {
            numero_da_bolsa: this.numero_da_bolsa,
            codigo_do_produto: this.codigo_do_produto,
            id_requisition: Number(this.idrequisicao),
            id_produto_requisicao: Number(this.id_produto_requisicao),
            sequencial_separacao: Number(this.sequencial_separacao),
          };

          console.log('a ser enviado', product);
          await this.addCheckIntegrity(product);
        }
      } catch {
       // this.close();
      }
    },
  },
};
</script>

<style>
</style>