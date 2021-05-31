<template>
  <div>
    <v-card flat v-if="content === 'listBloodComponents'">
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
              <span
                class="teal--text"
                v-if="product.status_da_enfermagem === 'S'"
                >Produto conforme</span
              >
              <span
                class="error--text"
                v-else-if="product.status_da_enfermagem === 'N'"
                >Produto não conforme para transfusão</span
              >
              <span class="orange--text lighten-3" v-else
                >Pendente de inspeção</span
              >
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
              @click="content = 'checkFormIntegrity'"
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
              @click="() => handleNonCompliance(product)"
            >
              <v-icon dark> mdi-alert-octagon </v-icon>
            </v-btn>
          </div>
        </v-list-item>
        <v-divider
          class="mr-3 ml-3"
          v-if="index < bloodComponents.length - 1"
        ></v-divider>
      </v-list>
    </v-card>
    <div v-else-if="content === 'checkFormIntegrity'">
      <CheckForm :requisition="requisition" :product="product" />
    </div>
    <div v-else>
      <ReportNonCompliance :requisition="requisition" :product="product" />
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style>
</style>
<script>
import { mapGetters, mapActions } from 'vuex';
import CheckForm from '@/components/application/administrator/pretransfusion/productIntegrity/CheckForm.vue';
import ReportNonCompliance from '@/components/application/administrator/pretransfusion/productIntegrity/ReportNonCompliance.vue';

export default {
  data() {
    return {
      visible: true,
      content: 'listBloodComponents',
      product: {},
    };
  },
  props: {
    requisition: {
      type: Object,
      required: false,
    },
  },
  components: {
    CheckForm,
    ReportNonCompliance,
  },
  computed: {
    ...mapGetters('requisition', ['getBloodComponentsIntegrity']),
    bloodComponents() {
      if (this.getBloodComponentsIntegrity.integrityProducts) {
        return this.getBloodComponentsIntegrity.integrityProducts;
      } else {
        this.close();
        return {};
      }
    },
  },
  methods: {
    ...mapActions('modal', [
      'closeCheckIntegrity',
      'checkForm',
      'reportNonCompliance',
    ]),
    close() {
      this.visible = false;
      this.closeCheckIntegrity();
    },
    ...mapActions('requisition', ['getAllNonConformities']),
    async handleNonCompliance(item) {
      if (this.getBloodComponentsIntegrity.integrityProducts) {
        this.product = item;
        await this.getAllNonConformities();
        this.content = '';
      }
    },
  },
};
</script>

<style>
</style>
