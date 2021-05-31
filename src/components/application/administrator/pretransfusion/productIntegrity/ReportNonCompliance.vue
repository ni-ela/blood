<template>
  <v-card flat width="518" class="ml-4">
    <v-list subheader>
      <v-subheader class="text-subtitle-1 pl-0"
        >Aspectos de não conformidade nesse produto:</v-subheader
      >
      <v-list-item-group
        multiple
        v-for="nonConformity in nonConformities"
        :key="nonConformity.codigo"
      >
        <v-list-item
          class="pl-0"
          @change="onChange(nonConformity.codigo)"
          :key="nonConformity.codigo"
        >
          <template v-slot:default="{ active }">
            <v-list-item-action class="mr-1">
              <v-checkbox :input-value="active" color="primary"></v-checkbox>
            </v-list-item-action>

            <v-list-item-content>
              <v-list-item-title>
                {{ nonConformity.descricao }}
              </v-list-item-title>
            </v-list-item-content>
          </template>
        </v-list-item>
      </v-list-item-group>
      <v-btn
        class="text-center text-uppercase mt-3"
        color="error"
        large
        @click.prevent="sendList()"
        block
        depressed
        dark
      >
        REPORTAR INTEGRIDADE
      </v-btn>
    </v-list>
  </v-card>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  data() {
    return {
      visible: true,
      codigos: [],
      loading: false,
    };
  },
  props: {
    requisition: {
      type: Object,
      required: false,
    },
    product: {
      type: Object,
      required: false,
    },
  },

  /*  async submit() {
      if (this.codigos) {
        let codigos_nao_conformidade = this.codigos.map(function (value) {
          return { codigo: `${value}` };
        });
        try { */
  //fechar modal de listagem de produto e fazer check de integridade
  // this.close();
  /*  let send = {
            id_produto_requisicao: this.product.id_produto_requisicao,
            numero_da_bolsa: this.product.numero_da_bolsa,
            sequencial_separacao: this.product.sequencial_separacao,
            codigo_produto: this.product.codigo_do_produto,
            codigos_nao_conformidade: codigos_nao_conformidade,
          };
          console.log('send', send);
          await this.addNonCompliance(send); */

  //this.loading = true;

  /* const notDeletedBeds = this.clinicSelected.leitos.filter(
            (element) => {
              return this.editClinic.leitos.find(
                (leito) => leito == element.descricao
              );
            }
          );

          const newBeds = this.editClinic.leitos.filter((element) => {
            return !this.clinicSelected.leitos.find(
              (leito) => leito.descricao == element
            );
          });

          await this.editClinicFetch({
            idClinica: this.clinicSelected.id,
            descricao: this.editClinic.description,
            ativo: this.editClinic.status,
            novosLeitos: newBeds.map((element) => {
              return {
                descricao: element,
                ativo: 'T',
              };
            }),
            leitos: notDeletedBeds.map((bed) => {
              return { idLeito: bed.idleito };
            }),
          });
          this.closeModal(); */
  /*  } catch (error) {
          this.keepModalOpen();
        }
      }
    }, */

  methods: {
    ...mapActions('modal', ['closeCheckIntegrity']),
    ...mapActions('requisition', ['addNonCompliance']),
    close() {
      this.visible = false;
      this.closeCheckIntegrity();
    },
    async onChange(val) {
      if (this.codigos.indexOf(val) < 0) {
        this.codigos.push(val);
        console.log('insere', val);
      } else {
        console.log('tira', val);
        this.codigos = this.codigos.filter(function (value) {
          return value !== val;
        });
      }
    },
    async sendList() {
      try {
        let codigos_nao_conformidade = this.codigos.map(function (value) {
          return { codigo: `${value}` };
        });
        let send = {
          id_produto_requisicao: this.product.id_produto_requisicao,
          numero_da_bolsa: this.product.numero_da_bolsa,
          sequencial_separacao: this.product.sequencial_separacao,
          codigo_produto: this.product.codigo_do_produto,
          codigos_nao_conformidade: codigos_nao_conformidade,
        };
        await this.addNonCompliance(send);
        //fazer check de integridade
      } catch {
       this.close();
      }
      setTimeout(this.close(), 7000);
    },
  },
  computed: {
    ...mapGetters('requisition', ['getNonConformities']),
    nonConformities() {
      if (this.getNonConformities) {
        return this.getNonConformities.nonConformities;
      } else {
       // this.close();
        return [];
      }
    },
  },
};
</script>

<style>
</style>