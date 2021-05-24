<template>
  <v-card flat>
    <v-list
      v-for="(product, index) in bloodComponents"
      :key="product.numero_da_bolsa"
    >
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="mb-3">{{
            product.numero_da_bolsa
          }}</v-list-item-title>
          <v-list-item-subtitle
            >Código do produto:
            {{ product.codigo_do_produto }}</v-list-item-subtitle
          >
          <v-list-item-subtitle
            >Sequencial:
            {{ product.sequencial_separacao }}</v-list-item-subtitle
          >
          <v-list-item-subtitle>
            Status da enfermagem:
            <span class="teal--text" v-if="product.status_da_enfermagem === 'S'"
              >Produto conforme</span
            >
            <span
              class="error--text"
              v-else-if="product.status_da_enfermagem === 'N'"
              >Produto não conforme para transfusão</span
            >
            <span class="orange--text lighten-3" v-else>Pendente de inspeção</span>
          </v-list-item-subtitle>
          <v-list-item-subtitle class="d-flex justify-start">
            <v-chip
              v-if="product.compativel === 'S'"
              class="mt-3"
              color="teal accent-4"
              dark
            >
              Bolsa compatível
            </v-chip>
            <v-chip class="mt-3" color="error" dark v-else>
              Bolsa incompatível
            </v-chip>
          </v-list-item-subtitle>
        </v-list-item-content>
        <div
          class="d-flex justify-end"
          v-if="product.status_da_enfermagem === null"
        >
          <v-btn
            class="mx-1"
            fab
            depressed
            dark
            data-testid="check-button"
            color="primary"
            @click="console.log('hi', id_produto_requisicao, sigla_produto)"
          >
            <v-icon dark>mdi-qrcode</v-icon>
          </v-btn>
          <v-btn
            class="mx-1"
            fab
            depressed
            color="error"
            dark
            data-testid="check-button"
            @click="console.log('hi')"
          >
            <v-icon dark> mdi-alert-octagon </v-icon>
          </v-btn>
        </div>

      </v-list-item>
        <v-divider class="mr-3 ml-3" v-if="index < bloodComponents.length - 1"></v-divider>
    </v-list>
  </v-card>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';
export default {
  /*props: {
    requisition: {
      type: Object,
      required: false,
    },
    integrityProducts: {
      type: Array,
      required: false,
    }
  },*/
  computed: {
    ...mapGetters('requisition', ['getBloodComponentsIntegrity']),
    bloodComponents() {
      console.log(
        'aa',
        this.getBloodComponentsIntegrity.integrityProducts[0].codigo_do_produto
      );
      return this.getBloodComponentsIntegrity
        ? this.getBloodComponentsIntegrity.integrityProducts
        : {};
    },
  },
  methods: {
    ...mapActions('modal', ['checkForm', 'reportNonCompliance']),
    parseDate(value) {
      return parseDateToDDMMYYYY(value);
    },
  },
};
</script>

<style>
</style>
